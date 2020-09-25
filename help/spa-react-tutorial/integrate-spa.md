---
title: Intégration d’une application d’une seule page | Prise en main de l’éditeur AEM d’application d’une seule page et réaction
description: Comprenez comment le code source d’une application d’une seule page (SPA) écrite dans React peut être intégré à un projet Adobe Experience Manager (AEM). Apprenez à utiliser des outils frontaux modernes, comme un serveur de développement webpack, pour développer rapidement l’application d’une seule page par rapport à l’API de modèle JSON AEM.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '2104'
ht-degree: 1%

---


# Intégration d’une application d’une seule page {#integrate-spa}

Comprenez comment le code source d’une application d’une seule page (SPA) écrite dans React peut être intégré à un projet Adobe Experience Manager (AEM). Apprenez à utiliser des outils frontaux modernes, comme un serveur de développement webpack, pour développer rapidement l’application d’une seule page par rapport à l’API de modèle JSON AEM.

## Intention

1. Comprendre comment le projet SPA est intégré à l’AEM avec les bibliothèques côté client.
2. Découvrez comment utiliser un serveur de développement webpack pour le développement frontal dédié.
3. Explorez l’utilisation d’un **proxy** et d’un fichier **fictif** statique pour le développement par rapport à l’API de modèle JSON AEM.

## Ce que vous allez construire

Ce chapitre ajoute un `Header` composant simple à l’application d’une seule page. Dans le processus d&#39;élaboration de ce `Header` composant statique, on utilisera plusieurs approches pour élaborer AEM ZPS.

![Nouvel en-tête dans AEM](./assets/integrate-spa/final-header-component.png)

*L’application d’une seule page est étendue pour ajouter un`Header`composant statique.*

## Conditions préalables

Examiner les outils et les instructions nécessaires à la mise en place d&#39;un environnement [de développement](overview.md#local-dev-environment)local.

### Obtention du code

1. Téléchargez le point de départ de ce didacticiel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Déployez la base de code sur une instance AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility) , ajoutez le `classic` profil :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) ou le retirer localement en passant à la branche `React/integrate-spa-solution`.

## Approche d&#39;intégration {#integration-approach}

Deux modules ont été créés dans le cadre du projet AEM : `ui.apps` et `ui.frontend`.

Le `ui.frontend` module est un projet [webpack](https://webpack.js.org/) qui contient tout le code source de l’application d’une seule page. La majorité des travaux de développement et de test de l’application d’une seule page seront réalisés dans le projet webpack. Lorsqu’une génération de production est déclenchée, l’application d’une seule page est créée et compilée à l’aide de webpack. Les artefacts compilés (CSS et Javascript) sont copiés dans le `ui.apps` module qui est ensuite déployé sur l’exécution AEM.

![ui.frontend architecture de haut niveau](assets/integrate-spa/ui-frontend-architecture.png)

*Description de haut niveau de l’intégration de l’application d’une seule page.*

Des informations supplémentaires sur la version frontale sont [disponibles ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## inspect : intégration de l’application d’une seule page {#inspect-spa-integration}

Ensuite, examinez le `ui.frontend` module pour comprendre l’application d’une seule page qui a été générée automatiquement par l’archétype [du projet](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)AEM.

1. Dans l&#39;IDE de votre choix, ouvrez l&#39;AEM Project for the WKND SPA. Ce didacticiel utilisera l&#39;IDE [du code](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)Visual Studio.

   ![VSCode - Projet d’application d’une seule page WKND AEM](./assets/integrate-spa/vscode-ide-openproject.png)

2. Développez et inspectez le `ui.frontend` dossier. Open the file `ui.frontend/package.json`

3. Sous `dependencies` cette section, vous devriez voir plusieurs éléments liés à `react` l’inclusion `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   Il `ui.frontend` s&#39;agit d&#39;une application React basée sur l&#39;application [](https://create-react-app.dev/) Create React ou l&#39;agence de notation de crédit (CRA) pour faire court. La `react-scripts` version indique la version de l&#39;ARC utilisée.

4. Il existe également trois dépendances avec `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   Les modules ci-dessus constituent l’ [SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) et offrent la fonctionnalité permettant de mapper les composants SPA à AEM Composants.

5. Le `package.json` fichier comporte plusieurs `scripts` définitions :

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Il s’agit de scripts de génération standard rendus [disponibles](https://create-react-app.dev/docs/available-scripts) par l’application Create React.

   La seule différence est l&#39;ajout de `&& clientlib` au `build` script. Cette instruction supplémentaire est chargée de copier l’application d’une seule page dans le `ui.apps` module en tant que bibliothèque côté client lors de la génération.

   Le module npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) est utilisé pour faciliter cela.

6. inspect le fichier `ui.frontend/clientlib.config.js`. Ce fichier de configuration est utilisé par [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) pour déterminer comment générer la bibliothèque cliente.

7. inspect le fichier `ui.frontend/pom.xml`. Ce fichier transforme le `ui.frontend` dossier en module [](http://maven.apache.org/guides/mini/guide-multiple-modules.html)Maven. Le `pom.xml` fichier a été mis à jour afin d’utiliser le [module externe](https://github.com/eirslett/frontend-maven-plugin) frontend-maven pour **tester** et **créer** l’application d’une seule page pendant une génération Maven.

8. inspect le fichier `index.js` à l&#39;adresse `ui.frontend/src/index.js`:

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

   `index.js` est le point d&#39;entrée du SPA. `ModelManager` est fourni par l’AEM SPA Editor JS SDK. Il est chargé d’appeler et d’injecter le `pageModel` (contenu JSON) dans l’application.

## ajouter un composant d’en-tête {#header-component}

Ensuite, ajoutez un nouveau composant à l’application d’une seule page et déployez les modifications sur une instance d’AEM locale.

1. Dans le `ui.frontend` module, sous `ui.frontend/src/components` créer un nouveau dossier nommé `Header`.
2. Create a file named `Header.js` beneath the `Header` folder.

   ![Dossier et fichier d’en-tête](assets/integrate-spa/header-folder-js.png)

3. Remplissez `Header.js` avec les éléments suivants :

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

   Au-dessus se trouve un composant React standard qui va générer une chaîne de texte statique.

4. Open the file `ui.frontend/src/App.js`. Il s’agit du point d’entrée de l’application.
5. Effectuez les mises à jour suivantes pour `App.js` inclure le fichier statique `Header`:

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

6. Ouvrez un nouveau terminal, accédez au `ui.frontend` dossier et exécutez la `npm run build` commande :

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

7. Accédez au dossier `ui.apps`. Vous `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` devriez voir que les fichiers SPA compilés ont été copiés à partir du dossier`ui.frontend/build` .

   ![Bibliothèque client générée dans ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

8. Revenez au terminal et accédez au `ui.apps` dossier. Exécutez la commande expert suivante :

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

   Le `ui.apps` package sera alors déployé sur une instance d’exécution locale de l’AEM.

9. Ouvrez un onglet de navigateur et accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Vous devriez maintenant voir le contenu du `Header` composant affiché dans l’application d’une seule page.

   ![Implémentation initiale de l’en-tête](./assets/integrate-spa/initial-header-implementation.png)

   Les étapes 6 à 8 sont exécutées automatiquement lors du déclenchement d’une génération Maven à partir de la racine du projet (c.-à-d. `mvn clean install -PautoInstallSinglePackage`). Vous devez maintenant comprendre les principes de base de l’intégration entre l’application d’une seule page et AEM bibliothèques côté client. Vous pouvez toujours modifier et ajouter `Text` des composants dans AEM sous le `Header` composant statique.

## Webpack Dev Server - Proxy de l’API JSON {#proxy-json}

Comme nous l’avons vu dans les exercices précédents, la génération et la synchronisation de la bibliothèque cliente vers une instance locale d’AEM prend quelques minutes. Ceci est acceptable pour les tests finaux, mais n&#39;est pas idéal pour la majorité du développement de l&#39;application d&#39;une seule page.

Un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) peut être utilisé pour développer rapidement l’application d’une seule page. L’application d’une seule page est pilotée par un modèle JSON généré par AEM. Au cours de cet exercice, le contenu JSON d’une instance en cours d’exécution d’AEM sera **propagé** dans le serveur de développement.

1. Revenez à l&#39;IDE et ouvrez le fichier `ui.frontend/package.json`.

   Recherchez une ligne comme celle-ci :

   ```json
   "proxy": "http://localhost:4502",
   ```

   L’application [](https://create-react-app.dev/docs/proxying-api-requests-in-development) Create React offre un mécanisme facile pour répondre aux demandes d’API proxy. Toutes les requêtes inconnues seront traitées par proxy via `localhost:4502`, l&#39;AEM local démarre rapidement.

2. Ouvrez une fenêtre de terminal et accédez au `ui.frontend` dossier. Exécutez la commande `npm start`:

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

3. Ouvrez un nouvel onglet de navigateur (s’il n’est pas déjà ouvert) et accédez à [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Serveur de développement Webpack - json proxy](./assets/integrate-spa/webpack-dev-server-1.png)

   Vous devriez voir le même contenu que dans AEM, mais sans aucune des fonctionnalités de création activées.

   >[!NOTE]
   >
   > En raison des exigences de sécurité de AEM, vous devrez être connecté à l&#39;instance AEM locale (http://localhost:4502) dans le même navigateur mais dans un autre onglet.

4. Revenez à l&#39;IDE et créez un nouveau dossier nommé `media` à `ui.frontend/src/media`.
5. Téléchargez et ajoutez le logo WKND suivant dans le `media` dossier :

   ![Logo WKND](./assets/integrate-spa/wknd-logo-dk.png)

6. Ouvrez `Header.js` à `ui.frontend/src/components/Header/Header.js` et importez le logo :

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. Effectuez les mises à jour suivantes pour inclure `Header.js` le logo dans l’en-tête :

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   Enregistrez les modifications dans `Header.js`.

8. Revenez au navigateur à l’adresse [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Les modifications apportées à l’application doivent être immédiatement répercutées.

   ![Logo ajouté à l&#39;en-tête](./assets/integrate-spa/added-logo-localhost.png)

   Vous pouvez continuer à mettre à jour le contenu dans AEM et à le voir se refléter dans **webpack-dev-server**, puisque nous effectuons un proxy pour le contenu.

9. Arrêtez le serveur de développement webpack avec `ctrl+c` dans le terminal.

## Serveur de développement Webpack - API JSON masqué {#mock-json}

Une autre méthode de développement rapide consiste à utiliser un fichier JSON statique pour faire office de modèle JSON. En &quot;moquant&quot; le fichier JSON, nous supprimons la dépendance sur une instance AEM locale. Il permet également à un développeur frontal de mettre à jour le modèle JSON afin de tester les fonctionnalités et d’apporter des modifications à l’API JSON qui seront ensuite implémentées par un développeur principal.

La configuration initiale du simulateur JSON **nécessite une instance** d’AEM locale.

1. Dans le navigateur, accédez à [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Il s’agit du fichier JSON exporté par AEM qui dirige l’application. Copiez la sortie JSON.

2. Revenez à l&#39;IDE, accédez à `ui.frontend/public` et ajoutez un nouveau dossier nommé `mock-content`.
3. Créez un nouveau fichier nommé `mock.model.json` sous `ui.frontend/public/mock-content`. Collez ici la sortie JSON de l’ **étape 1** .

   ![Fichier Json de modèle maquette](./assets/integrate-spa/mock-model-json-created.png)

4. Open the file `index.html` at `ui.frontend/public/index.html`. Mettez à jour la propriété metadata du modèle de page AEM pour qu’elle pointe vers une variable `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   L’utilisation d’une variable pour la valeur de `cq:pagemodel_root_url` permet de basculer plus facilement entre le modèle proxy et le modèle json simulé.

5. Ouvrez le fichier `ui.frontend/.env.development` et effectuez les mises à jour suivantes pour mettre en commentaire la valeur précédente pour `REACT_APP_PAGE_MODEL_PATH`:

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. Si l’application est en cours d’exécution, arrêtez le **webpack-dev-server**. Début du **webpack-dev-server** depuis le terminal :

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Accédez à [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) et vous devriez voir l’application d’une seule page avec le même contenu utilisé dans le json **proxy** .

7. Apportez une petite modification au `mock.model.json` fichier créé précédemment. Vous devriez voir le contenu mis à jour immédiatement reflété dans le **webpack-dev-server**.

   ![mise à jour du modèle factice json](./assets/integrate-spa/webpack-mock-model.gif)

Être en mesure de manipuler le modèle JSON et de voir ses effets sur une application d’une seule page peut aider un développeur à comprendre l’API du modèle JSON. Il permet également le développement frontal et dorsal en parallèle.

Vous pouvez désormais basculer entre les emplacements où consommer le contenu JSON en modifiant les entrées du `env.development` fichier :

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## ajouter des styles avec Sass

La meilleure pratique consiste à garder chaque composant modulaire et autonome. Une recommandation générale est d’éviter de réutiliser le même nom de classe CSS entre les composants, ce qui rend l’utilisation des préprocesseurs moins puissante. Ce projet utilisera [Sass](https://sass-lang.com/) pour quelques fonctions utiles comme les variables. Ce projet suivra également de manière plus ou moins stricte les conventions [d&#39;appellation](https://github.com/suitcss/suit/blob/master/doc/components.md)SUIT CSS. SUIT est une variante de notation BEM, Modificateur d’élément de bloc, utilisée pour créer des règles CSS cohérentes.

1. Ouvrez une fenêtre de terminal et arrêtez le **webpack-dev-server** si vous démarrez. Dans le `ui.frontend` dossier, saisissez la commande suivante pour [installer Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet):

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   Installer `sass` en tant que dépendance homologue :

   ```shell
   $ npm install sass --save
   ```

2. Installer `normalize-scss` pour normaliser les styles dans les différents navigateurs :

   ```shell
   $ npm install normalize-scss
   ```

3. Début le **webpack-dev-server** pour que les styles soient mis à jour en temps réel :

   ```shell
   $ npm start
   ```

   Utilisez l’approche Proxy of Mock pour gérer l’API du modèle JSON.

4. Revenez à l&#39;IDE et sous `ui.frontend/src` créer un nouveau dossier nommé `styles`.
5. Créez un nouveau fichier sous `ui.frontend/src/styles` le nom `_variables.scss` et renseignez-le avec les variables suivantes :

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. Renommez l’extension du fichier `index.css` à `ui.frontend/src/index.css`**`index.scss`**. Remplacez le contenu par le texte suivant :

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. Mettez à jour `ui.frontend/src/index.js` pour inclure le nouveau nom `index.scss`:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. Créez un nouveau fichier nommé `Header.scss` sous `ui.frontend/src/components/Header`. Remplissez le fichier avec les éléments suivants :

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. Inclure `Header.scss` en mettant à jour `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. Revenez au navigateur et au **webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![En-tête figé - serveur de développement webpack](./assets/integrate-spa/styled-header.png)

   Vous devriez maintenant voir les styles mis à jour ajoutés au `Header` composant.

## Déployer les mises à jour de l’application d’une seule page sur AEM

Les modifications apportées au `Header` logiciel ne sont actuellement visibles que par le biais du **webpack-dev-server**. Déployez l’application d’une seule page pour AEM voir les modifications.

1. Accédez à la racine du projet (`aem-guides-wknd-spa`) et déployez le projet vers AEM à l’aide de Maven :

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Vous devriez voir la mise à jour `Header` avec le logo et les styles appliqués.

   ![En-tête mis à jour dans AEM](./assets/integrate-spa/final-header-component.png)

   Maintenant que l’application d’une seule page est en cours d’AEM, la création peut se poursuivre.

## Dépannage de l&#39;erreur Node-Sass

Au cours du développement, vous pouvez rencontrer l’erreur suivante :

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

Cela peut se produire lorsque les versions locales de **Node.js** et **npm** diffèrent de ce qui est utilisé par le module externe [](https://github.com/eirslett/frontend-maven-plugin)frontend-maven. L’exécution de la commande `npm rebuild node-sass` peut temporairement corriger le problème ou supprimer le `ui.frontend/node_modules` dossier et le réinstaller.

Il y a aussi quelques moyens d&#39;y remédier de façon plus permanente.

* Assurez-vous que les versions locales de npm et Node.js correspondent aux versions utilisées par la version [Maven.](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* ajoutez l’étape d’exécution suivante à l’étape `ui.frontend/pom.xml` précédant l’ `npm run build` :

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## Félicitations ! {#congratulations}

Félicitations, vous avez mis à jour l&#39;APS et exploré l&#39;intégration avec AEM ! Vous connaissez maintenant deux approches différentes pour développer l’application d’une seule page par rapport à l’API modèle JSON AEM à l’aide d’un serveur **** webpack-dev.

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) ou le retirer localement en passant à la branche `React/integrate-spa-solution`.

### Étapes suivantes {#next-steps}

[Mapper des composants d’une application d’une seule page aux composants](map-components.md) AEM - Découvrez comment mapper des composants de réaction à des composants Adobe Experience Manager (AEM) avec l’ SDK JS de l’éditeur d’une seule page d’application d’une seule page. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques des composants d’une application d’une seule page dans AEM éditeur d’applications d’une seule page, à l’instar de la création AEM traditionnelle.
