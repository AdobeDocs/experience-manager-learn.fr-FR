---
title: Bibliothèques côté client et flux de travail frontal
description: Découvrez comment les bibliothèques côté client ou les clientlibs sont utilisés pour déployer et gérer les fichiers CSS et Javascript pour une implémentation de sites Adobe Experience Manager (AEM). Ce didacticiel explique également comment le module ui.frontend, un projet webpack, peut être intégré dans le processus de génération de bout en bout.
sub-product: sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 5%

---


# Bibliothèques côté client et flux de travail frontal {#client-side-libraries}

Découvrez comment les bibliothèques côté client ou les clientlibs sont utilisés pour déployer et gérer les fichiers CSS et Javascript pour une implémentation de sites Adobe Experience Manager (AEM). Ce didacticiel explique également comment le module [ui.frontend](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/archetype/uifrontend.html), un projet de [webpack](https://webpack.js.org/) découplé, peut être intégré au processus de génération de bout en bout.

## Conditions préalables {#prerequisites}

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

Il est également recommandé de consulter le didacticiel [Concepts de base des composants](component-basics.md#client-side-libraries) pour comprendre les fondamentaux des bibliothèques et des AEM côté client.

### Projet de démarrage

Consultez le code de ligne de base sur lequel le didacticiel s&#39;appuie :

1. Cloner le référentiel [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `client-side-libraries/start`

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. Déployez la base de code sur une instance AEM locale en utilisant vos compétences Maven :

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) ou vérifier le code localement en passant à la branche `client-side-libraries/solution`.

## Intention

1. Comprendre comment les bibliothèques côté client sont incluses dans une page via un modèle modifiable.
1. Découvrez comment utiliser le module UI.Frontend et un serveur de développement webpack pour le développement frontal dédié.
1. Comprendre le flux de travail complet de la diffusion de fichiers CSS et JavaScript compilés vers une implémentation de sites.

## Ce que vous allez créer {#what-you-will-build}

Dans ce chapitre, vous allez ajouter quelques styles de ligne de base pour le site WKND et le modèle de page de l’article afin de rapprocher l’implémentation des [modèles de conception de l’interface utilisateur](assets/pages-templates/wknd-article-design.xd). Vous allez utiliser un processus frontal avancé pour intégrer un projet webpack dans une bibliothèque cliente AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## Arrière-plan {#background}

Les bibliothèques côté client offrent un mécanisme d’organisation et de gestion des fichiers CSS et JavaScript nécessaires à une mise en oeuvre AEM Sites. Les objectifs de base des bibliothèques ou des clientlibs côté client sont les suivants :

1. Stocker les fichiers CSS/JS dans de petits fichiers discrets pour faciliter le développement et la maintenance
1. Gérer les dépendances sur des structures tierces de manière organisée
1. Réduisez le nombre de requêtes côté client en concaténant CSS/JS en une ou deux requêtes.

Vous trouverez plus d’informations sur l’utilisation des [bibliothèques côté client ici.](https://docs.adobe.com/content/help/fr-FR/experience-manager-65/developing/introduction/clientlibs.html)

Les bibliothèques côté client ont certaines limites. La prise en charge des langages frontaux les plus utilisés, tels que Sass, LESS et TypeScript, est particulièrement notable. Dans le tutoriel, nous examinerons comment le module **ui.frontend** peut aider à résoudre ce problème.

Déployez la base de code de démarrage sur une instance d’AEM locale et accédez à [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Cette page n&#39;a actuellement pas de style. Nous allons ensuite mettre en oeuvre des bibliothèques côté client pour la marque WKND afin d’ajouter du code CSS et Javascript à la page.

## Organisation des bibliothèques côté client {#organization}

Nous allons ensuite explorer l&#39;organisation des clientlibs générés par l&#39;archétype de projet [AEM ](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/archetype/overview.html).

![Organisation de bibliothèque cliente de haut niveau](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramme de haut niveau Organisation de la bibliothèque côté client et inclusion de page*

>[!NOTE]
>
> L&#39;organisation de bibliothèque côté client suivante est générée par AEM Project Archetype mais ne représente qu&#39;un point de départ. La façon dont un projet gère et distribue en fin de compte les feuilles de style CSS et JavaScript à une implémentation de sites peut varier considérablement en fonction des ressources, des compétences et des exigences.

1. En utilisant Eclipse ou un autre IDE, ouvrez le module **ui.apps**.
1. Développez le chemin `/apps/wknd/clientlibs` pour vue des clientlibs générés par l&#39;archétype.

   ![Clientlibs dans ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Nous examinerons ces clientlibs plus en détail ci-dessous.

1. Inspect les propriétés de `clientlibs/clientlib-base`.

   **clientlib-** basereprésente le niveau de base de CSS et de JavaScript requis pour le fonctionnement du site WKND. Notez la propriété `categories` définie sur `wknd.base`. `categories` est un mécanisme de balisage pour les clientlibs et c’est ainsi qu’ils peuvent être référencés.

   Notez la propriété `embed` et la propriété `String[]` des valeurs. La propriété `embed` intègre d’autres clientlibs en fonction de leur catégorie. **clientlib-** baseinclura toutes les bibliothèques clientes des composants principaux AEM nécessaires. Cela inclut les artefacts tels que javascript pour le carrousel, les composants de recherche rapide à utiliser. **clientlib-** basen’inclura pas de CSS et de JavaScript propres, mais incorporera simplement d’autres bibliothèques clientes. **clientlib-** baseincorpore le  **clientlib-** gridclientlib à la catégorie de  `wknd.grid`clientlib.

   Notez la propriété `allowProxy` définie sur `true`. Il est recommandé de toujours définir `allowProxy=true` sur clientlibs. La propriété `allowProxy` nous permet de stocker les clientlibs avec le code de notre application sous `/apps` **mais** fournit ensuite les clientlibs sur un chemin précédé de `/etc.clientlibs` afin d&#39;éviter d&#39;exposer tout code d&#39;application aux utilisateurs finaux. Vous trouverez plus d&#39;informations sur la propriété [allowProxy ici.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1. Inspect les propriétés de `clientlibs/clientlib-grid`.

   **clientlib-** gridis est chargé d’inclure/de générer la page CSS nécessaire au  [mode de ](https://docs.adobe.com/content/help/fr-FR/experience-manager-65/authoring/siteandpage/responsive-layout.html) mise en page pour travailler avec l’éditeur AEM Sites. **clientlib-** gridhad a une catégorie définie sur  `wknd.grid` et est incorporée via  **clientlib-base**.

   La grille peut être personnalisée pour utiliser différentes quantités de colonnes et de points d&#39;arrêt. Ensuite, nous allons mettre à jour les points d&#39;arrêt par défaut générés.

1. Mettez-le à jour `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`:

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   Cela modifiera les points d&#39;arrêt pour qu&#39;ils correspondent aux points d&#39;arrêt du modèle définis dans `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`.

   Notez que ce fichier fait réellement référence à un fichier `grid_base.less` sous `/libs` qui contient un mixin personnalisé pour générer la grille.

1. Inspect les propriétés de `clientlibs/clientlib-site`.

   **clientlib-** site contiendra tous les styles spécifiques au site pour la marque WKND. Notez la catégorie de `wknd.site`. Le code CSS et JavaScript qui génère cette bibliothèque cliente sera en fait conservé dans le module `ui.frontend`. Nous étudierons ensuite cette intégration.

1. Inspect les propriétés de `clientlibs/clientlib-dependencies`.

   **clientlib-** dependenciesis destiné à incorporer des dépendances tierces. Il s’agit d’une bibliothèque cliente distincte, de sorte qu’elle puisse être chargée en haut de la page HTML si nécessaire. Notez la catégorie de `wknd.dependencies`. Le code CSS et JavaScript qui génère cette bibliothèque cliente sera en fait conservé dans le module `ui.frontend`. Cette intégration sera explorée plus loin dans le tutoriel.

## Utilisation du module ui.frontend {#ui-frontend}

Nous allons ensuite explorer l&#39;utilisation du module **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**.

### Motivation

Les bibliothèques côté client présentent certaines limites en ce qui concerne la prise en charge de langages tels que [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Il y a aussi eu une explosion d&#39;outils open source tels que [NPM](https://www.npmjs.com/) et [webpack](https://webpack.js.org/) qui accélèrent et optimisent le développement frontal.

L&#39;idée de base derrière le module **ui.frontend** est de pouvoir utiliser de grands outils tels que NPM et Webpack pour gérer la majorité du développement frontal. Un élément d&#39;intégration clé intégré au module **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prend les artefacts CSS et JS compilés d&#39;un projet webpack/npm et les transforme en bibliothèques côté client AEM. Cela donne aux développeurs de premier plan une plus grande liberté pour choisir différents outils et technologies.

![intégration de l’architecture ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

### Utilisation

Nous allons maintenant ajouter quelques styles de base pour la marque WKND en ajoutant quelques fichiers Sass (`.scss` extension) via le module **ui.frontend**.

1. Ouvrez le module **ui.frontend** et accédez à `src/main/webpack/base/sass`.

   ![module ui.frontend](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. Créez un nouveau fichier nommé `_variables.scss` sous le dossier `src/main/webpack/base/sass`.
1. Remplissez `_variables.scss` avec les éléments suivants :

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   Sass nous permet de créer des variables, qui peuvent ensuite être utilisées dans différents fichiers pour assurer la cohérence. Notez les familles de polices. Plus loin dans le tutoriel, nous verrons comment inclure un appel aux polices Web Google, afin d&#39;utiliser ces polices.

1. Créez un autre fichier nommé `_elements.scss` sous `src/main/webpack/base/sass` et renseignez-le avec ce qui suit :

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   Notez que le fichier `_elements.scss` utilise les variables de l&#39;élément `_variables.scss`.

1. Mettez à jour `_shared.scss` sous `src/main/webpack/base/sass` pour inclure les fichiers `_elements.scss` et `_variables.scss`.

   ```css
   @import './variables';
   @import './elements';
   ```

1. Ouvrez un terminal de ligne de commande et installez le module **ui.frontend** à l&#39;aide de la commande `npm install` :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` ne doit être exécuté qu&#39;une seule fois, après un nouveau clone ou une nouvelle génération du projet.

1. Dans le même terminal, créez et déployez le module **ui.frontend** en utilisant la commande `npm run dev` :

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   La commande `npm run dev` doit générer et compiler le code source du projet Webpack et, en fin de compte, renseigner les **clientlib-site** et **clientlib-dependencies** dans le module **ui.apps**.

   >[!NOTE]
   >
   >Il existe également un profil `npm run prod` qui minimisera les données JS et CSS. Il s&#39;agit de la compilation standard chaque fois que la compilation webpack est déclenchée via Maven. Vous trouverez plus de détails sur le module [ui.frontend ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect le fichier `site.css` sous `ui.frontend/dist/clientlib-site/css/site.css`. Notez que la page CSS est principalement composée du contenu du fichier `_elements.scss` créé précédemment, mais que les variables ont été remplacées par des valeurs réelles.

   ![CSS du site distribué](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect le fichier `ui.frontend/clientlib.config.js`. Il s’agit du fichier de configuration d’un module externe npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator). **aem-clientlib-** generatorest l&#39;outil responsable de la transformation du code CSS/JavaScript compilé et de sa copie dans le  **fichier ui.** appsmodule.

1. Inspect le fichier `site.css` dans le module **ui.apps** à `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Il doit s&#39;agir d&#39;une copie identique du fichier `site.css` du module **ui.frontend**. Maintenant qu&#39;il se trouve dans le module **ui.apps**, il peut être déployé sur AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Puisque **clientlib-site** est en fait compilé pendant la génération, en utilisant **npm** ou **maven**, il peut en fait être ignoré du contrôle de code source dans le module **ui.apps**. Inspect le fichier `.gitignore` sous **ui.apps**.

>[!CAUTION]
>
> L&#39;utilisation du module **ui.frontend** peut ne pas être nécessaire pour tous les projets. Le module **ui.frontend** ajoute une complexité supplémentaire et s&#39;il n&#39;y a pas besoin/désir d&#39;utiliser certains de ces outils avancés de l&#39;interface (Sass, webpack, npm...), il peut être exagéré. C&#39;est pourquoi il est considéré comme une partie facultative de l&#39;archétype de projet AEM et l&#39;utilisation de bibliothèques côté client standard et de vanilla CSS et JavaScript continue d&#39;être entièrement pris en charge.

## Inclusion de page et de modèle {#page-inclusion}

Nous allons maintenant examiner comment le projet est configuré pour inclure les clientlibs dans les modèles/pages AEM. Une bonne pratique courante dans le développement Web consiste à inclure CSS dans l’en-tête HTML `<head>` et JavaScript juste avant de fermer la balise `</body>`.

1. Dans le module **ui.apps** accédez à `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`.

   ![Composant de page de structure](assets/client-side-libraries/customheaderlibs-html.png)

   Il s’agit du composant `page` utilisé pour effectuer le rendu de toutes les pages de l’implémentation WKND.

1. Ouvrez le fichier `customheaderlibs.html`. Remarquez les lignes `${clientlib.css @ categories='wknd.base'}`. Cela indique que la page CSS pour clientlib avec une catégorie `wknd.base` sera incluse via ce fichier, incluant en fait **clientlib-base** dans l&#39;en-tête de toutes nos pages.

1. Mettez à jour `customheaderlibs.html` pour inclure une référence aux styles de police Google que nous avons spécifiés plus tôt dans le module **ui.frontend**. Nous allons aussi commenter ContextHub pour le moment...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. Inspect le fichier `customfooterlibs.html`. Ce fichier, tel que `customheaderlibs.html`, doit être remplacé par l’implémentation de projets. Ici, la ligne `${clientlib.js @ categories='wknd.base'}` signifie que le code JavaScript de **clientlib-base** sera inclus au bas de toutes nos pages.

1. Créez et déployez le projet sur une instance AEM locale à l’aide de Maven :

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Accédez aux modèles WKND à l’adresse [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd).

1. Sélectionnez **Modèle de page d’article** et ouvrez-le dans l’éditeur de modèles.

   ![Sélectionner le modèle de page d’article](assets/client-side-libraries/open-article-page-template.png)

1. Cliquez sur l&#39;icône **Informations sur la page** et dans le menu, sélectionnez **Stratégie de page** pour ouvrir la boîte de dialogue **Stratégie de page**.

   ![Stratégie de page Modèle de page d&#39;article](assets/client-side-libraries/template-page-policy.png)

   *Informations sur la page > Stratégie de page*

1. Notez que les catégories pour `wknd.dependencies` et `wknd.site` sont répertoriées ici. Par défaut, les clientlibs configurés via la stratégie de page sont fractionnés afin d’inclure le CSS dans l’en-tête de page et le code JavaScript à l’extrémité du corps. Si vous le souhaitez, vous pouvez explicitement liste que le code JavaScript clientlib soit chargé dans l&#39;en-tête de page. C&#39;est le cas pour `wknd.dependencies`.

   ![Stratégie de page Modèle de page d&#39;article](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Il est également possible de référencer directement `wknd.site` ou `wknd.dependencies` à partir du composant de page, en utilisant le script `customheaderlibs.html` ou `customfooterlibs.html`, comme nous l&#39;avons vu plus tôt pour `wknd.base` clientlib. L’utilisation du modèle offre une certaine flexibilité en ce sens que vous pouvez choisir les clientlibs utilisés par modèle. Par exemple, si vous disposez d’une bibliothèque JavaScript très lourde qui ne sera utilisée que sur un modèle sélectionné.

1. Accédez à la page **LA Skateparks** créée à l&#39;aide du **Modèle de page d&#39;article** : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Vous devriez voir une différence dans les polices et certains styles de base appliqués pour indiquer que la page CSS créée dans le module **ui.frontend** fonctionne.

1. Cliquez sur l&#39;icône **Informations sur la page** et dans le menu, sélectionnez **Vue telle que publiée** pour ouvrir la page de l&#39;article en dehors de l&#39;éditeur AEM.

   ![Afficher comme publié(e) ](assets/client-side-libraries/view-as-published-article-page.png)

1. Vue de la source Page de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) et vous devriez être en mesure de voir les références clientlib suivantes dans le `<head>` :

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   Notez que les clientlibs utilisent le point de terminaison proxy `/etc.clientlibs`. Vous devriez également voir les inclusions clientlib suivantes en bas de la page :

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >Il est essentiel, du côté de la publication, que les bibliothèques clientes **ne soient pas** servies à partir de **/apps**, car ce chemin d’accès doit être limité pour des raisons de sécurité à l’aide de la section de filtre [Répartiteur](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La propriété [allowProxy](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la bibliothèque cliente garantit que le fichier CSS et JS sont diffusés à partir de **/etc.clientlibs**.

## Webpack DevServer {#webpack-dev-server}

Au cours des deux exercices précédents, nous avons pu mettre à jour plusieurs fichiers Sass dans le module **ui.frontend** et, par un processus de génération, voir ces changements reflétés dans l&#39;AEM. Nous allons maintenant nous pencher sur l&#39;utilisation d&#39;un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) pour développer rapidement nos styles frontaux.

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau affichées dans la vidéo :

1. Début du serveur de développement webpack en exécutant la commande suivante à partir du module **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Cette opération doit ouvrir une nouvelle fenêtre de navigateur à l’adresse [http://localhost:8080/](http://localhost:8080/) avec des balises statiques.
1. Copiez la source de la page de l’article de la page de l’article de LA skatepark à l’adresse [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Collez l&#39;annotation copiée de AEM dans le `index.html` du module **ui.frontend** situé sous `src/main/webpack/static`.
1. Modifiez le balisage copié et supprimez toutes les références aux **clientlib-site** et **clientlib-dependencies** :

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Nous pouvons supprimer ces références, car le serveur de développement webpack générera automatiquement ces artefacts.

1. Modifiez les fichiers `.scss` et voyez les modifications automatiquement répercutées dans le navigateur.
1. Examinez le fichier `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contient la configuration du webpack utilisée pour début du serveur webpack-dev-server. Notez qu’il effectue des proxy entre les chemins `/content` et `/etc.clientlibs` à partir d’une instance d’AEM exécutée localement. C’est ainsi que les images et autres clientlibs (non gérés par le code **ui.frontend**) sont rendues disponibles.

   >[!CAUTION]
   >
   > La source d’image de l’annotation statique pointe vers un composant d’image dynamique sur une instance d’AEM locale. Les images s’affichent rompues si le chemin d’accès à l’image change, si l’AEM n’est pas démarré ou si le navigateur utilise n’a pas ouvert de session dans l’instance AEM locale.
1. Vous pouvez **arrêter** le serveur webpack à partir de la ligne de commande en saisissant `CTRL+C`.

## Ensemble {#putting-it-together}

Ce didacticiel porte principalement sur les bibliothèques côté client et les workflows frontaux potentiels à intégrer à l&#39;AEM. C’est pourquoi nous accélérons l’implémentation en installant [client-side-libraries-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip), qui fournit certains styles par défaut pour les composants principaux utilisés dans le modèle de page de l’article :

* [Chemin de navigation](https://docs.adobe.com/content/help/fr/experience-manager-core-components/using/components/breadcrumb.html)
* [Télécharger](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [Image](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/image.html)
* [Liste](https://docs.adobe.com/content/help/fr/experience-manager-core-components/using/components/list.html)
* [Navigation](https://docs.adobe.com/content/help/fr/experience-manager-core-components/using/components/navigation.html)
* [Recherche rapide](https://docs.adobe.com/content/help/fr/experience-manager-core-components/using/components/quick-search.html)
* [Séparateur](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau affichées dans la vidéo :

1. Téléchargez [client-side-libraries-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) et décompressez le contenu sous `ui.frontend/src/main/webpack`. Le contenu du fichier compressé doit remplacer les dossiers suivants :

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. Prévisualisation des nouveaux styles à l’aide du serveur de développement webpack :

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Déployez la base de code sur une instance d’AEM locale pour afficher les nouveaux styles appliqués à l’article du parc de patins à roues alignées :

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## Félicitations ! {#congratulations}

Félicitations, la page d&#39;article a maintenant quelques styles cohérents qui correspondent à la marque WKND et vous êtes devenu familier avec le module **ui.frontend** !

### Étapes suivantes {#next-steps}

Découvrez comment mettre en oeuvre des styles individuels et réutiliser les composants principaux à l’aide de Experience Manager Style System. [Le développement avec le ](style-system.md) système de style couvre l’utilisation du système de style pour étendre les composants principaux avec des CSS propres à la marque et des configurations de stratégie avancées de l’éditeur de modèles.

Vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur la brach Git `client-side-libraries/solution`.

1. Cloner le référentiel [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `client-side-libraries/solution`.

## Outils et ressources supplémentaires {#additional-resources}

### aemfed {#develop-aemfed}

[****](https://aemfed.io/) aemfedest un outil de ligne de commande open-source qui peut être utilisé pour accélérer le développement frontal. Il est alimenté par [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) et [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

À un niveau élevé, **aemfed** est conçu pour écouter les modifications de fichier dans le module **ui.apps** et les synchronise automatiquement directement à une instance d&#39;AEM en cours d&#39;exécution. En fonction des modifications, un navigateur local actualise automatiquement les données, accélérant ainsi le développement frontal. Il est également conçu pour fonctionner avec le traceur Sling Log pour afficher automatiquement les erreurs côté serveur directement dans le terminal.

Si vous faites beaucoup de travail dans le module **ui.apps**, en modifiant les scripts HTL et en créant des composants personnalisés, **aemfed** peut être un outil très puissant à utiliser. [La documentation complète est disponible ici.](https://github.com/abmaonline/aemfed).

### Débogage des bibliothèques côté client {#debugging-clientlibs}

Avec différentes méthodes de **catégories** et **incorpore** pour inclure plusieurs bibliothèques clientes, il peut s’avérer difficile de résoudre les problèmes. aem expose plusieurs outils pour y remédier. L&#39;un des outils les plus importants est **Reconstruire les bibliothèques clientes**, ce qui obligera AEM à recompiler les fichiers LESS et à générer le fichier CSS.

* [**Effacer les bibliothèques**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  : Liste toutes les bibliothèques clientes enregistrées dans l’instance AEM.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Tester la sortie**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  : permet à un utilisateur de voir la sortie HTML attendue des inclusions clientlib en fonction de la catégorie.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Bibliothèques Validation**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  des dépendances : met en évidence les dépendances ou les catégories incorporées introuvables.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruire les bibliothèques**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  clientes : permet à un utilisateur de forcer l&#39;AEM à recréer toutes les bibliothèques clientes ou d&#39;invalider le cache des bibliothèques clientes. Cet outil est particulièrement efficace lorsque vous développez avec LESS, car cela peut forcer AEM à recompiler le CSS généré. En général, il est plus efficace d’invalider les caches, puis d’actualiser la page plutôt que de recréer toutes les bibliothèques. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruction de la bibliothèque cliente](assets/client-side-libraries/rebuild-clientlibs.png)
