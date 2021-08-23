---
title: Collecte de données de page avec Adobe Analytics
description: Utilisez la couche de données client Adobe orientée événement pour collecter des données sur l’activité des utilisateurs sur un site web créé avec Adobe Experience Manager. Découvrez comment utiliser des règles dans Experience Platform Launch pour écouter ces événements et envoyer des données à une suite de rapports Adobe Analytics.
version: cloud-service
topic: Intégrations
feature: Couche de données client Adobe
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2378'
ht-degree: 4%

---


# Collecte de données de page avec Adobe Analytics

Découvrez comment utiliser les fonctionnalités intégrées de la [couche de données client d’Adobe avec les composants principaux d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr) pour collecter des données sur une page dans Adobe Experience Manager Sites. [Experience Platform Launch et l’extension Adobe Analytics seront utilisés pour créer des règles permettant d’envoyer des données de page à Adobe Analytics.](https://www.adobe.com/experience-platform/launch.html)[](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html)

## Ce que vous allez créer

![Suivi des données de page](assets/collect-data-analytics/analytics-page-data-tracking.png)

Dans ce tutoriel, vous allez déclencher une règle Launch basée sur un événement de la couche de données client Adobe, ajouter des conditions pour le moment où la règle doit être déclenchée et envoyer le **Nom de page** et le **Modèle de page** d’une page AEM à Adobe Analytics.

### Objectifs {#objective}

1. Créez une règle basée sur les événements dans Launch en fonction des modifications apportées à la couche de données.
1. Mappage des propriétés de couche de données de page aux éléments de données dans Launch
1. Collectez les données de page et envoyez-les à Adobe Analytics avec la balise de page vue.

## Prérequis

Les éléments suivants sont requis :

* **Experience Platform** LaunchProperty
* **Adobe de l’identifiant de suite de rapports** Analytics/dev et du serveur de suivi. Consultez la documentation suivante pour [créer une suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Extension de navigateur ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) DebuggerExperience Platform. Captures d’écran de ce tutoriel extraites du navigateur Chrome.
* (Facultatif) AEM Site avec la [couche de données client d’Adobe activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Ce tutoriel utilisera le site public [https://wknd.site/us/en.html](https://wknd.site/us/en.html) mais vous êtes invité à utiliser votre propre site.

>[!NOTE]
>
> Besoin d’aide pour intégrer Launch et votre site AEM ? [Voir cette série](../experience-platform-launch/overview.md) de vidéos.

## Changement d’environnements Launch pour le site WKND

[https://wknd.](https://wknd.site) site est un site public créé à partir d’ [un ](https://github.com/adobe/aem-guides-wknd) projet Open Source conçu comme référence et  [](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) tutoriel pour les mises en oeuvre d’AEM.

Au lieu de configurer un environnement AEM et d’installer la base de code WKND, vous pouvez utiliser le débogueur Experience Platform pour **changer** la [https://wknd.site/](https://wknd.site/) en *votre* propriété Launch. Bien sûr, vous pouvez utiliser votre propre site d’AEM s’il dispose déjà de la [couche de données client d’Adobe activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Connectez-vous à Experience Platform Launch et [créez une propriété Launch](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (le cas échéant).
1. Vérifiez qu’une [bibliothèque Launch initiale a été créée](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) et convertie en [environnement ](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments.html) Launch.
1. Copiez le code incorporé Launch de l’environnement dans lequel votre bibliothèque a été publiée.

   ![Copier le code incorporé Launch](assets/collect-data-analytics/launch-environment-copy.png)

1. Dans votre navigateur, ouvrez un nouvel onglet et accédez à [https://wknd.site/](https://wknd.site/)
1. Ouvrez l’extension de navigateur du débogueur Experience Platform.

   ![Débogueur Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Accédez à **Launch** > **Configuration** et sous **Codes incorporés injectés** remplacez le code incorporé Launch existant par *votre* code incorporé copié à partir de l’étape 3.

   ![Remplacer le code incorporé](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Activez **Journalisation de la console** et **Verrouiller** le débogueur dans l’onglet WKND.

   ![Journalisation de la console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Vérifier la couche de données client Adobe sur le site WKND

Le [projet de référence WKND](https://github.com/adobe/aem-guides-wknd) est créé avec les composants principaux d’AEM et la [couche de données client d’Adobe est activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) par défaut. Vérifiez ensuite que la couche de données client d’Adobe est activée.

1. Accédez à [https://wknd.site](https://wknd.site).
1. Ouvrez les outils de développement du navigateur et accédez à la **console**. Exécutez la commande suivante :

   ```js
   adobeDataLayer.getState();
   ```

   Cette opération renvoie l’état actuel de la couche de données client Adobe.

   ![Adobe de l’état de la couche de données](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Développez la réponse et examinez l’entrée `page`. Vous devriez voir un schéma de données comme suit :

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Nous utiliserons les propriétés standard dérivées du [schéma de page](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page), `dc:title`, `xdm:language` et `xdm:template` de la couche de données pour envoyer les données de page à Adobe Analytics.

   >[!NOTE]
   >
   > Vous ne voyez pas l’objet JavaScript `adobeDataLayer` ? Vérifiez que la [couche de données client d’Adobe a été activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) sur votre site.

## Créer une règle Page chargée

La couche de données client Adobe est une couche de données pilotée par **event**. Lorsque la couche de données **Page** d’AEM est chargée, elle déclenche un événement `cmp:show`. Créez une règle qui sera déclenchée en fonction de l’événement `cmp:show` .

1. Accédez à Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez à la section **Règles** de l’interface utilisateur de Launch, puis cliquez sur **Créer une règle**.

   ![Créer une règle](assets/collect-data-analytics/analytics-create-rule.png)

1. Nommez la règle **Page chargée**.
1. Cliquez sur **Événements** **Ajouter** pour ouvrir l’assistant **Configuration des événements**.
1. Sous **Type d’événement**, sélectionnez **Code personnalisé**.

   ![Attribuez un nom à la règle et ajoutez l’événement de code personnalisé](assets/collect-data-analytics/custom-code-event.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Launch Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   Le fragment de code ci-dessus ajoute un écouteur d’événement en [insérant une fonction](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) dans la couche de données. Lorsque l’événement `cmp:show` est déclenché, la fonction `pageShownEventHandler` est appelée. Dans cette fonction, quelques contrôles d’intégrité sont ajoutés et un nouveau `event` est créé avec le dernier [état de la couche de données](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) pour le composant qui a déclenché l’événement.

   Ensuite, `trigger(event)` est appelé. `trigger()` est un nom réservé dans Launch qui &quot;déclenche&quot; la règle Launch. Nous transmettons l’objet `event` en tant que paramètre qui sera à son tour exposé par un autre nom réservé dans Launch nommé `event`. Les éléments de données de Launch peuvent désormais référencer diverses propriétés comme suit : `event.component['someKey']`.

1. Enregistrez les modifications.
1. Ensuite, sous **Actions** cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration de l’action**.
1. Sous **Type d’action**, sélectionnez **Code personnalisé**.

   ![Type d’action Custom Code](assets/collect-data-analytics/action-custom-code.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   L’objet `event` est transmis à partir de la méthode `trigger()` appelée dans l’événement personnalisé. `component` est la page active dérivée de la couche de données  `getState` dans l’événement personnalisé. Rappelez-vous de la section [Schéma de page](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) exposée par la couche de données afin de voir les différentes clés exposées en dehors de la zone.

1. Enregistrez les modifications et exécutez un [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) dans Launch pour convertir le code en [environnement](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments.html) utilisé sur votre site AEM.

   >[!NOTE]
   >
   > Il peut s’avérer très utile d’utiliser le [débogueur Adobe Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) pour passer le code incorporé à un environnement **de développement**.

1. Accédez à votre site AEM et ouvrez les outils de développement pour afficher la console. Actualisez la page. Vous devriez constater que les messages de la console ont été enregistrés :

   ![Messages de la console chargés en page](assets/collect-data-analytics/page-show-event-console.png)

## Création d’éléments de données

Créez ensuite plusieurs éléments de données pour capturer différentes valeurs de la couche de données client Adobe. Comme nous l’avons vu dans l’exercice précédent, il est possible d’accéder aux propriétés de la couche de données directement par le biais du code personnalisé. L’avantage de l’utilisation des éléments de données est qu’ils peuvent être réutilisés dans les règles de Launch.

Rappelez-vous d’avant le [Schéma de page](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) exposé par la couche de données :

Les éléments de données seront mappés aux propriétés `@type`, `dc:title` et `xdm:template`.

### Type de ressource de composant

1. Accédez à Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez à la section **Éléments de données** et cliquez sur **Créer un élément de données**.
1. Pour **Nom**, saisissez **Type de ressource de composant**.
1. Pour **Type d’élément de données**, sélectionnez **Code personnalisé**.

   ![Type de ressource de composant](assets/collect-data-analytics/component-resource-type-form.png)

1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Enregistrez les modifications.

   >[!NOTE]
   >
   > N’oubliez pas que l’objet `event` est rendu disponible et défini sur la portée en fonction de l’événement qui a déclenché la **règle** dans Launch. La valeur d’un élément de données n’est pas définie tant que l’élément de données n’est pas *référencé* dans une règle. Par conséquent, il est sans risque d’utiliser cet élément de données à l’intérieur d’une règle comme la règle **Page chargée** créée à l’étape précédente *mais* ne serait pas utilisable en toute sécurité dans d’autres contextes.

### Nom de page

1. Cliquez sur **Ajouter un élément de données**.
1. Pour **Nom**, saisissez **Nom de page**.
1. Pour **Type d’élément de données**, sélectionnez **Code personnalisé**.
1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Enregistrez les modifications.

### Modèle de page

1. Cliquez sur **Ajouter un élément de données**.
1. Pour **Nom**, saisissez **Modèle de page**.
1. Pour **Type d’élément de données**, sélectionnez **Code personnalisé**.
1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Enregistrez les modifications.

1. Vous devez maintenant avoir trois éléments de données dans votre règle :

   ![Éléments de données dans la règle](assets/collect-data-analytics/data-elements-page-rule.png)

## Ajout de l’extension Analytics

Ajoutez ensuite l’extension Analytics à votre propriété Launch. Nous devons envoyer ces données quelque part !

1. Accédez à Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez à **Extensions** > **Catalogue**
1. Recherchez l’extension **Adobe Analytics** et cliquez sur **Installer**.

   ![Extension Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Sous **Gestion des bibliothèques** > **Suites de rapports**, saisissez les identifiants de suite de rapports que vous souhaitez utiliser avec chaque environnement Launch.

   ![Saisie des identifiants de suite de rapports](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Vous pouvez utiliser une suite de rapports pour tous les environnements dans ce tutoriel, mais dans la vie réelle, vous souhaiterez utiliser des suites de rapports distinctes, comme illustré ci-dessous.

   >[!TIP]
   >
   >Nous vous recommandons d’utiliser l’option *Gérer la bibliothèque pour moi* comme paramètre de gestion des bibliothèques, car cela facilite la mise à jour de la bibliothèque `AppMeasurement.js`.

1. Cochez la case pour activer **Utiliser Activity Map**.

   ![Activer Use Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. Sous **Général** > **Serveur de suivi**, saisissez votre serveur de suivi, par exemple `tmd.sc.omtrdc.net`. Entrez votre serveur de suivi SSL si votre site prend en charge `https://`

   ![Renseignez les serveurs de tracking](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Cliquez sur **Enregistrer** pour enregistrer les modifications.

## Ajouter une condition à la règle Page chargée

Ensuite, mettez à jour la règle **Page chargée** pour utiliser l’élément de données **Type de ressource de composant** afin de vous assurer que la règle ne se déclenche que lorsque l’événement `cmp:show` correspond à la **page**. D’autres composants peuvent déclencher l’événement `cmp:show` ; par exemple, le composant Carrousel le déclenche lorsque les diapositives changent. Il est donc important d’ajouter une condition pour cette règle.

1. Dans l’interface utilisateur de Launch, accédez à la règle **Page chargée** créée précédemment.
1. Sous **Conditions** cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration de condition**.
1. Pour **Type de condition**, sélectionnez **Comparaison de valeurs**.
1. Définissez la première valeur du champ de formulaire sur `%Component Resource Type%`. Vous pouvez utiliser l’icône d’élément de données ![icône d’élément de données](assets/collect-data-analytics/cylinder-icon.png) pour sélectionner l’élément de données **Type de ressource de composant**. Laissez le comparateur défini sur `Equals`.
1. Définissez la seconde valeur sur `wknd/components/page`.

   ![Configuration de condition pour la règle chargée sur la page](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Il est possible d’ajouter cette condition dans la fonction de code personnalisé qui écoute l’événement `cmp:show` créé précédemment dans le tutoriel. Toutefois, l’ajout de cette règle dans l’interface utilisateur offre une plus grande visibilité aux utilisateurs supplémentaires qui peuvent avoir besoin d’apporter des modifications à la règle. De plus, nous pouvons utiliser notre élément de données .

1. Enregistrez les modifications.

## Définition des variables Analytics et déclenchement de la balise Page vue

Actuellement, la règle **Page chargée** génère simplement une instruction de console. Ensuite, utilisez les éléments de données et l’extension Analytics pour définir les variables Analytics comme une **action** dans la règle **Page chargée**. Nous définirons également une action supplémentaire pour déclencher la **balise de page vue** et envoyer les données collectées à Adobe Analytics.

1. Dans la règle **Page chargée** **remove** l’action **Core - Code personnalisé** (instructions de la console) :

   ![Suppression d’une action de code personnalisé](assets/collect-data-analytics/remove-console-statements.png)

1. Sous Actions, cliquez sur **Ajouter** pour ajouter une nouvelle action.
1. Définissez le type **Extension** sur **Adobe Analytics** et définissez le **Type d’action** sur **Définir des variables**.

   ![Définition de l’extension d’action sur Analytics Set Variables](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Dans le panneau principal, sélectionnez un **eVar** disponible et définissez comme valeur de l’élément de données **Modèle de page**. Utilisez l’icône Éléments de données ![Icône Éléments de données](assets/collect-data-analytics/cylinder-icon.png) pour sélectionner l’élément **Modèle de page**.

   ![Définir comme modèle de page d’eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Faites défiler l’écran vers le bas, sous **Paramètres supplémentaires** et définissez **Nom de page** sur l’élément de données **Nom de page** :

   ![Jeu de variables d’environnement de nom de page](assets/collect-data-analytics/page-name-env-variable-set.png)

   Enregistrez les modifications.

1. Ajoutez ensuite une action supplémentaire à droite de **Adobe Analytics - Set Variables** en appuyant sur l’icône **plus** :

   ![Ajout d’une action de lancement supplémentaire](assets/collect-data-analytics/add-additional-launch-action.png)

1. Définissez le type **Extension** sur **Adobe Analytics** et définissez le **Type d’action** sur **Envoyer la balise**. Puisqu’il est considéré comme une page vue, laissez le suivi par défaut défini sur **`s.t()`**.

   ![Action Envoyer la balise Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Enregistrez les modifications. La règle **Page chargée** doit maintenant avoir la configuration suivante :

   ![Configuration finale du lancement](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Prêtez attention à l’ `cmp:show` événement.
   * **2.** Vérifiez que l’événement a été déclenché par une page.
   * **3.** Définition de variables Analytics pour le nom de  **page** et le modèle de  **page**
   * **4.** Envoyer la balise de page vue Analytics
1. Enregistrez toutes les modifications et créez votre bibliothèque Launch, en effectuant la promotion vers l’environnement approprié.

## Validation de la balise de page vue et de l’appel Analytics

Maintenant que la règle **Page chargée** envoie la balise Analytics, vous devriez être en mesure de voir les variables de suivi Analytics à l’aide du débogueur Experience Platform.

1. Ouvrez le [site WKND](https://wknd.site/us/en.html) dans votre navigateur.
1. Cliquez sur l’icône Debugger ![Icône Debugger Experience Platform](assets/collect-data-analytics/experience-cloud-debugger.png) pour ouvrir Debugger Experience Platform.
1. Assurez-vous que le débogueur mappe la propriété Launch à *votre environnement de développement*, comme décrit précédemment et que la **journalisation de la console** est cochée.
1. Ouvrez le menu Analytics et vérifiez que la suite de rapports est définie sur *votre* suite de rapports. Le Nom de page doit également être renseigné :

   ![Débogueur de l’onglet Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Faites défiler la page vers le bas et développez **Requêtes réseau**. Vous devriez être en mesure de trouver l’**evar** défini pour le **modèle de page** :

   ![Evar et nom de page définis](assets/collect-data-analytics/evar-page-name-set.png)

1. Revenez au navigateur et ouvrez la console de développement. Cliquez sur **Carrousel** en haut de la page.

   ![Clic sur la page du carrousel](assets/collect-data-analytics/click-carousel-page.png)

1. Observez l’instruction de la console dans la console du navigateur :

   ![Condition non remplie](assets/collect-data-analytics/condition-not-met.png)

   En effet, le carrousel ne déclenche pas d’événement `cmp:show` *mais* en raison de la vérification du **type de ressource de composant**, aucun événement n’est déclenché.

   >[!NOTE]
   >
   > Si vous ne voyez aucun journal de la console, assurez-vous que **Journalisation de la console** est coché sous **Launch** dans le débogueur Experience Platform.

1. Accédez à une page d’article telle que [Ouest de l’Australie](https://wknd.site/us/en/magazine/western-australia.html). Observez que le nom de page et le type de modèle changent.

## Félicitations !

Vous venez d’utiliser la couche de données client et l’Experience Platform Launch Adobe pilotés par les événements pour collecter les données de page de données d’un site AEM et les envoyer à Adobe Analytics.

### Étapes suivantes

Consultez le tutoriel suivant pour savoir comment utiliser la couche de données client Adobe orientée événement pour [suivre les clics de composants spécifiques sur un site Adobe Experience Manager](track-clicked-component.md).
