---
title: Application React - AEM Exemple sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application React explique comment interroger du contenu à l’aide AEM API GraphQL à l’aide de requêtes persistantes.
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-11-09T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 6%

---

# React App{#react-app}

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application React explique comment interroger du contenu à l’aide AEM API GraphQL à l’aide de requêtes persistantes. Le client AEM sans affichage pour JavaScript est utilisé pour exécuter les requêtes persistantes GraphQL qui alimentent l’application.

![Application React avec AEM sans affichage](./assets/react-app/react-app.png)

Afficher la variable [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [tutoriel détaillé complet](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=fr) décrivant comment cette application React a été créée est disponible.

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## Configuration requise AEM

L’application React fonctionne avec les options de déploiement AEM suivantes. Tous les déploiements nécessitent la variable [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0) à installer.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=fr)
+ Configuration locale à l’aide de [le SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr?lang=en#install-local-aem-instances)

L’application React est conçue pour se connecter à une __Publication AEM__ , mais il peut toutefois sources du contenu à partir de l’auteur AEM si l’authentification est fournie dans la configuration de l’application React.

## Utilisation

1. Cloner le `adobe/aem-guides-wknd-graphql` référentiel :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifiez la variable `aem-guides-wknd-graphql/react-app/.env.development` fichier et définition `REACT_APP_HOST_URI` pour pointer vers votre AEM cible.

   Mettez à jour la méthode d’authentification lors de la connexion à une instance d’auteur.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   
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

1. Ouvrez un terminal et exécutez les commandes :

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Une nouvelle fenêtre de navigateur doit s’afficher [http://localhost:3000](http://localhost:3000)
1. Une liste des aventures du site de référence WKND doit s’afficher sur l’application.

## Le code

Vous trouverez ci-dessous un résumé de la création de l’application React, de sa connexion à AEM sans affichage pour récupérer du contenu à l’aide de requêtes persistantes GraphQL et de la manière dont ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Requêtes persistantes

En suivant AEM bonnes pratiques sans affichage, l’application React utilise AEM requêtes persistantes GraphQL pour interroger les données d’aventure. L’application utilise deux requêtes persistantes :

+ `wknd/adventures-all` requête persistante, qui renvoie toutes les aventures d’AEM avec un jeu abrégé de propriétés. Cette requête persistante entraîne la liste des aventures de la vue initiale.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` requête persistante, qui renvoie une seule aventure par `slug` (propriété personnalisée qui identifie de manière unique une aventure) avec un ensemble complet de propriétés. Cette requête persistante alimente les vues détaillées de l’aventure.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

AEM requêtes persistantes sont exécutées sur une GET HTTP et, par conséquent, la variable [AEM client sans affichage pour JavaScript](https://github.com/adobe/aem-headless-client-js) est utilisé pour [exécuter les requêtes GraphQL persistantes ;](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) par rapport à AEM et chargez le contenu de l’aventure dans l’application.

Chaque requête conservée possède un React correspondant. [useEffect](https://reactjs.org/docs/hooks-effect.html) hook `src/api/usePersistedQueries.js`, qui appelle de manière asynchrone le point de terminaison de requête persistant de la GET HTTP AEM et renvoie les données d’aventure.

Chaque fonction appelle à son tour la fonction `aemHeadlessClient.runPersistedQuery(...)`, exécution de la requête GraphQL conservée.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity) {
  ...
  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    const queryParameters = { activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryParameters);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all");
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

L’application React utilise deux vues pour présenter les données de l’aventure dans l’expérience web.

+ `src/components/Adventures.js`

   Appels `getAdventuresByActivity(..)` de `src/api/usePersistedQueries.js` et affiche les aventures renvoyées dans une liste.

+ `src/components/AdventureDetail.js`

   Appelle le `getAdventureBySlug(..)` en utilisant la variable `slug` param transmis via la sélection aventure sur le `Adventures` et affiche les détails d’une seule aventure.

### Variables d’environnement

Plusieurs [variables d&#39;environnement](https://create-react-app.dev/docs/adding-custom-environment-variables) sont utilisés pour se connecter à un environnement AEM. La valeur par défaut se connecte à la publication AEM en cours d’exécution à l’adresse `http://localhost:4503`. Mettez à jour le `.env.development` pour modifier la connexion AEM :

+ `REACT_APP_HOST_URI=http://localhost:4502`: Défini sur AEM hôte cible
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Définissez le chemin d’accès du point de terminaison GraphQL. Elle n’est pas utilisée par cette application React, car cette application utilise uniquement des requêtes persistantes.
+ `REACT_APP_AUTH_METHOD=`: Méthode d’authentification préférée. Facultatif, aucune authentification n’est utilisée par défaut.
   + `service-token`: Utiliser les informations d’identification du service pour obtenir un jeton d’accès sur AEM as a Cloud Service
   + `dev-token`: Utilisation du jeton de développement pour le développement local sur AEM as a Cloud Service
   + `basic`: Utilisation de l’utilisateur/de la transmission pour le développement local avec l’auteur AEM local
   + Laissez vide pour vous connecter à AEM sans authentification
+ `REACT_APP_AUTHORIZATION=admin:admin`: Définissez les informations d’identification d’authentification de base à utiliser lors de la connexion à un environnement d’auteur AEM (pour le développement uniquement). Si vous vous connectez à un environnement de publication, ce paramètre n’est pas nécessaire.
+ `REACT_APP_DEV_TOKEN`: Chaîne du jeton de développement. Pour vous connecter à une instance distante, en plus de l’authentification de base (user:pass), vous pouvez utiliser l’authentification du porteur avec le jeton DEV de la console Cloud.
+ `REACT_APP_SERVICE_TOKEN`: Chemin d’accès au fichier d’informations d’identification du service. Pour vous connecter à une instance distante, l’authentification peut également être effectuée avec le jeton de service (téléchargez le fichier depuis Developer Console).

### Demandes d’AEM de proxy

Lors de l’utilisation du serveur de développement webpack (`npm start`), le projet repose sur un [configuration du proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. Le fichier est configuré à l’adresse [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) et repose sur plusieurs variables d’environnement personnalisées définies sur `.env` et `.env.development`.

Si vous vous connectez à un environnement de création AEM, la variable [la méthode d’authentification doit être configurée.](#environment-variables).

### Partage de ressources cross-origin (CORS)

Cette application React repose sur une configuration CORS basée sur AEM s’exécutant sur l’environnement AEM cible et suppose que l’application React s’exécute sur `http://localhost:3000` en mode de développement. Le [Configuration CORS](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) fait partie de la variable [Site WKND](https://github.com/adobe/aem-guides-wknd).

![Configuration CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)
