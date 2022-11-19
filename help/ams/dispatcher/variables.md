---
title: Utilisation et compréhension des variables dans votre configuration AEM Dispatcher
description: Découvrez comment utiliser les variables dans vos fichiers de configuration de modules Apache et Dispatcher pour les placer au niveau supérieur.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# Utilisation et compréhension des variables

[Table des matières](./overview.md)

[&lt;- Précédent : Compréhension du cache](./understanding-cache.md)

Ce document explique comment tirer parti de la puissance des variables dans le serveur web Apache et dans les fichiers de configuration du module de Dispatcher.

## Variables

Apache prend en charge les variables et depuis la version 4.1.9 du module Dispatcher, il les prend également en charge !

Nous pouvons les exploiter pour réaliser une tonne de choses utiles telles que :

- Assurez-vous que tout ce qui est spécifique à l’environnement n’est pas en ligne dans les configurations, mais extrait pour vous assurer que les fichiers de configuration du développement fonctionnent en production avec la même sortie fonctionnelle.
- Active/désactive les fonctionnalités et modifie les niveaux de journal des fichiers non modifiables fournis par AMS et ne vous permet pas de modifier.
- Modifier les inclusions à utiliser en fonction de variables comme `RUNMODE` et `ENV_TYPE`
- Correspondance `DocumentRoot`s et `VirtualHost` Noms DNS entre les configurations Apache et les configurations de module.

## Utilisation des variables de ligne de base

Étant donné que les fichiers de ligne de base AMS sont en lecture seule et immuables, il existe des fonctionnalités qui peuvent être activées et désactivées et configurées en modifiant les variables qu’elles utilisent.

### Variables de ligne de base

Les variables par défaut AMS sont déclarées dans le fichier `/etc/httpd/conf.d/variables/ootb.vars`.  Ce fichier n’est pas modifiable, mais il existe pour s’assurer que les variables n’ont pas de valeurs &quot;null&quot;.  Ils sont inclus d’abord, puis après que nous les incluions. `/etc/httpd/conf.d/variables/ams_default.vars`.  Vous pouvez modifier ce fichier pour modifier les valeurs de ces variables ou même inclure les mêmes noms et valeurs de variable dans votre propre fichier !

Voici un exemple du contenu du fichier : `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Exemple 1 - Forcer SSL

Les variables affichées ci-dessus `AUHOR_FORCE_SSL`ou `PUBLISH_FORCE_SSL` peut être défini sur 1 pour activer les règles de réécriture qui forcent les utilisateurs finaux à être redirigés vers https lorsqu’ils arrivent sur une requête http.

Voici la syntaxe du fichier de configuration qui permet à ce bouton de fonctionner :

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

Comme vous pouvez le constater, les règles de réécriture incluent le code permettant de rediriger le navigateur des utilisateurs finaux, mais la variable définie sur 1 permet d’utiliser ou non le fichier.

### Exemple 2 - Niveau de journalisation

Les variables `DISP_LOG_LEVEL` peut être utilisé pour définir ce que vous souhaitez avoir pour le niveau de journal réellement utilisé dans la configuration en cours d’exécution.

Voici l’exemple de syntaxe qui existe dans les fichiers de configuration de ligne de base ams :

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Si vous devez augmenter le niveau de journalisation de Dispatcher, mettez simplement à jour la variable `ams_default.vars` variable `DISP_LOG_LEVEL` au niveau que vous souhaitez.

Les exemples de valeurs peuvent être un entier ou le mot :

| Niveau de journal | Valeur entière | Valeur Word |
| --- | --- | --- |
| Trace | 4 | trace |
| Déboguer | 3 | debug |
| Infos | 2 | informations |
| Avertissement | 1 | warn |
| Erreur | 0 | erreur |

### Exemple 3 - Listes autorisées

Les variables `AUTHOR_WHITELIST_ENABLED` et `PUBLISH_WHITELIST_ENABLED` peut être défini sur 1 pour appliquer des règles de réécriture qui incluent des règles permettant d’autoriser ou d’interdire le trafic d’utilisateurs finaux en fonction de leur adresse IP.  Le fait d’activer cette fonction doit être combiné à la création d’un fichier de règles de liste autorisée pour qu’il soit inclus.

Voici quelques exemples de syntaxe sur la manière dont la variable active les inclusions des fichiers de liste autorisée et un exemple de fichier de liste autorisée.

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

Comme vous pouvez le voir `sample_whitelist.rules` applique la restriction IP, mais le fait de changer de variable permet de l’inclure dans la variable `sample.vhost`

## Où placer les variables

### Arguments de démarrage du serveur web

AMS placera des variables spécifiques au serveur/à la topologie dans les arguments de démarrage du processus Apache dans le fichier . `/etc/sysconfig/httpd`

Ce fichier comporte des variables prédéfinies, comme illustré ci-dessous :

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

En raison du fait que ce fichier n’est inclus que lorsque le service démarre.  Un redémarrage du service est nécessaire pour récupérer les modifications.  Cela signifie qu’une recharge n’est pas suffisante, mais qu’un redémarrage est nécessaire.
</div>

### Fichiers variables (`.vars`)

Les variables personnalisées fournies par votre code doivent se trouver dans `.vars` fichiers dans le répertoire `/etc/httpd/conf.d/variables/`

Ces fichiers peuvent avoir toutes les variables personnalisées que vous souhaitez et vous trouverez quelques exemples de syntaxe dans les exemples de fichiers suivants :

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

Lors de la création de vos propres fichiers de variables, nommez-les en fonction de leur contenu et de respecter les normes d’attribution de noms fournies dans le manuel. [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  Dans l’exemple ci-dessus, vous pouvez constater que le fichier de variables héberge les différentes entrées DNS en tant que variables à utiliser dans les fichiers de configuration.

## Utilisation de variables

Maintenant que vous avez défini vos variables dans vos fichiers de variables, vous allez savoir comment les utiliser correctement dans vos autres fichiers de configuration.

Nous utiliserons l’exemple `.vars` fichiers ci-dessus pour illustrer un cas d’utilisation approprié.

Nous voulons inclure toutes les variables basées sur l’environnement globalement ; nous allons créer le fichier . `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Nous savons que lorsque le service httpd démarre, il extrait les variables définies par AMS dans `/etc/sysconfig/httpd` et possède le jeu de variables de `ENV_TYPE` et `RUNMODE`

Lorsque cette variable est globale `.conf` est extrait plus tôt, car l’ordre d’inclusion des fichiers dans `conf.d` est un ordre de chargement alphanumérique signifiant 000 dans le nom du fichier assure qu&#39;il se charge avant les autres fichiers du répertoire.

L’instruction d’inclusion utilise également une variable dans le nom de fichier.  Cela peut modifier le fichier qu’il chargera en fonction de la valeur contenue dans la variable `ENV_TYPE` et `RUNMODE` .

Si la variable `ENV_TYPE` la valeur est `dev` puis le fichier utilisé est :

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Si la variable `ENV_TYPE` la valeur est `stage` puis le fichier utilisé est :

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Si la variable `RUNMODE` la valeur est `preview` puis le fichier utilisé est :

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Lorsque ce fichier sera inclus, il nous permettra d’utiliser les noms de variables qui étaient stockés à l’intérieur.

Dans notre `/etc/httpd/conf.d/available_vhosts/weretail.vhost` Nous pouvons remplacer la syntaxe normale qui ne fonctionnait que pour dev :

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Avec une syntaxe plus récente qui utilise la puissance des variables pour fonctionner pour le développement, l’évaluation et la production :

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

Dans notre `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` Nous pouvons remplacer la syntaxe normale qui ne fonctionnait que pour dev :

```
"dev.weretail.com" 
"dev.weretail.net"
```

Avec une syntaxe plus récente qui utilise la puissance des variables pour fonctionner pour le développement, l’évaluation et la production :

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Ces variables sont réutilisées pour personnaliser considérablement les paramètres d’exécution sans avoir à déployer différents fichiers par environnement.  Vous pouvez essentiellement modéliser vos fichiers de configuration à l’aide de variables et inclure des fichiers en fonction de variables.

## Affichage des valeurs de variable

Parfois, lors de l’utilisation de variables, nous devons rechercher les valeurs qui peuvent se trouver dans nos fichiers de configuration.  Il existe un moyen d’afficher les variables résolues en exécutant les commandes suivantes sur le serveur :

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Présentation des variables dans votre configuration Apache compilée :

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Présentation des variables dans votre configuration de Dispatcher compilée :

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

A partir de la sortie des commandes, vous verrez les différences entre la variable dans le fichier de configuration et la sortie compilée.

Exemple de configuration

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost` :

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

Exécutez maintenant les commandes pour afficher la sortie compilée.

Configuration Apache compilée :

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Configuration Compilée de Dispatcher :

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Suivant -> Purge](./disp-flushing.md)