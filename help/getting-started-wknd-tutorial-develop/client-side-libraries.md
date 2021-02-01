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
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '3257'
ht-degree: 4%

---


# Bibliothèques côté client et flux de travail frontal {#client-side-libraries}

Découvrez comment les bibliothèques côté client ou les clientlibs sont utilisés pour déployer et gérer les fichiers CSS et Javascript pour une implémentation de sites Adobe Experience Manager (AEM). Ce didacticiel explique également comment le module [ui.frontend](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/archetype/uifrontend.html), un projet de [webpack](https://webpack.js.org/) découplé, peut être intégré au processus de génération de bout en bout.

## Conditions préalables {#prerequisites}

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

Il est également recommandé de consulter le didacticiel [Concepts de base des composants](component-basics.md#client-side-libraries) pour comprendre les fondamentaux des bibliothèques et des AEM côté client.

### Projet de démarrage

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes permettant d&#39;extraire le projet de démarrage.

Consultez le code de ligne de base sur lequel le didacticiel s&#39;appuie :

1. Consultez la branche `tutorial/client-side-libraries-start` de [GitHub](https://github.com/adobe/aem-guides-wknd).

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Déployez la base de code sur une instance AEM locale en utilisant vos compétences Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` aux commandes Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) ou vérifier le code localement en passant à la branche `tutorial/client-side-libraries-solution`.

## Intention

1. Comprendre comment les bibliothèques côté client sont incluses dans une page via un modèle modifiable.
1. Découvrez comment utiliser le module UI.Frontend et un serveur de développement webpack pour le développement frontal dédié.
1. Comprendre le flux de travail complet de la diffusion de fichiers CSS et JavaScript compilés vers une implémentation de sites.

## Ce que vous allez créer {#what-you-will-build}

Dans ce chapitre, vous allez ajouter quelques styles de ligne de base pour le site WKND et le modèle de page de l’article afin de rapprocher l’implémentation des [modèles de conception de l’interface utilisateur](assets/pages-templates/wknd-article-design.xd). Vous allez utiliser un processus frontal avancé pour intégrer un projet webpack dans une bibliothèque cliente AEM.

![Styles terminés](assets/client-side-libraries/finished-styles.png)

*Page d’article avec des styles de ligne de base appliqués*

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

1. A l’aide de VSCode ou d’un autre IDE, ouvrez le module **ui.apps**.
1. Développez le chemin `/apps/wknd/clientlibs` pour vue des clientlibs générés par l&#39;archétype.

   ![Clientlibs dans ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Nous examinerons ces clientlibs plus en détail ci-dessous.

1. Le tableau suivant récapitule les bibliothèques clientes. Pour plus d&#39;informations sur [l&#39;inclusion des bibliothèques clientes, voir ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nom | Description | Remarques |
   |-------------------| ------------| ------|
   | `clientlib-base` | Niveau de base de CSS et de JavaScript requis pour le fonctionnement du site WKND | incorpore les libs client du composant principal |
   | `clientlib-grid` | Génère le fichier CSS nécessaire pour que [Mode de mise en page](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) fonctionne. | Les points d&#39;arrêt pour tablette/mobile peuvent être configurés ici |
   | `clientlib-site` | Contient un thème spécifique au site pour le site WKND | Généré par le module `ui.frontend` |
   | `clientlib-dependencies` | Incorpore toutes les dépendances tierces | Généré par le module `ui.frontend` |

1. Observez que `clientlib-site` et `clientlib-dependencies` sont ignorés du contrôle de code source. Il s&#39;agit de la conception, car ces éléments seront générés au moment de la création par le module `ui.frontend`.

## Mettre à jour les styles de base {#base-styles}

Ensuite, mettez à jour les styles de base définis dans le module **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**. Les fichiers du module `ui.frontend` génèrent les bibliothèques `clientlib-site` et `clientlib-dependecies` qui contiennent le thème du site et toute dépendance tierce.

Les bibliothèques côté client présentent certaines limites en ce qui concerne la prise en charge de langages tels que [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Il existe un certain nombre d&#39;outils open source tels que [NPM](https://www.npmjs.com/) et [webpack](https://webpack.js.org/) qui accélèrent et optimisent le développement frontal. L&#39;objectif du module **ui.frontend** est de pouvoir utiliser ces outils pour gérer la majorité des fichiers source frontaux.

1. Ouvrez le module **ui.frontend** et accédez à `src/main/webpack/site`.
1. Ouvrez le fichier `main.scss`

   ![main.scss - point d’entrée](assets/client-side-libraries/main-scss.png)

   `main.scss` est le point d&#39;entrée de tous les fichiers Sass du  `ui.frontend` module. Il inclura le fichier `_variables.scss`, qui contient une série de variables de marque à utiliser dans les différents fichiers Sass du projet. Le fichier `_base.scss` est également inclus et définit certains styles de base pour les éléments HTML. Une expression régulière inclut tous les styles pour les styles de composants individuels sous `src/main/webpack/components`. Une autre expression régulière inclut tous les fichiers sous `src/main/webpack/site/styles`.

1. Inspectez le fichier `main.ts`. `main.ts` inclut  `main.scss` et inclut une expression régulière pour collecter tout  `.js` ou  `.ts` fichier du projet. Ce point d&#39;entrée sera utilisé par les [fichiers de configuration webpack](https://webpack.js.org/configuration/) comme point d&#39;entrée pour l&#39;ensemble du module `ui.frontend`.

1. Inspect les fichiers sous `src/main/webpack/site/styles` :

   ![Fichiers de style](assets/client-side-libraries/style-files.png)

   Ces fichiers présentent des styles pour les éléments globaux du modèle, tels que l’en-tête, le pied de page et le conteneur de contenu principal. Les règles CSS de ces fichiers cible différents éléments HTML `header`, `main` et `footer`. Ces éléments HTML ont été définis par des stratégies du chapitre précédent [Pages et modèles](./pages-templates.md).

1. Développez le dossier `components` sous `src/main/webpack` et examinez les fichiers.

   ![Fichiers Sass de composant](assets/client-side-libraries/component-sass-files.png)

   Chaque fichier est mappé à un composant principal tel que le composant [Accordéon](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Chaque composant principal est créé avec la notation [Modificateur d’élément de bloc](https://getbem.com/) ou la notation BEM pour faciliter la cible de classes CSS spécifiques avec des règles de style. Les fichiers sous `/components` ont été supprimés par l&#39;archétype de projet AEM avec les différentes règles BEM pour chaque composant.

1. Téléchargez le fichier WKND Base Styles **[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-src.zip)** et **décompressez** le fichier.

   ![Styles de base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Pour accélérer le didacticiel, nous avons fourni les différents fichiers Sass qui implémentent la marque WKND basée sur les composants principaux et la structure du modèle de page d&#39;article.

1. Remplacez le contenu de `ui.frontend/src` par les fichiers de l’étape précédente. Le contenu du fichier compressé doit remplacer les dossiers suivants :

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![Fichiers modifiés](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect les fichiers modifiés pour afficher les détails de l’implémentation du style WKND.

## Inspect de l’intégration ui.frontend {#ui-frontend-integration}

Un élément d&#39;intégration clé intégré au module **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prend les artefacts CSS et JS compilés d&#39;un projet webpack/npm et les transforme en bibliothèques côté client AEM.

![intégration de l’architecture ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

L&#39;archétype de projet AEM configure automatiquement cette intégration. Ensuite, explorez comment cela fonctionne.


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
   ```

   >[!CAUTION]
   >
   > Vous pouvez recevoir une erreur du type &quot;ERROR in ./src/main/webpack/site/main.scss&quot;.
   > Cela se produit généralement car votre environnement a changé depuis l’exécution de `npm install`.
   > Exécutez `npm rebuild node-sass` pour résoudre le problème. Cela se produira si la version de `npm` installée sur votre machine de développement local diffère de la version utilisée par Maven `frontend-maven-plugin` dans le fichier `aem-guides-wknd/pom.xml`. Vous pouvez résoudre ce problème de manière permanente en modifiant la version du fichier de publication afin qu’elle corresponde à votre version locale ou vice versa.

1. La commande `npm run dev` doit générer et compiler le code source du projet Webpack et, en fin de compte, renseigner les **clientlib-site** et **clientlib-dependencies** dans le module **ui.apps**.

   >[!NOTE]
   >
   >Il existe également un profil `npm run prod` qui minimisera les données JS et CSS. Il s&#39;agit de la compilation standard chaque fois que la compilation webpack est déclenchée via Maven. Vous trouverez plus de détails sur le module [ui.frontend ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect le fichier `site.css` sous `ui.frontend/dist/clientlib-site/css/site.css`. Il s’agit du fichier CSS compilé basé sur les fichiers source Sass.

   ![CSS du site distribué](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspectez le fichier `ui.frontend/clientlib.config.js`. Il s&#39;agit du fichier de configuration d&#39;un module externe npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) qui transforme le contenu de `/dist` en bibliothèque cliente et le déplace vers le module `ui.apps`.

1. Inspect le fichier `site.css` dans le module **ui.apps** à `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Il doit s&#39;agir d&#39;une copie identique du fichier `site.css` du module **ui.frontend**. Maintenant qu&#39;il se trouve dans le module **ui.apps**, il peut être déployé sur AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Puisque **clientlib-site** est compilé pendant la génération, en utilisant **npm** ou **maven**, il peut être ignoré en toute sécurité du contrôle de code source dans le module **ui.apps**. Inspect le fichier `.gitignore` sous **ui.apps**.

1. Synchronisez la bibliothèque `clientlib-site` avec une instance locale d&#39;AEM à l&#39;aide des outils de développement ou des compétences Maven.

   ![Synchroniser le site Clientlib](assets/client-side-libraries/sync-clientlib-site.png)

1. Ouvrez l&#39;article LA Skatepark en AEM à l&#39;adresse : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Styles de base mis à jour pour l’article](assets/client-side-libraries/updated-base-styles.png)

   Vous devriez maintenant voir les styles mis à jour pour l’article. Vous devrez peut-être effectuer une actualisation stricte pour effacer tous les fichiers CSS mis en cache par le navigateur.

   Ça commence à être beaucoup plus proche des maquettes !

   >[!NOTE]
   >
   > Les étapes ci-dessus pour générer et déployer le code ui.frontend vers AEM seront exécutées automatiquement lorsqu&#39;une build Maven est déclenchée à partir de la racine du projet `mvn clean install -PautoInstallSinglePackage`.

>[!CAUTION]
>
> L&#39;utilisation du module **ui.frontend** peut ne pas être nécessaire pour tous les projets. Le module **ui.frontend** ajoute une complexité supplémentaire et s&#39;il n&#39;y a pas besoin/désir d&#39;utiliser certains de ces outils avancés de l&#39;interface (Sass, webpack, npm...), il peut ne pas être nécessaire.

## Inclusion de page et de modèle {#page-inclusion}

Ensuite, examinons comment les clientlibs sont référencés dans la page AEM. Une bonne pratique courante dans le développement Web consiste à inclure CSS dans l’en-tête HTML `<head>` et JavaScript juste avant de fermer la balise `</body>`.

1. Dans le module **ui.apps** accédez à `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Composant de page de structure](assets/client-side-libraries/customheaderlibs-html.png)

   Il s’agit du composant `page` utilisé pour effectuer le rendu de toutes les pages de l’implémentation WKND.

1. Ouvrez le fichier `customheaderlibs.html`. Remarquez les lignes `${clientlib.css @ categories='wknd.base'}`. Cela indique que la page CSS pour clientlib avec une catégorie `wknd.base` sera incluse via ce fichier, incluant en fait **clientlib-base** dans l&#39;en-tête de toutes nos pages.

1. Mettez à jour `customheaderlibs.html` pour inclure une référence aux styles de police Google que nous avons spécifiés plus tôt dans le module **ui.frontend**.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspectez le fichier `customfooterlibs.html`. Ce fichier, tel que `customheaderlibs.html`, doit être remplacé par l’implémentation de projets. Ici, la ligne `${clientlib.js @ categories='wknd.base'}` signifie que le code JavaScript de **clientlib-base** sera inclus au bas de toutes nos pages.

1. Exportez le composant `page` vers le serveur AEM à l’aide des outils de développement ou de vos compétences Maven.

1. Accédez au modèle Page de l’article à l’adresse [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. Cliquez sur l&#39;icône **Informations sur la page** et dans le menu, sélectionnez **Stratégie de page** pour ouvrir la boîte de dialogue **Stratégie de page**.

   ![Stratégie de page Modèle de page d&#39;article](assets/client-side-libraries/template-page-policy.png)

   *Informations sur la page > Stratégie de page*

1. Notez que les catégories pour `wknd.dependencies` et `wknd.site` sont répertoriées ici. Par défaut, les clientlibs configurés via la stratégie de page sont fractionnés afin d’inclure le CSS dans l’en-tête de page et le code JavaScript à l’extrémité du corps. Si vous le souhaitez, vous pouvez explicitement liste que le code JavaScript clientlib soit chargé dans l&#39;en-tête de page. C&#39;est le cas pour `wknd.dependencies`.

   ![Stratégie de page Modèle de page d&#39;article](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Il est également possible de référencer directement `wknd.site` ou `wknd.dependencies` à partir du composant de page, en utilisant le script `customheaderlibs.html` ou `customfooterlibs.html`, comme nous l&#39;avons vu plus tôt pour `wknd.base` clientlib. L’utilisation du modèle offre une certaine flexibilité en ce sens que vous pouvez choisir les clientlibs utilisés par modèle. Par exemple, si vous disposez d’une bibliothèque JavaScript très lourde qui ne sera utilisée que sur un modèle sélectionné.

1. Accédez à la page **LA Skateparks** créée à l&#39;aide du **Modèle de page d&#39;article** : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Vous devriez voir une différence de police.

1. Cliquez sur l&#39;icône **Informations sur la page** et dans le menu, sélectionnez **Vue telle que publiée** pour ouvrir la page de l&#39;article en dehors de l&#39;éditeur AEM.

   ![Afficher comme publié(e) ](assets/client-side-libraries/view-as-published-article-page.png)

1. Vue de la source Page de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) et vous devriez être en mesure de voir les références clientlib suivantes dans le `<head>` :

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

   Notez que les clientlibs utilisent le point de terminaison proxy `/etc.clientlibs`. Vous devriez également voir les inclusions clientlib suivantes en bas de la page :

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >Il est essentiel, du côté de la publication, que les bibliothèques clientes **ne soient pas** servies à partir de **/apps**, car ce chemin d’accès doit être limité pour des raisons de sécurité à l’aide de la section de filtre [Répartiteur](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La propriété [allowProxy](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la bibliothèque cliente garantit que le fichier CSS et JS sont diffusés à partir de **/etc.clientlibs**.

## Webpack DevServer - Marquage statique {#webpack-dev-static}

Au cours des deux exercices précédents, nous avons pu mettre à jour plusieurs fichiers Sass dans le module **ui.frontend** et, par un processus de génération, voir ces changements reflétés dans l&#39;AEM. Ensuite, nous allons examiner une technique qui utilise un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) pour développer rapidement nos styles frontaux par rapport à **le code HTML** statique.

Cette technique est pratique si la majorité des styles et du code frontal sont réalisés par un développeur frontal dédié qui n&#39;a peut-être pas facilement accès à un environnement AEM. Cette technique permet également au FED d&#39;apporter des modifications directement au code HTML, qui peut ensuite être transmis à un développeur AEM pour les implémenter en tant que composants.

1. Copiez la source de la page de l’article de la page de l’article de LA skatepark à l’adresse [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. rouvrez votre IDE. Collez l&#39;annotation copiée de AEM dans le `index.html` du module **ui.frontend** situé sous `src/main/webpack/static`.
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

1. Début du serveur de développement webpack à partir d’un nouveau terminal en exécutant la commande suivante à partir du module **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Cette opération doit ouvrir une nouvelle fenêtre de navigateur à l’adresse [http://localhost:8080/](http://localhost:8080/) avec des balises statiques.

1. Modifiez le fichier `src/main/webpack/site/_variables.scss`. Remplacez la règle `$text-color` par la règle suivante :

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Enregistrez les modifications.

1. Les modifications doivent être automatiquement répercutées dans le navigateur sur [http://localhost:8080](http://localhost:8080).

   ![Modifications du serveur de développement webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Examinez le fichier `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contient la configuration du webpack utilisée pour début du serveur webpack-dev-server. Notez qu’il effectue des proxy entre les chemins `/content` et `/etc.clientlibs` à partir d’une instance d’AEM exécutée localement. C’est ainsi que les images et autres clientlibs (non gérés par le code **ui.frontend**) sont rendues disponibles.

   >[!CAUTION]
   >
   > La source d’image de l’annotation statique pointe vers un composant d’image dynamique sur une instance d’AEM locale. Les images s’affichent rompues si le chemin d’accès à l’image change, si l’AEM n’est pas démarré ou si le navigateur ne s’est pas connecté à l’instance AEM locale. Si la remise à une ressource externe est possible, il est également possible de remplacer les images par des références statiques.

1. Vous pouvez **arrêter** le serveur webpack à partir de la ligne de commande en saisissant `CTRL+C`.

## Webpack DevServer - Watch and aemsync {#webpack-dev-watch}

Une autre technique consiste à demander à Node.js de surveiller les modifications apportées aux fichiers src dans le module `ui.frontend`. Chaque fois qu&#39;un fichier change, il compile rapidement la bibliothèque cliente et utilise le module [aemsync](https://www.npmjs.com/package/aemsync) npm pour synchroniser les modifications sur un serveur AEM en cours d&#39;exécution.

1. Début du serveur de développement webpack en mode **watch** depuis un nouveau terminal en exécutant la commande suivante depuis le module **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. Cette opération compile les fichiers `src` et synchronise les modifications avec l&#39;AEM à l&#39;adresse [http://localhost:4502](http://localhost:4502).

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

1. Accédez à AEM et à l&#39;article LA Skateparks : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![Modifications déployées sur AEM](assets/client-side-libraries/changes-deployed-aem-watch.png)

   Les modifications doivent être déployées en AEM. Il y a un léger retard et vous devrez actualiser le navigateur manuellement pour voir les mises à jour. Toutefois, l’affichage direct des modifications dans AEM est bénéfique si vous travaillez avec de nouveaux composants et avec la création de boîtes de dialogue.

1. Rétablissez la modification sur `_variables.scss` et enregistrez les modifications. Les modifications doivent à nouveau être synchronisées avec l&#39;instance locale de l&#39;AEM après un léger retard.

1. Arrêtez le serveur de développement webpack et effectuez une compilation Maven complète à partir de la racine du projet :

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Là encore, le module `ui.frontend` est compilé, transformé en bibliothèques clientes et déployé en AEM via le module `ui.apps`. Mais cette fois, Maven nous fait tout pour nous.

## Félicitations! {#congratulations}

Félicitations, la page d&#39;article a maintenant quelques styles cohérents qui correspondent à la marque WKND et vous êtes devenu familier avec le module **ui.frontend** !

### Étapes suivantes {#next-steps}

Découvrez comment mettre en oeuvre des styles individuels et réutiliser les composants principaux à l’aide de Experience Manager Style System. [Le développement avec le ](style-system.md) système de style couvre l’utilisation du système de style pour étendre les composants principaux avec des CSS propres à la marque et des configurations de stratégie avancées de l’éditeur de modèles.

Vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur la brach Git `tutorial/client-side-libraries-solution`.

1. Cloner le référentiel [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `tutorial/client-side-libraries-solution`.

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
