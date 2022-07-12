---
title: Utilisation du SDK AEM sans affichage
description: Découvrez comment créer des requêtes GraphQL à l’aide du SDK AEM sans affichage.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 68970493802c7194bcb3ac3ac9ee10dbfb0fc55d
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 1%

---

# AEM SDK sans affichage

Le SDK AEM sans affichage est un ensemble de bibliothèques qui peuvent être utilisées par les clients pour interagir rapidement et facilement avec AEM API sans affichage sur HTTP.

Le SDK AEM sans affichage est disponible pour diverses plateformes :

+ [AEM SDK sans affichage pour les navigateurs côté client (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM SDK sans affichage pour server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM SDK sans affichage pour Java™](https://github.com/adobe/aem-headless-client-java)

## Requêtes GraphQL

AEM prend en charge les requêtes GraphQL définies par le client, mais il est AEM bonne pratique d’utiliser [requêtes GraphQL persistantes](#persisted-graphql-queries).

## Requêtes GraphQL persistantes

Requête avec AEM GraphQL à l’aide de requêtes persistantes (par opposition à [Requêtes GraphQL définies par le client](#graphl-queries)) permet aux développeurs de conserver une requête (mais pas ses résultats) dans AEM, puis de demander que la requête soit exécutée par son nom. Les requêtes persistantes sont similaires au concept des procédures stockées dans les bases de données SQL.

Les requêtes persistantes sont plus performantes que les requêtes GraphQL définies par le client, car les requêtes persistantes sont exécutées à l’aide de GET HTTP, qui peuvent être mises en cache sur les niveaux CDN et AEM Dispatcher. Les requêtes persistantes définissent également une API et dissocient la nécessité pour le développeur de comprendre les détails de chaque modèle de fragment de contenu.

### Exemples de code{#persisted-graphql-queries-code-examples}

Vous trouverez ci-dessous des exemples de code sur la manière d’exécuter une requête persistante GraphQL sur AEM.

+++ Exemple JavaScript

Installez le [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) en exécutant la fonction `npm install` à partir de la racine de votre projet Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Cet exemple de code montre comment effectuer des requêtes AEM à l’aide du [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) module npm utilisant `async/await` syntaxe. Le SDK AEM sans affichage pour JavaScript prend également en charge [Syntaxe des promesses](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Ce code suppose une requête persistante avec le nom . `wknd/adventureNames` a été créé sur AEM Author et publié sur AEM Publish.

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

Installez le [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) en exécutant la fonction `npm install` à partir de la racine de votre projet React.

```
$ npm i @adobe/aem-headless-client-js
```

Cet exemple de code montre comment utiliser la variable [React useEffect(..) hook](https://reactjs.org/docs/hooks-effect.html) pour exécuter un appel asynchrone à AEM GraphQL.

Utilisation `useEffect` pour que l’appel GraphQL asynchrone dans React soit utile, procédez comme suit :

1. Il fournit un wrapper synchrone pour l’appel asynchrone à AEM.
1. Cela réduit les AEM inutiles.

Ce code suppose une requête persistante avec le nom . `wknd-shared/adventure-by-slug` a été créé sur AEM Author et publié sur AEM Publish à l’aide de GraphiQL.

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

Appeler un React personnalisé `useEffect` d’un autre élément d’un composant React.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Nouveau `useEffect` des hooks peuvent être créés pour chaque requête persistante utilisée par l’application React.

+++

<p> </p>
