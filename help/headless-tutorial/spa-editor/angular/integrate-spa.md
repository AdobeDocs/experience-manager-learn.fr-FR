---
title: Intégration d’un SPA | Prise en main de l’éditeur et de l’Angular de SPA d’AEM
description: Découvrez comment le code source d’une application d’une seule page (SPA) écrite en Angular peut être intégré à un projet Adobe Experience Manager (AEM). Découvrez comment utiliser des outils front-end modernes, tels que l’outil d’interface de ligne de commande de l’Angular, pour développer rapidement le SPA par rapport à l’API de modèle JSON AEM.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: e9386885-86de-4e43-933c-2f0a2c04a2f2
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '2191'
ht-degree: 3%

---

# Intégration d’un SPA {#integrate-spa}

Découvrez comment le code source d’une application d’une seule page (SPA) écrite en Angular peut être intégré à un projet Adobe Experience Manager (AEM). Découvrez comment utiliser des outils front-end modernes, tels qu’un serveur de développement webpack, pour développer rapidement le SPA par rapport à l’API de modèle JSON AEM.

## Objectif

1. Découvrez comment le projet SPA est intégré à AEM avec les bibliothèques côté client.
2. Découvrez comment utiliser un serveur de développement local pour un développement front-end dédié.
3. Explorez l’utilisation d’un **proxy** et statique **mock** fichier à développer par rapport à l’API de modèle JSON AEM

## Ce que vous allez créer

Ce chapitre ajoute un `Header` au SPA. Au cours du processus de création de cette statique `Header` Nous utiliserons plusieurs approches pour AEM développement SPA.

![Nouvel en-tête dans AEM](./assets/integrate-spa/final-header-component.png)

*Le SPA est étendu pour ajouter un `Header` component*

## Prérequis

Examinez les outils et les instructions requis pour configurer une [environnement de développement local](overview.md#local-dev-environment).

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

   Si vous utilisez [AEM 6.x](overview.md#compatibility) ajoutez le `classic` profile:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou extraire le code localement en passant à la branche `Angular/integrate-spa-solution`.

## Approche d’intégration {#integration-approach}

Deux modules ont été créés dans le cadre du projet AEM : `ui.apps` et `ui.frontend`.

Le `ui.frontend` est un module [webpack](https://webpack.js.org/) qui contient tout le code source SPA. La majorité du développement et des tests SPA seront effectués dans le projet webpack. Lorsqu’une version de production est déclenchée, la SPA est créée et compilée à l’aide de webpack. Les artefacts compilés (CSS et Javascript) sont copiés dans la variable `ui.apps` qui est ensuite déployé sur le runtime AEM.

![architecture de haut niveau ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Description de haut niveau de l’intégration SPA.*

Des informations supplémentaires sur la version front-end peuvent être [ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect de l’intégration SPA {#inspect-spa-integration}

Ensuite, examinez le `ui.frontend` pour comprendre le SPA qui a été généré automatiquement par la variable [AEM archétype de projet](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. Dans l’IDE de votre choix, ouvrez le projet AEM pour le SPA WKND. Ce tutoriel utilisera la variable [IDE Visual Studio Code](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html?lang=fr#microsoft-visual-studio-code).

   ![VSCode - AEM projet WKND SPA](./assets/integrate-spa/vscode-ide-openproject.png)

2. Développez et examinez le `ui.frontend` dossier. Ouvrez le fichier `ui.frontend/package.json`

3. Sous , `dependencies` vous devriez voir plusieurs `@angular`:

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

   Le `ui.frontend` est un module [Angular de l’application](https://angular.io) généré en utilisant la variable [Outil de ligne de commande Angular](https://angular.io/cli) qui inclut le routage.

4. Il existe également trois dépendances dotées du préfixe `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Les modules ci-dessus constituent la variable [AEM SDK JS de l’éditeur SPA](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) et fournir la fonctionnalité permettant de mapper SPA composants à AEM composants.

5. Dans le `package.json` fichier plusieurs `scripts` sont définies :

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Ces scripts sont basés sur des [Commandes d’interface de ligne de commande d’Angular](https://angular.io/cli/build) mais ont été légèrement modifiés pour fonctionner avec le projet AEM plus vaste.

   `start` : exécute l’application d’Angular localement à l’aide d’un serveur web local. Il a été mis à jour pour remplacer le contenu de l’instance AEM locale.

   `build` - compile l’application d’Angular pour la distribution en production. L’ajout de `&& clientlib` est chargé de copier les SPA compilés dans la variable `ui.apps` en tant que bibliothèque côté client lors d’une génération. Module npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) est utilisé pour faciliter cette opération.

   Vous trouverez plus d’informations sur les scripts disponibles. [here](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspectez le fichier `ui.frontend/clientlib.config.js`. Ce fichier de configuration est utilisé par [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) pour déterminer comment générer la bibliothèque cliente.

7. Inspectez le fichier `ui.frontend/pom.xml`. Ce fichier transforme la variable `ui.frontend` dans un dossier [Module Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). Le `pom.xml` a été mis à jour afin d’utiliser la variable [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) to **test** et **build** SPA lors d’une génération Maven.

8. Inspect du fichier `app.component.ts` at `ui.frontend/src/app/app.component.ts`:

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

   `app.component.js` est le point d’entrée du SPA. `ModelManager` est fourni par le SDK JS de l’éditeur d’AEM SPA. Il est chargé d’appeler et d’injecter la variable `pageModel` (le contenu JSON) dans l’application.

## Ajout d’un composant d’en-tête {#header-component}

Ajoutez ensuite un nouveau composant au SPA et déployez les modifications sur une instance AEM locale pour voir l’intégration.

1. Ouvrez une nouvelle fenêtre de terminal et accédez au dossier `ui.frontend` :

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Installer [Interface de ligne de commande d’Angular](https://angular.io/cli#installing-angular-cli) global Utilisé pour générer des composants d’Angular ainsi que pour créer et diffuser l’application d’Angular via le **ng** .

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > La version de **@angular/cli** utilisé par ce projet est **9.1.7**. Il est recommandé de conserver la synchronisation des versions de l’interface de ligne de commande d’Angular.

3. Créer `Header` en exécutant l’interface de ligne de commande de l’Angular `ng generate component` à partir de la fonction `ui.frontend` dossier.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Cela créera un squelette pour le nouveau composant En-tête d’Angular à l’adresse `ui.frontend/src/app/components/header`.

4. Ouvrez le `aem-guides-wknd-spa` dans l’IDE de votre choix. Accédez au dossier `ui.frontend/src/app/components/header`. 

   ![Chemin d’accès du composant d’en-tête dans l’IDE](assets/integrate-spa/header-component-path.png)

5. Ouvrir le fichier `header.component.html` et remplacez le contenu par ce qui suit :

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Notez qu’il affiche du contenu statique, de sorte que ce composant d’Angular ne nécessite aucun ajustement par défaut généré. `header.component.ts`.

6. Ouvrir le fichier **app.component.html** at  `ui.frontend/src/app/app.component.html`. Ajoutez le composant `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Cela inclut la variable `header` avant tout le contenu de la page.

7. Ouvrez un nouveau terminal et accédez au `ui.frontend` et exécutez le `npm run build` command :

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Accédez au dossier `ui.apps`. Sous `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` vous devriez constater que les fichiers SPA compilés ont été copiés à partir de la fonction`ui.frontend/build` dossier.

   ![Bibliothèque cliente générée dans ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

9. Revenez au terminal et accédez au `ui.apps` dossier. Exécutez la commande Maven suivante :

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

   Cela déploie le `ui.apps` vers une instance d’exécution locale d’AEM.

10. Ouvrez un onglet de navigateur et accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Vous devriez maintenant voir le contenu de la variable `Header` du composant affiché dans le SPA.

   ![Implémentation initiale de l’en-tête](assets/integrate-spa/initial-header-implementation.png)

   Étapes **7-9** sont exécutées automatiquement lors du déclenchement d’une version Maven à partir de la racine du projet (c.-à-d. `mvn clean install -PautoInstallSinglePackage`). Vous devez maintenant comprendre les principes de base de l’intégration entre les bibliothèques SPA et AEM côté client. Notez que vous pouvez toujours modifier et ajouter des `Text` des composants dans AEM, mais la variable `Header` n’est pas modifiable.

## Serveur de développement Webpack - Proxy de l’API JSON {#proxy-json}

Comme vous l’avez vu dans les exercices précédents, l’exécution d’une version et la synchronisation de la bibliothèque cliente avec une instance locale d’AEM prend quelques minutes. Cela est acceptable pour les tests finaux, mais n’est pas idéal pour la majorité du développement SPA.

A [serveur de développement webpack](https://webpack.js.org/configuration/dev-server/) peut être utilisé pour développer rapidement le SPA. Le SPA est piloté par un modèle JSON généré par AEM. Dans cet exercice, le contenu JSON d’une instance d’AEM en cours d’exécution sera **proxy** dans le serveur de développement configuré par le [Angular de projet](https://angular.io/guide/build).

1. Revenez à l’IDE et ouvrez le fichier . **proxy.conf.json** at `ui.frontend/proxy.conf.json`.

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

   Le [Angular app](https://angular.io/guide/build#proxying-to-a-backend-server) fournit un mécanisme facile pour proxy des requêtes d’API. Les modèles spécifiés dans `context` sont par proxy. `localhost:4502`, l’AEM local quickstart.

2. Ouvrir le fichier **index.html** at `ui.frontend/src/index.html`. Il s’agit du fichier de HTML racine utilisé par le serveur de développement.

   Notez qu’il existe une entrée pour `base href="/"`. Le [balise de base](https://angular.io/guide/deployment#the-base-tag) est essentiel pour que l’application puisse résoudre les URL relatives.

   ```html
   <base href="/">
   ```

3. Ouvrez une fenêtre de terminal et accédez au `ui.frontend` dossier. Exécutez la commande `npm start` :

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

5. Revenez à l’IDE et créez un dossier nommé `img` at `ui.frontend/src/assets`.
6. Téléchargez et ajoutez le logo WKND suivant au `img` folder:

   ![Logo WKND](./assets/integrate-spa/wknd-logo-dk.png)

7. Ouvrir **header.component.html** at `ui.frontend/src/app/components/header/header.component.html` et incluez le logo :

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

   Vous pouvez continuer à mettre à jour le contenu dans **AEM** et les voir se refléter dans **serveur de développement webpack**, car nous effectuons un proxy sur le contenu. Notez que les modifications de contenu ne sont visibles que dans la variable **serveur de développement webpack**.

9. Arrêtez le serveur web local avec `ctrl+c` dans le terminal.

## Serveur de développement Webpack - API JSON de simulation {#mock-json}

Une autre méthode de développement rapide consiste à utiliser un fichier JSON statique pour agir comme modèle JSON. En &quot;moquant&quot; le JSON, nous supprimons la dépendance sur une instance d’AEM locale. Il permet également à un développeur front-end de mettre à jour le modèle JSON afin de tester la fonctionnalité et d’apporter des modifications à l’API JSON qui seront ensuite implémentées par un développeur principal.

La configuration initiale du simulateur JSON fait **nécessite une instance d’AEM locale**.

1. Dans le navigateur, accédez à [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Il s’agit du fichier JSON exporté par AEM qui dirige l’application. Copiez la sortie JSON.

2. Revenez à l’IDE et accédez à `ui.frontend/src` et ajouter de nouveaux dossiers nommés **mocks** et **json** pour correspondre à la structure de dossiers suivante :

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Créez un fichier nommé **en.model.json** sous `ui.frontend/public/mocks/json`. Collez la sortie JSON à partir de **Étape 1** ici.

   ![Simulation du fichier Json du modèle](assets/integrate-spa/mock-model-json-created.png)

4. Création d’un fichier **proxy.mock.conf.json** sous `ui.frontend`. Remplissez le fichier avec les éléments suivants :

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

   Cette configuration de proxy réécrit les requêtes commençant par `/content/wknd-spa-angular/us` avec `/mocks/json` et diffusez le fichier JSON statique correspondant, par exemple :

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Ouvrir le fichier **angular.json**. Ajouter un nouveau **dev** configuration avec une mise à jour **ressources** pour référencer le tableau **mocks** dossier créé.

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

   Créer un **dev** La configuration de garantit que la variable **mocks** n’est utilisé que pendant le développement et n’est jamais déployé vers AEM dans une version de production.

6. Dans le **angular.json** , mettez à jour la variable **browserTarget** pour utiliser la nouvelle configuration **dev** configuration :

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

7. Ouvrir le fichier `ui.frontend/package.json` et ajoutez une nouvelle **start:mock** pour référencer la commande **proxy.mock.conf.json** fichier .

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

8. En cas d’exécution, arrêtez la variable **serveur de développement webpack**. Démarrez le **serveur de développement webpack** en utilisant la variable **start:mock** script :

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Accédez à [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) et vous devriez voir la même SPA, mais le contenu est maintenant extrait du **mock** Fichier JSON.

9. Apportez une petite modification au **en.model.json** fichier créé précédemment. Le contenu mis à jour doit être immédiatement répercuté dans la variable **serveur de développement webpack**.

   ![mise à jour json du modèle mock](./assets/integrate-spa/webpack-mock-model.gif)

   Être en mesure de manipuler le modèle JSON et de voir les effets sur un SPA en direct peut aider un développeur à comprendre l’API du modèle JSON. Il permet également le développement front-end et back-end en parallèle.

## Ajout de styles avec Sass

Ensuite, un style mis à jour sera ajouté au projet. Ce projet ajoutera [Sass](https://sass-lang.com/) prise en charge de quelques fonctionnalités utiles telles que les variables.

1. Ouvrez une fenêtre de terminal et arrêtez le **serveur de développement webpack** si a commencé. À l’intérieur de `ui.frontend` saisissez la commande suivante pour mettre à jour l’application Angular à traiter **.scss** fichiers .

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Cette opération met à jour la variable `angular.json` avec une nouvelle entrée au bas du fichier :

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Installer `normalize-scss` pour normaliser les styles dans les navigateurs :

   ```shell
   $ npm install normalize-scss --save
   ```

3. Revenez à l’IDE et sous `ui.frontend/src` créer un dossier nommé `styles`.
4. Créez un fichier sous `ui.frontend/src/styles` named `_variables.scss` et renseignez-le avec les variables suivantes :

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

5. Renommer l’extension du fichier **styles.css** at `ui.frontend/src/styles.css` to **styles.scss**. Remplacez le contenu par ce qui suit :

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

6. Mettre à jour **angular.json** et renommer toutes les références à **style.css** avec **styles.scss**. Il doit y avoir 3 références.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Mettre à jour les styles d’en-tête

Ajoutez ensuite certains styles spécifiques à la marque au **En-tête** à l’aide de Sass.

1. Démarrez le **serveur de développement webpack** pour voir les styles mis à jour en temps réel :

   ```shell
   $ npm run start:mock
   ```

2. Sous `ui.frontend/src/app/components/header` re-name **header.component.css** to **header.component.scss**. Remplissez le fichier avec les éléments suivants :

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

3. Mettre à jour **header.component.ts** référence **header.component.scss**:

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

4. Revenez au navigateur et au **serveur de développement webpack**:

   ![En-tête stylisé - serveur de développement webpack](assets/integrate-spa/styled-header.png)

   Vous devriez maintenant voir les styles mis à jour ajoutés au **En-tête** composant.

## Déployer SPA mises à jour dans AEM

Les modifications apportées à la variable **En-tête** sont actuellement visibles uniquement par l’intermédiaire de la fonction **serveur de développement webpack**. Déployez le SPA mis à jour pour AEM afficher les modifications.

1. Arrêtez le **serveur de développement webpack**.
2. Accédez à la racine du projet. `/aem-guides-wknd-spa` et déployez le projet vers AEM à l’aide de Maven :

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Vous devriez voir la mise à jour **En-tête** avec le logo et les styles appliqués :

   ![En-tête mis à jour dans AEM](assets/integrate-spa/final-header-component.png)

   Maintenant que la SPA mise à jour est dans AEM, la création peut continuer.

## Félicitations ! {#congratulations}

Félicitations, vous avez mis à jour le SPA et exploré l&#39;intégration avec AEM ! Vous connaissez désormais deux approches différentes pour développer la SPA par rapport à l’API de modèle JSON AEM à l’aide d’une **serveur de développement webpack**.

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou extraire le code localement en passant à la branche `Angular/integrate-spa-solution`.

### Étapes suivantes {#next-steps}

[Mappage des composants SPA aux composants AEM](map-components.md) - Découvrez comment mapper les composants Angular aux composants Adobe Experience Manager (AEM) avec le SDK JS d’AEM éditeur. Le mappage de composants permet aux auteurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme dans la création d’ traditionnelle.
