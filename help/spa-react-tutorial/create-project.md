---
title: SPA Editor Project | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment utiliser un projet Maven Adobe Experience Manager (AEM) comme point de départ d’une application React intégrée à AEM Editor.
sub-product: sites
feature: Éditeur SPA, AEM Archétype de projet
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 375df47a13b1820911a7ceb73af0dad15c68740e
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 3%

---


# SPA Éditeur de projet {#spa-editor-project}

Découvrez comment utiliser un projet Maven Adobe Experience Manager (AEM) comme point de départ d’une application React intégrée à AEM Editor.

## Intention

1. Découvrez la structure d’un nouveau projet AEM SPA Editor créé à partir d’un archétype Maven.
2. Déployez le projet de démarrage sur une instance locale d’AEM.

## Ce que vous allez créer

Dans ce chapitre, un nouveau projet AEM sera déployé, selon l’[archétype de projet AEM](https://github.com/adobe/aem-project-archetype). Le projet AEM sera démarré avec un point de départ très simple pour le SPA React. Le projet utilisé dans ce chapitre servira de base à une mise en oeuvre de la SPA WKND et sera élaboré dans les prochains chapitres.

![Projet de démarrage de WKND SPA React](./assets/create-project/wknd-spa-react.png)

*Hiérarchie du site de départ pour le SPA WKND.*

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment). Assurez-vous qu’une nouvelle instance d’Adobe Experience Manager, démarrée en mode **author**, s’exécute localement.

## Obtention du projet

Il existe plusieurs options pour créer un projet Maven multi-module pour AEM. Ce tutoriel utilisait la dernière version [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) comme base du code du tutoriel. Des modifications ont été apportées au code du projet afin de prendre en charge plusieurs versions d’AEM. Veuillez consulter [la note sur la compatibilité ascendante](overview.md#compatibility).

>[!CAUTION]
>
> Il est recommandé d’utiliser la version **la plus récente** de l’[archétype](https://github.com/adobe/aem-project-archetype) pour générer un nouveau projet pour une mise en oeuvre concrète. Les projets AEM doivent cibler une seule version d’AEM à l’aide de la propriété `aemVersion` de l’archétype.

1. Téléchargez le point de départ de ce tutoriel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/create-project-start
   ```

2. La structure de dossiers et de fichiers suivante représente le projet AEM généré par l’archétype Maven sur le système de fichiers local :

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. Les propriétés suivantes ont été utilisées lors de la génération du projet AEM à partir de l’[archétype de projet AEM](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14) :

   | Propriétés | Valeur |
   |-----------------|-------------------------------------|
   | aemVersion | nuage |
   | appTitle | WKND SPA React |
   | appId | wknd-spa-response |
   | groupId | com.adobe.aem.guides |
   | frontendModule | response |
   | package | com.adobe.aem.guides.wknd.spa.react |
   | includeExamples | n |

   >[!NOTE]
   >
   > Notez la propriété `frontendModule=react` . Cela indique à AEM Project Archetype de démarrer le projet avec une base de code React [démarrée ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) à utiliser avec AEM Editor.

## Création du projet

Ensuite, compilez, compilez et déployez le code du projet sur une instance locale d’AEM à l’aide de Maven.

1. Vérifiez qu’une instance d’AEM s’exécute localement sur le port **4502**.
2. Depuis le terminal de ligne de commande, vérifiez que Maven est installé :

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Exécutez la commande Maven ci-dessous à partir du répertoire `aem-guides-wknd-spa` pour créer et déployer le projet vers AEM :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility) :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   Les multiples modules du projet doivent être compilés et déployés sur AEM.

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-react 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-react ..................................... SUCCESS [  0.523 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [  8.069 s]
   [INFO] wknd-spa-react.ui.frontend - UI Frontend ........... SUCCESS [01:23 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  0.830 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  4.654 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  1.607 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.384 s]
   [INFO] WKND SPA React - Integration Tests Bundles ......... SUCCESS [  0.770 s]
   [INFO] WKND SPA React - Integration Tests Launcher ........ SUCCESS [  1.407 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.055 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  01:44 min
   ```

   Le profil Maven ***autoInstallSinglePackage*** compile les modules individuels du projet et déploie un seul module sur l’instance AEM. Par défaut, ce package est déployé sur une instance d’AEM s’exécutant localement sur le port **4502** et avec les informations d’identification **admin:admin**.

4. Accédez à **[!UICONTROL Package Manager]** sur votre instance d’AEM locale : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Vous devriez voir trois packages pour `wknd-spa-react.all`, `wknd-spa-react.ui.apps` et `wknd-spa-react.ui.content`.

   ![Packages de SPA WKND](./assets/create-project/package-manager.png)

   *AEM Gestionnaire de modules*

   Tout le code personnalisé nécessaire au projet sera regroupé dans ces modules et installé sur le runtime AEM.

6. Vous devriez également voir plusieurs packages pour `spa.project.core` et `core.wcm.components`. Ces dépendances sont automatiquement incluses par l’archétype. Vous trouverez plus d’informations sur [AEM composants principaux ici](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html).

   `spa.project.core` est une dépendance nécessaire pour générer l’API de modèle JSON attendue par l’éditeur SPA.

## Création de contenu

Ouvrez ensuite le SPA de démarrage généré par l’archétype et mettez à jour une partie du contenu.

1. Accédez à la console **[!UICONTROL Sites]** : [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   Le SPA WKND comprend une structure de site de base avec un pays, une langue et une page d’accueil. Cette hiérarchie est basée sur les valeurs par défaut de l’archétype pour `language_country` et `isSingleCountryWebsite`. Ces valeurs peuvent être remplacées en mettant à jour les [propriétés disponibles](https://github.com/adobe/aem-project-archetype#available-properties) lors de la génération d’un projet.

2. Ouvrez la page **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA React Home Page]** en sélectionnant la page et en cliquant sur le bouton **[!UICONTROL Modifier]** dans la barre de menus :

   ![console du site](./assets/create-project/open-home-page.png)

3. Un composant **[!UICONTROL Texte]** a déjà été ajouté à la page. Vous pouvez modifier ce composant comme tout autre composant dans AEM.

   ![Mettre à jour le composant Texte](./assets/create-project/update-text-component.gif)

4. Ajoutez un composant **[!UICONTROL Texte]** supplémentaire à la page.

   Notez que l’expérience de création est similaire à celle d’une page AEM Sites traditionnelle. Actuellement, un nombre limité de composants peuvent être utilisés. D’autres éléments seront ajoutés au cours du tutoriel.

## Inspect de l’application d’une seule page

Vérifiez ensuite qu’il s’agit d’une application d’une seule page qui utilise les outils de développement de votre navigateur.

1. Dans **[!UICONTROL Éditeur de page]**, cliquez sur le bouton **[!UICONTROL Informations sur la page]** > **[!UICONTROL Afficher comme publié]** :

   ![Bouton Afficher comme publié(e)](./assets/create-project/view-as-published.png)

   Un nouvel onglet s’ouvre alors avec le paramètre de requête `?wcmmode=disabled` qui désactive effectivement l’éditeur d’AEM : [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Affichez la source de la page et notez que le contenu textuel **[!DNL Hello World]** ou tout autre contenu est introuvable. Vous devriez plutôt voir du code HTML comme suit :

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` est le SPA React chargé sur la page et responsable du rendu du contenu.

   Cependant, *d’où vient le contenu ?*

3. Revenez à l’onglet : [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Ouvrez les outils de développement du navigateur et examinez le trafic réseau de la page lors d’une actualisation. Affichez les requêtes **XHR** :

   ![Requêtes XHR](./assets/create-project/xhr-requests.png)

   Une demande doit être envoyée à [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Contient tout le contenu, au format JSON, qui pilotera le SPA.

5. Dans un nouvel onglet, ouvrez [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   La requête `en.model.json` représente le modèle de contenu qui pilotera l’application. Inspect de la sortie JSON et vous devriez être en mesure de trouver le fragment de code représentant le ou les composants **[!UICONTROL Texte]**.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   Dans le chapitre suivant, nous allons examiner la manière dont ce contenu JSON est mappé des composants AEM aux composants SPA pour former la base de l’expérience de l’éditeur d’.

   >[!NOTE]
   >
   > Il peut s’avérer utile d’installer une extension de navigateur pour formater automatiquement la sortie JSON.

## Félicitations !  {#congratulations}

Félicitations, vous venez de créer votre premier projet SPA Éditeur d&#39;AEM !

La SPA est assez simple. Dans les prochains chapitres, d’autres fonctionnalités seront ajoutées.

### Étapes suivantes {#next-steps}

[Intégrer le SPA](integrate-spa.md)  : découvrez comment le code source SPA est intégré au projet d’AEM et comprenez les outils disponibles pour développer rapidement le .
