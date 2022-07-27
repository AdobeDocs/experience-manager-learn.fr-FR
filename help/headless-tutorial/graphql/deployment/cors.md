---
title: Configuration CORS pour AEM GraphQL
description: Découvrez comment configurer le partage de ressources cross-origin (CORS) pour l’utiliser avec AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 4%

---


# Partage de ressources cross-origin (CORS)

Le partage des ressources cross-origin (CORS) d’Adobe Experience Manager as a Cloud Service facilite les propriétés web non AEM pour effectuer des appels côté client basés sur un navigateur vers AEM API GraphQL.

>[!TIP]
>
> Les configurations suivantes sont des exemples. Veillez à les ajuster pour qu’elles s’alignent sur les exigences de votre projet.



## exigence CORS

La norme CORS est requise pour les connexions basées sur un navigateur aux API GraphQL AEM, lorsque le client se connectant à AEM n’est PAS servi depuis la même origine (également appelée hôte ou domaine) que l’.

| Type de client | Application d’une seule page (SPA) | Composant Web/JS | Mobile | Serveur vers serveur |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Configuration CORS requise | ✔ | ✔ | ✘ | ✘ |

## Configuration OSGi

La fabrique de configuration OSGi d’AEM CORS définit les critères d’autorisation pour accepter les requêtes HTTP CORS.

| Le client se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Nécessite une configuration OSGi CORS | ✔ | ✔ | ✔ |


L’exemple ci-dessous définit une configuration OSGi pour AEM Publish (`../config.publish/..`), mais peuvent être ajoutés à [tout dossier runmode pris en charge](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Les propriétés de configuration des clés sont les suivantes :

+ `alloworigin` et/ou `alloworiginregexp` indique les origines sur lesquelles le client se connecte à AEM web s’exécute.
+ `allowedpaths` spécifie le ou les modèles de chemin d’URL autorisés à partir des origines spécifiées. Pour prendre en charge AEM requêtes persistantes GraphQL, utilisez le modèle suivant : `"/graphql/execute.json.*"`
+ `supportedmethods` spécifie les méthodes HTTP autorisées pour les requêtes CORS. Ajouter `GET`, pour prendre en charge AEM requêtes persistantes GraphQL.

[En savoir plus sur la configuration OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=fr)


Cet exemple de configuration prend en charge l’utilisation AEM requêtes persistantes GraphQL. Pour utiliser des requêtes GraphQL définies par le client, ajoutez une URL de point d’entrée GraphQL dans `allowedpaths` et `POST` to `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```


## Configuration de Dispatcher

Le Dispatcher du service AEM Publish (et Preview) doit être configuré pour prendre en charge CORS.

| Le client se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Nécessite une configuration CORS de Dispatcher | ✘ | ✔ | ✔ |

### Autorisation des en-têtes CORS sur les requêtes HTTP

Mettez à jour le `clientheaders.any` pour autoriser les en-têtes de requête HTTP `Origin`,  `Access-Control-Request-Method` et `Access-Control-Request-Headers` à être transmis à AEM, ce qui permet à la requête HTTP d’être traitée par la variable [Configuration AEM CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

### Diffuser des en-têtes de réponse HTTP CORS

Configuration de la ferme de serveurs de Dispatcher en mémoire cache **En-têtes de réponse HTTP CORS** pour s’assurer qu’elles sont incluses lorsque AEM requêtes persistantes GraphQL sont diffusées à partir du cache de Dispatcher en ajoutant la variable `Access-Control-...` en-têtes de la liste des en-têtes de cache.

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

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
