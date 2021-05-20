---
title: Bibliothèques côté client et processus front-end
description: Découvrez comment les bibliothèques côté client ou clientlibs sont utilisées pour déployer et gérer CSS et JavaScript pour une implémentation de sites Adobe Experience Manager (AEM). Ce tutoriel explique également comment le module ui.frontend, un projet webpack, peut être intégré au processus de génération de bout en bout.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Composants principaux, archétype de projet AEM
topic: Gestion de contenu, développement
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '3299'
ht-degree: 4%

---


# Bibliothèques côté client et processus front-end {#client-side-libraries}

Découvrez comment les bibliothèques côté client ou clientlibs sont utilisées pour déployer et gérer CSS et JavaScript pour une implémentation de sites Adobe Experience Manager (AEM). Ce tutoriel explique également comment le module [ui.frontend](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/archetype/uifrontend.html), un projet [webpack](https://webpack.js.org/) découplé, peut être intégré au processus de génération de bout en bout.

## Prérequis {#prerequisites}

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment).

Il est également recommandé de consulter le tutoriel [Notions de base sur les composants](component-basics.md#client-side-libraries) pour comprendre les principes de base des bibliothèques et AEM côté client.

### Projet de démarrage

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes d’extraction du projet de démarrage.

Consultez le code de ligne de base sur lequel le tutoriel s’appuie :

1. Extrayez la branche `tutorial/client-side-libraries-start` à partir de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Déployez la base de code sur une instance d’AEM locale à l’aide de vos compétences Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` à toute commande Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) ou extraire le code localement en passant à la branche `tutorial/client-side-libraries-solution`.

## Objectifs

1. Découvrez comment les bibliothèques côté client sont incluses sur une page via un modèle modifiable.
1. Découvrez comment utiliser le module UI.Frontend et un serveur de développement Webpack pour le développement front-end dédié.
1. Comprenez le workflow de bout en bout de la diffusion de code CSS et JavaScript compilé sur une implémentation de Sites.

## Ce que vous allez créer {#what-you-will-build}

Dans ce chapitre, vous allez ajouter des styles de ligne de base pour le site WKND et le modèle de page de l’article afin de rapprocher l’implémentation des [maquettes de conception de l’interface utilisateur](assets/pages-templates/wknd-article-design.xd). Vous allez utiliser un workflow front-end avancé pour intégrer un projet webpack dans une bibliothèque cliente AEM.

![Styles terminés](assets/client-side-libraries/finished-styles.png)

*Page d’article avec des styles de ligne de base appliqués*

## Arrière-plan {#background}

Les bibliothèques côté client offrent un mécanisme d’organisation et de gestion des fichiers CSS et JavaScript nécessaires à une mise en oeuvre AEM Sites. Les objectifs de base des bibliothèques côté client ou clientlibs sont les suivants :

1. Stocker les fichiers CSS/JS dans des petits fichiers discrets pour faciliter le développement et la maintenance
1. Gérer les dépendances sur les structures tierces de manière organisée
1. Réduisez le nombre de requêtes côté client en concaténant CSS/JS en une ou deux requêtes.

Vous trouverez plus d’informations sur l’utilisation des [bibliothèques côté client ici.](https://docs.adobe.com/content/help/fr-FR/experience-manager-65/developing/introduction/clientlibs.html)

Les bibliothèques côté client présentent certaines limites. Le plus notable est une prise en charge limitée des langages front-end populaires tels que Sass, LESS et TypeScript. Dans le tutoriel, nous allons examiner comment le module **ui.frontend** peut vous aider à résoudre ce problème.

Déployez la base de code de démarrage vers une instance d’AEM locale et accédez à [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Cette page n’a actuellement pas de style. Nous allons ensuite mettre en oeuvre des bibliothèques côté client pour la marque WKND afin d’ajouter du code CSS et Javascript à la page.

## Organisation des bibliothèques côté client {#organization}

Nous allons ensuite explorer l’organisation des bibliothèques clientes générées par l’[archétype de projet AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/archetype/overview.html).

![Organisation de bibliothèque cliente de haut niveau](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramme de haut niveau Organisation de la bibliothèque côté client et inclusion de page*

>[!NOTE]
>
> L’organisation de bibliothèque côté client suivante est générée par AEM Project Archetype, mais ne représente qu’un point de départ. La manière dont un projet gère et diffuse finalement du code CSS et JavaScript vers une implémentation de sites peut varier considérablement en fonction des ressources, des compétences et des exigences.

1. À l’aide de VSCode ou d’un autre IDE, ouvrez le module **ui.apps** .
1. Développez le chemin `/apps/wknd/clientlibs` pour afficher les bibliothèques clientes générées par l’archétype.

   ![Clientlibs dans ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Nous examinerons ces clientlibs plus en détail ci-dessous.

1. Le tableau suivant résume les bibliothèques clientes. Vous trouverez plus d’informations sur [l’inclusion des bibliothèques clientes ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nom | Description | Remarques |
   |-------------------| ------------| ------|
   | `clientlib-base` | Niveau de base du code CSS et JavaScript nécessaire au fonctionnement du site WKND | incorpore les bibliothèques clientes des composants principaux. |
   | `clientlib-grid` | Génère le CSS nécessaire au [mode de mise en page](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) pour qu’il fonctionne. | Les points d’arrêt pour mobiles/tablettes peuvent être configurés ici. |
   | `clientlib-site` | Contient un thème spécifique au site WKND | Généré par le module `ui.frontend` |
   | `clientlib-dependencies` | Incorpore toutes les dépendances tierces | Généré par le module `ui.frontend` |

1. Observez que `clientlib-site` et `clientlib-dependencies` sont ignorés du contrôle source. Cela se fait par conception, car ils seront générés au moment de la création par le module `ui.frontend`.

## Mettre à jour les styles de base {#base-styles}

Mettez ensuite à jour les styles de base définis dans le module **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**. Les fichiers du module `ui.frontend` génèrent les bibliothèques `clientlib-site` et `clientlib-dependecies` qui contiennent le thème Site et toutes les dépendances tierces.

Les bibliothèques côté client présentent certaines limites en ce qui concerne la prise en charge de langages tels que [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Il existe un certain nombre d’outils open source tels que [NPM](https://www.npmjs.com/) et [webpack](https://webpack.js.org/) qui accélèrent et optimisent le développement front-end. L’objectif du module **ui.frontend** est de pouvoir utiliser ces outils pour gérer la majorité des fichiers source front-end.

1. Ouvrez le module **ui.frontend** et accédez à `src/main/webpack/site`.
1. Ouvrez le fichier `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` est le point d’entrée de tous les fichiers Sass du  `ui.frontend` module. Il inclura le fichier `_variables.scss`, qui contient une série de variables de marque à utiliser dans différents fichiers Sass du projet. Le fichier `_base.scss` est également inclus et définit certains styles de base pour les éléments HTML. Une expression régulière inclut tous les styles pour les styles de composants individuels sous `src/main/webpack/components`. Une autre expression régulière inclut tous les fichiers sous `src/main/webpack/site/styles`.

1. Inspectez le fichier `main.ts`. `main.ts` inclut  `main.scss` et inclut une expression régulière pour collecter tout  `.js` ou  `.ts` fichier dans le projet. Ce point d’entrée sera utilisé par les [fichiers de configuration webpack](https://webpack.js.org/configuration/) comme point d’entrée pour l’ensemble du module `ui.frontend`.

1. Inspect les fichiers sous `src/main/webpack/site/styles` :

   ![Fichiers de style](assets/client-side-libraries/style-files.png)

   Ces styles de fichiers pour les éléments globaux du modèle, tels que le conteneur En-tête, Pied de page et contenu principal. Les règles CSS de ces fichiers ciblent différents éléments HTML `header`, `main` et `footer`. Ces éléments HTML ont été définis par des stratégies dans le chapitre [Pages et modèles](./pages-templates.md) précédent.

1. Développez le dossier `components` sous `src/main/webpack` et examinez les fichiers.

   ![Fichiers Sass de composant](assets/client-side-libraries/component-sass-files.png)

   Chaque fichier est mappé à un composant principal comme le [composant d’accordéon](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Chaque composant principal est créé avec la notation [Block Element Modifier](https://getbem.com/) ou BEM afin de faciliter le ciblage de classes CSS spécifiques avec des règles de style. Les fichiers situés sous `/components` ont été bloqués par l’archétype de projet AEM avec les différentes règles BEM pour chaque composant.

1. Téléchargez les styles de base WKND **[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** et **décompressez le fichier**.

   ![Styles de base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Pour accélérer le tutoriel, nous avons fourni plusieurs fichiers Sass qui implémentent la marque WKND en fonction des composants principaux et de la structure du modèle de page d’article.

1. Remplacez le contenu de `ui.frontend/src` par les fichiers de l’étape précédente. Le contenu du fichier zip doit remplacer les dossiers suivants :

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![Fichiers modifiés](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect les fichiers modifiés pour afficher les détails de l’implémentation du style WKND.

## Inspect de l’intégration ui.frontend {#ui-frontend-integration}

Un élément d’intégration clé intégré au module **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prend les artefacts CSS et JS compilés d’un projet webpack/npm et les transforme en bibliothèques côté client AEM.

![Intégration de l’architecture ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

L’archétype de projet AEM configure automatiquement cette intégration. Ensuite, découvrez comment cela fonctionne.


1. Ouvrez un terminal de ligne de commande et installez le module **ui.frontend** à l’aide de la commande `npm install` :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` ne doit être exécuté qu’une seule fois, à la suite d’un nouveau clone ou d’une nouvelle génération du projet.

1. Dans le même terminal, créez et déployez le module **ui.frontend** à l’aide de la commande `npm run dev` :

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > Vous pouvez recevoir une erreur du type &quot;ERREUR dans ./src/main/webpack/site/main.scss&quot;.
   > Cela se produit généralement car votre environnement a changé depuis l’exécution de `npm install`.
   > Exécutez `npm rebuild node-sass` pour résoudre le problème. Cela se produit si la version de `npm` installée sur votre machine de développement locale diffère de la version utilisée par Maven `frontend-maven-plugin` dans le fichier `aem-guides-wknd/pom.xml`. Vous pouvez corriger ce problème de manière permanente en modifiant la version du fichier pom pour qu’elle corresponde à votre version locale ou vice versa.

1. La commande `npm run dev` doit créer et compiler le code source pour le projet Webpack et renseigner en fin de compte **clientlib-site** et **clientlib-dependencies** dans le module **ui.apps**.

   >[!NOTE]
   >
   >Il existe également un profil `npm run prod` qui minimise les éléments JS et CSS. Il s’agit de la compilation standard chaque fois que la version de webpack est déclenchée via Maven. Vous trouverez plus d’informations sur le module [ui.frontend ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect le fichier `site.css` sous `ui.frontend/dist/clientlib-site/css/site.css`. Il s’agit de la page CSS compilée basée sur les fichiers source Sass.

   ![CSS du site distribué](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspectez le fichier `ui.frontend/clientlib.config.js`. Il s’agit du fichier de configuration d’un module externe npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) qui transforme le contenu de `/dist` en bibliothèque cliente et le déplace vers le module `ui.apps`.

1. Inspect le fichier `site.css` dans le module **ui.apps** à l’adresse `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Il doit s’agir d’une copie identique du fichier `site.css` du module **ui.frontend**. Maintenant qu’il se trouve dans le module **ui.apps**, il peut être déployé sur AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Étant donné que **clientlib-site** est compilé pendant la génération, à l’aide de **npm** ou **maven**, il peut être ignoré en toute sécurité du contrôle source dans le module **ui.apps**. Inspect le fichier `.gitignore` sous **ui.apps**.

1. Synchronisez la bibliothèque `clientlib-site` avec une instance locale d’AEM à l’aide des outils de développement ou des compétences Maven.

   ![Synchroniser le site de bibliothèque cliente](assets/client-side-libraries/sync-clientlib-site.png)

1. Ouvrez l’article LA Skatepark dans AEM à l’adresse : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Mise à jour des styles de base de l’article](assets/client-side-libraries/updated-base-styles.png)

   Vous devriez maintenant voir les styles mis à jour pour l’article. Vous devrez peut-être effectuer une actualisation difficile afin d’effacer les fichiers CSS mis en cache par le navigateur.

   Ça commence à ressembler beaucoup plus aux maquettes !

   >[!NOTE]
   >
   > Les étapes ci-dessus pour créer et déployer le code ui.frontend vers AEM seront exécutées automatiquement lorsqu’une version Maven est déclenchée à partir de la racine du projet `mvn clean install -PautoInstallSinglePackage`.

>[!CAUTION]
>
> L’utilisation du module **ui.frontend** n’est peut-être pas nécessaire pour tous les projets. Le module **ui.frontend** ajoute une complexité supplémentaire et, s’il n’est pas nécessaire/souhaité d’utiliser certains de ces outils front-end avancés (Sass, webpack, npm...), il n’est peut-être pas nécessaire.

## Inclusion de page et de modèle {#page-inclusion}

Examinons ensuite la manière dont les bibliothèques clientes sont référencées dans la page AEM. Une bonne pratique courante dans le développement web consiste à inclure CSS dans l’en-tête HTML `<head>` et JavaScript juste avant de fermer la balise `</body>` .

1. Dans le module **ui.apps** , accédez à `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Composant de page de structure](assets/client-side-libraries/customheaderlibs-html.png)

   Il s’agit du composant `page` utilisé pour effectuer le rendu de toutes les pages dans l’implémentation WKND.

1. Ouvrez le fichier `customheaderlibs.html`. Remarquez les lignes `${clientlib.css @ categories='wknd.base'}`. Cela indique que le CSS de la bibliothèque cliente avec une catégorie `wknd.base` sera inclus via ce fichier, y compris en fait **clientlib-base** dans l’en-tête de toutes nos pages.

1. Mettez à jour `customheaderlibs.html` pour inclure une référence aux styles de police Google que nous avons spécifiés précédemment dans le module **ui.frontend**.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspectez le fichier `customfooterlibs.html`. Ce fichier, comme `customheaderlibs.html`, est destiné à être remplacé par l’implémentation de projets. Ici, la ligne `${clientlib.js @ categories='wknd.base'}` signifie que le code JavaScript de **clientlib-base** sera inclus au bas de toutes nos pages.

1. Exportez le composant `page` vers le serveur AEM à l’aide des outils de développement ou de vos compétences Maven.

1. Accédez au modèle Page d’article à l’adresse [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Cliquez sur l’icône **Informations sur la page** et, dans le menu, sélectionnez **Stratégie de page** pour ouvrir la boîte de dialogue **Stratégie de page**.

   ![Stratégie de page du modèle d’article](assets/client-side-libraries/template-page-policy.png)

   *Informations sur la page > Stratégie de page*

1. Notez que les catégories pour `wknd.dependencies` et `wknd.site` sont répertoriées ici. Par défaut, les clientlibs configurées via la stratégie de page sont fractionnées afin d’inclure le CSS dans l’en-tête de page et le JavaScript à la fin du corps. Si vous le souhaitez, vous pouvez indiquer explicitement que le code JavaScript clientlib doit être chargé dans l’en-tête de la page. C’est le cas pour `wknd.dependencies`.

   ![Stratégie de page du modèle d’article](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Il est également possible de référencer `wknd.site` ou `wknd.dependencies` directement à partir du composant de page, à l’aide du script `customheaderlibs.html` ou `customfooterlibs.html`, comme nous l’avons vu plus tôt pour la bibliothèque cliente `wknd.base`. L’utilisation du modèle offre une certaine flexibilité dans le sens où vous pouvez choisir les clientlibs utilisées par modèle. Par exemple, si vous disposez d’une bibliothèque JavaScript très lourde qui ne sera utilisée que sur un modèle sélectionné.

1. Accédez à la page **LA Skateparks** créée à l’aide du **Modèle de page d’article** : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Vous devriez constater une différence dans les polices.

1. Cliquez sur l’icône **Informations sur la page** et, dans le menu, sélectionnez **Afficher comme publié** pour ouvrir la page de l’article en dehors de l’éditeur d’AEM.

   ![Afficher comme publié(e) ](assets/client-side-libraries/view-as-published-article-page.png)

1. Affichez la source Page de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) et vous devriez pouvoir voir les références clientlib suivantes dans le `<head>` :

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   Notez que les clientlibs utilisent le point de terminaison `/etc.clientlibs` proxy. Vous devriez également voir les inclusions clientlib suivantes au bas de la page :

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Si vous suivez la version 6.5/6.4, les bibliothèques côté client ne seront pas automatiquement réduites. Consultez la documentation sur le [Gestionnaire de bibliothèques HTML pour activer la minfication (recommandé)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Il est essentiel, du côté publication, que les bibliothèques clientes **ne soient pas** servies depuis **/apps**, car ce chemin d’accès doit être limité pour des raisons de sécurité à l’aide de la section [Filtre de Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La [propriété allowProxy](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la bibliothèque cliente garantit que les fichiers CSS et JS sont diffusés à partir de **/etc.clientlibs**.

## Webpack DevServer - Static Markup {#webpack-dev-static}

Dans les deux exercices précédents, nous avons pu mettre à jour plusieurs fichiers Sass dans le module **ui.frontend** et, par un processus de génération, voir ces modifications répercutées dans AEM. Ensuite, nous allons étudier des techniques qui utilisent [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) pour développer rapidement nos styles front-end par rapport à **static** HTML.

Cette technique est pratique si la plupart des styles et du code frontal sont effectués par un développeur front-end dédié qui n’a peut-être pas un accès facile à un environnement AEM. Cette technique permet également au FED d’apporter des modifications directement au code HTML, qui peut ensuite être transféré à un développeur d’AEM pour implémenter en tant que composants.

1. Copiez la source de la page de l’article de LA skatepark à l’adresse [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Ouvrez à nouveau votre IDE. Collez la balise copiée de l’AEM dans la balise `index.html` du module **ui.frontend** sous `src/main/webpack/static`.
1. Modifiez la balise copiée et supprimez toutes les références à **clientlib-site** et **clientlib-dependencies** :

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Nous pouvons supprimer ces références, car le serveur de développement webpack génère ces artefacts automatiquement.

1. Démarrez le serveur de développement webpack à partir d’un nouveau terminal en exécutant la commande suivante depuis le module **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Cela devrait ouvrir une nouvelle fenêtre de navigateur à l’adresse [http://localhost:8080/](http://localhost:8080/) avec des balises statiques.

1. Modifiez le fichier `src/main/webpack/site/_variables.scss` . Remplacez la règle `$text-color` par ce qui suit :

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Enregistrez les modifications.

1. Les modifications doivent automatiquement être répercutées dans le navigateur sur [http://localhost:8080](http://localhost:8080).

   ![Modifications du serveur de développement webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Examinez le fichier `/aem-guides-wknd.ui.frontend/webpack.dev.js` . Contient la configuration webpack utilisée pour démarrer le serveur webpack-dev-server. Notez qu’il effectue un proxy entre les chemins `/content` et `/etc.clientlibs` à partir d’une instance d’AEM en cours d’exécution locale. C’est ainsi que les images et autres clientlibs (non gérés par le code **ui.frontend**) sont disponibles.

   >[!CAUTION]
   >
   > La source d’image du balisage statique pointe vers un composant d’image dynamique sur une instance d’AEM locale. Les images apparaissent rompues si le chemin d’accès à l’image change, si AEM n’est pas démarré ou si le navigateur ne s’est pas connecté à l’instance AEM locale. Si vous transférez à une ressource externe, il est également possible de remplacer les images par des références statiques.

1. Vous pouvez **arrêter** le serveur webpack à partir de la ligne de commande en tapant `CTRL+C`.

## Webpack DevServer - Watch et aemsync {#webpack-dev-watch}

Une autre technique consiste à demander à Node.js de surveiller les modifications apportées aux fichiers src dans le module `ui.frontend`. Chaque fois qu’un fichier change, il compile rapidement la bibliothèque cliente et utilise le module npm [aemsync](https://www.npmjs.com/package/aemsync) pour synchroniser les modifications avec un serveur AEM en cours d’exécution.

1. Démarrez le serveur de développement webpack en mode **watch** à partir d’un nouveau terminal en exécutant la commande suivante depuis le module **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. Cela compilera les fichiers `src` et synchronisera les modifications avec AEM à l’adresse [http://localhost:4502](http://localhost:4502)

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. Accédez à AEM et à l’article LA Skateparks : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![Modifications déployées dans AEM](assets/client-side-libraries/changes-deployed-aem-watch.png)

   Les modifications doivent être déployées vers AEM. Il y a un léger retard et vous devrez actualiser le navigateur manuellement pour voir les mises à jour. Toutefois, l’affichage direct des modifications dans AEM est bénéfique si vous utilisez de nouveaux composants et la création de boîtes de dialogue.

1. Rétablissez la modification sur `_variables.scss` et enregistrez les modifications. Les modifications doivent à nouveau être synchronisées avec l’instance locale de l’AEM après un léger délai.

1. Arrêtez le serveur de développement webpack et générez une version complète de Maven à partir de la racine du projet :

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Là encore, le module `ui.frontend` est compilé, transformé en bibliothèques clientes et déployé dans AEM via le module `ui.apps`. Cependant, cette fois, Maven accomplit tout pour nous.

## Félicitations !  {#congratulations}

Félicitations, la page Article comporte désormais des styles cohérents qui correspondent à la marque WKND et vous êtes familiarisé avec le module **ui.frontend** .

### Étapes suivantes {#next-steps}

Découvrez comment mettre en oeuvre des styles individuels et réutiliser les composants principaux à l’aide du système de style du Experience Manager. [Le développement avec le ](style-system.md) système de style couvre l’utilisation du système de style pour étendre les composants principaux avec des CSS spécifiques à la marque et des configurations de stratégie avancées de l’éditeur de modèles.

Affichez le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue le code et déployez-le localement sur la branche Git `tutorial/client-side-libraries-solution`.

1. Cloner le référentiel [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `tutorial/client-side-libraries-solution` .

## Outils et ressources supplémentaires {#additional-resources}

### aemfed {#develop-aemfed}

[****](https://aemfed.io/) aemfedest un outil de ligne de commande Open Source qui peut être utilisé pour accélérer le développement front-end. Il est optimisé par [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) et [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

A un niveau élevé, **aemfed** est conçu pour écouter les modifications de fichier dans le module **ui.apps** et les synchronise automatiquement directement à une instance AEM en cours d’exécution. En fonction des modifications, un navigateur local s’actualise automatiquement, accélérant ainsi le développement frontal. Il est également conçu pour fonctionner avec le traceur de journal Sling afin d’afficher automatiquement les erreurs côté serveur directement dans le terminal.

Si vous effectuez beaucoup de travail dans le module **ui.apps**, que vous modifiez des scripts HTL et créez des composants personnalisés, **aemfed** peut être un outil très puissant à utiliser. [Vous trouverez la documentation complète ici.](https://github.com/abmaonline/aemfed)

### Débogage des bibliothèques côté client {#debugging-clientlibs}

Avec différentes méthodes **categories** et **incorpore** pour inclure plusieurs bibliothèques clientes, il peut être fastidieux de résoudre le problème. AEM expose plusieurs outils pour faciliter cette tâche. L’un des outils les plus importants est **Recréer les bibliothèques clientes**, ce qui forcera AEM à recompiler tous les fichiers LESS et à générer le CSS.

* [**Effacer les bibliothèques**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  : répertorie toutes les bibliothèques clientes enregistrées dans l’instance AEM.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Sortie de test**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  : permet à un utilisateur de voir la sortie HTML attendue des inclusions clientlib en fonction de la catégorie.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validation**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  des dépendances des bibliothèques : met en évidence les dépendances ou les catégories incorporées introuvables.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruire les bibliothèques clientes**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  : permet à un utilisateur de forcer AEM à reconstruire toutes les bibliothèques clientes ou d’invalider le cache des bibliothèques clientes. Cet outil est particulièrement efficace lors du développement avec LESS, car cela peut forcer AEM à recompiler le CSS généré. En règle générale, il est plus efficace d’invalider les caches, puis d’effectuer une actualisation de page plutôt que de recréer toutes les bibliothèques. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruire la bibliothèque cliente](assets/client-side-libraries/rebuild-clientlibs.png)
