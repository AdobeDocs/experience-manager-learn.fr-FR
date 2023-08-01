---
title: Application React - Exemple AEM Headless
description: Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application React montre comment interroger le contenu à l’aide des API GraphQL d’AEM en utilisant des requêtes persistantes.
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM sans affichage as a Cloud Service" before-title="false"
source-git-commit: 6f874fd3da09ce808920a7f8ea3386beda726272
workflow-type: tm+mt
source-wordcount: '898'
ht-degree: 96%

---

# Application React{#react-app}

Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application React explique comment interroger du contenu à l’aide des API GraphQL d’AEM en utilisant des requêtes persistantes. Le client AEM Headless pour JavaScript est utilisé pour exécuter les requêtes persistantes GraphQL qui alimentent l’application.

![Application React avec AEM Headless.](./assets/react-app/react-app.png)

Afficher le [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

Un [tutoriel détaillé complet](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=fr) décrivant comment cette application React a été créée est disponible.

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [Node.js v18](https://nodejs.org/fr/)
+ [Git](https://git-scm.com/)

## Configuration requise d’AEM

L’application React fonctionne avec les options de déploiement AEM suivantes. Tous les déploiements nécessitent que la version [v3.0.0 ou supérieure du site WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) soit installée.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=fr)
+ Configuration locale à l’aide du [SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr)
   + Nécessite [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)

L’application React est conçue pour se connecter à un environnement de __Publication AEM__, cependant, elle peut s’approvisionner en contenu auprès de l’environnement de création AEM si l’authentification est fournie dans la configuration de l’application React.

## Utilisation

1. Clonez le référentiel `adobe/aem-guides-wknd-graphql` :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifiez le fichier `aem-guides-wknd-graphql/react-app/.env.development` et définissez `REACT_APP_HOST_URI` de manière à pointer vers votre cible AEM.

   Mettez à jour la méthode d’authentification si vous vous connectez à une instance de création.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. Ouvrez un terminal et exécutez les commandes :

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Une nouvelle fenêtre de navigateur doit s’afficher dans [http://localhost:3000](http://localhost:3000).
1. Une liste des Adventures du site de référence WKND doit s’afficher sur l’application.

## Le code

Vous trouverez ci-dessous un résumé sur la façon dont l’application React est construite, comment elle se connecte à AEM Headless pour récupérer le contenu à l’aide de requêtes persistantes GraphQL, et comment ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Requêtes persistantes

Conformément aux bonnes pratiques d’AEM Headless, l’application React utilise les requêtes GraphQL persistantes d’AEM pour interroger les données de l’Adventure. L’application utilise deux requêtes persistantes :

+ La requête persistante `wknd/adventures-all`, qui renvoie toutes les Adventures dans AEM avec un ensemble abrégé de propriétés. Cette requête persistante génère la liste des Adventures de la vue initiale.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ La requête persistante `wknd/adventure-by-slug`, qui renvoie une seule Adventure par `slug` (propriété personnalisée qui identifie de manière unique une Adventure) avec un ensemble complet de propriétés. Cette requête persistante alimente les vues détaillées de l’Adventure.

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Exécuter la requête persistante GraphQL

Les requêtes persistantes d’AEM sont exécutées sur une requête HTTP GET et, par conséquent, le [client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js) est utilisé pour [exécuter les requêtes GraphQL persistantes](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options—promiseany) par rapport à AEM et charger le contenu de l’Adventure dans l’application.

Chaque requête persistante possède un hook [useEffect](https://reactjs.org/docs/hooks-effect.html) React correspondant dans `src/api/usePersistedQueries.js`, qui appelle de manière asynchrone le point d’entrée de requête persistante HTTP GET d’AEM et renvoie les données des Adventures.

Chaque fonction appelle à son tour la `aemHeadlessClient.runPersistedQuery(...)`, afin d’exécuter la requête GraphQL persistante.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}
```

### Vues

L’application React utilise deux vues pour présenter les données de l’Adventure dans l’expérience web.

+ `src/components/Adventures.js`

  Elle appelle `getAdventuresByActivity(..)` à partir de `src/api/usePersistedQueries.js` et affiche les Adventures renvoyées dans une liste.

+ `src/components/AdventureDetail.js`

  Elle appelle `getAdventureBySlug(..)` en utilisant le paramètre `slug` transmis via la sélection des Adventures sur le composant `Adventures` et affiche les détails d’une seule Adventure.

### Variables d’environnement

Plusieurs [variables d’environnement](https://create-react-app.dev/docs/adding-custom-environment-variables) sont utilisées pour se connecter à un environnement AEM. La valeur par défaut se connecte au service de publication AEM en cours d’exécution à l’adresse `http://localhost:4503`. Mettez à jour le fichier `.env.development` pour modifier la connexion d’AEM :

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com` : définissez sur l’hôte cible AEM.
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json` : définissez le chemin d’accès du point d’entrée GraphQL. Ceci n’est pas utilisé par cette application React, qui utilise uniquement des requêtes persistantes.
+ `REACT_APP_AUTH_METHOD=` : méthode d’authentification préférée. Elle est facultative, aucune authentification n’est utilisée par défaut.
   + `service-token` : utilisez les informations d’identification du service pour obtenir un jeton d’accès sur AEM as a Cloud Service.
   + `dev-token` : utilisez un jeton de développement pour le développement local sur AEM as a Cloud Service.
   + `basic` : utilisez un nom d’utilisateur et un mot de passe pour le développement local sur l’instance locale de création d’AEM.
   + Laissez vide pour vous connecter à AEM sans authentification.
+ `REACT_APP_AUTHORIZATION=admin:admin` : définissez les informations d’identification d’authentification de base à utiliser lors de la connexion à un environnement de création AEM (pour le développement uniquement). Si vous vous connectez à un environnement de publication, ce paramètre n’est pas nécessaire.
+ `REACT_APP_DEV_TOKEN` : chaîne du jeton de développement. Pour vous connecter à une instance distante, en plus de l’authentification de base (user:pass), vous pouvez utiliser l’authentification du porteur avec le jeton de développement de la console Cloud.
+ `REACT_APP_SERVICE_TOKEN` : chemin d’accès au fichier d’informations d’identification du service. Pour vous connecter à une instance distante, l’authentification peut également être effectuée avec le jeton de service (téléchargez le fichier depuis la Developer Console).

### Requêtes AEM proxy

Lors de l’utilisation du serveur de développement webpack (`npm start`), le projet repose sur une [configuration du proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) utilisant `http-proxy-middleware`. Le fichier est configuré sur [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) et repose sur plusieurs variables d’environnement personnalisées définies sur `.env` et `.env.development`.

Si vous vous connectez à un environnement de création AEM, [la méthode d’authentification correspondante doit être configurée](#environment-variables).

### Partage de ressources entre origines multiples (CORS)

Cette application React repose sur une configuration CORS basée sur AEM s’exécutant sur l’environnement AEM cible et suppose que l’application React s’exécute sur `http://localhost:3000` en mode de développement.  Consultez la section[AEM documentation sur le déploiement sans affichage](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html) pour plus d’informations sur la configuration et la configuration de CORS.
