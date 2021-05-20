---
title: Intégration d’un SPA | Prise en main de l’éditeur et de l’Angular de SPA d’AEM
description: Découvrez comment le code source d’une application d’une seule page (SPA) écrite en Angular peut être intégré à un projet Adobe Experience Manager (AEM). Découvrez comment utiliser des outils front-end modernes, tels que l’outil d’interface de ligne de commande de l’Angular, pour développer rapidement le SPA par rapport à l’API de modèle JSON AEM.
sub-product: sites
feature: Éditeur de SPA
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2205'
ht-degree: 3%

---


# Intégrer un SPA {#integrate-spa}

Découvrez comment le code source d’une application d’une seule page (SPA) écrite en Angular peut être intégré à un projet Adobe Experience Manager (AEM). Découvrez comment utiliser des outils front-end modernes, tels qu’un serveur de développement webpack, pour développer rapidement le SPA par rapport à l’API de modèle JSON AEM.

## Intention

1. Découvrez comment le projet SPA est intégré à AEM avec les bibliothèques côté client.
2. Découvrez comment utiliser un serveur de développement local pour un développement front-end dédié.
3. Explorez l’utilisation d’un fichier **proxy** et statique **mock** pour le développement par rapport à l’API de modèle JSON AEM.

## Ce que vous allez créer

Ce chapitre ajoute un composant `Header` simple au SPA. Dans le processus de création de ce composant `Header` statique, plusieurs approches de développement d’AEM SPA seront utilisées.

![Nouvel en-tête dans AEM](./assets/integrate-spa/final-header-component.png)

*Le SPA est étendu pour ajouter un  `Header` composant statique.*

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Obtention du code

1. Téléchargez le point de départ de ce tutoriel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Déployez la base de code sur une instance d’AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility), ajoutez le profil `classic` :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou extraire le code localement en passant à la branche `Angular/integrate-spa-solution`.

## Approche d’intégration {#integration-approach}

Deux modules ont été créés dans le cadre du projet AEM : `ui.apps` et `ui.frontend`.

Le module `ui.frontend` est un projet [webpack](https://webpack.js.org/) qui contient tout le code source SPA. La majorité du développement et des tests SPA seront effectués dans le projet webpack. Lorsqu’une version de production est déclenchée, la SPA est créée et compilée à l’aide de webpack. Les artefacts compilés (CSS et Javascript) sont copiés dans le module `ui.apps` qui est ensuite déployé sur le runtime AEM.

![architecture de haut niveau ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Description de haut niveau de l’intégration SPA.*

Vous trouverez des informations supplémentaires sur la version front-end [ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect de l’intégration SPA {#inspect-spa-integration}

Examinez ensuite le module `ui.frontend` pour comprendre le SPA généré automatiquement par l’[archétype de projet AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. Dans l’IDE de votre choix, ouvrez le projet AEM pour le SPA WKND. Ce tutoriel utilisera [Visual Studio Code IDE](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM projet WKND SPA](./assets/integrate-spa/vscode-ide-openproject.png)

2. Développez et inspectez le dossier `ui.frontend`. Ouvrez le fichier `ui.frontend/package.json`

3. Sous `dependencies`, vous devriez voir plusieurs éléments liés à `@angular` :

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   Le module `ui.frontend` est une [application d’Angular](https://angular.io) générée à l’aide de l’[outil d’interface de ligne de commande d’Angular](https://angular.io/cli) qui inclut le routage.

4. Il existe également trois dépendances dotées du préfixe `@adobe` :

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Les modules ci-dessus constituent le [AEM SDK JS de l’éditeur SPA](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) et fournissent la fonctionnalité permettant de mapper les composants SPA aux composants.

5. Dans le fichier `package.json` plusieurs `scripts` sont définis :

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Ces scripts sont basés sur des [commandes d’interface de ligne de commande d’Angular](https://angular.io/cli/build) courantes, mais ont été légèrement modifiés pour fonctionner avec le projet d’AEM plus vaste.

   `start` : exécute l’application d’Angular localement à l’aide d’un serveur web local. Il a été mis à jour pour remplacer le contenu de l’instance AEM locale.

   `build` - compile l’application d’Angular pour la distribution en production. L’ajout de `&& clientlib` est responsable de la copie du SPA compilé dans le module `ui.apps` en tant que bibliothèque côté client lors d’une génération. Le module npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) est utilisé pour faciliter cette opération.

   Vous trouverez plus de détails sur les scripts disponibles [ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspectez le fichier `ui.frontend/clientlib.config.js`. Ce fichier de configuration est utilisé par [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) pour déterminer comment générer la bibliothèque cliente.

7. Inspectez le fichier `ui.frontend/pom.xml`. Ce fichier transforme le dossier `ui.frontend` en module [Maven](http://maven.apache.org/guides/mini/guide-multiple-modules.html). Le fichier `pom.xml` a été mis à jour afin d’utiliser [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) en **test** et **build** le SPA lors d’une génération Maven.

8. Inspect le fichier `app.component.ts` à `ui.frontend/src/app/app.component.ts` :

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` est le point d’entrée du SPA. `ModelManager` est fourni par le SDK JS de l’éditeur d’AEM SPA. Il est chargé d’appeler et d’injecter le `pageModel` (contenu JSON) dans l’application.

## Ajouter un composant d’en-tête {#header-component}

Ajoutez ensuite un nouveau composant au SPA et déployez les modifications sur une instance AEM locale pour voir l’intégration.

1. Ouvrez une nouvelle fenêtre de terminal et accédez au dossier `ui.frontend` :

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Installer [l’Angular CLI](https://angular.io/cli#installing-angular-cli) globalement Utilisé pour générer des composants d’Angular ainsi que pour créer et servir l’application d’Angular via la commande **ng**.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > La version de **@angular/cli** utilisée par ce projet est **9.1.7**. Il est recommandé de conserver la synchronisation des versions de l’interface de ligne de commande d’Angular.

3. Créez un composant `Header` en exécutant la commande d’Angular CLI `ng generate component` depuis le dossier `ui.frontend`.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Cela créera un squelette pour le nouveau composant Angular Header à `ui.frontend/src/app/components/header`.

4. Ouvrez le projet `aem-guides-wknd-spa` dans l’IDE de votre choix. Accédez au dossier `ui.frontend/src/app/components/header`. 

   ![Chemin d’accès du composant d’en-tête dans l’IDE](assets/integrate-spa/header-component-path.png)

5. Ouvrez le fichier `header.component.html` et remplacez le contenu par ce qui suit :

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Notez que cela affiche du contenu statique, de sorte que ce composant d’Angular ne nécessite aucun ajustement par défaut de la balise `header.component.ts` générée.

6. Ouvrez le fichier **app.component.html** à l’adresse `ui.frontend/src/app/app.component.html`. Ajoutez le composant `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Cela inclut le composant `header` au-dessus de tout le contenu de la page.

7. Ouvrez un nouveau terminal, accédez au dossier `ui.frontend` et exécutez la commande `npm run build` :

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Accédez au dossier `ui.apps`. Sous `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular`, vous devriez voir que les fichiers SPA compilés ont été copiés à partir du dossier `ui.frontend/build` .

   ![Bibliothèque cliente générée dans ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

9. Revenez au terminal et accédez au dossier `ui.apps` . Exécutez la commande Maven suivante :

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

10. Ouvrez un onglet de navigateur et accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Vous devriez maintenant voir le contenu du composant `Header` affiché dans le SPA.

   ![Implémentation initiale de l’en-tête](assets/integrate-spa/initial-header-implementation.png)

   Les étapes **7-9** sont exécutées automatiquement lors du déclenchement d’une version Maven à partir de la racine du projet (c’est-à-dire `mvn clean install -PautoInstallSinglePackage`). Vous devez maintenant comprendre les principes de base de l’intégration entre les bibliothèques SPA et AEM côté client. Notez que vous pouvez toujours modifier et ajouter des composants `Text` dans AEM, mais le composant `Header` n’est pas modifiable.

## Serveur de développement Webpack : proxy de l’API JSON {#proxy-json}

Comme vous l’avez vu dans les exercices précédents, l’exécution d’une version et la synchronisation de la bibliothèque cliente avec une instance locale d’AEM prend quelques minutes. Cela est acceptable pour les tests finaux, mais n’est pas idéal pour la majorité du développement SPA.

Un [serveur de développement webpack](https://webpack.js.org/configuration/dev-server/) peut être utilisé pour développer rapidement le SPA. Le SPA est piloté par un modèle JSON généré par AEM. Dans cet exercice, le contenu JSON d’une instance en cours d’exécution de l’AEM sera **proxy** vers le serveur de développement configuré par le [projet d’Angular](https://angular.io/guide/build).

1. Revenez à l’IDE et ouvrez le fichier **proxy.conf.json** à l’adresse `ui.frontend/proxy.conf.json`.

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   [L’application d’Angular](https://angular.io/guide/build#proxying-to-a-backend-server) fournit un mécanisme facile pour proxy des requêtes d’API. Les modèles spécifiés dans `context` sont proxy via `localhost:4502`, l’AEM quickstart local.

2. Ouvrez le fichier **index.html** à l’adresse `ui.frontend/src/index.html`. Il s’agit du fichier HTML racine utilisé par le serveur de développement.

   Notez qu’il existe une entrée pour `base href="/"`. La [balise de base](https://angular.io/guide/deployment#the-base-tag) est essentielle pour que l’application puisse résoudre les URL relatives.

   ```html
   <base href="/">
   ```

3. Ouvrez une fenêtre de terminal et accédez au dossier `ui.frontend` . Exécutez la commande `npm start` :

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. Ouvrez un nouvel onglet du navigateur (s’il n’est pas déjà ouvert) et accédez à [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Serveur de développement Webpack - json proxy](assets/integrate-spa/webpack-dev-server-1.png)

   Vous devriez voir le même contenu que dans AEM, mais sans aucune des fonctionnalités de création activées.

5. Revenez à l’IDE et créez un dossier nommé `img` à l’adresse `ui.frontend/src/assets`.
6. Téléchargez et ajoutez le logo WKND suivant au dossier `img` :

   ![Logo WKND](./assets/integrate-spa/wknd-logo-dk.png)

7. Ouvrez **header.component.html** à l’adresse `ui.frontend/src/app/components/header/header.component.html` et insérez le logo :

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   Enregistrez les modifications dans **header.component.html**.

8. Revenez au navigateur. Les modifications apportées à l’application devraient immédiatement être répercutées.

   ![Logo ajouté à l’en-tête](assets/integrate-spa/added-logo-localhost.png)

   Vous pouvez continuer à effectuer des mises à jour de contenu dans **AEM** et les voir se répercuter dans **le serveur de développement webpack**, puisque nous effectuons des proxys du contenu. Notez que les modifications de contenu ne sont visibles que dans le **serveur de développement webpack**.

9. Arrêtez le serveur web local avec `ctrl+c` dans le terminal.

## Serveur de développement Webpack - Mock JSON API {#mock-json}

Une autre méthode de développement rapide consiste à utiliser un fichier JSON statique pour agir comme modèle JSON. En &quot;moquant&quot; le JSON, nous supprimons la dépendance sur une instance d’AEM locale. Il permet également à un développeur front-end de mettre à jour le modèle JSON afin de tester la fonctionnalité et d’apporter des modifications à l’API JSON qui seront ensuite implémentées par un développeur principal.

La configuration initiale du fichier JSON de simulation requiert **une instance d’AEM locale**.

1. Dans le navigateur, accédez à [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Il s’agit du fichier JSON exporté par AEM qui dirige l’application. Copiez la sortie JSON.

2. Revenez à l’IDE et accédez à `ui.frontend/src` et ajoutez de nouveaux dossiers nommés **mocks** et **json** pour correspondre à la structure de dossiers suivante :

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Créez un fichier nommé **en.model.json** sous `ui.frontend/public/mocks/json`. Collez la sortie JSON de **Étape 1** ici.

   ![Simulation du fichier Json du modèle](assets/integrate-spa/mock-model-json-created.png)

4. Créez un nouveau fichier **proxy.mock.conf.json** sous `ui.frontend`. Remplissez le fichier avec les éléments suivants :

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   Cette configuration de proxy réécrit les requêtes commençant par `/content/wknd-spa-angular/us` avec `/mocks/json` et diffusant le fichier JSON statique correspondant, par exemple :

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Ouvrez le fichier **angular.json**. Ajoutez une nouvelle configuration **dev** avec un tableau **assets** mis à jour pour référencer le dossier **mocks** créé.

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![Angular du dossier de mise à jour des ressources de développement JSON](assets/integrate-spa/dev-assets-update-folder.png)

   La création d’une configuration **dev** dédiée garantit que le dossier **mocks** n’est utilisé que pendant le développement et n’est jamais déployé sur AEM dans une version de production.

6. Dans le fichier **angular.json** , mettez à jour la configuration **browserTarget** pour utiliser la nouvelle configuration **dev** :

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![Angular de la mise à jour du développement de build JSON](assets/integrate-spa/angular-json-build-dev-update.png)

7. Ouvrez le fichier `ui.frontend/package.json` et ajoutez une nouvelle commande **start:mock** pour référencer le fichier **proxy.mock.conf.json**.

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   L’ajout d’une nouvelle commande permet de basculer facilement entre les configurations de proxy.

8. En cas d’exécution actuelle, arrêtez le **serveur de développement webpack**. Démarrez le **serveur de développement webpack** à l’aide du script **start:mock** :

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Accédez à [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) et vous devriez voir le même SPA, mais le contenu est maintenant extrait du fichier **mock** JSON.

9. Apportez une petite modification au fichier **en.model.json** créé précédemment. Le contenu mis à jour doit immédiatement être reflété dans le **serveur de développement webpack**.

   ![mise à jour json du modèle mock](./assets/integrate-spa/webpack-mock-model.gif)

   Être en mesure de manipuler le modèle JSON et de voir les effets sur un SPA en direct peut aider un développeur à comprendre l’API du modèle JSON. Il permet également le développement front-end et back-end en parallèle.

## Ajout de styles avec Sass

Ensuite, un style mis à jour sera ajouté au projet. Ce projet ajoutera la prise en charge de [Sass](https://sass-lang.com/) pour quelques fonctionnalités utiles telles que les variables.

1. Ouvrez une fenêtre de terminal et arrêtez le **serveur de développement webpack** si vous démarrez. Dans le dossier `ui.frontend` , saisissez la commande suivante pour mettre à jour l’application Angular afin de traiter les fichiers **.scss**.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Le fichier `angular.json` est alors mis à jour avec une nouvelle entrée au bas du fichier :

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Installez `normalize-scss` pour normaliser les styles dans tous les navigateurs :

   ```shell
   $ npm install normalize-scss --save
   ```

3. Revenez à l’IDE et sous `ui.frontend/src` créez un dossier nommé `styles`.
4. Créez un nouveau fichier sous `ui.frontend/src/styles` nommé `_variables.scss` et renseignez-le avec les variables suivantes :

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
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. Renommez l’extension du fichier **styles.css** à `ui.frontend/src/styles.css` en **styles.scss**. Remplacez le contenu par ce qui suit :

   ```scss
   /* styles.scss * /
   
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
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. Mettez à jour **angular.json** et renommez toutes les références à **style.css** avec **styles.scss**. Il doit y avoir 3 références.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Mettre à jour les styles d’en-tête

Ajoutez ensuite certains styles spécifiques à la marque au composant **En-tête** à l’aide de Sass.

1. Démarrez le **serveur de développement webpack** pour afficher les styles mis à jour en temps réel :

   ```shell
   $ npm run start:mock
   ```

2. Sous `ui.frontend/src/app/components/header` renommez **header.component.css** en **header.component.scss**. Remplissez le fichier avec les éléments suivants :

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. Mettez à jour **header.component.js** pour référencer **header.component.scss** :

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. Revenez au navigateur et au **serveur de développement webpack** :

   ![En-tête stylisé - serveur de développement webpack](assets/integrate-spa/styled-header.png)

   Vous devriez maintenant voir les styles mis à jour ajoutés au composant **En-tête** .

## Déployer SPA mises à jour dans AEM

Les modifications apportées à l’**en-tête** ne sont actuellement visibles que par le biais du **serveur de développement webpack**. Déployez le SPA mis à jour pour AEM afficher les modifications.

1. Arrêtez le **serveur de développement webpack**.
2. Accédez à la racine du projet `/aem-guides-wknd-spa` et déployez le projet vers AEM à l’aide de Maven :

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Vous devriez voir l’**en-tête** mis à jour avec le logo et les styles appliqués :

   ![En-tête mis à jour dans AEM](assets/integrate-spa/final-header-component.png)

   Maintenant que la SPA mise à jour est dans AEM, la création peut continuer.

## Félicitations !  {#congratulations}

Félicitations, vous avez mis à jour le SPA et exploré l&#39;intégration avec AEM ! Vous connaissez désormais deux approches différentes pour développer le SPA par rapport à l’API de modèle JSON AEM à l’aide d’un **serveur de développement webpack**.

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou extraire le code localement en passant à la branche `Angular/integrate-spa-solution`.

### Étapes suivantes {#next-steps}

[Mappage SPA composants à AEM composants](map-components.md)  : découvrez comment mapper des composants d’Angular à des composants Adobe Experience Manager () avec le SDK JS de l’éditeur d’. Le mappage de composants permet aux auteurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme dans la création d’ traditionnelle.
