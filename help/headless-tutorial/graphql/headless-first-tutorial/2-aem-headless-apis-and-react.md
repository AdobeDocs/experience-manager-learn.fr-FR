---
title: API AEM Headless et React - Premier tutoriel AEM Headless
description: Découvrez comment couvrir la récupération des données de fragments de contenu des API GraphQL d’AEM et leur affichage dans l’application React.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 100%

---

# API AEM Headless et React

Bienvenue dans ce chapitre de tutoriel où nous allons explorer la configuration d’une application React pour établir une connexion avec les API Adobe Experience Manager (AEM) Headless à l’aide du SDK AEM Headless. Nous allons passer en revue la récupération des données de fragments de contenu des API GraphQL d’AEM et leur affichage dans l’application React.

Les API AEM Headless permettent d’accéder au contenu AEM à partir de n’importe quelle application cliente. Nous vous guiderons tout au long de la configuration de votre application React pour vous connecter aux API AEM Headless à l’aide du SDK AEM Headless. Cette configuration établit un canal de communication réutilisable entre votre application React et AEM.

Ensuite, nous utiliserons le SDK AEM Headless pour récupérer les données de fragments de contenu des API GraphQL d’AEM. Les fragments de contenu dans AEM fournissent une gestion de contenu structurée. En utilisant le SDK AEM Headless, vous pouvez facilement interroger et récupérer des données de fragments de contenu à l’aide de GraphQL.

Une fois que nous aurons les données de fragment de contenu, nous les intégrerons à votre application React. Vous apprendrez à formater et afficher les données de manière attrayante. Nous aborderons les bonnes pratiques relatives à la gestion et au rendu des données de fragments de contenu dans les composants React, afin d’assurer une intégration transparente à l’interface utilisateur de votre application.

Tout au long du tutoriel, nous vous proposons des explications, des exemples de code et des conseils pratiques. D’ici la fin, vous pourrez configurer votre application React pour vous connecter aux API AEM Headless, récupérer les données de fragments de contenu à l’aide du SDK AEM Headless et les afficher facilement dans votre application React. Commençons.


## Cloner l’application React

1. Clonez l’application depuis [Github](https://github.com/lamontacrook/headless-first/tree/main) en exécutant la commande suivante sur la ligne de commande.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Passez au référentiel `headless-first` et installez les dépendances.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configurer l’application React

1. Créez un fichier nommé `.env` à la racine du projet. Dans `.env`, définissez les valeurs suivantes :

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Vous pouvez récupérer un jeton de développement dans Cloud Manager. Connectez-vous à [Adobe Cloud Manager](https://experience.adobe.com/). Cliquez sur __Experience Manager > Cloud Manager__. Sélectionnez le Programme approprié, puis cliquez sur les points de suspension à côté d’Environnement.

   ![AEM Developer Console.](./assets/2/developer-console.png)

   1. Cliquez sur l’onglet __Intégrations__.
   1. Cliquez sur l’__onglet Jeton local et sur le bouton Obtenir un jeton de développement local__.
   1. Copiez le jeton d’accès contenu entre les guillemets.
   1. Collez le jeton copié comme valeur pour `REACT_APP_TOKEN` dans le fichier `.env`.
   1. Créons maintenant l’application en exécutant `npm ci` sur la ligne de commande.
   1. Démarrez l’application React en exécutant `npm run start` sur la ligne de commande.
   1. Dans [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils), un fichier nommé `context.js` inclut le code permettant de définir les valeurs dans le fichier `.env` dans le contexte de l’application.

## Démarrer l’application React

1. Démarrez l’application React en exécutant `npm run start` sur la ligne de commande.

   ```
   $ npm run start
   ```

   L’application React démarre et ouvre une fenêtre de navigateur pour `http://localhost:3000`. Les modifications apportées à l’application React seront automatiquement rechargées dans le navigateur.

## Connexion aux API AEM Headless

1. Pour connecter l’application React à AEM as a Cloud Service, ajoutons quelques éléments à `App.js`. Dans l’import `React`, ajoutez `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importez `AppContext` depuis le fichier `context.js`.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Dans le code de l’application, définissez une variable contextuelle.

   ```javascript
   const context = useContext(AppContext);
   ```

   Enfin, placez le code de retour dans `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   À titre de référence, `App.js` devrait désormais ressembler à ce qui suit.

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. Importez le SDK `AEMHeadless`. Ce SDK est une bibliothèque d’assistance utilisée par l’application pour interagir avec les API AEM Headless.

   Ajoutez cette instruction d’import à `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Ajoutez l’élément `{ useContext, useEffect, useState }` suivant à l’instruction d’import ` React`.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importez le `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Dans le composant `Home`, obtenez la variable `context` à partir du `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Initialisez le SDK AEM Headless dans un élément `useEffect()`, puisque le SDK AEM Headless doit changer lorsque la variable `context` change.

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > Il existe un fichier `context.js` sous `/utils` qui lit des éléments du fichier `.env`. À titre de référence, l’`context.url` est l’URL de l’environnement AEM as a Cloud Service. Le `context.endpoint` est le chemin d’accès complet au point d’entrée créé dans la leçon précédente. Enfin, le `context.token` est le jeton de développement.


1. Créez un état React qui expose le contenu provenant du SDK AEM Headless.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Connectez l’application à AEM. Utilisez la requête persistante créée dans la leçon précédente. Ajoutez le code suivant dans l’élément `useEffect` une fois le SDK AEM Headless initialisé. Faites en sorte que l’élément `useEffect` dépende de la variable `context` comme illustré ci-dessous.


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. Ouvrez la vue Réseau des outils de développement pour passer en revue la requête GraphQL.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Outils de développement Chrome.](./assets/2/dev-tools.png)

   Le SDK AEM Headless code la requête pour GraphQL et ajoute les paramètres fournis. Vous pouvez ouvrir la requête dans le navigateur.

   >[!NOTE]
   >
   > Puisque la demande est envoyée à l’environnement de création, vous devez disposer d’une connexion à l’environnement dans un autre onglet du même navigateur.


## Rendre le contenu du fragment de contenu

1. Affichez les fragments de contenu dans l’application. Renvoyez un élément `<div>` avec le titre du teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Le champ Titre du teaser doit s’afficher à l’écran.

1. La dernière étape consiste à ajouter le teaser à la page. Un composant de teaser React est inclus dans le package. Tout d’abord, incluons l’import. En haut du fichier `home.js`, ajoutez la ligne :

   `import Teaser from '../../components/teaser/teaser';`

   Mettez à jour l’instruction de retour :

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   Vous devriez maintenant voir le teaser avec le contenu inclus dans le fragment.


## Étapes suivantes

Félicitations. Vous avez correctement mis à jour l’application React afin de l’intégrer aux API AEM Headless à l’aide du SDK AEM Headless.

Créez ensuite un composant Liste d’images plus complexe qui effectue dynamiquement le rendu des fragments de contenu référencés à partir d’AEM.

[Chapitre suivant : créer un composant Liste d’images](./3-complex-components.md)
