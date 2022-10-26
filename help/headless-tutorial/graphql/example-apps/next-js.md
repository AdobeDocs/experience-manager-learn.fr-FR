---
title: Next.js - AEM exemple sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application Next.js explique comment interroger le contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 2%

---

# Application Next.js

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application Next.js explique comment interroger le contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes. Le client AEM sans affichage pour JavaScript est utilisé pour exécuter les requêtes persistantes GraphQL qui alimentent l’application.

![Application Next.js avec AEM sans affichage](./assets/next-js/next-js.png)

Afficher la variable [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [Node.js v16+](https://nodejs.org/en/)
+ [npm 8+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## Configuration requise AEM

L’application Next.js fonctionne avec les options de déploiement AEM suivantes. Tous les déploiements requis [WKND Shared v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest), [Site WKND v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest), ou la variable [Module complémentaire de démonstration de référence](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html) à installer dans l&#39;environnement as a Cloud Service AEM.

Cet exemple d’application Next.js est conçu pour se connecter à __Publication AEM__ service.

### Exigences d’auteur AEM

Next.js est conçu pour se connecter à __Publication AEM__ et accéder au contenu non protégé. Le fichier Next.js peut être configuré pour se connecter à l’auteur AEM via le `.env` propriétés décrites ci-dessous. Les images diffusées à partir de l’auteur AEM nécessitent une authentification. Par conséquent, l’utilisateur accédant à l’application Next.js doit également être connecté à l’auteur AEM.

## Utilisation

1. Cloner le `adobe/aem-guides-wknd-graphql` référentiel :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifiez la variable `aem-guides-wknd-graphql/next-js/.env.local` fichier et définition `NEXT_PUBLIC_AEM_HOST` au service AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Si vous vous connectez au service AEM Author, l’authentification doit être fournie, car le service AEM Author est sécurisé par défaut.

   Pour utiliser un jeu de compte AEM local `AEM_AUTH_METHOD=basic` et indiquez le nom d’utilisateur et le mot de passe dans la variable `AEM_AUTH_USER` et `AEM_AUTH_PASSWORD` propriétés.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Pour utiliser une [Jeton de développement local as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` et indiquez la valeur du jeton de développement complet dans la variable `AEM_AUTH_DEV_TOKEN` .

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Modifiez la variable `aem-guides-wknd-graphql/next-js/.env.local` fichier et valider  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` est défini sur le point d’entrée AEM GraphQL approprié.

   Lors de l’utilisation de [WKND partagé](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) ou [Site WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), utilisez le `wknd-shared` Point d’entrée de l’API GraphQL.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

   Lors de l’utilisation de [Module complémentaire de démonstration de référence](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html), utilisez le `aem-demo-assets` Point d’entrée de l’API GraphQL.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=aem-demo-assets
   ...
   ```

1. Ouvrez une invite de commande et démarrez l’application Next.js à l’aide des commandes suivantes :

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Une nouvelle fenêtre de navigateur ouvre l’application Next.js à l’adresse [http://localhost:3000](http://localhost:3000)
1. L’application Next.js affiche une liste des aventures. La sélection d’une aventure ouvre ses détails dans une nouvelle page.

## Le code

Vous trouverez ci-dessous un résumé de la création de l’application Next.js, de sa connexion à AEM sans affichage pour récupérer du contenu à l’aide de requêtes persistantes GraphQL et de la manière dont ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Requêtes persistantes

En suivant AEM bonnes pratiques sans affichage, l’application Next.js utilise AEM requêtes persistantes GraphQL pour interroger les données d’aventure. L’application utilise deux requêtes persistantes :

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

Chaque requête conservée possède une fonction correspondante dans `src/lib//aem-headless-client.js`, qui appelle le point d’entrée AEM GraphQL et renvoie les données d’aventure.

Chaque fonction appelle à son tour la fonction `aemHeadlessClient.runPersistedQuery(...)`, exécutant la requête GraphQL conservée.

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

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### Pages

L’application Next.js utilise deux pages pour présenter les données sur l’aventure.

+ `src/pages/index.js`

   Utilisations [getServerSideProps() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) pour appeler `getAllAdventures()` et affiche chaque aventure sous forme de carte.

   L’utilisation de `getServerSiteProps()` permet le rendu côté serveur de cette page Next.js.

+ `src/pages/adventures/[...slug].js`

   A [Route dynamique de Next.js](https://nextjs.org/docs/routing/dynamic-routes) qui affiche les détails d’une seule aventure. Cet itinéraire dynamique prérécupère les données de chaque aventure à l’aide de [getStaticProps() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) via un appel à `getAdventureBySlug(..)` en utilisant la variable `slug` param transmis via la sélection aventure sur le `adventures/index.js` page.

   L’itinéraire dynamique permet de prérécupérer les détails de toutes les aventures à l’aide de [getStaticPaths() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) et renseigner toutes les permutations d’itinéraire possibles en fonction de la liste complète des aventures renvoyées par la requête GraphQL.  `getAdventurePaths()`

   L’utilisation de `getStaticPaths()` et `getStaticProps(..)` autorisait la génération statique du site de ces pages Next.js.

## Configuration du déploiement

Les applications Next.js, en particulier dans le contexte du rendu côté serveur (SSR) et de la génération côté serveur (SSG), ne nécessitent pas de configurations de sécurité avancées telles que le partage des ressources cross-origin (CORS).

Cependant, si le fichier Next.js effectue des requêtes HTTP à AEM à partir du contexte du client, des configurations de sécurité dans AEM peuvent être requises. Consultez la section [Tutoriel sur le déploiement d’applications d’une seule page AEM sans affichage](../deployment/spa.md) pour plus d’informations.

