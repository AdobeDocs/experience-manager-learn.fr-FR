---
title: Développer pour le partage de ressources entre origines multiples (CORS) avec AEM
description: Court exemple d’utilisation de CORS pour accéder au contenu d’AEM à partir d’une application web externe via du code JavaScript côté client.
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 100%

---

# Développer pour le partage de ressources entre origines multiples (CORS)

Court exemple d’utilisation de [!DNL CORS] pour accéder au contenu d’AEM à partir d’une application web externe via du code JavaScript côté client. Cet exemple utilise la configuration OSGi CORS pour activer l’accès à CORS sur AEM. L’approche de configuration OSGi est viable dans les cas suivants :

* Une origine unique accède au contenu de l’instance de publication AEM
* L’accès CORS est requis pour l’instance de création AEM.

Si un accès de plusieurs origines à l’instance de publication AEM est requis, reportez-vous à [cette documentation](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=fr#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

Dans cette vidéo :

* **www.example.com** mappe à localhost via `/etc/hosts`.
* **aem-publish.local** mappe à localhost via `/etc/hosts`.
* SimpleHTTPServer (wrapper du module [[!DNL Python] SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) diffuse la page du HTML via le port 8000.
   * _N’est plus disponible dans l’App Store Mac. Utilisez une solution similaire, telle que [Jeeves](https://apps.apple.com/fr/app/jeeves-local-http-server/id980824182?mt=12)._
* Le [!DNL AEM Dispatcher] s’exécute sur [!DNL Apache HTTP Web Server] 2.4 et renvoie les requêtes de proxy de `aem-publish.local` vers `localhost:4503`.

Pour plus d’informations, voir [Présentation du partage de ressources entre origines multiples (CORS) dans AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com - HTML et JavaScript

Cette page web a une logique :

1. En cliquant sur le bouton,
1. elle transforme une requête [!DNL AJAX GET] en `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`,
1. elle récupère le `jcr:title` de la réponse JSON,
1. et elle injecte le `jcr:title` dans le DOM.

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

La configuration d’usine OSGi pour [!DNL Cross-Origin Resource Sharing] est disponible via :

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

## Configuration du Dispatcher {#dispatcher-configuration}

### Autoriser les en-têtes de requête CORS

Pour autoriser les [en-têtes de requête HTTP à être transmis à AEM pour traitement](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#specifying-the-http-headers-to-pass-through-clientheaders), ils doivent être autorisés dans la configuration `/clientheaders` de Dispatcher.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Mettre en cache des en-têtes de réponse CORS

Pour permettre la mise en cache et la diffusion des en-têtes CORS sur le contenu mis en cache, ajoutez la [configuration /cache /headers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#caching-http-response-headers) suivante au fichier `dispatcher.any` de l’instance de publication AEM.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

**Redémarrez l’application de serveur web** après avoir apporté des modifications au fichier `dispatcher.any`.

Il est probable que l’effacement complet du cache soit nécessaire pour que les en-têtes soient correctement mis en cache dans la requête suivante après une mise à jour de la configuration `/cache /headers`.

## Documents annexes {#supporting-materials}

* [Jeeves pour macOS](https://apps.apple.com/fr/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (compatible avec Windows/macOS/Linux)

* [Présentation du partage de ressources entre origines multiples (CORS) dans AEM](./understand-cross-origin-resource-sharing.md)
* [Partage de ressources entre origines multiples (W3C)](https://www.w3.org/TR/cors/)
* [Contrôle d’accès HTTP (Mozilla MDN)](https://developer.mozilla.org/fr/docs/Web/HTTP/CORS)
