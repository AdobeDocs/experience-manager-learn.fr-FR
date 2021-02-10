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
source-git-commit: c657eefa69b383c1b1a9e2845276245d3db00e6f
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 0%

---


# Développer pour le partage des ressources entre Origines (CORS)

Un court exemple d’exploitation de [!DNL CORS] pour accéder au contenu AEM à partir d’une application Web externe via JavaScript côté client.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

Dans cette vidéo :

* **www.example.** commaps to localhost via  `/etc/hosts`
* **aem-publish.** localmaps vers localhost via  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)  (wrapper de  [[!DNL Python]SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) diffuse la page HTML via le port 8000.
* [!DNL AEM Dispatcher] s’exécute sur la  [!DNL Apache HTTP Web Server] version 2.4 et la requête de proxy inverse est envoyée  `aem-publish.local` à  `localhost:4503`.

Pour plus d&#39;informations, consultez [Comprendre le partage des ressources entre Origines (CORS) dans AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML et JavaScript

Cette page Web est logique :

1. En cliquant sur le bouton
1. Demande [!DNL AJAX GET] à `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Récupère la réponse `jcr:title` du fichier JSON.
1. injecte le `jcr:title` dans le DOM

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

## Configuration du répartiteur {#dispatcher-configuration}

Pour permettre la mise en cache et la diffusion des en-têtes CORS sur le contenu mis en cache, ajoutez les éléments suivants [/configuration clientheaders](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) à tous les fichiers AEM Publish `dispatcher.any` pris en charge.

```
/cache { 
  ...
  /clientheaders {
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

**Redémarrez l’** application de serveur Web après avoir apporté des modifications au  `dispatcher.any` fichier.

Il est probable que l&#39;effacement complet du cache soit nécessaire pour s&#39;assurer que les en-têtes sont correctement mis en cache lors de la demande suivante après une mise à jour de la configuration `/clientheaders`.

## Documents d&#39;appui {#supporting-materials}

* [aem usine de configuration OSGi pour les stratégies de partage de ressources entre Origines](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer pour macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)  (compatible Windows/macOS/Linux)

* [Comprendre le partage des ressources entre Origines (CORS) en AEM](./understand-cross-origin-resource-sharing.md)
* [Partage des ressources entre Origines (W3C)](https://www.w3.org/TR/cors/)
* [CONTRÔLE D&#39;ACCÈS HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

