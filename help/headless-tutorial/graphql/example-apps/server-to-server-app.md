---
title: Application Node.js serveur à serveur - AEM exemple sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application Node.js côté serveur explique comment interroger le contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Application Node.js serveur à serveur

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application serveur à serveur explique comment interroger le contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes et l’imprimer sur un terminal.

![Application Node.js serveur à serveur avec AEM sans affichage](./assets/server-to-server-app/server-to-server-app.png)

Afficher la variable [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## Configuration requise AEM

L’application Node.js fonctionne avec les options de déploiement AEM suivantes. Tous les déploiements nécessitent la variable [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) à installer.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=fr)
+ Éventuellement, [informations d’identification du service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) si vous autorisez des requêtes (par exemple, la connexion au service AEM Author).

Cette application Node.js peut se connecter à AEM Author ou AEM Publish en fonction des paramètres de ligne de commande.

## Utilisation

1. Cloner le `adobe/aem-guides-wknd-graphql` référentiel :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Ouvrez un terminal et exécutez les commandes :

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. L’application peut être exécutée à l’aide de la commande :

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   Par exemple, pour exécuter l’application sur AEM Publish sans autorisation :

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   Pour exécuter l’application par rapport à l’auteur AEM avec autorisation :

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Une liste JSON des aventures du site de référence WKND doit être imprimée dans le terminal.

## Le code

Vous trouverez ci-dessous un résumé de la création de l’application Node.js serveur à serveur, de la manière dont elle se connecte à AEM sans affichage pour récupérer du contenu à l’aide de requêtes persistantes GraphQL et de la manière dont ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app).

Le cas d’utilisation courant pour les AEM sans affichage serveur consiste à synchroniser les données de fragments de contenu d’AEM dans d’autres systèmes. Toutefois, cette application est intentionnellement simple et imprime les résultats JSON de la requête persistante.

### Requêtes persistantes

En suivant AEM bonnes pratiques sans affichage, l’application utilise AEM requêtes persistantes GraphQL pour interroger les données d’aventure. L’application utilise deux requêtes persistantes :

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

### Création d’AEM client sans affichage

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

AEM requêtes persistantes sont exécutées sur une GET HTTP et, par conséquent, la variable [AEM client sans affichage pour Node.js](https://github.com/adobe/aem-headless-client-nodejs) est utilisé pour [exécuter les requêtes GraphQL persistantes ;](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) par rapport à AEM et récupère le contenu de l’aventure.

La requête conservée est appelée en appelant `aemHeadlessClient.runPersistedQuery(...)`, et transmission du nom de requête GraphQL persistant. Une fois que GraphQL renvoie les données, transmettez-les à la méthode simplifiée `doSomethingWithDataFromAEM(..)` , qui imprime les résultats, mais envoie généralement les données à un autre système, ou génère une sortie basée sur les données récupérées.

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
