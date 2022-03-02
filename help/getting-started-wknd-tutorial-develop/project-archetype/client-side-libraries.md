---
title: Bibliothèques côté client et processus front-end
description: Découvrez comment les bibliothèques côté client ou clientlibs sont utilisées pour déployer et gérer CSS et JavaScript pour une implémentation de sites Adobe Experience Manager (AEM). Ce tutoriel explique également comment le module ui.frontend, un projet webpack, peut être intégré au processus de génération de bout en bout.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
source-git-commit: 1a73d7ee1f71a5bd78114398f04e98a894847957
workflow-type: tm+mt
source-wordcount: '2882'
ht-degree: 4%

---

# Bibliothèques côté client et processus front-end {#client-side-libraries}

Découvrez comment les bibliothèques côté client ou clientlibs sont utilisées pour déployer et gérer CSS et JavaScript pour une implémentation de sites Adobe Experience Manager (AEM). Ce tutoriel explique également comment [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=fr) module, un découplé [webpack](https://webpack.js.org/) , peut être intégré au processus de génération de bout en bout.

## Prérequis {#prerequisites}

Examinez les outils et les instructions requis pour configurer une [environnement de développement local](overview.md#local-dev-environment).

Il est également recommandé de consulter la section [Principes de base des composants](component-basics.md#client-side-libraries) tutoriel pour comprendre les principes de base des bibliothèques et AEM côté client.

### Projet de démarrage

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes d’extraction du projet de démarrage.

Consultez le code de ligne de base sur lequel le tutoriel s’appuie :

1. Consultez la section `tutorial/client-side-libraries-start` branche à partir de [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > Si vous utilisez AEM version 6.5 ou 6.4, ajoutez la variable `classic` profile à n’importe quelle commande Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) ou extraire le code localement en passant à la branche `tutorial/client-side-libraries-solution`.

## Objectifs

1. Découvrez comment les bibliothèques côté client sont incluses sur une page via un modèle modifiable.
1. Découvrez comment utiliser le module UI.Frontend et un serveur de développement Webpack pour le développement front-end dédié.
1. Comprenez le workflow de bout en bout de la diffusion de code CSS et JavaScript compilé sur une implémentation de Sites.

## Ce que vous allez créer {#what-you-will-build}

Dans ce chapitre, vous allez ajouter des styles de ligne de base pour le site WKND et le modèle de page de l’article afin de rapprocher l’implémentation de la [Modèles de conception d’interface utilisateur](assets/pages-templates/wknd-article-design.xd). Vous allez utiliser un workflow front-end avancé pour intégrer un projet webpack dans une bibliothèque cliente AEM.

![Styles terminés](assets/client-side-libraries/finished-styles.png)

*Page d’article avec des styles de ligne de base appliqués*

## Arrière-plan {#background}

Les bibliothèques côté client offrent un mécanisme d’organisation et de gestion des fichiers CSS et JavaScript nécessaires à une mise en oeuvre AEM Sites. Les objectifs de base des bibliothèques côté client ou clientlibs sont les suivants :

1. Stocker les fichiers CSS/JS dans des petits fichiers discrets pour faciliter le développement et la maintenance
1. Gérer les dépendances sur les structures tierces de manière organisée
1. Réduisez le nombre de requêtes côté client en concaténant CSS/JS en une ou deux requêtes.

Vous trouverez plus d’informations sur l’utilisation des [bibliothèques côté client ici.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=fr)

Les bibliothèques côté client présentent certaines limites. Le plus notable est une prise en charge limitée des langages front-end populaires tels que Sass, LESS et TypeScript. Dans le tutoriel, nous allons examiner la manière dont le **ui.frontend** peut vous aider à résoudre ce problème.

Déployez la base de code de démarrage sur une instance d’AEM locale et accédez à [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Cette page n’a actuellement pas de style. Nous allons ensuite mettre en oeuvre des bibliothèques côté client pour la marque WKND afin d’ajouter du code CSS et Javascript à la page.

## Organisation des bibliothèques côté client {#organization}

Nous allons ensuite explorer l’organisation des clientlibs générées par la variable [AEM Archétype de projet](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr).

![Organisation de bibliothèque cliente de haut niveau](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramme de haut niveau Organisation de la bibliothèque côté client et inclusion de page*

>[!NOTE]
>
> L’organisation de bibliothèque côté client suivante est générée par AEM Project Archetype, mais ne représente qu’un point de départ. La manière dont un projet gère et diffuse finalement du code CSS et JavaScript vers une implémentation de sites peut varier considérablement en fonction des ressources, des compétences et des exigences.

1. À l’aide de VSCode ou d’un autre IDE, ouvrez le **ui.apps** module .
1. Développer le chemin `/apps/wknd/clientlibs` pour afficher les bibliothèques clientes générées par l’archétype.

   ![Clientlibs dans ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Nous examinerons ces clientlibs plus en détail ci-dessous.

1. Le tableau suivant résume les bibliothèques clientes. Plus d’informations sur [y compris les bibliothèques clientes se trouvent ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nom | Description | Remarques |
   |-------------------| ------------| ------|
   | `clientlib-base` | Niveau de base du code CSS et JavaScript nécessaire au fonctionnement du site WKND | incorpore les bibliothèques clientes des composants principaux. |
   | `clientlib-grid` | Génère le CSS nécessaire pour [Mode Mise en page](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) au travail. | Les points d’arrêt pour mobiles/tablettes peuvent être configurés ici. |
   | `clientlib-site` | Contient un thème spécifique au site WKND | Générée par le `ui.frontend` module |
   | `clientlib-dependencies` | Incorpore toutes les dépendances tierces | Générée par le `ui.frontend` module |

1. Observez que `clientlib-site` et `clientlib-dependencies` sont ignorées du contrôle source. Cela se fait par conception, car elles seront générées au moment de la création par la variable `ui.frontend` module .

## Mise à jour des styles de base {#base-styles}

Mettez ensuite à jour les styles de base définis dans le **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** module . Les fichiers de la variable `ui.frontend` génère la variable `clientlib-site` et `clientlib-dependecies` bibliothèques qui contiennent le thème Site et toutes les dépendances tierces.

Les bibliothèques côté client présentent certaines limites en ce qui concerne la prise en charge de langues telles que [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Il existe un certain nombre d&#39;outils open source comme [NPM](https://www.npmjs.com/) et [webpack](https://webpack.js.org/) qui accélèrent et optimisent le développement front-end. L’objectif de la variable **ui.frontend** est de pouvoir utiliser ces outils pour gérer une majorité de fichiers source front-end.

1. Ouvrez le **ui.frontend** et accédez à `src/main/webpack/site`.
1. Ouvrez le fichier `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)
bibliothèques côté client/main-scss

   `main.scss` est le point d’entrée de tous les fichiers Sass dans la variable `ui.frontend` module . Elle comprend la variable `_variables.scss` qui contient une série de variables de marque à utiliser dans différents fichiers Sass du projet. Le `_base.scss` est également inclus et définit certains styles de base pour les éléments de HTML. Une expression régulière comprend tous les styles pour les styles de composants individuels sous `src/main/webpack/components`. Une autre expression régulière inclut tous les fichiers sous `src/main/webpack/site/styles`.

1. Inspectez le fichier `main.ts`. Elle comprend `main.scss` et une expression régulière pour collecter n’importe quelle `.js` ou `.ts` fichiers du projet. Ce point d’entrée sera utilisé par la variable [fichiers de configuration webpack](https://webpack.js.org/configuration/) comme point d’entrée pour l’ensemble `ui.frontend` module .

1. Inspect des fichiers situés sous `src/main/webpack/site/styles`:

   ![Fichiers de style](assets/client-side-libraries/style-files.png)

   Ces styles de fichiers pour les éléments globaux du modèle, tels que le conteneur En-tête, Pied de page et contenu principal. Les règles CSS de ces fichiers ciblent différents éléments de HTML. `header`, `main`, et  `footer`. Ces éléments de HTML ont été définis par des stratégies dans le chapitre précédent. [Pages et modèles](./pages-templates.md).

1. Développez l’objet `components` Dossier sous `src/main/webpack` et inspectez les fichiers.

   ![Fichiers Sass de composant](assets/client-side-libraries/component-sass-files.png)

   Chaque fichier est mappé à un composant principal comme [Composant d’accordéon](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Chaque composant principal est créé avec [Block Element Modifier](https://getbem.com/) ou notation BEM pour faciliter le ciblage de classes CSS spécifiques avec des règles de style. Les fichiers sous `/components` ont été bloqués par l’archétype de projet AEM avec les différentes règles BEM pour chaque composant.

1. Téléchargement des styles de base WKND **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** et **unzip** le fichier .

   ![Styles de base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Pour accélérer le tutoriel, nous avons fourni plusieurs fichiers Sass qui implémentent la marque WKND en fonction des composants principaux et de la structure du modèle de page d’article.

1. Remplacer le contenu de `ui.frontend/src` avec les fichiers de l’étape précédente. Le contenu du fichier zip doit remplacer les dossiers suivants :

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Fichiers modifiés](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect les fichiers modifiés pour afficher les détails de l’implémentation du style WKND.

## Inspect de l’intégration ui.frontend {#ui-frontend-integration}

Élément d’intégration clé intégré à **ui.frontend** module, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prend les artefacts CSS et JS compilés d’un projet webpack/npm et les transforme en bibliothèques côté client AEM.

![Intégration de l’architecture ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

L’archétype de projet AEM configure automatiquement cette intégration. Ensuite, découvrez comment cela fonctionne.


1. Ouvrez un terminal de ligne de commande et installez le **ui.frontend** à l’aide du module `npm install` command :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` ne doit être exécuté qu’une seule fois, à la suite d’un nouveau clone ou d’une nouvelle génération du projet.

1. Démarrez le serveur de développement webpack dans **watch** en exécutant la commande suivante :

   ```shell
   $ npm run watch
   ```

1. Cette opération compilera la variable `src` dans le fichier `ui.frontend` et synchroniser les modifications avec AEM à l’adresse [http://localhost:4502](http://localhost:4502)

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

1. La commande `npm run watch` renseigne finalement la variable **clientlib-site** et **clientlib-dependencies** dans le **ui.apps** qui est ensuite synchronisé automatiquement avec AEM.

   >[!NOTE]
   >
   >Il y a également un `npm run prod` qui minimise les éléments JS et CSS. Il s’agit de la compilation standard chaque fois que la version de webpack est déclenchée via Maven. Plus d’informations sur [module ui.frontend se trouve ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect du fichier `site.css` sous `ui.frontend/dist/clientlib-site/site.css`. Il s’agit de la page CSS compilée basée sur les fichiers source Sass.

   ![CSS du site distribué](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspectez le fichier `ui.frontend/clientlib.config.js`. Il s’agit du fichier de configuration d’un module externe npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) qui transforme le contenu de `/dist` dans une bibliothèque cliente et la déplace vers le `ui.apps` module .

1. Inspect du fichier `site.css` dans le **ui.apps** module à `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Il doit s’agir d’une copie identique de la variable `site.css` du fichier **ui.frontend** module . Maintenant qu&#39;il est dans **ui.apps** il peut être déployé sur AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Depuis **clientlib-site** est compilé au cours de la création, à l’aide de l’une des méthodes suivantes : **npm** ou **maven**, il peut être ignoré en toute sécurité du contrôle source dans la variable **ui.apps** module . Inspect `.gitignore` fichier sous **ui.apps**.

1. Ouvrez l’article LA Skatepark dans AEM à l’adresse : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Mise à jour des styles de base de l’article](assets/client-side-libraries/updated-base-styles.png)

   Vous devriez maintenant voir les styles mis à jour pour l’article. Vous devrez peut-être effectuer une actualisation difficile afin d’effacer les fichiers CSS mis en cache par le navigateur.

   Ça commence à ressembler beaucoup plus aux maquettes !

   >[!NOTE]
   >
   > Les étapes ci-dessus pour créer et déployer le code ui.frontend vers AEM seront exécutées automatiquement lorsqu’une version Maven est déclenchée à la racine du projet. `mvn clean install -PautoInstallSinglePackage`.

## Apporter un changement de style

Apportez ensuite une légère modification au `ui.frontend` pour afficher le module `npm run watch` Déployez automatiquement les styles sur l’instance d’AEM locale.

1. Dans le `ui.frontend` ouvrez le fichier : `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Mettez à jour le `$brand-primary` variable de couleur :

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Enregistrez les modifications.

1. Revenez au navigateur et actualisez la page AEM pour afficher les mises à jour :

   ![Bibliothèques côté client](assets/client-side-libraries/style-update-brand-primary.png)

1. Rétablissement de la modification sur le `$brand-primary` coloration et arrêt de la création du webpack à l’aide de la commande `CTRL+C`.

>[!CAUTION]
>
> L’utilisation de la variable **ui.frontend** n’est peut-être pas nécessaire pour tous les projets. Le **ui.frontend** ajoute une complexité supplémentaire ; s’il n’est pas nécessaire ou souhaité d’utiliser certains de ces outils front-end avancés (Sass, webpack, npm..), cela peut ne pas être nécessaire.

## Inclusion de page et de modèle {#page-inclusion}

Examinons ensuite la manière dont les bibliothèques clientes sont référencées dans la page AEM. Une bonne pratique courante dans le développement web consiste à inclure CSS dans l’en-tête de HTML. `<head>` et JavaScript avant de fermer `</body>` balise .

1. Accédez au modèle Page d’article à l’adresse [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Cliquez sur le bouton **Informations sur la page** et dans le menu, sélectionnez **Stratégie de page** pour ouvrir le **Stratégie de page** boîte de dialogue.

   ![Règle de page du modèle de page Article](assets/client-side-libraries/template-page-policy.png)

   *Informations sur la page > Stratégie de page*

1. Notez que les catégories pour `wknd.dependencies` et `wknd.site` sont répertoriées ici. Par défaut, les clientlibs configurées via la stratégie de page sont fractionnées afin d’inclure le CSS dans l’en-tête de page et le JavaScript à la fin du corps. Si vous le souhaitez, vous pouvez indiquer explicitement que le code JavaScript clientlib doit être chargé dans l’en-tête de la page. C’est le cas pour `wknd.dependencies`.

   ![Règle de page du modèle de page Article](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Il est également possible de référencer la variable `wknd.site` ou `wknd.dependencies` directement à partir du composant de page, à l’aide de la méthode `customheaderlibs.html` ou `customfooterlibs.html` , comme nous l’avons vu plus tôt pour la fonction `wknd.base` clientlib. L’utilisation du modèle offre une certaine flexibilité dans le sens où vous pouvez choisir les clientlibs utilisées par modèle. Par exemple, si vous disposez d’une bibliothèque JavaScript très lourde qui ne sera utilisée que sur un modèle sélectionné.

1. Accédez au **LA Skateparks** page créée à l’aide de **Modèle de page d’article**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Cliquez sur le bouton **Informations sur la page** et dans le menu, sélectionnez **Afficher comme publié(e)** pour ouvrir la page d’article en dehors de l’éditeur d’AEM.

   ![Afficher comme publié(e)](assets/client-side-libraries/view-as-published-article-page.png)

1. Afficher la source de la page de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) et vous devriez pouvoir voir les références clientlib suivantes dans le `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Notez que les clientlibs utilisent le proxy. `/etc.clientlibs` point de terminaison . Vous devriez également voir les inclusions clientlib suivantes au bas de la page :

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Si vous suivez la version 6.5/6.4, les bibliothèques côté client ne seront pas automatiquement réduites. Consultez la documentation relative à la [Gestionnaire de bibliothèques de HTMLs pour activer la minfication (recommandé)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=fr#using-preprocessors).

   >[!WARNING]
   >
   >Il est essentiel, du côté publication, que les bibliothèques clientes soient **not** servi depuis **/apps** car ce chemin d’accès doit être limité pour des raisons de sécurité à l’aide de la variable [Section de filtrage de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). Le [allowProxy, propriété](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la bibliothèque cliente s’assure que les fichiers CSS et JS sont diffusés à partir de **/etc.clientlibs**.

### Étapes suivantes {#next-steps}

Découvrez comment mettre en oeuvre des styles individuels et réutiliser les composants principaux à l’aide du système de style du Experience Manager. [Développement avec le système de style](style-system.md) couvre l’utilisation du système de style pour étendre les composants principaux avec des CSS spécifiques à la marque et des configurations de stratégie avancées de l’éditeur de modèles.

Afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passer en revue et déployer le code localement sur la branche Git `tutorial/client-side-libraries-solution`.

1. Cloner le [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) référentiel.
1. Consultez la section `tutorial/client-side-libraries-solution` branche.

## Outils et ressources supplémentaires {#additional-resources}

### Webpack DevServer - Markup statique {#webpack-dev-static}

Dans les deux exercices précédents, nous avons pu mettre à jour plusieurs fichiers Sass dans la variable **ui.frontend** et, par le biais d’un processus de génération, voir ces modifications répercutées dans AEM. Ensuite, nous allons examiner des techniques qui tirent parti d’une [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) pour développer rapidement nos styles front-end par rapport à **static** HTML.

Cette technique est pratique si la plupart des styles et du code frontal sont effectués par un développeur front-end dédié qui n’a peut-être pas un accès facile à un environnement AEM. Cette technique permet également au FED d’apporter des modifications directement au HTML, qui peut ensuite être transféré à un développeur d’AEM pour implémenter en tant que composants.

1. Copiez la source de la page de l’article de LA skatepark à l’adresse [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Ouvrez à nouveau votre IDE. Collez la balise copiée de l’AEM dans le `index.html` dans le **ui.frontend** module sous `src/main/webpack/static`.
1. Modifiez la balise copiée et supprimez toutes les références à **clientlib-site** et **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Nous pouvons supprimer ces références, car le serveur de développement webpack génère ces artefacts automatiquement.

1. Démarrez le serveur de développement webpack à partir d’un nouveau terminal en exécutant la commande suivante depuis l’ **ui.frontend** module :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Cela devrait ouvrir une nouvelle fenêtre de navigateur à l’adresse [http://localhost:8080/](http://localhost:8080/) avec les balises statiques.

1. Modifier le fichier `src/main/webpack/site/_variables.scss` fichier . Remplacez la variable `$text-color` avec les éléments suivants :

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Enregistrez les modifications.

1. Les modifications doivent automatiquement être répercutées dans le navigateur sur [http://localhost:8080](http://localhost:8080).

   ![Modifications du serveur de développement webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Consultez la section `/aem-guides-wknd.ui.frontend/webpack.dev.js` fichier . Contient la configuration webpack utilisée pour démarrer le serveur webpack-dev-server. Notez qu’il effectue des proxys des chemins. `/content` et `/etc.clientlibs` à partir d’une instance d’AEM en cours d’exécution locale. Voici comment les images et autres clientlibs (non gérés par la variable **ui.frontend** (code) sont disponibles.

   >[!CAUTION]
   >
   > La source d’image du balisage statique pointe vers un composant d’image dynamique sur une instance d’AEM locale. Les images apparaissent rompues si le chemin d’accès à l’image change, si AEM n’est pas démarré ou si le navigateur ne s’est pas connecté à l’instance AEM locale. Si vous transférez à une ressource externe, il est également possible de remplacer les images par des références statiques.

1. Vous pouvez **stop** le serveur webpack à partir de la ligne de commande en saisissant `CTRL+C`.

### aemfed {#develop-aemfed}

[**aemfed**](https://aemfed.io/) est un outil de ligne de commande Open Source qui peut être utilisé pour accélérer le développement frontal. Il est alimenté par  [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) et [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

À un niveau élevé **aemfed** est conçu pour écouter les modifications apportées aux fichiers dans la variable **ui.apps** et les synchronise automatiquement directement sur une instance AEM en cours d’exécution. En fonction des modifications, un navigateur local s’actualise automatiquement, accélérant ainsi le développement frontal. Il est également conçu pour fonctionner avec le traceur de journal Sling afin d’afficher automatiquement les erreurs côté serveur directement dans le terminal.

Si vous effectuez beaucoup de travail au sein de la **ui.apps** module, modification des scripts HTL et création de composants personnalisés, **aemfed** peut être un outil très puissant à utiliser. [Vous trouverez la documentation complète ici .](https://github.com/abmaonline/aemfed).

### Débogage des bibliothèques côté client {#debugging-clientlibs}

Avec différentes méthodes **categories** et **incorpore** pour inclure plusieurs bibliothèques clientes, le dépannage peut s’avérer fastidieux. AEM expose plusieurs outils pour faciliter cette tâche. L’un des outils les plus importants est **Reconstruire les bibliothèques clientes** qui force AEM à recompiler les fichiers LESS et à générer le CSS.

* [**Effacer les bibliothèques**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Répertorie toutes les bibliothèques clientes enregistrées dans l’instance AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Test Output**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - permet à un utilisateur d’afficher la sortie de HTML attendue des inclusions clientlib en fonction de la catégorie. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validation des dépendances des bibliothèques**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - met en évidence les dépendances ou les catégories incorporées introuvables. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruire les bibliothèques clientes**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - permet à un utilisateur de forcer AEM à recréer toutes les bibliothèques clientes ou d’invalider le cache des bibliothèques clientes. Cet outil est particulièrement efficace lors du développement avec LESS, car cela peut forcer AEM à recompiler le CSS généré. En règle générale, il est plus efficace d’invalider les caches, puis d’effectuer une actualisation de page plutôt que de recréer toutes les bibliothèques. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruire la bibliothèque cliente](assets/client-side-libraries/rebuild-clientlibs.png)
