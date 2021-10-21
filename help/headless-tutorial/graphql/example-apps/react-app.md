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
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 6%

---


# React App

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Une application React est fournie. Elle indique comment interroger le contenu à l’aide des API GraphQL d’AEM. Le client AEM sans affichage pour JavaScript est utilisé pour exécuter les requêtes GraphQL qui alimentent l’application.

![React Application](./assets/react-screenshot.png)

Un tutoriel détaillé complet est disponible [here](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=fr).

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Conditions AEM

L’application est conçue pour se connecter à une AEM **Auteur** ou **Publier** environnement avec la dernière version d’ [Site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) installé.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=fr)

Nous vous recommandons [déploiement du site de référence WKND dans un environnement Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Une configuration locale à l’aide de [le SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) ou [Fichier jar QuickStart AEM 6.5](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) peut également être utilisé.

## Mode d’emploi

1. Cloner le `aem-guides-wknd-graphql` référentiel :

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifiez la variable `aem-guides-wknd/react-app/.env.development` et assurez-vous que `REACT_APP_HOST_URI` pointe vers votre instance AEM cible. Mettez à jour la méthode d’authentification (en cas de connexion à une instance d’auteur).

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
1. Une liste des aventures du site de référence WKND doit s’afficher sur l’application.

## Le code

Vous trouverez ci-dessous un bref résumé des fichiers et du code importants utilisés pour alimenter l’application. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### AEM client sans affichage pour JavaScript

Le [AEM client sans affichage](https://github.com/adobe/aem-headless-client-js) est utilisé pour exécuter la requête GraphQL. AEM Client headless fournit deux méthodes pour exécuter des requêtes, [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) et [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` exécute une requête GraphQL standard pour AEM contenu et est le type d’exécution de requête le plus courant.

[Requêtes persistantes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) sont une fonctionnalité d’AEM qui met en cache les résultats d’une requête GraphQL, puis rend le résultat disponible sur GET. Les requêtes persistantes doivent être utilisées pour les requêtes courantes qui seront exécutées sans cesse. Dans cette application, la liste des aventures est la première requête exécutée sur l&#39;écran d&#39;accueil. Il s’agit d’une requête très populaire et une requête persistante doit donc être utilisée. `runPersistedQuery` exécute une requête sur un point de terminaison de requête persistant.

`src/api/useGraphQL.js` est un [React Effet Hook](https://reactjs.org/docs/hooks-overview.html#effect-hook) qui écoute les modifications apportées au paramètre `query` et `path`. If `query` est vide, puis une requête persistante est utilisée en fonction de la variable `path`. C’est là que le client AEM sans affichage est créé et utilisé pour récupérer des données.

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

`Adventures.js` - Affiche une liste de cartes d’aventures.