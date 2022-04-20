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
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# AEM SDK sans affichage

Le SDK AEM sans affichage est un ensemble de bibliothèques qui peuvent être utilisées par les clients pour interagir rapidement et facilement avec AEM API sans affichage sur HTTP.

Le SDK AEM sans affichage est disponible pour diverses plateformes :

+ [AEM SDK sans affichage pour les navigateurs côté client (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM SDK sans affichage pour server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM SDK sans affichage pour Java™](https://github.com/adobe/aem-headless-client-java)

## Requêtes GraphQL

Requête avec AEM GraphQL à l’aide de requêtes (par opposition à [requêtes GraphQL persistantes](#persisted-graphql-queries)) permet aux développeurs de définir des requêtes dans le code, en spécifiant exactement le contenu à demander à AEM.

Les requêtes GraphQL ont tendance à être moins performantes que les requêtes persistantes, car elles sont exécutées à l’aide du POST HTTP, qui est moins facile à mettre en cache sur les niveaux CDN et Dispatcher AEM.

### Exemples de code{#graphql-queries-code-examples}

Vous trouverez ci-dessous des exemples de code expliquant comment exécuter une requête GraphQL sur AEM.

+++ Exemple JavaScript

Installez le [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) en exécutant la fonction `npm install` à partir de la racine de votre projet Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Cet exemple de code montre comment effectuer des requêtes AEM à l’aide du [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) module npm utilisant `async/await` syntaxe. Le SDK AEM sans affichage pour JavaScript prend également en charge [Syntaxe des promesses](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
```

+++


+++ React useEffect(..) example

Installez le [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) en exécutant la fonction `npm install` à partir de la racine de votre projet React.

```
$ npm i @adobe/aem-headless-client-js
```

Cet exemple de code montre comment utiliser la variable [React useEffect(..) hook](https://reactjs.org/docs/hooks-effect.html) pour exécuter un appel asynchrone à AEM GraphQL.

Utilisation `useEffect` pour que l’appel GraphQL asynchrone dans React soit utile, car il :

1. Fournit un wrapper synchrone pour l’appel asynchrone à AEM.
1. Réduit les AEM inutiles.

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

Importez et utilisez les `useGraphQL` Connectez-vous au composant React pour effectuer une requête AEM.

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## Requêtes GraphQL persistantes

Requête avec AEM GraphQL à l’aide de requêtes persistantes (par opposition à [requêtes GraphQL standard](#graphl-queries)) permet aux développeurs de conserver une requête (mais pas ses résultats) dans AEM, puis de demander que la requête soit exécutée par son nom. Les requêtes persistantes sont similaires au concept des procédures stockées dans les bases de données SQL.

Les requêtes persistantes ont tendance à être plus performantes que les requêtes GraphQL standard, car les requêtes persistantes sont exécutées à l’aide de GET HTTP, qui sont plus faciles à mettre en cache sur les niveaux CDN et AEM Dispatcher. Les requêtes persistantes définissent également une API et dissocient la nécessité pour le développeur de comprendre les détails de chaque modèle de fragment de contenu.

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
```

+++

+++ React useEffect(..) example

Installez le [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) en exécutant la fonction `npm install` à partir de la racine de votre projet React.

```
$ npm i @adobe/aem-headless-client-js
```

Cet exemple de code montre comment utiliser la variable [React useEffect(..) hook](https://reactjs.org/docs/hooks-effect.html) pour exécuter un appel asynchrone à AEM GraphQL.

Utilisation `useEffect` pour que l’appel GraphQL asynchrone dans React soit utile, procédez comme suit :

1. Il fournit un wrapper synchrone pour l’appel asynchrone à AEM.
1. Cela réduit les AEM inutiles.

Ce code suppose une requête persistante avec le nom . `wknd/adventureNames` a été créé sur AEM Author et publié sur AEM Publish.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

Et appelez ce code à partir d’un autre emplacement du code React.

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
