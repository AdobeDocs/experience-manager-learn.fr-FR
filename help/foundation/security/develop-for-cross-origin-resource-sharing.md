---
title: Développer pour le partage des ressources cross-origin (CORS) avec des AEM
description: Un court exemple d’utilisation de CORS pour accéder au contenu AEM à partir d’une application web externe via du code JavaScript côté client.
version: 6.3, 6,4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
topic: Sécurité
role: Developer
level: Beginner
feature: null
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---


# Développer pour le partage des ressources cross-origin (CORS)

Un court exemple d’utilisation de [!DNL CORS] pour accéder AEM contenu à partir d’une application web externe via du code JavaScript côté client.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

Dans cette vidéo :

* **www.example.** commap vers localhost via  `/etc/hosts`
* **aem-publish.** localmap vers localhost via  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)  (wrapper de  [[!DNL Python]&#39;s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) diffuse la page HTML via le port 8000.
* [!DNL AEM Dispatcher] s’exécute sur la version  [!DNL Apache HTTP Web Server] 2.4 et la requête de proxy inverse est envoyée  `aem-publish.local` à  `localhost:4503`.

Pour plus d’informations, voir [Comprendre le partage des ressources cross-origin (CORS) dans AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML et JavaScript

Cette page Web a une logique :

1. En cliquant sur le bouton
1. Transmet une requête [!DNL AJAX GET] à `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Récupère la réponse `jcr:title` JSON
1. injecte le `jcr:title` dans le DOM.

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

## Configuration d’usine OSGi

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

## Configuration de Dispatcher {#dispatcher-configuration}

Pour permettre la mise en cache et la diffusion des en-têtes CORS sur le contenu mis en cache, ajoutez la [/clientheaders configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) suivante à tous les fichiers `dispatcher.any` de publication AEM pris en charge.

```
/cache { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Redémarrez l’** application du serveur web après avoir apporté des modifications au  `dispatcher.any` fichier .

Il est probable que l’effacement complet du cache soit nécessaire pour s’assurer que les en-têtes sont correctement mis en cache dans la requête suivante après une mise à jour de la configuration `/clientheaders`.

## Documents complémentaires {#supporting-materials}

* [AEM fabrique de configuration OSGi pour les stratégies de partage des ressources cross-origin](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer pour macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)  (compatible avec Windows/macOS/Linux)

* [Comprendre le partage des ressources cross-origin (CORS) dans AEM](./understand-cross-origin-resource-sharing.md)
* [Partage des ressources cross-origin (W3C)](https://www.w3.org/TR/cors/)
* [Contrôle d’accès HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

