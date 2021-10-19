---
title: Plans de site
description: Découvrez comment optimiser votre optimisation du référencement en créant des plans de site pour AEM Sites.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
source-git-commit: 5bdff2eafaa28aff722b12607b1278539072be62
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---


# Plans de site

Découvrez comment optimiser votre optimisation du référencement en créant des plans de site pour AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Ressources

+ [Documentation AEM plan de site](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentation du plan de site Apache Sling](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [AEM Composants WCM principaux Github](https://github.com/adobe/aem-core-wcm-components)
   + Ajout de la fonctionnalité Plan du site dans la version 2.17.6
+ [Documentation du plan du site Sitemap.org](https://www.sitemaps.org/protocol.html)
+ [Documentation du fichier d’index Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## Configurations

### org.apache.sling.sitemap.impl.SitemapScheduler~wknd.cfg.json

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

Définit la variable [Configuration d’usine OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) pour la fréquence (en utilisant [expressions cron](http://www.cronmaker.com)) les plans de site seront regénérés/générés et mis en cache dans AEM.

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### filters.any

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

Autorisez les requêtes HTTP pour les fichiers d’index et de plan de site.

```
...

# Allow AEM WCM Core Components sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### rewrite.rules

`dispatcher/src/conf.d/rewrites/rewrite.rules`

Assurez-vous que `.xml` les requêtes HTTP sitemap sont acheminées vers la page d’AEM sous-jacente correcte. Si le raccourcissement des URL n’est pas utilisé ou si des mappages Sling sont utilisés pour obtenir le raccourcissement des URL, cette configuration n’est pas nécessaire.

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
