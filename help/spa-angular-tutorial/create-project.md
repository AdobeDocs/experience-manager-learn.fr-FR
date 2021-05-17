---
title: Projet de l’éditeur SPA | Prise en main de l’éditeur et de l’Angular SPA d’AEM
description: Apprenez à utiliser un projet Adobe Experience Manager (AEM) Maven comme point de départ pour une application d'Angular intégrée à AEM Éditeur.
sub-product: sites
feature: SPA éditeur, AEM Archétype de projet
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 375df47a13b1820911a7ceb73af0dad15c68740e
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 3%

---


# Projet de l&#39;éditeur SPA {#create-project}

Apprenez à utiliser un projet Adobe Experience Manager (AEM) Maven comme point de départ pour une application d&#39;Angular intégrée à AEM Éditeur.

## Intention

1. Comprendre la structure d&#39;un nouveau projet AEM SPA Editor créé à partir d&#39;un archétype Maven.
2. Déployez le projet de démarrage sur une instance locale d’AEM.

## Ce que vous allez créer

Dans ce chapitre, un nouveau projet AEM sera déployé, sur la base de l&#39;[AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Le projet AEM sera amorcé avec un point de départ très simple pour l&#39;SPA de l&#39;Angular. Le projet utilisé dans le présent chapitre servira de base à la mise en oeuvre de la SPA WKND et sera élaboré sur la base des prochains chapitres.

![Projet de démarrage de l&#39;Angular WKND SPA](./assets/create-project/what-you-will-build.png)

*Un message classique de Hello World.*

## Prérequis

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment). Assurez-vous qu’une nouvelle instance d’Adobe Experience Manager, démarrée en mode **auteur**, s’exécute localement.

## Obtenir le projet

Il existe plusieurs options pour créer un projet Maven Multi-module pour AEM. Ce didacticiel a utilisé l&#39;archétype de projet [AEM le plus récent ](https://github.com/adobe/aem-project-archetype) comme base du code du tutoriel. Des modifications ont été apportées au code du projet afin de prendre en charge plusieurs versions des AEM. Veuillez consulter [la note sur la compatibilité ascendante](overview.md#compatibility).

>[!CAUTION]
>
>Il est recommandé d’utiliser la version **la plus récente** de l’[archétype](https://github.com/adobe/aem-project-archetype) pour générer un nouveau projet pour une mise en oeuvre dans le monde réel. Les projets AEM doivent cible une version unique de AEM en utilisant la propriété `aemVersion` de l&#39;archétype.

1. Téléchargez le point de départ de ce didacticiel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. La structure de dossiers et de fichiers suivante représente le projet AEM généré par l&#39;archétype Maven sur le système de fichiers local :

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

3. Les propriétés suivantes ont été utilisées lors de la génération du projet AEM à partir de l&#39;archétype [AEM Project ](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14) :

   | Propriétés | Valeur |
   |-----------------|---------------------------------------|
   | aemVersion | nuage |
   | appTitle | Angular SPA WKND |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | package | com.adobe.aem.guides.wknd.spa.angular |
   | includeExample | n |

   >[!NOTE]
   >
   > Notez la propriété `frontendModule=angular`. Cela indique à l&#39;archétype de projet AEM de démarrer le projet avec une base de code d&#39;Angular [démarrateur ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) à utiliser avec l&#39;AEM Éditeur de SPA.

## Création du projet

Ensuite, compilez, générez et déployez le code du projet sur une instance locale d&#39;AEM à l&#39;aide de Maven.

1. Assurez-vous qu’une instance d’AEM s’exécute localement sur le port **4502**.
2. Depuis le terminal de ligne de commande, vérifiez que Maven est installé :

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Exécutez la commande ci-dessous Maven à partir du répertoire `aem-guides-wknd-spa` pour générer et déployer le projet vers AEM :

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility) :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   Les multiples modules du projet doivent être compilés et déployés en AEM.

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Le profil Maven ***autoInstallSinglePackage*** compile les modules individuels du projet et déploie un package unique sur l’instance AEM. Par défaut, ce package sera déployé sur une instance AEM s’exécutant localement sur le port **4502** et avec les informations d’identification **admin:admin**.

4. Accédez à **[!UICONTROL Package Manager]** sur votre instance d’AEM locale : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Vous devriez voir trois packages pour `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` et `wknd-spa-angular.ui.content`.

   ![Packages WKND SPA](./assets/create-project/package-manager.png)

   Tout le code personnalisé nécessaire au projet sera assemblé dans ces packages et installé sur l&#39;exécution AEM.

6. Vous devriez également voir plusieurs paquets pour `spa.project.core` et `core.wcm.components`. Il s&#39;agit de dépendances automatiquement incluses par l&#39;archétype. Vous trouverez plus d&#39;informations sur [AEM Composants principaux ici](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html).

## Contenu de l’auteur

Ensuite, ouvrez le SPA d’accueil généré par l’archétype et mettez à jour une partie du contenu.

1. Accédez à la console **[!UICONTROL Sites]** : [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   Le SPA WKND comprend une structure de base du site avec un pays, une langue et une page d&#39;accueil. Cette hiérarchie est basée sur les valeurs par défaut de l&#39;archétype pour `language_country` et `isSingleCountryWebsite`. Ces valeurs peuvent être remplacées en mettant à jour les [propriétés disponibles](https://github.com/adobe/aem-project-archetype#available-properties) lors de la génération d&#39;un projet.

2. Ouvrez la page **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** en la sélectionnant et en cliquant sur le bouton **[!UICONTROL Modifier]** dans la barre de menus :

   ![console du site](./assets/create-project/open-home-page.png)

3. Un composant **[!UICONTROL Texte]** a déjà été ajouté à la page. Vous pouvez modifier ce composant comme tout autre composant de l&#39;AEM.

   ![Mettre à jour le composant de texte](./assets/create-project/update-text-component.gif)

4. Ajoutez un autre composant **[!UICONTROL Texte]** à la page.

   Notez que l’expérience de création est similaire à celle d’une page AEM Sites traditionnelle. Actuellement, un nombre limité de composants peuvent être utilisés. D&#39;autres seront ajoutés au cours du tutoriel.

## Inspect pour l’application d’une seule page

Ensuite, vérifiez qu’il s’agit d’une application d’une seule page utilisant les outils de développement de votre navigateur.

1. Dans **[!UICONTROL Éditeur de page]**, cliquez sur le menu **[!UICONTROL Informations sur la page]** > **[!UICONTROL Vue telle que publiée]** :

   ![Vue en tant que bouton Publié](./assets/create-project/view-as-published.png)

   Un nouvel onglet s&#39;ouvre avec le paramètre de requête `?wcmmode=disabled` qui désactive effectivement l&#39;éditeur d&#39;AEM : [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. Vue de la source de la page et notez que le contenu de texte **[!DNL Hello World]** ou tout autre contenu est introuvable. A la place, vous devriez voir du code HTML comme suit :

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` est l’Angular SPA chargé sur la page et responsable du rendu du contenu.

   *D&#39;où vient le contenu ?*

3. Revenir à l’onglet : [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. Ouvrez les outils de développement du navigateur et inspectez le trafic réseau de la page pendant une actualisation. Vue des demandes **XHR** :

   ![Requêtes XHR](./assets/create-project/xhr-requests.png)

   Une demande doit être envoyée à [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Contient tout le contenu, mis en forme dans JSON, qui pilotera le SPA.

5. Dans un nouvel onglet, ouvrez [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   La requête `en.model.json` représente le modèle de contenu qui pilotera l&#39;application. Inspect la sortie JSON et vous devriez être en mesure de trouver le fragment représentant les composants **[!UICONTROL Texte]**.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   Dans le chapitre suivant, nous allons examiner comment le contenu JSON est mappé de Composants AEM aux Composants SPA pour former la base de l’expérience de l’éditeur d’.

   >[!NOTE]
   >
   > Il peut s’avérer utile d’installer une extension de navigateur pour formater automatiquement la sortie JSON.

## Félicitations !  {#congratulations}

Félicitations, vous venez de créer votre premier projet AEM SPA Editor !

C&#39;est assez simple à l&#39;heure actuelle, mais dans les prochains chapitres, d&#39;autres fonctionnalités seront ajoutées.

### Étapes suivantes {#next-steps}

[Intégrer le SPA](integrate-spa.md)  - Découvrez comment le code source SPA est intégré au projet AEM et comprendre les outils disponibles pour développer rapidement le .
