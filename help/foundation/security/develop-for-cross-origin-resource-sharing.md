---
title: Développer pour le partage des ressources cross-origin (CORS) avec des AEM
description: Un court exemple d’utilisation de CORS pour accéder au contenu AEM à partir d’une application web externe via du code JavaScript côté client.
version: 6,4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# Développer pour le partage des ressources cross-origin (CORS)

Un court exemple d’effet de levier [!DNL CORS] pour accéder au contenu AEM d&#39;une application web externe via du code JavaScript côté client.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

Dans cette vidéo :

* **www.example.com** mappe à localhost via `/etc/hosts`
* **aem-publish.local** mappe à localhost via `/etc/hosts`
* SimpleHTTPServer (wrapper pour [[!DNL Python]s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) diffuse la page du HTML via le port 8000.
   * _Plus disponible dans Mac App Store. Utilisez des [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] s’exécute sur [!DNL Apache HTTP Web Server] 2.4 et requête de proxy inverse vers `aem-publish.local` to `localhost:4503`.

Pour plus d’informations, voir [Comprendre le partage des ressources cross-origin (CORS) dans AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML et JavaScript

Cette page Web a une logique :

1. En cliquant sur le bouton
1. Se transforme en [!DNL AJAX GET] de `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Récupère la variable `jcr:title` former la réponse JSON ;
1. Injecte la variable `jcr:title` dans le DOM

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

Pour permettre la mise en cache et la diffusion des en-têtes CORS sur le contenu mis en cache, ajoutez les éléments suivants : [/clientheaders configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) à toutes les instances de prise en charge de la publication AEM `dispatcher.any` fichiers .

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

**Redémarrer l’application de serveur web** après avoir apporté des modifications à la variable `dispatcher.any` fichier .

Il est probable que l’effacement complet du cache soit nécessaire pour s’assurer que les en-têtes sont correctement mis en cache dans la requête suivante après une `/clientheaders` mise à jour de la configuration.

## Documents annexes {#supporting-materials}

* [AEM fabrique de configuration OSGi pour les stratégies de partage des ressources cross-origin](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Jeeves pour macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (compatible avec Windows/macOS/Linux)

* [Comprendre le partage des ressources cross-origin (CORS) dans AEM](./understand-cross-origin-resource-sharing.md)
* [Partage des ressources cross-origin (W3C)](https://www.w3.org/TR/cors/)
* [Contrôle d’accès HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
