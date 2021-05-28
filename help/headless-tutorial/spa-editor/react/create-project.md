---
title: Créer un projet | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment générer un projet Maven Adobe Experience Manager (AEM) comme point de départ d’une application React intégrée à AEM Editor.
sub-product: sites
feature: Éditeur SPA, AEM Archétype de projet
version: cloud-service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 3%

---


# Créer un projet {#spa-editor-project}

Découvrez comment générer un projet Maven Adobe Experience Manager (AEM) comme point de départ d’une application React intégrée à AEM Editor.

## Intention

1. Générez un projet activé pour l’éditeur de SPA à l’aide de l’archétype de projet AEM.
2. Déployez le projet de démarrage sur une instance locale d’AEM.

## Ce que vous allez créer {#what-build}

Dans ce chapitre, un nouveau projet AEM sera généré, basé sur l’[AEM archétype de projet](https://github.com/adobe/aem-project-archetype). Le projet AEM sera démarré avec un point de départ très simple pour le SPA React.

**Qu’est-ce qu’un projet Maven ?** -  [Apache ](https://maven.apache.org/) Mavenis est un outil de gestion logicielle pour la création de projets. *Toutes les mises en oeuvre d’Adobe Experience* Manager utilisent des projets Maven pour créer, gérer et déployer du code personnalisé en plus des AEM.

**Qu’est-ce qu’un archétype Maven ?** - Un  [ ](https://maven.apache.org/archetype/index.html) archétype Maven est un modèle ou un modèle permettant de générer de nouveaux projets. L’archétype de projet AEM nous permet de générer un nouveau projet avec un espace de noms personnalisé et d’inclure une structure de projet qui suit les bonnes pratiques, ce qui accélère considérablement notre projet.

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment). Assurez-vous qu’une nouvelle instance d’Adobe Experience Manager, démarrée en mode **author**, s’exécute localement.

## Création du projet {#create}

>[!NOTE]
>
>Ce tutoriel utilise la version **27** de l’archétype. Il est toujours recommandé d’utiliser la version **la plus récente** de l’archétype pour générer un nouveau projet.

1. Ouvrez un terminal de ligne de commande et saisissez la commande Maven suivante :

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=27 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Si le ciblage AEM 6.5.5+, remplacez `aemVersion="cloud"` par `aemVersion="6.5.5"`. Si le ciblage est version 6.4.8+, utilisez `aemVersion="6.4.8"`.

   Notez la propriété `frontendModule=react` . Cela indique à AEM Project Archetype de démarrer le projet avec une base de code React [démarrée ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) à utiliser avec AEM Editor. Les propriétés telles que `appTitle`, `appId`, `artifactId` et `groupId` sont utilisées pour identifier le projet et l’objectif.

   Une liste complète des propriétés disponibles pour la configuration d’un projet [se trouve ici](https://github.com/adobe/aem-project-archetype#available-properties).

1. La structure de dossiers et de fichiers suivante sera générée par l’archétype Maven sur votre système de fichiers local :

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- all/
       |--- analyse/
       |--- core/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- it.tests/
       |--- dispatcher/
       |--- analyse/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
   ```

   Chaque dossier représente un module Maven individuel. Dans ce tutoriel, nous allons principalement travailler avec le module `ui.frontend`, qui est l’application React. Vous trouverez plus d’informations sur les modules individuels dans la [documentation AEM Project Archetype ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## Déployer et créer le projet

Ensuite, compilez, compilez et déployez le code du projet sur une instance locale d’AEM à l’aide de Maven.

1. Vérifiez qu’une instance d’AEM s’exécute localement sur le port **4502**.
1. À partir de la ligne de commande, accédez au répertoire du projet `aem-guides-wknd-spa.react`.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. Exécutez la commande suivante pour créer et déployer l’ensemble du projet vers AEM :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Le build prend environ une minute et doit se terminer par le message suivant :

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Le profil Maven `autoInstallSinglePackage` compile les modules individuels du projet et déploie un seul module sur l’instance AEM. Par défaut, ce package sera déployé sur une instance AEM s’exécutant localement sur le port **4502** et avec les informations d’identification `admin:admin`.

1. Accédez à **Package Manager** sur votre instance d’AEM locale : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Vous devriez voir plusieurs packages avec le préfixe `aem-guides-wknd-spa.react`.

   ![Packages de SPA WKND](assets/create-project/package-manager.png)

   *AEM Gestionnaire de modules*

   Tout le code personnalisé nécessaire au projet sera regroupé dans ces packages et installé dans l’environnement AEM.

## Création de contenu

Ouvrez ensuite le SPA de démarrage généré par l’archétype et mettez à jour une partie du contenu.

1. Accédez à la console **Sites** : [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   Le SPA WKND comprend une structure de site de base avec un pays, une langue et une page d’accueil. Cette hiérarchie est basée sur les valeurs par défaut de l’archétype pour `language_country` et `isSingleCountryWebsite`. Ces valeurs peuvent être remplacées en mettant à jour les [propriétés disponibles](https://github.com/adobe/aem-project-archetype#available-properties) lors de la génération d’un projet.

2. Ouvrez la **us** > **en** > **WKND SPA page d’accueil React** en sélectionnant la page et en cliquant sur le bouton **Modifier** dans la barre de menus :

   ![console du site](./assets/create-project/open-home-page.png)

3. Un composant **Texte** a déjà été ajouté à la page. Vous pouvez modifier ce composant comme tout autre composant dans AEM.

   ![Mettre à jour le composant Texte](./assets/create-project/update-text-component.gif)

4. Ajoutez un composant **Texte** supplémentaire à la page.

   Notez que l’expérience de création est similaire à celle d’une page AEM Sites traditionnelle. Actuellement, un nombre limité de composants peuvent être utilisés. D’autres éléments seront ajoutés au cours du tutoriel.

## Inspect de l’application d’une seule page

Vérifiez ensuite qu’il s’agit d’une application d’une seule page qui utilise les outils de développement de votre navigateur.

1. Dans **Éditeur de page**, cliquez sur le bouton **Informations sur la page** > **Afficher comme publié** :

   ![Bouton Afficher comme publié(e)](./assets/create-project/view-as-published.png)

   Un nouvel onglet s’ouvre alors avec le paramètre de requête `?wcmmode=disabled` qui désactive effectivement l’éditeur d’AEM : [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Affichez la source de la page et notez que le contenu textuel **[!DNL Hello World]** ou tout autre contenu est introuvable. Vous devriez plutôt voir du code HTML comme suit :

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
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

[Intégrer un SPA](integrate-spa.md)  : découvrez comment le code source SPA est intégré au projet d’AEM et comprenez les outils disponibles pour développer rapidement le .
