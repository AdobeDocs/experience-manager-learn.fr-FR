---
title: Disposition de base du fichier d’AMS Dispatcher
description: Découvrez la disposition de base des fichiers Apache et Dispatcher.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 439
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1132'
ht-degree: 100%

---

# Disposition de base du fichier

[Table des matières](./overview.md)

[&lt;- Précédent : Qu’est-ce que le « Dispatcher » ?](./what-is-the-dispatcher.md)

Ce document explique l’ensemble de fichiers de configuration standard AMS et la réflexion derrière cette norme de configuration.

## Structure de dossiers Enterprise Linux par défaut

Dans AMS, l’installation de base utilise Enterprise Linux comme système d’exploitation de base. Lors de l’installation du serveur web Apache, un fichier d’installation par défaut est défini. Voici les fichiers par défaut qui sont installés lors de l’installation des RPM de base fournis par le référentiel yum.

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

Lorsque vous suivez et respectez la conception/structure de l’installation, les avantages sont les suivants :

- Prise en charge plus facile d’une disposition prévisible.
- Immédiatement compréhensible par toute personne ayant travaillé sur des installations HTTPD Enterprise Linux dans le passé.
- Permet des cycles de correction entièrement pris en charge par le système d’exploitation sans conflit ni réglages manuels.
- Évite les violations SELinux de contextes de fichiers mal étiquetés.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>
Les images des serveurs Adobe Managed Services comportent généralement de petits lecteurs racines du système d’exploitation.Nous mettons nos données dans un volume distinct qui est généralement monté dans « /mnt ».
Ensuite, nous utilisons ce volume au lieu des valeurs par défaut pour les répertoires par défaut suivants.

`DocumentRoot`
- Valeur par défaut : `/var/www/html`
- AMS : `/mnt/var/www/html`

`Log Directory`
- Par défaut : `/var/log/httpd`
- AMS : `/mnt/var/log/httpd`

Gardez à l’esprit que les anciens et les nouveaux répertoires sont mappés à nouveau au point de montage d’origine pour éliminer toute confusion.
L’utilisation d’un volume distinct n’est pas essentielle, mais il est intéressant de la prendre en considération.
</div>

## Modules complémentaires AMS

Modules complémentaires AMS ajoutés à l’installation de base du serveur web Apache.

### Racines de document

Racines de document AMS par défaut :
- Création :
   - `/mnt/var/www/author/`
- Publication :
   - `/mnt/var/www/html/`
- Maintenance de la capture et du contrôle d’intégrité
   - `/mnt/var/www/default/`

### Évaluation et activation des répertoires VirtualHost

Les répertoires suivants vous permettent de créer des fichiers de configuration avec une zone d’évaluation que vous pouvez utiliser sur les fichiers et activer uniquement lorsqu’ils sont prêts.
- `/etc/httpd/conf.d/available_vhosts/`
   - Ce dossier héberge tous vos fichiers/VirtualHost appelés `.vhost`.
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Lorsque vous êtes en mesure d’utiliser les fichiers `.vhost`, vous disposez d’un lien symbolique de dossier `available_vhosts` que vous utilisez à l’aide d’un chemin d’accès relatif dans le répertoire `enabled_vhosts`.

### Répertoires `conf.d` additionnels

Il existe d’autres éléments communs aux configurations Apache et nous avons créé des sous-répertoires pour permettre une façon propre de séparer ces fichiers et de ne pas avoir tous les fichiers dans un seul répertoire.

#### Répertoire de réécritures

Ce répertoire peut contenir tous les fichiers `_rewrite.rules` que vous créez qui contiennent votre syntaxe RewriteRulesyntax classique impliquant le module [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) des serveurs web Apache.

- `/etc/httpd/conf.d/rewrites/`

#### Répertoire des listes autorisées

Ce répertoire peut contenir tous les fichiers `_whitelist.rules` que vous créez qui contiennent votre syntaxe classique `IP Allow` ou `Require IP` impliquant les [contrôles d’accès](https://httpd.apache.org/docs/2.4/howto/access.html) des serveurs web Apache.

- `/etc/httpd/conf.d/whitelists/`

#### Répertoire des variables

Ce répertoire peut contenir tous les fichiers `.vars` que vous créez qui contiennent des variables que vous pouvez utiliser dans vos fichiers de configuration.

- `/etc/httpd/conf.d/variables/`

### Répertoire de configuration spécifique au module de Dispatcher

Le serveur web Apache est très extensible et lorsqu’un module comporte de nombreux fichiers de configuration, il est recommandé de créer votre propre répertoire de configuration sous le répertoire de base d’installation plutôt que d’encombrer le répertoire par défaut.

Nous suivons les bonnes pratiques et créons le nôtre.

#### Répertoire du fichier de configuration du module

- `/etc/httpd/conf.dispatcher.d/`

#### Évaluation et batterie activée

Les répertoires suivants vous permettent de créer des fichiers de configuration avec une zone d’évaluation que vous pouvez utiliser sur les fichiers et activer uniquement lorsqu’ils sont prêts.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Ce dossier héberge l’ensemble de vos fichiers `/myfarm {` appelés `_farm.any`.
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Lorsque vous êtes en mesure d’utiliser le fichier de batterie, vous devez, dans le dossier available_farms, établir lien symbolique à l’aide d’un chemin relatif dans le répertoire enabled_farms.

### Répertoires `conf.dispatcher.d` supplémentaires

Il existe d’autres éléments qui sont des sous-sections des configurations de fichiers de ferme de Dispatcher et nous avons créé des sous-répertoires pour permettre de clairement séparer ces fichiers et de ne pas avoir tous les fichiers dans un seul répertoire.

#### Répertoire du cache

Ce répertoire contient l’ensemble des fichiers `_cache.any` et `_invalidate.any` que vous créez et qui contiennent vos règles sur la manière dont le module doit gérer les éléments de mise en cache provenant d’AEM ainsi que la syntaxe des règles d’invalidation.Plus de détails sur cette section sont disponibles [ici](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#configuring-the-dispatcher-cache-cache).

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Répertoire des en-têtes client

Ce répertoire peut contenir tous les fichiers `_clientheaders.any` que vous créez et qui contiennent des listes d’en-têtes client que vous souhaitez transmettre à AEM lorsqu’une requête arrive.Vous trouverez plus d’informations sur cette section [ici](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr).

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Répertoire des filtres

Ce répertoire peut contenir tous les fichiers `_filters.any` que vous créez qui contiennent toutes vos règles de filtrage pour bloquer ou autoriser le trafic à travers le Dispatcher pour atteindre AEM.

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Répertoire des rendus

Ce répertoire peut contenir tous les fichiers `_renders.any` que vous créez contenant les détails de connectivité de chaque serveur back-end à partir duquel le Dispatcher consomme du contenu.

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Répertoire des hôtes virtuels

Ce répertoire peut contenir tous les fichiers `_vhosts.any` que vous créez contenant une liste des noms de domaine et des chemins d’accès à associer à une batterie de serveurs spécifique à un serveur back-end particulier.

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Structure de dossiers complète

AMS a structuré chacun des fichiers avec des extensions de fichier personnalisées dans le but d’éviter les problèmes/conflits d’espace de noms et toute confusion.

Voici un exemple de jeu de fichiers standard à partir d’un déploiement AMS par défaut :

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## Suivre un idéal

Enterprise Linux possède des cycles de correction pour le package Apache Webserver (httpd).

Moins vous modifiez les fichiers installés par défaut, mieux cela fonctionnera. En effet, si des correctifs de sécurité ou des améliorations de configuration sont appliqués via la commande RPM/Yum, les correctifs ne seront pas appliqués sur un fichier modifié.

À la place, cela crée un fichier `.rpmnew` en regard de l’original.Cela signifie ne pas faire certaines modifications que vous auriez pu souhaiter et créer plus de mémoire dans vos dossiers de configuration.

Par exemple, le RPM lors de l’installation de la mise à jour vérifie si `httpd.conf` est à l’état `unaltered` pour *remplacer* le fichier et accéder aux mises à jour essentielles.  Si le `httpd.conf` était `altered`, alors il *ne remplacera pas* le fichier et créera à la place un fichier de référence appelé `httpd.conf.rpmnew`. Les nombreux correctifs souhaités seront dans ce fichier qui ne s’applique pas au démarrage du service.

Enterprise Linux a été configuré correctement pour gérer ce cas d’utilisation plus efficacement.Vous avez accès à des zones dans lesquelles vous pouvez étendre ou remplacer les valeurs par défaut ayant été définies pour vous.Dans l’installation de base de httpd, vous trouverez le fichier `/etc/httpd/conf/httpd.conf` qui contient une syntaxe telle que :

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

L’idée est qu’Apache souhaite que vous étendiez les modules et les configurations en ajoutant de nouveaux fichiers aux répertoires `/etc/httpd/conf.d/` et `/etc/httpd/conf.modules.d/` avec une extension de fichier `.conf`.

Comme exemple parfait lors de l’ajout du module Dispatcher à Apache, vous créez un module `.so` dans ` /etc/httpd/modules/` puis vous l’incluez en ajoutant un fichier dans `/etc/httpd/conf.modules.d/02-dispatcher.conf` avec les contenus pour charger votre fichier de module `.so`.

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>
nous n’avons modifié aucun fichier existant fourni par Apache.Au lieu de cela, nous avons simplement ajouté les nôtres aux répertoires utilisés.
</div><br/>

Maintenant, nous utilisons notre module dans le fichier <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> qui initialise notre module et charge le fichier de configuration initial spécifique au module.

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Encore une fois, vous remarquerez que nous avons ajouté des fichiers et des modules, mais que nous n’avons modifié aucun fichier original.Cela nous donne les fonctionnalités souhaitées et nous protège des correctifs manquants ainsi que de la compatibilité maximale avec chaque mise à niveau du package.

[Suivant -> Explication des fichiers de configuration](./explanation-config-files.md)
