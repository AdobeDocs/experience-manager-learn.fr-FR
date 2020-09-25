---
title: Développer pour le partage des ressources entre Origines (CORS) avec les AEM
description: Un court exemple d’utilisation de CORS pour accéder au contenu AEM à partir d’une application Web externe via du code JavaScript côté client.
version: 6.3, 6,4, 6.5
sub-product: fondation, services de contenu, sites
feature: null
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# Développer pour le partage des ressources entre Origines (CORS)

Un court exemple d’exploitation [!DNL CORS] de l’accès au contenu AEM à partir d’une application Web externe via du code JavaScript côté client.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

Dans cette vidéo :

* **www.example.com** mappe vers localhost via `/etc/hosts`
* **aem-publish.local** mappe vers localhost via `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) (wrapper pour SimpleHTTPServer [de](https://docs.python.org/2/library/simplehttpserver.html)[ ! DNL Python]) diffuse la page HTML via le port 8000.
* [!DNL AEM Dispatcher] s’exécute sur la [!DNL Apache HTTP Web Server] version 2.4 et la requête de proxy inverse est envoyée à `aem-publish.local``localhost:4503`.

Pour plus d&#39;informations, consultez [Comprendre le partage de ressources entre Origines (CORS) dans AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML et JavaScript

Cette page Web est logique :

1. En cliquant sur le bouton
1. Demande [!DNL AJAX GET] à `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Récupère le `jcr:title` formulaire avec la réponse JSON
1. Injecte la variable dans le `jcr:title` DOM

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## Configuration en usine OSGi

La fabrique de configuration OSGi pour [!DNL Cross-Origin Resource Sharing] est disponible via :

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Dispatcher configuration {#dispatcher-configuration}

Pour autoriser la mise en cache et la diffusion des en-têtes sur le contenu mis en cache, ajoutez la configuration suivante à tous les [!DNL CORS] `dispatcher.any` fichiers de publication AEM pris en charge.

```
/cache { 
  ...
  /headers {
      "Access-Control-Allow-Origin",
      "Access-Control-Expose-Headers",
      "Access-Control-Max-Age",
      "Access-Control-Allow-Credentials",
      "Access-Control-Allow-Methods",
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Redémarrez l’application** de serveur Web après avoir apporté des modifications au `dispatcher.any` fichier.

Il est probable que l’effacement complet du cache soit nécessaire pour s’assurer que les en-têtes sont correctement mis en cache lors de la demande suivante après une mise à jour de la `/headers` configuration.

## Documents de support {#supporting-materials}

* [aem usine de configuration OSGi pour les stratégies de partage de ressources entre Origines](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer pour macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (compatible Windows/macOS/Linux)

* [Comprendre le partage des ressources entre Origines (CORS) en AEM](./understand-cross-origin-resource-sharing.md)
* [Partage des ressources entre Origines (W3C)](https://www.w3.org/TR/cors/)
* [CONTRÔLE D&#39;ACCÈS HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

