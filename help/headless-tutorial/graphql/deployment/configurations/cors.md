---
title: Configuration CORS pour AEM GraphQL
description: Découvrez comment configurer le partage des ressources cross-origin (CORS) pour l’utiliser avec AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
source-git-commit: 36b4217a899b462296d4389bc96a644da97d5da4
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 92%

---

# Partage de ressources entre origines multiples (CORS)

Le partage des ressources cross-origin (CORS) d’Adobe Experience Manager as a Cloud Service facilite les propriétés web non-AEM pour effectuer des appels côté client basés sur un navigateur vers les API GraphQL d’AEM.

L’article suivant décrit comment configurer _origine unique_ accès à un ensemble spécifique de points de terminaison AEM sans affichage via CORS. Une origine unique signifie que seul un accès à un domaine non AEM unique est AEM, par exemple, https://app.example.com se connectant à https://www.example.com. L’accès à plusieurs origines peut ne pas fonctionner avec cette approche en raison de problèmes de mise en cache.

>[!TIP]
>
> Les configurations suivantes sont des exemples. Veillez à les ajuster pour qu’elles s’alignent sur les exigences de votre projet.

## Exigence CORS

CORS est obligatoire pour les connexions par navigateur aux API GraphQL d’AEM, quand le client qui se connecte à AEM n’est PAS pris en charge à partir de la même origine (également appelée hôte ou domaine) qu’AEM.

| Type de client | [Application monopage (SPA)](../spa.md) | [Composant Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Serveur à serveur](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Configuration CORS requise | ✔ | ✔ | ✘ | ✘ |

## Configuration OSGi

La configuration OSGi de CORS AEM définit les critères d’autorisation pour accepter les requêtes HTTP CORS.

| Le client se connecte à : | l’instance de création AEM, | Publication AEM | Prévisualisation AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Nécessite une configuration OSGi CORS | ✔ | ✔ | ✔ |


L’exemple ci-dessous définit une configuration OSGi pour la Publicaton AEM (`../config.publish/..`), mais peut être ajouté à [tout dossier de mode d’exécution pris en charge](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Les principales propriétés de configuration sont :

+ `alloworigin` et/ou `alloworiginregexp` indique les origines sur lesquelles opère le client qui se connecte à AEM web.
+ `allowedpaths` indique les modèles de chemin d’URL autorisés à partir des origines spécifiées.
   + Pour prendre en charge les requêtes persistantes GraphQL d’AEM, ajoutez le motif : `/graphql/execute.json.*`
   + Pour prendre en charge les fragments d’expérience, ajoutez le motif : `/content/experience-fragments/.*`
+ `supportedmethods` spécifie les méthodes HTTP autorisées pour les requêtes CORS. Ajoutez `GET`, pour prendre en charge les requêtes persistantes AEM GraphQL (et Fragments d’expérience).

[En savoir plus sur la configuration OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=fr)

Cet exemple de configuration prend en charge l’utilisation des requêtes persistantes AEM GraphQL. Pour utiliser des requêtes GraphQL définies par le client, ajoutez une URL de point de terminaison GraphQL dans `allowedpaths` et `POST` à `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
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
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### Demandes d’API AEM GraphQL autorisées

Pour accéder aux API AEM GraphQL qui nécessitent une autorisation (généralement Création AEM ou du contenu protégé sur la Publication AEM), assurez-vous que la configuration CORS OSGi comporte les valeurs supplémentaires :

+ `supportedheaders` répertorie également `"Authorization"`
+ `supportscredentials` est défini sur `true`

Les requêtes autorisées aux API AEM GraphQL qui nécessitent une configuration CORS sont peu courantes, car elles se produisent généralement dans le contexte [d’applications serveur à serveur](../server-to-server.md) et ne nécessitent donc pas de configuration CORS. Les applications basées sur un navigateur qui nécessitent des configurations CORS, comme les [applications à page unique](../spa.md) ou les [composants Web](../web-component.md), utilisent généralement une autorisation, car il est difficile de sécuriser les informations d’identification.

Par exemple, ces deux paramètres sont définis comme suit dans une configuration d’usine OSGi `CORSPolicyImpl` :

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

## Configuration du Dispatcher

Le Dispatcher du service Publication AEM (et aperçu) doit être configuré pour prendre en charge CORS.

| Le client se connecte à : | l’instance de création AEM, | Publication AEM | Prévisualisation AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Nécessite une configuration CORS de Dispatcher | ✘ | ✔ | ✔ |

### Autoriser les en-têtes CORS sur les requêtes HTTP

Mettez à jour le fichier `clientheaders.any` pour autoriser les en-têtes de requête HTTP `Origin`, `Access-Control-Request-Method`, et `Access-Control-Request-Headers` à être transmis à AEM, ce qui permet à la requête HTTP d’être traitée par la [Configuration AEM CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Exemple de configuration du Dispatcher

+ [Vous pouvez consulter un exemple de configuration des _en-têtes client_ du Dispatcher dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### Diffuser des en-têtes de réponse HTTP CORS

Configurez la propriété /farm du Dispatcher pour mettre en cache les **en-têtes de réponse HTTP CORS** afin qu’ils soient inclus lorsque les requêtes persistantes AEM GraphQL sont diffusées à partir du cache du Dispatcher en ajoutant les en-têtes `Access-Control-...` à la liste des en-têtes de cache.

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

#### Exemple de configuration du Dispatcher

+ [Consultez un exemple de la configuration des _En-têtes de réponse HTTP CORS_ du Dispatcher dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
