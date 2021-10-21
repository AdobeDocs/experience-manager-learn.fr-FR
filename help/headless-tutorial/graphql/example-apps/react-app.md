---
title: Application React - AEM Exemple sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Une application React est fournie. Elle indique comment interroger le contenu à l’aide des API GraphQL d’AEM. Le client AEM sans affichage pour JavaScript est utilisé pour exécuter les requêtes GraphQL qui alimentent l’application.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 0ab14016c27d3b91252f3cbf5f97550d89d4a0c9
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---


# React App

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Une application React est fournie. Elle indique comment interroger le contenu à l’aide des API GraphQL d’AEM. Le client AEM sans affichage pour JavaScript est utilisé pour exécuter les requêtes GraphQL qui alimentent l’application.

![React Application](./assets/react-screenshot.png)

A full step by step tutorial is available [here](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=fr).

## Prérequis {#prerequisites}

The following tools should be installed locally:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Conditions AEM

The application is designed to connect to an AEM **Author** or **Publish** environment with the latest release of the [WKND Reference site](https://github.com/adobe/aem-guides-wknd/releases/latest) installed.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=fr)

Nous vous recommandons [déploiement du site de référence WKND dans un environnement Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). A local setup using [the AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) or [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) can also be used.

## Mode d’emploi

1. Cloner le `aem-guides-wknd-graphql` référentiel :

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifiez la variable `aem-guides-wknd/react-app/.env.development` et assurez-vous que `REACT_APP_HOST_URI` pointe vers votre instance AEM cible. Update the authentication method (if connecting to an author instance).

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
   ```

1. Ouvrez un terminal et exécutez les commandes :

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```
1. Une nouvelle fenêtre de navigateur doit s’afficher [http://localhost:3000](http://localhost:3000)
1. A list of adventures from the WKND reference site should be displayed on the application.

## The code

Vous trouverez ci-dessous un bref résumé des fichiers et du code importants utilisés pour alimenter l’application. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### AEM Headless Client for JavaScript

The [AEM Headless Client](https://github.com/adobe/aem-headless-client-js) is used to execute the GraphQL query. AEM Client headless fournit deux méthodes pour exécuter des requêtes, [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) et [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` exécute une requête GraphQL standard pour AEM contenu et est le type d’exécution de requête le plus courant.

[Requêtes persistantes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) sont une fonctionnalité d’AEM qui met en cache les résultats d’une requête GraphQL, puis rend le résultat disponible sur GET. Les requêtes persistantes doivent être utilisées pour les requêtes courantes qui seront exécutées sans cesse. In this application the list of Adventures is the first query executed on the home screen. This will be a very popular query and therefore a persisted query should be used. `runPersistedQuery` exécute une requête sur un point de terminaison de requête persistant.

`src/api/useGraphQL.js` is a [React Effect Hook](https://reactjs.org/docs/hooks-overview.html#effect-hook) that listens for changes to the parameter `query` and `path`. If `query` est vide, puis une requête persistante est utilisée en fonction de la variable `path`. C’est là que le client AEM sans affichage est créé et utilisé pour récupérer des données.

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### Contenu aventure

L’application affiche principalement une liste d’aventures et permet aux utilisateurs de cliquer sur les détails d’une aventure.

`Adventures.js` - Affiche une liste de cartes d’aventures.  L’état initial utilise la variable [Requêtes persistantes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) qui [préconditionné](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) avec le site de référence WKND. Le point de terminaison est `/wknd/adventures-all`. Plusieurs boutons permettent à l’utilisateur de tester les résultats de filtrage en fonction d’une activité :

```javascript
function filterQuery(activity) {
  return `
    {
      adventureList (filter: {
        adventureActivity: {
          _expressions: [
            {
              value: "${activity}"
            }
          ]
        }
      }){
        items {
          _path
        adventureTitle
        adventurePrice
        adventureTripLength
        adventurePrimaryImage {
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
  `;
}
```

`AdventureDetail.js` - Affiche une vue détaillée de l’aventure. Effectue une requête graphQL basée sur le chemin d’accès à l’aventure qui est analysé à partir de l’URL :

```javascript
//parse the content fragment from the url
const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
...
function adventureDetailQuery(_path) {
  return `{
    adventureByPath (_path: "${_path}") {
      item {
        _path
          adventureTitle
          adventureActivity
          adventureType
          adventurePrice
          adventureTripLength
          adventureGroupSize
          adventureDifficulty
          adventurePrice
          adventurePrimaryImage {
            ... on ImageRef {
              _path
              mimeType
              width
              height
            }
          }
          adventureDescription {
            html
          }
          adventureItinerary {
            html
          }
      }
    }
  }
  `;
}
```

### Variables d’environnement

Plusieurs [variables d&#39;environnement](https://create-react-app.dev/docs/adding-custom-environment-variables) sont utilisés par ce projet pour se connecter à un environnement AEM. La valeur par défaut se connecte à un environnement de création AEM s’exécutant à l’adresse http://localhost:4502. Si vous souhaitez modifier ce comportement, mettez à jour la variable `.env.development` en conséquence :

* `REACT_APP_HOST_URI=http://localhost:4502` - Défini sur AEM hôte cible
* `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json` - Set the GraphQL endpoint path
* `REACT_APP_AUTH_METHOD=` - The preferred authentication method. Optional, as per default no authentication is used.
   * `service-token` - use Service token exchange for Cloud Env PROD
   * `dev-token` - Utilisation du jeton de développement pour le développement local avec Cloud Env
   * `basic` : utilisez user/pass pour le développement local avec l’environnement d’auteur local
   * leave blank to use no authentication method
* `REACT_APP_AUTHORIZATION=admin:admin` - set basic auth credentials to use if connecting to an AEM Author environment (for development only). If connecting to a Publish environment, this setting is not necessary.
* `REACT_APP_DEV_TOKEN` - Chaîne du jeton de développement. To connect to remote instance, beside Basic auth (user:pass) you can use Bearer auth with DEV token from Cloud console
* `REACT_APP_SERVICE_TOKEN` - Chemin d’accès au fichier de jeton de service. Pour vous connecter à une instance distante, l’authentification peut également être effectuée avec le jeton de service (télécharger le fichier depuis la console Cloud).

### Requêtes d’API de proxy

Lors de l’utilisation du serveur de développement webpack (`npm start`), le projet repose sur un [configuration du proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. Le fichier est configuré à l’adresse [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) et repose sur plusieurs variables d’environnement personnalisées définies sur `.env` et `.env.development`.

Si vous vous connectez à un environnement de création AEM, la méthode d’authentification correspondante doit être configurée.

### CORS - Partage des ressources cross-origin

Ce projet repose sur une configuration CORS s’exécutant sur l’environnement AEM cible et suppose que l’application s’exécute sur http://localhost:3000 en mode de développement. Le [Configuration des RCO](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) fait partie de la variable [Site de référence WKND](https://github.com/adobe/aem-guides-wknd).

![Configuration CORS](assets/cross-origin-resource-sharing-configuration.png)

*Exemple de configuration CORS pour l’environnement de création*