---
title: Application Node.js serveur à serveur - Exemple AEM Headless
description: Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application Node.js côté serveur montre comment interroger du contenu à l’aide des API GraphQL d’AEM et des requêtes persistantes.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
source-git-commit: d3ee129cb228f02d5a5846465400c04ce81dfbb5
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 100%

---

# Application Node.js serveur à serveur

Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application serveur à serveur montre comment interroger du contenu à l’aide des API GraphQL d’AEM en utilisant des requêtes persistantes et l’imprimer sur un terminal.

![Application Node.js serveur à serveur avec AEM Headless.](./assets/server-to-server-app/server-to-server-app.png)

Afficher le [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [Node.js v18](https://nodejs.org/fr)
+ [Git](https://git-scm.com/)

## Configuration requise d’AEM

L’application Node.js fonctionne avec les options de déploiement d’AEM suivantes. Tous les déploiements nécessitent que la version [v3.0.0 ou supérieure du site WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) soit installée.

+ [AEM as a Cloud Service.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=fr)
+ Vous aurez éventuellement besoin des [informations d’identification du service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=fr) si vous autorisez des requêtes (par exemple, la connexion au service création AEM).

Cette application Node.js peut se connecter à l’instance de création ou de publication AEM en fonction des paramètres de ligne de commande.

## Utilisation

1. Clonez le référentiel `adobe/aem-guides-wknd-graphql` :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Ouvrez un terminal et exécutez les commandes :

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. L’application peut être exécutée à l’aide de la commande :

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   Par exemple, pour exécuter l’application sur l’instance de publication AEM sans autorisation, saisissez les commandes suivantes :

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   Pour exécuter l’application sur l’instance de création AEM avec autorisation, saisissez les commandes :

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Une liste JSON des Adventures du site de référence WKND doit être imprimée dans le terminal.

## Le code

Vous trouverez ci-dessous un résumé de la création de l’application Node.js serveur à serveur, de la manière dont elle se connecte à AEM Headless pour récupérer du contenu à l’aide des requêtes persistantes GraphQL et de la façon dont ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

Un cas d’utilisation courant des applications AEM Headless serveur à serveur consiste à synchroniser les données de fragments de contenu d’AEM dans d’autres systèmes. Toutefois, cette application est volontairement simple et imprime les résultats JSON de la requête persistante.

### Requêtes persistantes

Conformément aux bonnes pratiques d’AEM Headless, l’application utilise les requêtes persistantes GraphQL d’AEM pour interroger les données d’Adventure. L’application utilise deux requêtes persistantes :

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

### Créer un client AEM Headless

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### Exécuter la requête persistante GraphQL

Les requêtes persistantes d’AEM sont exécutées à l’aide de la méthode HTTP GET et, par conséquent, le [client AEM Headless pour Node.js](https://github.com/adobe/aem-headless-client-nodejs) est utilisé pour [exécuter les requêtes persistantes GraphQL](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) sur AEM et récupérer le contenu de l’Adventure.

La requête persistante est appelée en appelant `aemHeadlessClient.runPersistedQuery(...)` et en transmettant le nom de la requête persistante GraphQL. Une fois que GraphQL renvoie les données, transmettez-les à la fonction simplifiée `doSomethingWithDataFromAEM(..)`. Cette dernière imprime les résultats, mais envoie généralement les données à un autre système, ou génère une sortie basée sur les données récupérées.

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
