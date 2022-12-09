---
title: Configuration CORS pour AEM GraphQL
description: Découvrez comment configurer le partage de ressources cross-origin (CORS) pour l’utiliser avec AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: 6f1000db880c3126a01fa0b74abdb39ffc38a227
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 3%

---


# Partage de ressources cross-origin (CORS)

Le partage des ressources cross-origin (CORS) d’Adobe Experience Manager as a Cloud Service facilite les propriétés web non AEM pour effectuer des appels côté client basés sur un navigateur vers AEM API GraphQL.

>[!TIP]
>
> Les configurations suivantes sont des exemples. Veillez à les ajuster pour qu’elles s’alignent sur les exigences de votre projet.

## exigence CORS

La norme CORS est requise pour les connexions basées sur un navigateur aux API GraphQL d’AEM, lorsque le client se connectant à AEM n’est PAS diffusé à partir de la même origine (également appelée hôte ou domaine) que l’.

| Type de client | [Application d’une seule page (SPA)](../spa.md) | [Composant Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Serveur à serveur](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Configuration CORS requise | ✔ | ✔ | ✘ | ✘ |

## Configuration OSGi

La fabrique de configuration OSGi d’AEM CORS définit les critères d’autorisation pour accepter les requêtes HTTP CORS.

| Le client se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Nécessite une configuration OSGi CORS | ✔ | ✔ | ✔ |


L’exemple ci-dessous définit une configuration OSGi pour AEM Publish (`../config.publish/..`), mais peuvent être ajoutés à [tout dossier de mode d’exécution pris en charge](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Les propriétés de configuration des clés sont les suivantes :

+ `alloworigin` et/ou `alloworiginregexp` spécifie les origines sur lesquelles le client se connecte à AEM web s’exécute.
+ `allowedpaths` spécifie les modèles de chemin d’URL autorisés à partir des origines spécifiées.
   + Pour prendre en charge AEM requêtes persistantes GraphQL, ajoutez le modèle suivant : `/graphql/execute.json.*`
   + Pour prendre en charge les fragments d’expérience, ajoutez le modèle suivant : `/content/experience-fragments/.*`
+ `supportedmethods` spécifie les méthodes HTTP autorisées pour les requêtes CORS. Ajouter `GET`, pour prendre en charge AEM requêtes persistantes GraphQL (et les fragments d’expérience).

[En savoir plus sur la configuration OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=fr)

Cet exemple de configuration prend en charge l’utilisation AEM requêtes persistantes GraphQL. Pour utiliser des requêtes GraphQL définies par le client, ajoutez une URL de point de terminaison GraphQL dans `allowedpaths` et `POST` to `supportedmethods`.

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
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
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
    "HEAD",
    "OPTIONS"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### Demandes d’API GraphQL autorisées AEM

Lors de l’accès aux API GraphQL d’AEM qui nécessitent une autorisation (généralement l’auteur AEM ou le contenu protégé sur AEM Publish), assurez-vous que la configuration OSGi CORS comporte les valeurs supplémentaires :

+ `supportedheaders` également des listes `"Authorization"`
+ `supportscredentials` est défini sur `true`

Les requêtes autorisées envoyées aux API GraphQL AEM qui nécessitent une configuration CORS sont inhabituelles, car elles se produisent généralement dans le contexte de [applications serveur à serveur](../server-to-server.md) et ne nécessitent donc pas de configuration CORS. Applications basées sur un navigateur qui nécessitent des configurations CORS, telles que [applications d’une seule page](../spa.md) ou [Composants web](../web-component.md), utilisez généralement l’autorisation, car il est difficile de sécuriser les informations d’identification .

Par exemple, ces deux paramètres sont définis comme suit dans une `CORSPolicyImpl` Configuration d’usine OSGi :

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### Exemple de configuration OSGi

+ [Vous trouverez un exemple de configuration OSGi dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Configuration de Dispatcher

Le Dispatcher du service AEM Publish (et Preview) doit être configuré pour prendre en charge CORS.

| Le client se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Nécessite une configuration CORS de Dispatcher | ✘ | ✔ | ✔ |

### Autorisation des en-têtes CORS sur les requêtes HTTP

Mettez à jour le `clientheaders.any` pour autoriser les en-têtes de requête HTTP `Origin`,  `Access-Control-Request-Method`, et `Access-Control-Request-Headers` à être transmis à AEM, ce qui permet à la requête HTTP d’être traitée par la variable [Configuration AEM CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Exemple de configuration Dispatcher

+ [Exemple de Dispatcher _en-têtes client_ La configuration se trouve dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### Diffuser des en-têtes de réponse HTTP CORS

Configuration de la ferme de serveurs de Dispatcher pour le cache **En-têtes de réponse HTTP CORS** pour s’assurer qu’elles sont incluses lorsque AEM requêtes persistantes GraphQL sont diffusées à partir du cache de Dispatcher en ajoutant la variable `Access-Control-...` en-têtes de la liste des en-têtes de cache.

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

#### Exemple de configuration Dispatcher

+ [Exemple de Dispatcher _En-têtes de réponse HTTP CORS_ La configuration se trouve dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
