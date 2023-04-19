---
title: Utiliser le SDK d’AEM Headless
description: Découvrez comment effectuer des requêtes GraphQL à l’aide du SDK d’AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 31948793786a2c430533d433ae2b9df149ec5fc0
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 91%

---

# SDK AEM Headless

Le SDK d’AEM Headless est un ensemble de bibliothèques qui peuvent être utilisées par les clients pour interagir rapidement et facilement avec les API d’AEM Headless via HTTP.

Le SDK d’AEM Headless est disponible pour diverses plateformes :

+ [SDK AEM découplé pour les navigateurs côté client (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [SDK AEM découplé pour les Node.js côté serveur (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [SDK AEM découplé pour Java™](https://github.com/adobe/aem-headless-client-java)

## Requêtes GraphQL persistantes

Effectuer des requêtes sur AEM avec GraphQL à l’aide de requêtes persistantes (par opposition aux [requêtes GraphQL définies par le client](#graphl-queries)) permet aux développeurs et développeuses de conserver une requête (mais pas ses résultats) dans AEM, puis de demander que la requête soit exécutée par nom. Les requêtes persistantes sont similaires au concept des procédures stockées dans les bases de données SQL.

Les requêtes persistantes sont plus performantes que les requêtes GraphQL définies par le client, car les requêtes persistantes sont exécutées à l’aide de HTTP GET, qui peut être mis en cache sur les niveaux de réseau CDN et du Dispatcher d’AEM. Les requêtes persistantes définissent également une API et dissocient la nécessité pour le développeur ou la développeuse de comprendre les détails de chaque modèle de fragment de contenu.

### Exemples de code{#persisted-graphql-queries-code-examples}

Vous trouverez ci-dessous des exemples de code sur la manière d’exécuter une requête GraphQL persistante sur AEM.

+++ Exemple JavaScript

Installez [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) en exécutant la commande `npm install` à partir de la racine de votre projet Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Cet exemple de code montre comment effectuer des requêtes sur AEM à l’aide du module npm [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) utilisant une syntaxe `async/await`. Le SDK d’AEM Headless pour JavaScript prend également en charge la [syntaxe de promesse](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Ce code suppose qu’une requête persistante avec le nom `wknd/adventureNames` a été créée sur l’instance de création AEM et publiée sur l’instance de publication AEM.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ React useEffect(..) exemple

Installez [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) en exécutant la commande `npm install` à partir de la racine de votre projet React.

```
$ npm i @adobe/aem-headless-client-js
```

Cet exemple de code montre comment utiliser le [hook React useEffect(..) ](https://reactjs.org/docs/hooks-effect.html) pour exécuter un appel asynchrone vers AEM GraphQL.

L’utilisation de `useEffect` pour effectuer l’appel GraphQL asynchrone dans React est utile car :

1. cela fournit un wrapper synchrone pour l’appel asynchrone à AEM ;
1. cela réduit les nouvelles requêtes inutiles dans AEM.

Ce code suppose qu‘une requête persistante avec le nom `wknd-shared/adventure-by-slug` a été créée sur l’instance de création AEM et publiée sur l’instance de publication AEM à l’aide de GraphiQL.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
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

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

Appelez le hook React personnalisé `useEffect` depuis un autre endroit dans un composant React.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

De nouveaux hooks `useEffect` peuvent être créés pour chaque requête persistante utilisée par l’application React.

+++

<p> </p>

## Requêtes GraphQL

AEM prend en charge les requêtes GraphQL définies par le client, mais il est recommandé d’utiliser les [requêtes GraphQL persistantes](#persisted-graphql-queries) dans AEM.

## Webpack 5+

Le SDK JS AEM sans affichage comporte des dépendances sur `util` qui n’est pas inclus par défaut dans Webpack 5+. Si vous utilisez Webpack 5+ et recevez l’erreur suivante :

```
Compiled with problems:
× ERROR in ./node_modules/@adobe/aio-lib-core-errors/src/AioCoreSDKErrorWrapper.js 12:13-28
Module not found: Error: Can't resolve 'util' in '/Users/me/Code/wknd-headless-examples/node_modules/@adobe/aio-lib-core-errors/src'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
    - install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "util": false }
```

Ajoutez ce qui suit : `devDependencies` à `package.json` fichier :

```json
  "devDependencies": {
    "buffer": "npm:buffer@^6.0.3",
    "crypto": "npm:crypto-browserify@^3.12.0",
    "http": "npm:stream-http@^3.2.0",
    "https": "npm:https-browserify@^1.0.0",
    "stream": "npm:stream-browserify@^3.0.0",
    "util": "npm:util@^0.12.5",
    "zlib": "npm:browserify-zlib@^0.2.0"
  },
```

Exécutez ensuite `npm install` pour installer les dépendances.
