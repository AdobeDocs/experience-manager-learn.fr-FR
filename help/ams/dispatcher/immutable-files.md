---
title: Fichiers en lecture seule ou non modifiables du Dispatcher AMS
description: Découvrir pourquoi certains fichiers sont en lecture seule ou non modifiables et comment effectuer les modifications fonctionnelles que vous souhaitez
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
duration: 376
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 100%

---

# Fichiers en lecture seule ou non modifiables dans AMS

[Table des matières](./overview.md)

[&lt;- Précédent : journaux courants](./common-logs.md)

## Description

Ce document décrit les fichiers verrouillés et à ne pas modifier, ainsi que la manière de définir correctement les paramètres de configuration souhaités.

Lorsqu’AMS fournit un système, il déploie une configuration de base qui rend tout fonctionnel et sécurisé.Ce sont des choses qu’AMS veut garantir comme base de fonctionnalités et de sécurité.Pour ce faire, certains fichiers sont marqués comme étant en lecture seule et non modifiables afin que vous ne puissiez pas les modifier.

La disposition ne vous empêche pas de modifier leur comportement et de remplacer les modifications dont vous avez besoin.Au lieu de modifier ces fichiers, vous recouvrez votre propre fichier qui remplace l’original.

Cela permet également de garantir que lorsqu’AMS corrige les Dispatchers avec les derniers correctifs et améliorations de sécurité, vos fichiers ne seront pas modifiés.Vous pouvez ensuite continuer à bénéficier des améliorations et adopter uniquement les modifications que vous souhaitez.
![Montre une piste de bowling avec une boule qui roule le long de la piste.  La boule comporte une flèche avec le mot « you » (vous).  Les rails des gouttières sont levés et les mots « immutable files » (fichiers non modifiables) apparaissent au-dessus.](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
Comme illustré dans l’image ci-dessus, les fichiers non modifiables ne vous empêchent pas de jouer.Ils vous empêchent juste de nuire à vos performances et vous maintiennent sur la piste.Cette méthode nous permet d’accéder aux quelques fonctionnalités clés :

- Les personnalisations sont gérées dans leurs propres espaces sécurisés.
- Le recouvrement de modifications personnalisées reflète les méthodes de recouvrement dans AEM.
- La correction des configurations AMS peut être effectuée sans modifier les personnalisations.
- Les tests de l’installation de base par rapport aux configurations personnalisées peuvent être effectués simultanément pour aider à déterminer si les problèmes sont dus à des personnalisations ou à autre chose. Quels fichiers ?


Voici une liste standard de fichiers déployés avec un Dispatcher :

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

Pour déterminer les fichiers non modifiables, vous pouvez exécuter la commande suivante sur un Dispatcher pour les afficher :

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

Voici un exemple de réponse indiquant quels fichiers sont non modifiables :

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

Les variables vous permettent d’apporter des modifications fonctionnelles sans modifier les fichiers de configuration eux-mêmes.Certains éléments de la configuration peuvent être ajustés en ajustant les valeurs des variables.Voici un exemple à partir du fichier `/etc/httpd/conf.d/dispatcher_vhost.conf` :

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

Découvrez comment la directive DispatcherLogLevel comporte une variable de `DISP_LOG_LEVEL` au lieu de la valeur normale que vous devriez voir.Au-dessus de cette section de code, une instruction d’inclusion s’affiche également dans un fichier de variables.Nous allons ensuite regarder le fichier de variables `/etc/httpd/conf.d/variables/ams_default.vars`.Voici le contenu de ce fichier de variables :

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Vous pouvez voir ci-dessus que la valeur actuelle de la variable `DISP_LOG_LEVEL` est `info`.Nous pouvons l’ajuster pour le tracking ou le débogage, ou l’ajuster sur la valeur numérique/le niveau de votre choix.Désormais, tout ce qui contrôle le niveau de journalisation s’ajustera automatiquement.

### Méthode de recouvrement

Veuillez vous familiariser avec les fichiers d’inclusion de niveau supérieur, car ils constituent le point de départ de toute personnalisation.Pour commencer avec un exemple simple, nous avons un scénario dans lequel nous voulons ajouter un nouveau nom de domaine que nous prévoyons de pointer vers ce Dispatcher.L’exemple de domaine que nous utiliserons est we-retail.adobe.com.Nous allons commencer par copier un fichier de configuration existant vers un nouveau fichier où nous pourrons ajouter nos modifications :

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

Nous avons copié le fichier aem_publish.vhost existant, car il contient déjà ce dont nous avons besoin pour que les choses fonctionnent et nous ne voulons pas réinventer un démarrage déjà fort.Nous allons maintenant éditer le nouveau fichier weretail.vhost et y apporter les modifications nécessaires.

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

Maintenant, nous avons mis à jour notre `ServerName` et notre `ServerAlias` pour correspondre aux nouveaux noms de domaine, ainsi que pour mettre à jour d’autres en-têtes de chemin de navigation.Nous allons maintenant activer notre nouveau fichier pour permettre à Apache de savoir comment utiliser notre nouveau fichier :

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Le serveur web Apache sait maintenant qu’il doit générer du trafic pour ce domaine, mais nous devons toujours informer le module du Dispatcher qu’il a un nouveau nom de domaine à respecter.Nous allons commencer par créer un nouveau fichier `*_vhost.any` `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` et à l’intérieur de ce fichier, nous allons placer le nom de domaine que nous voulons respecter :

```
"we-retail.adobe.com"
```

Nous devons maintenant créer un nouveau fichier de batterie qui utilisera notre nouveau fichier d’entrée vhost, et nous allons commencer par copier un fichier de démarrage fort dans notre nouveau fichier.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Permet d’afficher les modifications à apporter à ce fichier de batterie.

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

Nous avons maintenant mis à jour le nom de batterie et l’inclusion utilisée dans la section `/virtualhosts` de la configuration de la batterie.Nous devons activer ce nouveau fichier de batterie afin qu’il puisse être utilisé dans la configuration en cours d’exécution :

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Il ne nous reste plus qu’à charger à nouveau le service de serveur web et à utiliser notre nouveau domaine.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Notez que nous avons uniquement modifié les éléments dont nous avions besoin pour modifier et exploiter les inclusions et le code existants fournis avec les fichiers de configuration de base.Nous n’avons qu’à délimiter l’élément que nous devons modifier.Cela facilite la tâche et nous permet de conserver moins de code.
</div>

[Suivant -> Contrôle de l’intégrité du Dispatcher](./health-check.md)
