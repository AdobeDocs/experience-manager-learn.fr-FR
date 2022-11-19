---
title: Fichiers en lecture seule ou non modifiables AMS Dispatcher
description: Comprendre pourquoi certains fichiers sont en lecture seule ou non modifiables et comment effectuer les modifications fonctionnelles que vous souhaitez
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 1%

---


# Fichiers en lecture seule ou non modifiables dans AMS

[Table des matières](./overview.md)

[&lt;- Précédent : Journaux courants](./common-logs.md)

## Description

Ce document décrit les fichiers verrouillés et à ne pas modifier, ainsi que la manière de définir correctement les paramètres de configuration souhaités.

Lorsqu’AMS fournit un système, il déploie une configuration de base qui rend tout fonctionnel et sécurisé.  Ce sont des choses qu’AMS veut garantir comme base de fonctionnalités et de sécurité.  Pour ce faire, certains fichiers sont marqués comme étant en lecture seule et immuables afin que vous ne puissiez pas les modifier.

La mise en page ne vous empêche pas de modifier leur comportement et de remplacer les modifications dont vous avez besoin.  Au lieu de modifier ces fichiers, vous superposez votre propre fichier qui remplace l’original.

Cela vous permet également d’être assuré que lorsqu’AMS corrige les dispatchers avec les derniers correctifs et améliorations de sécurité, ils ne modifieront pas vos fichiers.  Vous pouvez ensuite continuer à bénéficier des améliorations et adopter uniquement les modifications souhaitées.
![Affiche une piste de bowling avec une balle qui roule le long de la piste.  La balle a une flèche avec le mot qui vous montre.  Les pare-chocs sont surmontés et ils ont les mots fichiers immuables au-dessus.](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
Comme illustré dans l&#39;image ci-dessus, les fichiers immuables ne vous empêchent pas de jouer au jeu.  Ils vous empêchent juste de faire du mal à votre performance et vous maintiennent dans la file.  Cette méthode nous permet d’accéder aux quelques fonctionnalités clés :

- Les personnalisations sont gérées dans leurs propres espaces sécurisés.
- Superposition de modifications personnalisées reflète celle des méthodes de superposition dans AEM
- Les configurations AMS de correspondance peuvent être effectuées sans modifier les personnalisations.
- Les tests de l’installation de base par rapport aux configurations personnalisées peuvent être effectués simultanément pour aider à déterminer si les problèmes sont dus à des personnalisations ou à autre chose. Quels fichiers ?


Voici une liste standard de fichiers déployés avec un Dispatcher :

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
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

Pour déterminer les fichiers non modifiables, vous pouvez exécuter la commande suivante sur un Dispatcher pour afficher :

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

Voici un exemple de réponse parmi les fichiers non modifiables :

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## Comment apporter des modifications

### Variables

Les variables vous permettent d’apporter des modifications fonctionnelles sans modifier les fichiers de configuration eux-mêmes.  Certains éléments de la configuration peuvent être ajustés en ajustant les valeurs des variables.  Exemple que nous pouvons mettre en surbrillance à partir du fichier `/etc/httpd/conf.d/dispatcher_vhost.conf` est illustré ici :

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

Découvrez comment la directive DispatcherLogLevel comporte une variable `DISP_LOG_LEVEL` au lieu de la valeur normale que vous voyez là.  Au-dessus de cette section de code, une instruction d’inclusion s’affiche également dans un fichier de variables.  Le fichier de variable `/etc/httpd/conf.d/variables/ams_default.vars` C&#39;est là que nous voulons regarder.  Voici le contenu de ce fichier de variables :

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Vous pouvez voir ci-dessus que la valeur actuelle de `DISP_LOG_LEVEL` est `info`.  Nous pouvons l’ajuster pour suivre ou déboguer, ou pour la valeur/le niveau de nombre de votre choix.  Désormais, partout où le contrôle du niveau de journalisation s’ajuste automatiquement.

### Méthode de recouvrement

Veuillez comprendre les fichiers d’inclusion de niveau supérieur, car ils seront le point de départ de toute personnalisation.  Pour commencer avec un exemple simple, nous avons un scénario dans lequel nous voulons ajouter un nouveau nom de domaine que nous prévoyons de pointer vers ce Dispatcher.  L’exemple de domaine que nous utiliserons est we-retail.adobe.com.  Nous allons commencer par copier un fichier de configuration existant vers un nouveau fichier où nous pourrons ajouter nos modifications :

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

Nous avons copié le fichier aem_publish.vhost existant car il contient déjà ce dont nous avons besoin pour que les choses fonctionnent et nous ne voulons pas réinventer un démarrage déjà fort.  Maintenant, nous éditons le nouveau fichier weretail.vhost et apportons les modifications nécessaires.

Avant :

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Après :

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Maintenant, nous avons mis à jour notre `ServerName` et `ServerAlias` pour correspondre aux nouveaux noms de domaine, ainsi que pour mettre à jour d’autres en-têtes de chemin de navigation.  Nous allons maintenant activer notre nouveau fichier pour permettre à Apache de savoir comment utiliser notre nouveau fichier :

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Le serveur web Apache sait maintenant que le domaine doit générer du trafic, mais nous devons toujours informer le module de Dispatcher qu’il a un nouveau nom de domaine à honorer.  Nous allons commencer par créer une nouvelle `*_vhost.any` fichier `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` et à l&#39;intérieur de ce fichier, nous allons placer le nom de domaine que nous voulons honorer :

```
"we-retail.adobe.com"
```

Maintenant, nous devons créer un nouveau fichier de ferme qui utilisera notre nouveau fichier d&#39;entrée vhost, et nous allons commencer par copier un fichier de démarrage solide dans notre nouveau fichier.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Permet d’afficher les modifications que nous devrons apporter à ce fichier de ferme.

Avant :

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

Après :

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

Nous avons maintenant mis à jour le nom de la ferme et l’inclusion qu’elle utilise dans la variable `/virtualhosts` de la configuration de la ferme.  Nous devons activer ce nouveau fichier de ferme afin qu’il puisse l’utiliser dans la configuration en cours d’exécution :

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Maintenant nous rechargerions simplement le service de serveur web et utiliserions notre nouveau domaine !

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Notez que nous avons uniquement modifié les éléments dont nous avions besoin pour modifier et exploiter les inclusions et le code existants fournis avec les fichiers de configuration de ligne de base.  Nous n&#39;avons qu&#39;à délimiter l&#39;élément que nous devons changer.  Cela facilite la tâche et nous permet de conserver moins de code.
</div>