---
title: Utiliser et comprendre des variables dans votre configuration AEM Dispatcher
description: Découvrez comment utiliser les variables dans vos fichiers de configuration de modules Apache et Dispatcher pour les placer au niveau supérieur.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 294
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 100%

---

# Utiliser et comprendre les variables

[Table des matières](./overview.md)

[&lt;- Précédent : Présentation du cache](./understanding-cache.md)

Ce document explique comment tirer parti de la puissance des variables dans le serveur web Apache et dans les fichiers de configuration du module Dispatcher.

## Variables

Tout comme la version 4.1.9 du module Dispather, Apache prend en charge les variables.

Nous pouvons les exploiter pour réaliser un grand nombre de choses utiles telles que :

- Vérifier que tout ce qui est spécifique à l’environnement n’est pas en ligne dans les configurations, mais extrait pour garantir que les fichiers de configuration de développement fonctionnent dans la production avec la même sortie fonctionnelle.
- Modifier les fonctionnalités et les niveaux de journal des fichiers non modifiables qu’AMS fournit et ne vous permet pas de modifier.
- Modifier les inclusions à utiliser en fonction de variables comme `RUNMODE` et `ENV_TYPE`.
- Faire correspondre les noms DNS `DocumentRoot` et `VirtualHost` entre les configurations Apache et de module.

## Utiliser des variables de ligne de base

Étant donné que les fichiers de ligne de base AMS sont en lecture seule et non modifiables, il existe des fonctionnalités qui peuvent être activées ou désactivées et configurées en modifiant les variables qu’elles utilisent.

### Variables de ligne de base

Les variables par défaut AMS sont définies dans le fichier `/etc/httpd/conf.d/variables/ootb.vars`.  Ce fichier n’est pas modifiable, mais il existe pour s’assurer que les variables n’ont pas de valeurs « null ».  Elles sont incluses en premier, puis nous incluons `/etc/httpd/conf.d/variables/ams_default.vars`.  Vous pouvez modifier ce fichier pour modifier les valeurs de ces variables ou même inclure les mêmes noms de variable et valeurs dans votre propre fichier.

Voici un exemple des contenus du fichier `/etc/httpd/conf.d/variables/ams_default.vars` :

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Exemple 1 - Force SSL

Les variables indiquées au-dessus de `AUHOR_FORCE_SSL`, ou `PUBLISH_FORCE_SSL`, peuvent être définies sur 1 pour activer les règles de réécriture qui forcent les utilisateurs et utilisatrices finaux à être redirigés vers HTTPS lorsqu’ils arrivent avec une requête HTTP.

Voici la syntaxe du fichier de configuration qui permet à ce bouton de fonctionner :

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

Comme vous pouvez le constater, les règles de réécriture incluent le code permettant de rediriger le navigateur des utilisateurs et utilisatrices finaux, mais la variable définie sur 1 permet d’utiliser ou non le fichier.

### Exemple 2 - Niveau de journalisation

Les variables `DISP_LOG_LEVEL` peuvent être utilisées pour définir ce que vous souhaitez avoir pour le niveau de journal réellement utilisé dans la configuration en cours d’exécution.

Voici l’exemple de syntaxe qui existe dans les fichiers de configuration de ligne de base AMS :

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Si vous devez augmenter le niveau de journalisation de Dispatcher, mettez simplement à jour la variable `ams_default.vars` `DISP_LOG_LEVEL` au niveau que vous souhaitez.

Les exemples de valeurs peuvent être un entier ou le mot :

| Niveau de journal | Valeur entière | Valeur de mot |
| --- | --- | --- |
| Trace (suivi) | 4 | trace (suivi) |
| Déboguer | 3 | Déboguer |
| Informations | 2 | Informations |
| Avertissement | 1 | warn (avertissement) |
| Erreur | 0 | Erreur |

### Exemple 3 - Listes autorisées

Les variables `AUTHOR_WHITELIST_ENABLED` et `PUBLISH_WHITELIST_ENABLED` peuvent être définies sur 1 pour appliquer des règles de réécriture qui incluent des règles permettant d’autoriser ou de refuser le trafic de la personne finale en fonction de l’adresse IP.Le fait d’activer cette fonction doit être combiné à la création d’un fichier de règles de liste autorisée pour qu’il soit inclus.

Voici quelques exemples de syntaxe sur la manière dont la variable active les inclusions des fichiers de liste autorisée et un exemple de fichier de liste autorisée :

`sample.vhost` :

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules` :

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

Comme vous pouvez le voir, le fichier `sample_whitelist.rules` applique la restriction IP, mais le fait de changer de variable permet de l’inclure dans `sample.vhost`.

## Où placer les variables ?

### Arguments de démarrage du serveur web

AMS place des variables spécifiques au serveur/à la topologie dans les arguments de démarrage du processus Apache dans le fichier `/etc/sysconfig/httpd`.

Ce fichier comporte des variables prédéfinies, comme illustré ci-dessous :

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

Il ne s’agit pas d’éléments que vous pouvez modifier, mais que vous pouvez exploiter dans vos fichiers de configuration.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

En raison du fait que ce fichier n’est inclus qu’au démarrage du service,un redémarrage du service est obligatoire pour récupérer les modifications.Cela signifie que le fait de charger à nouveau n’est pas suffisant, mais qu’un redémarrage est nécessaire.
</div>

### Fichiers variables (`.vars`)

Les variables personnalisées fournies par votre code doivent se trouver dans les fichiers `.vars` dans le répertoire `/etc/httpd/conf.d/variables/`.

Ces fichiers peuvent avoir toutes les variables personnalisées que vous souhaitez et vous trouverez quelques exemples de syntaxe dans les exemples de fichiers suivants :

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars` :

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars` :

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars` :

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

Lors de la création de vos propres fichiers de variables, nommez-les en fonction de leur contenu et de manière à respecter les normes de nommage fournies dans le manuel [ici](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html?lang=fr#naming-convention).Dans l’exemple ci-dessus, vous pouvez constater que le fichier de variables héberge les différentes entrées DNS en tant que variables à utiliser dans les fichiers de configuration.

## Utiliser des variables

Maintenant que vous avez défini vos variables dans vos fichiers de variables, vous devez apprendre comment les utiliser correctement dans vos autres fichiers de configuration.

Nous utiliserons l’exemple des fichiers `.vars` ci-dessus pour illustrer un cas d’utilisation approprié.

Il faut inclure toutes les variables basées sur l’environnement de manière globale. Nous allons créer le fichier `/etc/httpd/conf.d/000_load_env_vars.conf`.

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Nous savons que lorsque le service HTTPD démarre, il extrait les variables définies par AMS dans `/etc/sysconfig/httpd` et possède le jeu de variables de `ENV_TYPE` et `RUNMODE`.

Lorsque ce fichier global `.conf` est extrait, il l’est en avance, car l’ordre d’inclusion des fichiers dans `conf.d` est un ordre de chargement alphanumérique, ce qui signifie que 000 dans le nom de fichier garantit son chargement avant les autres fichiers du répertoire.

L’instruction d’inclusion utilise également une variable dans le nom de fichier.Cela peut modifier le fichier qu’il chargera en fonction de la valeur contenue dans les variables `ENV_TYPE` et `RUNMODE`.

Si la valeur `ENV_TYPE` est `dev`, le fichier utilisé est alors le suivant :

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Si la valeur `ENV_TYPE` est `stage`, le fichier utilisé est alors le suivant :

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Si la valeur `RUNMODE` est `preview`, le fichier utilisé est alors le suivant :

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Une fois inclus, le fichier nous permettra d’utiliser les noms de variables qui étaient stockés à l’intérieur.

Dans notre fichier `/etc/httpd/conf.d/available_vhosts/weretail.vhost`, nous pouvons remplacer la syntaxe normale qui ne fonctionnait que pour le développement :

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Avec une syntaxe plus récente utilisant la puissance des variables pour le développement, l’évaluation et la production :

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

Dans notre fichier `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any`, nous pouvons remplacer la syntaxe normale qui ne fonctionnait que pour le développement :

```
"dev.weretail.com" 
"dev.weretail.net"
```

Avec une syntaxe plus récente utilisant la puissance des variables pour le développement, l’évaluation et la production :

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Ces variables ont une très grande capacité de réutilisation pour personnaliser les paramètres d’exécution sans avoir à déployer différents fichiers par environnement.Vous pouvez surtout modéliser vos fichiers de configuration grâce aux variables et aux fichiers d’inclusion basés sur des variables.

## Afficher des valeurs de variable

Parfois, lors de l’utilisation de variables, nous devons rechercher les valeurs qui peuvent se trouver dans nos fichiers de configuration.  Il existe un moyen d’afficher les variables résolues en exécutant les commandes suivantes sur le serveur :

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Présentation des variables dans votre configuration Apache compilée :

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Présentation des variables dans votre configuration de Dispatcher compilée :

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

Dans la sortie des commandes, vous verrez les différences entre la variable dans le fichier de configuration et la sortie compilée.

Exemple de configuration

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost` :

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
```

Maintenant, exécutez les commandes pour voir les sorties compilées.

Configuration Apache compilée :

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Configuration Dispatcher compilée :

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Suivant -> Purge](./disp-flushing.md)
