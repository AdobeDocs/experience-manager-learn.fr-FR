---
title: Next.js - Exemple AEM Headless
description: Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application Next.js explique comment interroger du contenu à l’aide des API GraphQL d’AEM et de requêtes persistantes.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM sans affichage as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
source-git-commit: 29b9e4a23d8f4ae0494fc43f76f7449062364843
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 92%

---

# Application Next.js

Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application Next.js explique comment interroger du contenu à l’aide des API GraphQL d’AEM et de requêtes persistantes. Le client AEM Headless pour JavaScript est utilisé pour exécuter les requêtes persistantes GraphQL qui alimentent l’application.

![Application Next.js avec AEM Headless.](./assets/next-js/next-js.png)

Afficher le [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## Configuration requise d’AEM

L’application Next.js fonctionne avec les options de déploiement AEM suivantes. Tous les déploiements requis [WKND Shared v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) ou [Site WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) à installer dans l&#39;environnement as a Cloud Service AEM.

Cet exemple d’application Next.js est conçu pour se connecter au service de __publication AEM__.

### Configuration requise pour le service de création AEM

Next.js est conçu pour se connecter au service de __publication AEM__ et accéder au contenu non protégé. Le fichier Next.js peut être configuré pour se connecter au service de création AEM via les propriétés `.env` décrites ci-dessous. Les images diffusées à partir du service de création AEM nécessitent une authentification. Par conséquent, la personne accédant à l’application Next.js doit également être connectée au service de création AEM.

## Utilisation

1. Clonez le référentiel `adobe/aem-guides-wknd-graphql` :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifiez le fichier `aem-guides-wknd-graphql/next-js/.env.local` et définissez `NEXT_PUBLIC_AEM_HOST` sur le service AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Si vous vous connectez au service de création AEM, l’authentification doit être fournie, car ce service est sécurisé par défaut.

   Pour utiliser un compte AEM local, définissez `AEM_AUTH_METHOD=basic` et indiquez le nom d’utilisateur et le mot de passe dans les propriétés `AEM_AUTH_USER` et `AEM_AUTH_PASSWORD`.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Pour utiliser un [Jeton de développement local AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=fr#generating-the-access-token), définissez `AEM_AUTH_METHOD=dev-token` et indiquez la valeur du jeton de développement complet dans la propriété `AEM_AUTH_DEV_TOKEN`.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Modifiez le fichier `aem-guides-wknd-graphql/next-js/.env.local` et vérifiez que `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` est défini sur le point d’entrée GraphQL AEM approprié.

   Lors de l’utilisation de [WKND Shared](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) ou de [WKND Site](https://github.com/adobe/aem-guides-wknd/releases/latest), utilisez le point d’entrée de l’API GraphQL `wknd-shared`.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. Ouvrez une invite de commande et démarrez l’application Next.js à l’aide des commandes suivantes :

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Une nouvelle fenêtre du navigateur ouvre l’application Next.js à l’adresse [http://localhost:3000](http://localhost:3000).
1. L’application Next.js affiche une liste des Adventures. La sélection d’une Adventure ouvre ses détails dans une nouvelle page.

## Le code

Vous trouverez ci-dessous un résumé de la création de l’application Next.js, de sa connexion à AEM Headless pour récupérer du contenu à l’aide des requêtes persistantes GraphQL, et de la manière dont ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Requêtes persistantes

Selon les bonnes pratiques d’AEM Headless, l’application Next.js utilise des requêtes persistantes GraphQL d’AEM pour interroger les données d’Adventure. L’application utilise deux requêtes persistantes :

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

Chaque requête persistante possède une fonction correspondante dans `src/lib//aem-headless-client.js`, qui appelle le point d’entrée GraphQL d’AEM et renvoie les données d’Adventure.

Chaque fonction appelle à son tour la fonction `aemHeadlessClient.runPersistedQuery(...)`, afin d’exécuter la requête GraphQL persistante.

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### Pages

L’application Next.js utilise deux pages pour présenter les données d’Adventure.

+ `src/pages/index.js`

  Elle utilise [getServerSideProps() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) pour appeler `getAllAdventures()` et affiche chaque Adventure sous forme de carte.

  L’utilisation de `getServerSiteProps()` permet le rendu côté serveur de cette page Next.js.

+ `src/pages/adventures/[...slug].js`

  Un [itinéraire dynamique Next.js](https://nextjs.org/docs/routing/dynamic-routes) qui affiche les détails d’une seule Adventure. Cet itinéraire dynamique prérécupère les données de chaque aventure à l’aide de [getStaticProps() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) via un appel à `getAdventureBySlug(slug, queryVariables)` en utilisant la variable `slug` param transmis via la sélection aventure sur le `adventures/index.js` et `queryVariables` pour contrôler le format, la largeur et la qualité de l’image.

  L’itinéraire dynamique permet de prérécupérer les détails de toutes les Adventures à l’aide de [getStaticPaths() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) et de renseigner toutes les permutations d’itinéraire possibles en fonction de la liste complète des Adventures renvoyées par la requête GraphQL.`getAdventurePaths()`

  L’utilisation de `getStaticPaths()` et `getStaticProps(..)` a permis la génération statique du site de ces pages Next.js.

## Configuration du déploiement

Les applications Next.js, en particulier en matière de rendu côté serveur (SSR) et de génération côté serveur (SSG), ne nécessitent pas de configurations de sécurité avancées telles que le partage de ressources entre origines multiples (CORS).

Cependant, si le Next.js effectue des requêtes HTTP à AEM à partir du contexte du client, des configurations de sécurité dans AEM peuvent être requises. Pour plus d’informations, consultez le [tutoriel sur le déploiement d’applications monopages d’AEM Headless](../deployment/spa.md).
