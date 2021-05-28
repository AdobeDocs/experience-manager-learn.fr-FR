---
title: Intégration d’un SPA | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment le code source d’une application d’une seule page (SPA) écrite dans React peut être intégré à un projet Adobe Experience Manager (AEM). Découvrez comment utiliser des outils front-end modernes, tels qu’un serveur de développement webpack, pour développer rapidement le SPA par rapport à l’API de modèle JSON AEM.
sub-product: sites
feature: Éditeur de SPA
version: cloud-service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1851'
ht-degree: 3%

---


# Intégrer la SPA {#developer-workflow}

Découvrez comment le code source d’une application d’une seule page (SPA) écrite dans React peut être intégré à un projet Adobe Experience Manager (AEM). Découvrez comment utiliser des outils front-end modernes, tels qu’un serveur de développement webpack, pour développer rapidement le SPA par rapport à l’API de modèle JSON AEM.

## Intention

1. Découvrez comment le projet SPA est intégré à AEM avec les bibliothèques côté client.
2. Découvrez comment utiliser un serveur de développement Webpack pour le développement front-end dédié.
3. Explorez l’utilisation d’un fichier **proxy** et **mock** statique pour le développement par rapport à l’API de modèle JSON AEM.

## Ce que vous allez créer

Dans ce chapitre, vous allez apporter plusieurs petites modifications au SPA afin de comprendre comment il est intégré à AEM.
Ce chapitre ajoute un composant `Header` simple au SPA. Dans le processus de création de ce composant **statique** `Header`, plusieurs approches de développement d’AEM SPA seront utilisées.

![Nouvel en-tête dans AEM](./assets/integrate-spa/final-header-component.png)

*Le SPA est étendu pour ajouter un  `Header` composant statique.*

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment). Ce chapitre est la suite du chapitre [Créer un projet](create-project.md), mais vous n’avez besoin que d’un projet AEM SPA opérationnel.

## Approche d’intégration {#integration-approach}

Deux modules ont été créés dans le cadre du projet AEM : `ui.apps` et `ui.frontend`.

Le module `ui.frontend` est un projet [webpack](https://webpack.js.org/) qui contient tout le code source SPA. La majorité du développement et des tests SPA seront effectués dans le projet webpack. Lorsqu’une version de production est déclenchée, la SPA est créée et compilée à l’aide de webpack. Les artefacts compilés (CSS et Javascript) sont copiés dans le module `ui.apps` qui est ensuite déployé sur le runtime AEM.

![architecture de haut niveau ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Description de haut niveau de l’intégration SPA.*

Vous trouverez des informations supplémentaires sur la version front-end [ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect de l’intégration SPA {#inspect-spa-integration}

Examinez ensuite le module `ui.frontend` pour comprendre le SPA généré automatiquement par l’[archétype de projet AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. Dans l’IDE de votre choix, ouvrez votre projet AEM. Ce tutoriel utilisera [Visual Studio Code IDE](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM projet WKND SPA](./assets/integrate-spa/vscode-ide-openproject.png)

1. Développez et inspectez le dossier `ui.frontend`. Ouvrez le fichier `ui.frontend/package.json`

1. Sous `dependencies`, vous devriez voir plusieurs éléments liés à `react` y compris `react-scripts`

   `ui.frontend` est une application React basée sur [Create React App](https://create-react-app.dev/) ou CRA, en bref. La version `react-scripts` indique la version de CRA utilisée.

1. Il existe également plusieurs dépendances dotées du préfixe `@adobe` :

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   Les modules ci-dessus constituent le [AEM SDK JS de l’éditeur SPA](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) et fournissent la fonctionnalité permettant de mapper les composants SPA aux composants.

   Sont également inclus [AEM Composants WCM - Mise en oeuvre principale React](https://github.com/adobe/aem-react-core-wcm-components-base) et [AEM Composants WCM - Éditeur de Spa - Mise en oeuvre principale React](https://github.com/adobe/aem-react-core-wcm-components-spa). Il s’agit d’un ensemble de composants réutilisables de l’interface utilisateur qui correspondent aux composants AEM prêts à l’emploi. Ils sont conçus pour être utilisés en l’état et mis en forme afin de répondre aux besoins de votre projet.

1. Dans le fichier `package.json`, plusieurs `scripts` sont définis :

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Il s’agit de scripts de génération standard rendus [disponibles](https://create-react-app.dev/docs/available-scripts) par l’application Create React.

   La seule différence est l’ajout de `&& clientlib` au script `build`. Cette instruction supplémentaire est chargée de copier le SPA compilé dans le module `ui.apps` en tant que bibliothèque côté client lors d’une génération.

   Le module npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) est utilisé pour faciliter cette opération.

1. Inspectez le fichier `ui.frontend/clientlib.config.js`. Ce fichier de configuration est utilisé par [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) pour déterminer comment générer la bibliothèque cliente.

1. Inspectez le fichier `ui.frontend/pom.xml`. Ce fichier transforme le dossier `ui.frontend` en module [Maven](http://maven.apache.org/guides/mini/guide-multiple-modules.html). Le fichier `pom.xml` a été mis à jour afin d’utiliser [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) en **test** et **build** le SPA lors d’une génération Maven.

1. Inspect le fichier `index.js` à `ui.frontend/src/index.js` :

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js` est le point d’entrée du SPA. `ModelManager` est fourni par le SDK JS de l’éditeur d’AEM SPA. Il est chargé d’appeler et d’injecter le `pageModel` (contenu JSON) dans l’application.

1. Inspect le fichier `import-component.js` à `ui.frontend/src/import-components.js`. Ce fichier importe les **composants principaux React** prêts à l’emploi et les rend disponibles pour le projet. Nous allons examiner le mappage du contenu AEM aux composants SPA dans le prochain chapitre.

## Ajouter un composant SPA statique {#static-spa-component}

Ajoutez ensuite un nouveau composant au SPA et déployez les modifications sur une instance AEM locale. Ce sera un changement simple, juste pour illustrer comment le SPA est mis à jour.

1. Dans le module `ui.frontend` , sous `ui.frontend/src/components` créez un dossier nommé `Header`.
1. Créez un fichier nommé `Header.js` sous le dossier `Header`.

   ![Dossier et fichier d’en-tête](assets/create-project/header-folder-js.png)

1. Remplissez `Header.js` avec les éléments suivants :

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   Vous trouverez ci-dessus un composant React standard qui génère une chaîne de texte statique.

1. Ouvrez le fichier `ui.frontend/src/App.js`. Il s’agit du point d’entrée de l’application.
1. Effectuez les mises à jour suivantes de `App.js` pour inclure la balise `Header` statique :

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

1. Ouvrez un nouveau terminal, accédez au dossier `ui.frontend` et exécutez la commande `npm run build` :

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

1. Accédez au dossier `ui.apps`. Sous `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react`, vous devriez voir que les fichiers SPA compilés ont été copiés à partir du dossier `ui.frontend/build` .

   ![Bibliothèque cliente générée dans ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

1. Revenez au terminal et accédez au dossier `ui.apps` . Exécutez la commande Maven suivante :

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   Le package `ui.apps` sera ainsi déployé sur une instance d’exécution locale d’AEM.

1. Ouvrez un onglet de navigateur et accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Vous devriez maintenant voir le contenu du composant `Header` affiché dans le SPA.

   ![Implémentation initiale de l’en-tête](./assets/integrate-spa/initial-header-implementation.png)

   Les étapes ci-dessus sont exécutées automatiquement lors du déclenchement d’une version Maven à partir de la racine du projet (c’est-à-dire `mvn clean install -PautoInstallSinglePackage`). Vous devez maintenant comprendre les principes de base de l’intégration entre les bibliothèques SPA et AEM côté client. Notez que vous pouvez toujours modifier et ajouter des composants `Text` dans AEM sous le composant `Header` statique.

## Serveur de développement Webpack : proxy de l’API JSON {#proxy-json}

Comme vous l’avez vu dans les exercices précédents, l’exécution d’une version et la synchronisation de la bibliothèque cliente avec une instance locale d’AEM prend quelques minutes. Cela est acceptable pour les tests finaux, mais n’est pas idéal pour la majorité du développement SPA.

Un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) peut être utilisé pour développer rapidement le SPA. Le SPA est piloté par un modèle JSON généré par AEM. Dans cet exercice, le contenu JSON d’une instance en cours d’exécution de l’AEM sera **proxy** vers le serveur de développement.

1. Revenez à l’IDE et ouvrez le fichier `ui.frontend/package.json`.

   Recherchez une ligne comme celle-ci :

   ```json
   "proxy": "http://localhost:4502",
   ```

   La fonction [Créer une application React](https://create-react-app.dev/docs/proxying-api-requests-in-development) fournit un mécanisme facile pour proxy des requêtes d’API. Toutes les requêtes inconnues seront traitées par proxy via `localhost:4502`, l’AEM quickstart local.

1. Ouvrez une fenêtre de terminal et accédez au dossier `ui.frontend` . Exécutez la commande `npm start` :

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

1. Ouvrez un nouvel onglet du navigateur (s’il n’est pas déjà ouvert) et accédez à [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Serveur de développement Webpack - json proxy](./assets/integrate-spa/webpack-dev-server-1.png)

   Vous devriez voir le même contenu que dans AEM, mais sans aucune des fonctionnalités de création activées.

   >[!NOTE]
   >
   > En raison des exigences de sécurité d’AEM, vous devrez être connecté à l’instance AEM locale (http://localhost:4502) dans le même navigateur, mais dans un autre onglet.

1. Revenez à l’IDE et créez un fichier nommé `Header.css` dans le dossier `src/components/Header`.
1. Renseignez `Header.css` avec les éléments suivants :

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![VSCode IDE](assets/integrate-spa/header-css-update.png)

1. Ouvrez à nouveau `Header.js` et ajoutez la ligne suivante pour référencer `Header.css` :

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   Enregistrez les modifications.

1. Accédez à [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) pour afficher les modifications de style automatiquement répercutées.

1. Ouvrez le fichier `Page.css` dans `ui.frontend/src/components/Page`. Apportez les modifications suivantes pour corriger le remplissage :

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. Revenez au navigateur à l’adresse [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Les modifications apportées à l’application devraient immédiatement être répercutées.

   ![Style ajouté à l’en-tête](assets/integrate-spa/added-logo-localhost.png)

   Vous pouvez continuer à effectuer des mises à jour de contenu dans AEM et les voir répercutées dans **webpack-dev-server**, puisque nous effectuons un proxy pour le contenu.

1. Arrêtez le serveur de développement webpack avec `ctrl+c` dans le terminal.

## Déployer SPA mises à jour dans AEM

Les modifications apportées à `Header` ne sont actuellement visibles que par le biais de **webpack-dev-server**. Déployez le SPA mis à jour pour AEM afficher les modifications.

1. Accédez à la racine du projet (`aem-guides-wknd-spa`) et déployez le projet vers AEM à l’aide de Maven :

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Les styles `Header` et mis à jour doivent être appliqués.

   ![En-tête mis à jour dans AEM](assets/integrate-spa/final-header-component.png)

   Maintenant que la SPA mise à jour est dans AEM, la création peut continuer.

## Félicitations !  {#congratulations}

Félicitations, vous avez mis à jour le SPA et exploré l&#39;intégration avec AEM ! Vous savez comment développer la SPA par rapport à l’API de modèle JSON AEM à l’aide d’un **webpack-dev-server**.

### Étapes suivantes {#next-steps}

[Mappage SPA composants à AEM composants](map-components.md)  : découvrez comment mapper les composants React aux composants Adobe Experience Manager () avec le SDK JS de l’éditeur d’. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme pour la création d’ traditionnelle.

## (Bonus) Serveur de développement Webpack - Mock JSON API {#mock-json}

Une autre méthode de développement rapide consiste à utiliser un fichier JSON statique pour agir comme modèle JSON. En &quot;moquant&quot; le JSON, nous supprimons la dépendance sur une instance d’AEM locale. Il permet également à un développeur front-end de mettre à jour le modèle JSON afin de tester la fonctionnalité et d’apporter des modifications à l’API JSON qui seront ensuite implémentées par un développeur principal.

La configuration initiale du fichier JSON de simulation requiert **une instance d’AEM locale**.

1. Revenez à l’IDE, accédez à `ui.frontend/public` et ajoutez un nouveau dossier nommé `mock-content`.
1. Créez un nouveau fichier nommé `mock.model.json` sous `ui.frontend/public/mock-content`.
1. Dans le navigateur, accédez à [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Il s’agit du fichier JSON exporté par AEM qui dirige l’application. Copiez la sortie JSON.

1. Collez la sortie JSON de l’étape précédente dans le fichier `mock.model.json`.

   ![Simulation du fichier Json du modèle](./assets/integrate-spa/mock-model-json-created.png)

1. Ouvrez le fichier `index.html` dans `ui.frontend/public/index.html`. Mettez à jour la propriété de métadonnées du modèle de page AEM pour qu’elle pointe vers une variable `%REACT_APP_PAGE_MODEL_PATH%` :

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   L’utilisation d’une variable pour la valeur de `cq:pagemodel_root_url` facilite le basculement entre le modèle proxy et le modèle json de simulation.

1. Ouvrez le fichier `ui.frontend/.env.development` et effectuez les mises à jour suivantes pour ajouter la valeur précédente pour `REACT_APP_PAGE_MODEL_PATH` :

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. En cas d’exécution actuelle, arrêtez le **serveur webpack-dev-server**. Démarrez le **webpack-dev-server** à partir du terminal :

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Accédez à [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) et vous devriez voir le SPA avec le même contenu utilisé dans le json **proxy**.

1. Apportez une petite modification au fichier `mock.model.json` créé précédemment. Le contenu mis à jour devrait immédiatement être reflété dans la balise **webpack-dev-server**.

   ![mise à jour json du modèle mock](./assets/integrate-spa/webpack-mock-model.gif)

Être en mesure de manipuler le modèle JSON et de voir les effets sur un SPA en direct peut aider un développeur à comprendre l’API du modèle JSON. Il permet également le développement front-end et back-end en parallèle.

Vous pouvez désormais basculer sur l’emplacement où utiliser le contenu JSON en faisant basculer les entrées dans le fichier `env.development` :

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
