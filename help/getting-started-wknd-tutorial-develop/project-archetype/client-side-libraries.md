---
title: Bibliothèques clientes et workflow front-end
description: Découvrez comment utiliser les bibliothèques clientes pour déployer et gérer le code CSS et JavaScript pour une implémentation d’Adobe Experience Manager (AEM) Sites. Découvrez comment le module ui.frontend, un projet webpack, peut être intégré au processus de génération de bout en bout.
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
source-git-commit: 68a7f263284fdf9cfcf82572b8e1e1c0c01e4b55
workflow-type: tm+mt
source-wordcount: '2799'
ht-degree: 99%

---

# Bibliothèques clientes et workflow front-end {#client-side-libraries}

Apprenez comment les bibliothèques côté client (clientlibs) sont utilisées afin de déployer et de gérer le code CSS et JavaScript pour une implémentation d’Adobe Experience Manager (AEM) Sites. Ce tutoriel explique également comment le module [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=fr), un projet [webpack](https://webpack.js.org/) découplé, peut être intégré au processus de génération de bout en bout.

## Prérequis {#prerequisites}

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

Il est également recommandé de consulter le tutoriel [Principes de base des composants](component-basics.md#client-side-libraries) pour mieux comprendre les principes de base des bibliothèques côté client et d’AEM.

### Projet de démarrage

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes de consultation du projet de démarrage.

Consultez le code de ligne de base sur lequel le tutoriel s’appuie :

1. Consultez la branche `tutorial/client-side-libraries-start` à partir de [GitHub](https://github.com/adobe/aem-guides-wknd).

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Déployez la base de code sur une instance locale d’AEM à l’aide de vos compétences Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` à n’importe quelle commande Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) ou consulter le code localement en passant à la branche `tutorial/client-side-libraries-solution`.

## Objectifs

1. Découvrez comment les bibliothèques côté client sont incluses sur une page via un modèle modifiable.
1. Découvrez comment utiliser le module `ui.frontend` et un serveur de développement webpack pour le développement front-end dédié.
1. Comprenez le workflow de bout en bout de la diffusion de code CSS et JavaScript compilé sur une implémentation de Sites.

## Ce que vous allez construire {#what-build}

Dans ce chapitre, vous ajoutez quelques styles de ligne de base pour le site WKND et le modèle de page d’article afin de rapprocher l’implémentation des [maquettes de conception d’interface utilisateur](assets/pages-templates/wknd-article-design.xd). Vous utilisez un workflow front-end avancé pour intégrer un projet webpack dans une bibliothèque cliente AEM.

![Styles terminés.](assets/client-side-libraries/finished-styles.png)

*Page d’article avec des styles de ligne de base appliqués*

## Contexte {#background}

Les bibliothèques côté client offrent un mécanisme d’organisation et de gestion des fichiers CSS et JavaScript nécessaires à une implémentation d’AEM Sites. Les objectifs de base des bibliothèques côté client ou clientlibs sont les suivants :

1. Stocker le code CSS et JS dans des petits fichiers discrets pour faciliter le développement et la maintenance.
1. Gérer de manière organisée les dépendances des structures tierces.
1. Réduire le nombre de requêtes côté client en concaténant le code CSS et JS en une ou deux requêtes.

Vous trouverez plus d’informations sur l’utilisation des [bibliothèques côté client ici.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=fr)

Les bibliothèques côté client présentent certaines limites. La plus notable est la prise en charge limitée des langages front-end populaires tels que Sass, LESS et TypeScript. Dans le tutoriel, regardons comment le module **ui.frontend** peut vous aider à résoudre ce problème.

Déployez la base de code de démarrage sur une instance AEM locale et accédez à [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Cette page n’a pas de style. Implémentons des bibliothèques côté client pour la marque WKND afin d’ajouter du code CSS et JavaScript à la page.

## Organisation des bibliothèques côté client {#organization}

Explorons ensuite l’organisation des bibliothèques clientes générées par la variable [Archétype de projet AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr).

![Organisation de bibliothèque cliente de haut niveau.](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramme de haut niveau d’organisation de la bibliothèque côté client et d’inclusion de page*

>[!NOTE]
>
> L’organisation de bibliothèque côté client suivante est générée par l’archétype de projet AEM, mais ne représente qu’un point de départ. La manière dont un projet gère et diffuse finalement du code CSS et JavaScript vers une implémentation de Sites peut varier considérablement en fonction des ressources, des compétences et des exigences.

1. À l’aide de VSCode ou d’un autre IDE, ouvrez le module **ui.apps**.
1. Développez le chemin `/apps/wknd/clientlibs` pour afficher les bibliothèques clientes générées par l’archétype.

   ![Bibliothèques clientes dans ui.apps.](assets/client-side-libraries/four-clientlib-folders.png)

   Dans la section ci-dessous, ces bibliothèques clientes sont examinées plus en détail.

1. Le tableau suivant résume les bibliothèques clientes. Plus d’informations sur [l’inclusion des bibliothèques clientes se trouvent ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=fr#developing).

   | Nom | Description | Remarques |
   |-------------------| ------------| ------|
   | `clientlib-base` | Niveau de base du code CSS et JavaScript nécessaire au fonctionnement de WKND Site. | Incorpore les bibliothèques clientes des composants principaux. |
   | `clientlib-grid` | Génère le code CSS nécessaire au fonctionnement du [Mode de disposition](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=fr). | Les points d’arrêt pour mobiles/tablettes peuvent être configurés ici. |
   | `clientlib-site` | Contient un thème spécifique au site WKND. | Généré par le module `ui.frontend`. |
   | `clientlib-dependencies` | Incorpore toutes les dépendances tierces. | Généré par le module `ui.frontend`. |

1. Observez que les éléments `clientlib-site` et `clientlib-dependencies` sont ignorés du contrôle de code source. Cela est tout à fait normal, car ces éléments sont générés au moment de la création par le module `ui.frontend`.

## Mettre à jour les styles de base {#base-styles}

Mettez ensuite à jour les styles de base définis dans le module **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=fr)**. Les fichiers du module `ui.frontend` génèrent les bibliothèques `clientlib-site` et `clientlib-dependecies`, qui contiennent le thème du site et toutes les dépendances tierces.

Les bibliothèques côté client ne prennent pas en charge les langages plus avancés tels que [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Il existe plusieurs outils open source, tels que [NPM](https://www.npmjs.com/) et [webpack](https://webpack.js.org/), qui accélèrent et optimisent le développement front-end. L’objectif du module **ui.frontend** est de vous aider à utiliser ces outils pour gérer la plupart des fichiers source front-end.

1. Ouvrez le module **ui.frontend** et accédez à `src/main/webpack/site`.
1. Ouvrez le fichier `main.scss`.

   ![Point d’entrée main.scss.](assets/client-side-libraries/main-scss.png)

   `main.scss` est le point d’entrée des fichiers Sass dans le module `ui.frontend`. Il comprend le ficher `_variables.scss`, qui contient une série de variables de marque à utiliser dans les différents fichiers Sass du projet. Le fichier `_base.scss` est également inclus et définit certains styles de base pour les éléments HTML. Une expression régulière inclut les styles des composants individuels sous `src/main/webpack/components`. Une autre expression régulière inclut les fichiers sous `src/main/webpack/site/styles`.

1. Inspectez le fichier `main.ts`. Il comprend `main.scss` et une expression régulière permettant de collecter n’importe quel fichier `.js` ou `.ts` du projet. Ce point d’entrée est utilisé par les [fichiers de configuration webpack](https://webpack.js.org/configuration/) comme point d’entrée pour l’ensemble du module `ui.frontend`.

1. Inspectez les fichiers sous `src/main/webpack/site/styles` :

   ![Fichiers de style.](assets/client-side-libraries/style-files.png)

   Ces fichiers définissent le style des éléments globaux du modèle, tels que l’en-tête, le pied de page et le contenu principal. Les règles CSS de ces fichiers ciblent différents éléments HTML, tels que `header`, `main`, et `footer`. Ces éléments HTML ont été définis par des stratégies dans le chapitre précédent, [Pages et modèles](./pages-templates.md).

1. Développez le dossier `components` sous `src/main/webpack` et inspectez les fichiers.

   ![Fichiers Sass de composant.](assets/client-side-libraries/component-sass-files.png)

   Chaque fichier est mappé à un composant principal comme le [Composant d’accordéon](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=fr). Chaque composant principal est créé avec la notation [Block Element Modifier](https://getbem.com/), ou BEM, pour faciliter le ciblage de classes CSS spécifiques avec des règles de style. Les fichiers sous `/components` ont été bouchés par l’archétype de projet AEM avec les différentes règles BEM pour chaque composant.

1. Téléchargez les styles de base WKND **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** et **décompressez** le fichier.

   ![Styles de base WKND.](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Pour accélérer le tutoriel, plusieurs fichiers Sass qui implémentent la marque WKND en fonction des composants principaux et de la structure du modèle de page d’article sont fournis.

1. Remplacez le contenu de `ui.frontend/src` avec les fichiers de l’étape précédente. Le contenu du fichier zip doit remplacer les dossiers suivants :

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Fichiers modifiés.](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspectez les fichiers modifiés pour afficher les détails de l’implémentation du style WKND.

## Inspecter l’intégration ui.frontend {#ui-frontend-integration}

Élément d’intégration clé intégré au module **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prend les artefacts CSS et JS compilés d’un projet webpack/npm et les transforme en bibliothèques côté client AEM.

![Intégration de l’architecture ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

L’archétype de projet AEM configure automatiquement cette intégration. Découvrez ensuite comment cela fonctionne.


1. Ouvrez un terminal de ligne de commande et installez le module **ui.frontend** à l’aide de la commande `npm install` :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >L’exécution de `npm install` n’est nécessaire qu’une seule fois, comme après un nouveau clone ou une nouvelle génération du projet.

1. Démarrez le serveur de développement webpack avec le mode **watch** en exécutant la commande suivante :

   ```shell
   $ npm run watch
   ```

1. Cette opération compile les fichiers sources à partir du module `ui.frontend` et synchronise les modifications avec AEM à l’adresse [http://localhost:4502](http://localhost:4502)

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

1. La commande `npm run watch` renseigne finalement **clientlib-site** et **clientlib-dependencies** dans le module **ui.apps** qui est ensuite synchronisé automatiquement avec AEM.

   >[!NOTE]
   >
   >Il y a également un profil `npm run prod` qui minifie les éléments JS et CSS. Il s’agit de la compilation standard chaque fois que la version de webpack est déclenchée via Maven. Vous trouverez plus d’informations sur le module [ui.frontend ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=fr).

1. Inspectez le fichier `site.css` sous `ui.frontend/dist/clientlib-site/site.css`. Il s’agit de la page CSS compilée basée sur les fichiers source Sass.

   ![CSS du site distribué](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspectez le fichier `ui.frontend/clientlib.config.js`. Il s’agit du fichier de configuration d’un plug-in npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) qui transforme le contenu de `/dist` en une bibliothèque cliente et la déplace vers le module `ui.apps`.

1. Inspectez le fichier `site.css` dans le module **ui.apps** à `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Il doit sagir d&#39;une copie identique du fichier `site.css` du module **ui.frontend**. Il peut être déployé sur AEM maintenant qu&#39;il est dans le module **ui.apps**.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Depuis que **clientlib-site** est compilé au cours de la création, à l’aide de **npm** ou **maven**, il peut être ignoré en toute sécurité du contrôle source dans le module **ui.apps**. Inspectez le fichier `.gitignore` sous **ui.apps**.

1. Ouvrez l’article LA Skatepark dans AEM à l’adresse : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Mise à jour des styles de base de l’article](assets/client-side-libraries/updated-base-styles.png)

   Vous devriez maintenant voir les styles mis à jour pour l’article. Vous devrez peut-être effectuer une actualisation complète pour effacer les fichiers CSS mis en cache par le navigateur.

   Ça commence à ressembler beaucoup plus aux maquettes !

   >[!NOTE]
   >
   > Les étapes ci-dessus pour créer et déployer le code ui.frontend vers AEM sont exécutées automatiquement lorsqu’une version Maven est déclenchée à la racine du projet `mvn clean install -PautoInstallSinglePackage`.

## Modifier le style

Apportez ensuite une légère modification au module `ui.frontend` pour voir le `npm run watch` déployer automatiquement les styles vers l&#39;instance AEM locale.

1. À partir de là, le module `ui.frontend` ouvre le fichier : `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Mettez à jour la variable de couleur `$brand-primary` :

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Enregistrez les modifications.

1. Revenez au navigateur et actualisez la page AEM pour afficher les mises à jour :

   ![Bibliothèques côté client](assets/client-side-libraries/style-update-brand-primary.png)

1. Revenez sur la modification de la couleur `$brand-primary` et arrêtez la construction du webpack en utilisant la commande `CTRL+C`.

>[!CAUTION]
>
> L’utilisation du module **ui.frontend** n’est peut-être pas nécessaire pour tous les projets. Le module **ui.frontend** ajoute une complexité supplémentaire ; il n&#39;est pas obligatoire s’il n’est pas nécessaire ou souhaitable d’utiliser certains de ces outils front-end avancés (Sass, webpack, npm..).

## Inclusion de page et de modèle {#page-inclusion}

Examinons ensuite la manière dont les bibliothèques clientes sont référencées dans la page AEM. Une bonne pratique courante dans le développement web consiste à inclure le CSS dans l’en-tête HTML `<head>` et JavaScript juste avant de fermer la balise `</body>`.

1. Accédez au modèle de page d’article à l’adresse [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Cliquez sur l’icône **Informations sur la page** et dans le menu, sélectionnez **Politique de page** pour ouvrir la boîte de dialogue **Politique de page**.

   ![Politique de page du menu du modèle de page d&#39;article](assets/client-side-libraries/template-page-policy.png)

   *Informations sur la page > Politique de page*

1. Notez que les catégories pour `wknd.dependencies` et `wknd.site` sont répertoriées ici. Par défaut, les bibliothèques clientes configurées via la politique de page sont fractionnées afin d’inclure le CSS dans l’en-tête de page et le JavaScript à la fin du corps. Vous pouvez répertorier explicitement le code JavaScript de la bibliothèque cliente à charger dans l’en-tête de la page. C’est le cas pour `wknd.dependencies`.

   ![Politique de page du menu du modèle de page d’article](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Il est également possible de référencer `wknd.site` ou `wknd.dependencies` directement à partir du composant de page, à l’aide du script `customheaderlibs.html` ou `customfooterlibs.html`. L’utilisation du modèle offre une certaine flexibilité dans le sens où vous pouvez choisir les bibliothèques clientes utilisées pour chaque modèle. Par exemple, si vous avez une bibliothèque JavaScript volumineuse qui ne peut être utilisée que sur certains modèles.

1. Accédez à la page **LA Skateparks** créée à l’aide du **Modèle de page d’article** : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Cliquez sur l&#39;icône **Informations sur la page** et dans le menu, sélectionnez **Voir comme publié** pour ouvrir la page de l’article sans l’éditeur AEM.

   ![Affichage comme publié.](assets/client-side-libraries/view-as-published-article-page.png)

1. Affichez la source de la page de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) pour voir les références de la bibliothèque cliente suivante dans le `<head>` :

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Notez que les bibliothèques clientes utilisent le proxy de point d’entrée `/etc.clientlibs`. Vous devriez également voir que la bibliothèque cliente suivante est incluse au bas de la page :

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Pour AEM 6.5/6.4, les bibliothèques côté client ne sont pas automatiquement réduites. Consultez la documentation sur le [Gestionnaire de bibliothèques HTML pour activer la réduction (recommandé)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=fr#using-preprocessors).

   >[!WARNING]
   >
   >Il est essentiel, du côté publication, que les bibliothèques clientes ne soient **pas** alimentées depuis **/apps**, car ce chemin d’accès doit être limité pour des raisons de sécurité à l’aide de la [Section de filtrage Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#example-filter-section). La [propriété allowProxy](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=fr#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la bibliothèque cliente s’assure que les fichiers CSS et JS sont diffusés depuis **/etc.clientlibs**.

### Étapes suivantes {#next-steps}

Découvrez comment mettre en œuvre des styles individuels et réutiliser les composants principaux à l’aide du système de style d’Experience Manager. Le [Développement avec le système de style](style-system.md) couvre l’utilisation du système de style pour étendre les composants principaux avec des configurations CSS et de politiques avancées spécifiques à la marque de l’éditeur de modèles.

Affichez le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou révisez et déployez le code localement sur la branche Git `tutorial/client-side-libraries-solution`.

1. Clonez le référentiel [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `tutorial/client-side-libraries-solution`.

## Outils et ressources supplémentaires {#additional-resources}

### Webpack DevServer - Balisage statique {#webpack-dev-static}

Dans les deux exercices précédents, plusieurs fichiers Sass dans le module **ui.frontend** ont été mis à jour et, par le biais d’un processus de création, on peut voir que ces modifications se répercutent sur AEM. Examinons ensuite une technique qui utilise [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) pour développer rapidement les styles front-end par rapport au HTML **statique**.

Cette technique est utile si la plupart des styles et du code front-end sont effectués par un développeur front-end dédié dont l’accès à un environnement AEM n’est pas toujours évident. Cette technique permet également au FED d&#39;apporter des modifications directement au HTML, qui peut ensuite être transmis à un développeur ou une développeuse AEM pour être implémenté en tant que composants.

1. Copiez la source de la page de l’article LA skatepark à l’adresse [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Ouvrez à nouveau votre IDE. Collez la balise copiée d’AEM sur `index.html` dans le module **ui.frontend** sous `src/main/webpack/static`.
1. Modifiez la balise copiée et supprimez toutes les références à **clientlib-site** et **clientlib-dependencies** :

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Supprimez ces références, car le webpack dev server génère automatiquement ces artefacts.

1. Démarrez le webpack dev server à partir d’un nouveau terminal en exécutant la commande suivante depuis le module **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Cela devrait ouvrir une nouvelle fenêtre de navigateur à l’adresse [http://localhost:8080/](http://localhost:8080/) avec les balises statiques.

1. Modifiez le fichier `src/main/webpack/site/_variables.scss`. Remplacez la règle `$text-color` par la suivante :

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Enregistrez les modifications.

1. Les modifications doivent automatiquement être répercutées dans le navigateur sur [http://localhost:8080](http://localhost:8080).

   ![Modifications du webpack dev server local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Consultez le fichier `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contient la configuration webpack utilisée pour lancer le webpack-dev-server. Il envoie par proxy les chemins `/content` et `/etc.clientlibs` à partir d’une instance d’AEM exécutée localement. Voici comment les images et autres bibliothèques clientes (non gérés par le code **ui.frontend**) sont disponibles.

   >[!CAUTION]
   >
   > La source d’image du balisage statique pointe vers un composant d’image dynamique sur une instance AEM locale. Les images apparaissent altérées si le chemin d’accès à l’image change, si AEM n’est pas lancé ou si le navigateur ne s’est pas connecté à l’instance locale AEM. En cas de remise à une ressource externe, il est également possible de remplacer les images par des références statiques.

1. Vous pouvez **arrêter** le webpack server à partir de la ligne de commande en saisissant `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io)** est un outil de ligne de commande Open Source qui peut être utilisé pour accélérer le développement frontal. Il est alimenté par [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://browsersync.io/), et [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

À un niveau élevé, la variable `aemfed` est conçue pour prendre en compte les modifications apportées aux fichiers dans le module **ui.apps** et les synchroniser automatiquement directement avec une instance AEM en cours d’exécution. En fonction des modifications, un navigateur local s’actualise automatiquement, accélérant ainsi le développement frontal. Il est également conçu pour fonctionner avec le traceur de journal Sling afin d’afficher automatiquement les erreurs côté serveur directement dans le terminal.

**aemfed** peut être un outil très utile si vous effectuez beaucoup de travail dans le module **ui.apps**, modifiez des scripts HTL et créez des composants personnalisés. [La documentation complète se trouve ici](https://github.com/abmaonline/aemfed).

### Débogage des bibliothèques côté client {#debugging-clientlibs}

En utilisant différentes méthodes de **catégories** et **d’incorporations** pour intégrer plusieurs bibliothèques clientes, la résolution des problèmes peut s’avérer fastidieuse. AEM propose plusieurs outils pour faciliter cette tâche. L’un des outils les plus intéressant est la **Reconstruction des bibliothèques clientes** qui force AEM à recompiler les fichiers LESS et à générer le code CSS.

* [**Extraction des bibliothèques**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : répertorie les bibliothèques clientes enregistrées dans l’instance AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Test de la sortie**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : permet à un utilisateur ou une utilisatrice d’afficher la sortie HTML prévue des inclusions de bibliothèque cliente en fonction de la catégorie. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validation des dépendances des bibliothèques**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : met en évidence les dépendances ou les catégories incorporées introuvables. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruction des bibliothèques clientes**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : permet à un utilisateur ou une utilisatrice de forcer AEM à reconstruire les bibliothèques clientes ou d’invalider le cache des bibliothèques clientes. Cet outil est efficace lors du développement avec LESS, car il peut forcer AEM à recompiler le code CSS généré. En règle générale, il est plus efficace d’invalider les caches, puis d’effectuer une actualisation de page plutôt que de recréer les bibliothèques. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![Reconstruction de bibliothèque cliente.](assets/client-side-libraries/rebuild-clientlibs.png)
