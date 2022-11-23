---
title: Contrôle de l’intégrité du Dispatcher AMS
description: AMS fournit un script cgi-bin de contrôle de l’intégrité que les équilibreurs de charge du cloud exécuteront pour voir si AEM est sain et doit rester en service pour le trafic public.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: d6b7d63ba02ca73d6c1674d90db53c6eebab3bd2
workflow-type: tm+mt
source-wordcount: '1136'
ht-degree: 1%

---

# Contrôle de l’intégrité du Dispatcher AMS

[Table des matières](./overview.md)

[&lt;- Précédent : Fichiers en lecture seule](./immutable-files.md)

Lorsque Dispatcher est installé sur une ligne de base AMS, il est fourni avec quelques loisirs.  L’une de ces fonctionnalités est un ensemble de scripts de contrôle de l’intégrité.
Ces scripts permettent à l’équilibreur de charge qui contourne la pile AEM de savoir quels pattes sont saines et de les garder en service.

![GIF animé affichant le flux de trafic](assets/load-balancer-healthcheck/health-check.gif "Étapes de contrôle de l’intégrité")

## Vérification de l’intégrité de l’équilibreur de charge de base

Lorsque le trafic client passe par Internet pour atteindre votre instance AEM, il passe par un équilibreur de charge.

![L’image affiche le flux de trafic entre Internet et aem via un répartiteur de charge.](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "load-balancer-traffic-flow")

Chaque requête provenant de l’équilibreur de charge tourne vers chaque instance.  L’équilibreur de charge dispose d’un mécanisme de contrôle de l’intégrité intégré pour s’assurer qu’il envoie du trafic vers un hôte sain.

La vérification par défaut est généralement une vérification de port pour voir si les serveurs ciblés dans l’équilibreur de charge écoutent le trafic de port qui se produit (c’est-à-dire TCP 80 et 443).

> `Note:` Bien que cela fonctionne, il n&#39;a pas de réelle jauge sur la santé de l&#39;AEM.  Il vérifie uniquement si Dispatcher (serveur web Apache) est en cours d’exécution.

## Contrôle d’intégrité AMS

Pour éviter d’envoyer du trafic à un dispatcher sain qui dirige une instance d’AEM non saine, AMS a créé quelques ajouts qui évaluent l’intégrité de la jambe et pas seulement celle du dispatcher.

![L’image montre les différentes pièces pour que le contrôle de l’intégrité fonctionne.](assets/load-balancer-healthcheck/health-check-pieces.png "morceaux de contrôle-santé")

Le contrôle de l’intégrité comprend les éléments suivants :
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

Nous allons décrire ce que chaque pièce est configurée et leur importance.

### Package AEM

Pour indiquer si AEM fonctionne, vous devez procéder à une compilation de page de base et diffuser la page.  Adobe Managed Services a créé un package de base contenant la page de test.  La page teste que le référentiel est opérationnel et que les ressources et le modèle de page peuvent être rendus.

![L’image affiche le module AMS dans le gestionnaire de modules CRX](assets/load-balancer-healthcheck/health-check-package.png "pack de contrôle-santé")

Voici la page.  Il affiche l’identifiant du référentiel de l’installation.

![L’image affiche la page représentant AMS](assets/load-balancer-healthcheck/health-check-page.png "health-check-page")

> `Note:` Nous nous assurons que la page ne peut pas être mise en cache.  Il ne vérifierait pas l’état réel si chaque fois il renvoyait une page mise en cache.

C&#39;est le point d&#39;entrée de poids léger que nous pouvons tester pour voir que l&#39;AEM est opérationnel.

### Configuration de l’équilibreur de charge

Nous configurons les équilibreurs de charge pour qu’ils pointent vers un point de terminaison CGI-BIN au lieu d’utiliser une vérification de port.

![L’image affiche la configuration du contrôle de l’intégrité de l’équilibreur de charge AWS.](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![L’image affiche la configuration de contrôle de l’intégrité de l’équilibreur de charge Azure](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Hôtes virtuels Apache Health Check

#### Hôte virtuel CGI-BIN `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

Il s’agit de la variable `<VirtualHost>` Fichier de configuration Apache qui permet l’exécution des fichiers CGI-bin.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` Les fichiers cgi-bin sont des scripts qui peuvent être exécutés.  Il peut s’agir d’un vecteur d’attaque vulnérable et ces scripts qu’AMS utilise ne sont pas publiquement accessibles uniquement par l’équilibreur de charge à tester.


#### Hôtes virtuels de maintenance non sains

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

Ces fichiers sont nommés `000_` comme préfixe exprès.  Il est délibérément configuré pour utiliser le même nom de domaine que le site actif.  Ce fichier a l’intention d’être activé lorsque le contrôle de l’intégrité détecte un problème avec l’un des AEM principaux.  Ensuite, proposez une page d’erreur au lieu d’un code de réponse HTTP 503 sans page.  Il volera le trafic de la normale `.vhost` car il est chargé avant cela. `.vhost` lors du partage du même fichier `ServerName` ou `ServerAlias`.  Résultat : les pages destinées à un domaine particulier accèdent au vhost malsain au lieu de celui par défaut, ce qui génère un trafic normal.

Lorsque les scripts de contrôle de l’intégrité s’exécutent, ils déconnectent leur état d’intégrité actuel.  Une fois par minute, une tâche cronjob s’exécute sur le serveur qui recherche des entrées non saines dans le journal.  S’il détecte que l’instance d’AEM de création n’est pas saine, il active alors le lien symbolique :

Entrée du journal :

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron sélectionne l’erreur et réagit :

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

Vous pouvez contrôler si les sites de création ou de publication peuvent charger cette page d’erreur en configurant le paramètre du mode de rechargement dans `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

Options valides :
- auteur 
   - Il s’agit de l’option par défaut.
   - Cela met en place une page de maintenance pour l’auteur lorsqu’il n’est pas sain.
- publish
   - Cette option met en place une page de maintenance pour l’éditeur lorsqu’il n’est pas sain.
- toutes
   - Cette option met en place une page de maintenance pour l’auteur ou l’éditeur ou les deux s’ils ne sont plus sains.
- aucune
   - Cette option ignore cette fonctionnalité du contrôle de l’intégrité.

Lorsque vous observez le `VirtualHost` pour ces paramètres, ils chargent le même document qu’une page d’erreur pour chaque requête qui se produit lorsqu’elle est activée :

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

Le code de réponse est toujours un `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

Au lieu d’une page vierge, ils obtiendront cette page à la place.

![L’image affiche la page de maintenance par défaut.](assets/load-balancer-healthcheck/unhealthy-page.png "page malsaine")

### Scripts CGI-Bin

Il existe 5 scripts différents qui peuvent être configurés par l’ingénieur du service client dans les paramètres de l’équilibreur de charge. Ils modifient le comportement ou les critères lorsque vous souhaitez extraire un répartiteur de l’équilibreur de charge.

#### /bin/checkauthor

Lorsqu’il est utilisé, ce script vérifie et consigne toutes les instances qu’il dirige, mais ne renvoie une erreur que si la variable `author` L’instance AEM n’est pas saine

> `Note:` Gardez à l’esprit que si l’instance d’AEM de publication n’était pas saine, Dispatcher resterait en service pour permettre le trafic vers l’instance d’AEM de création.

#### /bin/checkpublish (par défaut)

Lorsqu’il est utilisé, ce script vérifie et consigne toutes les instances qu’il dirige, mais ne renvoie une erreur que si la variable `publish` L’instance AEM n’est pas saine

> `Note:` Gardez à l’esprit que si l’instance d’AEM de création n’était pas saine, Dispatcher resterait en service pour autoriser le trafic vers l’instance d’AEM de publication.

#### /bin/checkether

Lorsqu’il est utilisé, ce script vérifie et consigne toutes les instances qu’il dirige, mais ne renvoie une erreur que si la variable `author` ou le `publisher` L’instance AEM n’est pas saine

> `Note:` Gardez à l’esprit que si l’instance d’AEM de publication ou l’instance d’AEM de création n’était pas saine, le dispatcher se retirait du service.  En d’autres termes, si l’un d’eux était sain, il ne recevrait pas non plus de trafic.

#### /bin/checkboth

Lorsqu’il est utilisé, ce script vérifie et consigne toutes les instances qu’il dirige, mais ne renvoie une erreur que si la variable `author` et le `publisher` Les instances AEM ne sont pas saines

> `Note:` Gardez à l’esprit que si l’instance d’AEM de publication ou l’instance d’AEM de création n’était pas saine, Dispatcher ne se retirait pas du service.  En d’autres termes, si l’un d’eux n’était pas sain, il continuerait à recevoir du trafic et à envoyer des erreurs aux personnes demandant des ressources.

#### /bin/health

Lorsqu’il est utilisé, ce script vérifie et consigne toutes les instances qu’il dirige, mais il renvoie simplement sain, que AEM renvoie ou non une erreur.

> `Note:` Ce script est utilisé lorsque le contrôle de l’intégrité ne fonctionne pas comme vous le souhaitez et permet à un remplacement de conserver AEM instances dans l’équilibreur de charge.