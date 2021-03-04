---
title: Collecte de données de page avec Adobe Analytics
description: Utilisez la couche de données client Adobe pilotée par le événement pour collecter des données sur l’activité des utilisateurs sur un site Web créé avec Adobe Experience Manager. Découvrez comment utiliser des règles dans l’Experience Platform Launch pour écouter ces événements et envoyer des données à une suite de rapports Adobe Analytics.
feature: analyses
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
topic: Intégrations
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2419'
ht-degree: 5%

---


# Collecte de données de page avec Adobe Analytics

Découvrez comment utiliser les fonctionnalités intégrées de la couche de données client [Adobe avec les composants principaux de l&#39;AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/data-layer/overview.html) pour collecter des données sur une page dans Adobe Experience Manager Sites. [Experience Platform Launch et l’extension Adobe Analytics seront utilisés pour créer des règles permettant d’envoyer des données de page à Adobe Analytics.](https://www.adobe.com/experience-platform/launch.html)[](https://docs.adobe.com/content/help/fr-FR/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)

## Ce que vous allez créer

![Suivi des données de page](assets/collect-data-analytics/analytics-page-data-tracking.png)

Dans ce didacticiel, vous allez déclencher une règle de lancement basée sur un événement de la couche de données du client Adobe, ajouter des conditions pour le moment où la règle doit être déclenchée et envoyer les **Nom de page** et **Modèle de page** d&#39;une page AEM à Adobe Analytics.

### Objectifs {#objective}

1. Créez une règle générée par un événement dans Lancement en fonction des modifications apportées à la couche de données.
1. Faire correspondre les propriétés de couche de données de page aux éléments de données au lancement
1. Collecte des données de page et envoi à l’Adobe Analytics avec la balise de vue de page

## Conditions préalables

Les éléments suivants sont requis :

* **Experience Platform** LaunchProperty
* **Adobe** Analytics/dev report suite ID et serveur de suivi. Consultez la documentation suivante pour [créer une nouvelle suite de rapports](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Extension ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) du navigateur de débogage Experience Platform. Captures d’écran de ce didacticiel capturées dans le navigateur Chrome.
* (Facultatif) Site AEM avec la couche de données du client [Adobe activée](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Ce tutoriel utilisera le site accessible au public [https://wknd.site/us/en.html](https://wknd.site/us/en.html) mais vous êtes invité à utiliser votre propre site.

>[!NOTE]
>
> Besoin d&#39;aide pour intégrer Launch et votre site AEM ? [Voir cette série](../experience-platform-launch/overview.md) de vidéos.

## Changer les Environnements de lancement pour le site WKND

[https://wknd.](https://wknd.site) site est un site accessible au public créé à partir  [d’un ](https://github.com/adobe/aem-guides-wknd) projet open source conçu comme référence et  [](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) didacticiel pour les implémentations AEM.

Au lieu de configurer un environnement AEM et d’installer la base de code WKND, vous pouvez utiliser le débogueur Experience Platform pour **basculer** la [https://wknd.site/](https://wknd.site/) en *votre* propriété Launch. Bien sûr, vous pouvez utiliser votre propre site AEM s&#39;il a déjà activé la [couche de données du client d&#39;Adobe ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Connectez-vous à l’Experience Platform Launch et [créez une propriété Launch](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (si vous ne l’avez pas déjà fait).
1. Assurez-vous qu’une bibliothèque de lancement [initiale a été créée](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library) et promue en un environnement de lancement [](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html).
1. Copiez le code incorporé de lancement de l’environnement dans lequel votre bibliothèque a été publiée.

   ![Copier le code incorporé de lancement](assets/collect-data-analytics/launch-environment-copy.png)

1. Dans votre navigateur, ouvrez un nouvel onglet et accédez à [https://wknd.site/](https://wknd.site/)
1. Ouvrez l’extension de navigateur du débogueur Experience Platform.

   ![Débogueur Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Accédez à **Lancement** > **Configuration** et sous **Codes incorporés injectés** remplacez le code incorporé de lancement existant par *votre* code incorporé copié à l’étape 3.

   ![Remplacer le code incorporé](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Activez **Consignation de la console** et **Verrouiller** le débogueur dans l&#39;onglet WKND.

   ![Journalisation de la console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Vérifier la couche de données du client Adobe sur le site WKND

Le projet de référence [WKND](https://github.com/adobe/aem-guides-wknd) est créé avec AEM composants principaux et la couche de données client [Adobe est activée](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) par défaut. Ensuite, vérifiez que la couche de données du client Adobe est activée.

1. Accédez à [https://wknd.site](https://wknd.site).
1. Ouvrez les outils de développement du navigateur et accédez à la **Console**. Exécutez la commande suivante :

   ```js
   adobeDataLayer.getState();
   ```

   Cette opération renvoie l&#39;état actuel de la couche de données du client Adobe.

   ![Etat de couche de données d&#39;Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Développez la réponse et examinez l&#39;entrée `page`. Un schéma de données doit s’afficher comme suit :

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

   Nous utiliserons les propriétés standard dérivées du [schéma de page](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page), `dc:title`, `xdm:language` et `xdm:template` de la couche de données pour envoyer les données de page à Adobe Analytics.

   >[!NOTE]
   >
   > Vous ne voyez pas l’objet javascript `adobeDataLayer` ? Assurez-vous que la couche de données client [Adobe a été activée](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) sur votre site.

## Créer une règle chargée de page

La couche de données client de l&#39;Adobe est une couche de données pilotée **événement**. Lorsque la couche de données AEM **Page** est chargée, elle déclenche un événement `cmp:show`. Créez une règle qui sera déclenchée en fonction du événement `cmp:show`.

1. Accédez à l&#39;Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez à la section **Règles** de l’interface utilisateur de lancement, puis cliquez sur **Créer une règle**.

   ![Créer une règle](assets/collect-data-analytics/analytics-create-rule.png)

1. Nommez la règle **Page chargée**.
1. Cliquez sur **Événements** **Ajouter** pour ouvrir l&#39;Assistant **Configuration de Événement**.
1. Sous **Type d&#39;événement** sélectionnez **Code personnalisé**.

   ![Nommez la règle et ajoutez le événement de code personnalisé.](assets/collect-data-analytics/custom-code-event.png)

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

   Le fragment de code ci-dessus ajoute un écouteur de événement en [poussant une fonction](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) dans la couche de données. Lorsque le événement `cmp:show` est déclenché, la fonction `pageShownEventHandler` est appelée. Dans cette fonction, quelques vérifications d&#39;intégrité sont ajoutées et un nouveau `event` est construit avec l&#39;état le plus récent [de la couche de données](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) pour le composant qui a déclenché le événement.

   Ensuite, `trigger(event)` est appelé. `trigger()` est un nom réservé dans Lancement et déclenche la règle de lancement. Nous transmettons l&#39;objet `event` en tant que paramètre qui sera à son tour exposé par un autre nom réservé dans Lancement nommé `event`. Les éléments de données du lancement peuvent désormais référencer diverses propriétés telles que : `event.component['someKey']`.

1. Enregistrez les modifications.
1. Ensuite, sous **Actions**, cliquez sur **Ajouter** pour ouvrir l&#39;Assistant **Configuration de l&#39;action**.
1. Sous **Type d&#39;action**, choisissez **Code personnalisé**.

   ![Type d’action Code personnalisé](assets/collect-data-analytics/action-custom-code.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   L&#39;objet `event` est transmis à partir de la méthode `trigger()` appelée dans le événement personnalisé. `component` est la page active dérivée de la couche de données  `getState` dans le événement personnalisé. Rappelez-vous du [schéma de page](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) exposé par la couche de données afin de voir les différentes clés exposées hors de la boîte.

1. Enregistrez les modifications et exécutez une [build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) dans Lancement pour promouvoir le code dans l&#39;environnement [](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) utilisé sur votre site AEM.

   >[!NOTE]
   >
   > Il peut s’avérer très utile d’utiliser le [débogueur de Adobe Experience Platform](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) pour passer le code incorporé à un environnement **Développement**.

1. Accédez à votre site AEM et ouvrez les outils de développement pour vue à la console. Actualisez la page et vous devriez voir que les messages de la console ont été enregistrés :

   ![Messages de la console chargés en page](assets/collect-data-analytics/page-show-event-console.png)

## Créer des éléments de données

Créez ensuite plusieurs éléments de données pour capturer différentes valeurs de la couche de données du client Adobe. Comme nous l&#39;avons vu dans l&#39;exercice précédent, il est possible d&#39;accéder aux propriétés de la couche de données directement par le biais du code personnalisé. L’avantage de l’utilisation des éléments de données est qu’ils peuvent être réutilisés dans les règles de lancement.

Rappelez d&#39;avant le [schéma de page](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) exposé par la couche de données :

Les éléments de données seront mappés aux propriétés `@type`, `dc:title` et `xdm:template`.

### Type de ressource composant

1. Accédez à l&#39;Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez à la section **Éléments de données** et cliquez sur **Créer un élément de données**.
1. Pour **Nom**, saisissez **Type de ressource de composant**.
1. Pour **Type d’élément de données**, sélectionnez **Code personnalisé**.

   ![Type de ressource composant](assets/collect-data-analytics/component-resource-type-form.png)

1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Enregistrez les modifications.

   >[!NOTE]
   >
   > Rappelez-vous que l’objet `event` est rendu disponible et que sa portée est définie en fonction du événement qui a déclenché la **règle** dans le lancement. La valeur d’un élément de données n’est pas définie tant que l’élément de données n’est pas *référencé* dans une règle. Par conséquent, il est prudent d’utiliser cet élément de données dans une règle, comme la règle **Chargé de page** créée à l’étape précédente *mais* ne serait pas utilisable en toute sécurité dans d’autres contextes.

### Nom de page

1. Cliquez sur **Ajouter l’élément de données**.
1. Pour **Nom**, saisissez **Nom de la page**.
1. Pour **Type d’élément de données**, sélectionnez **Code personnalisé**.
1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Enregistrez les modifications.

### Modèle de page

1. Cliquez sur **Ajouter l’élément de données**.
1. Pour **Nom**, saisissez **Modèle de page**.
1. Pour **Type d’élément de données**, sélectionnez **Code personnalisé**.
1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Enregistrez les modifications.

1. Vous devez désormais inclure trois éléments de données dans votre règle :

   ![Éléments de données dans la règle](assets/collect-data-analytics/data-elements-page-rule.png)

## Ajouter l’extension Analytics

Ajoutez ensuite l’extension Analytics à votre propriété Launch. Nous devons envoyer ces données quelque part !

1. Accédez à l&#39;Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez à **Extensions** > **Catalogue**
1. Recherchez l’extension **Adobe Analytics** et cliquez sur **Installer**.

   ![Extension Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Sous **Gestion des bibliothèques** > **Report Suites**, entrez les identifiants de suite de rapports que vous souhaitez utiliser avec chaque environnement de lancement.

   ![Entrer les identifiants des suites de rapports](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Vous pouvez utiliser une seule suite de rapports pour tous les environnements de ce didacticiel, mais dans la vie réelle, vous souhaitez utiliser des suites de rapports distinctes, comme le montre l&#39;image ci-dessous.

   >[!TIP]
   >
   >Nous vous recommandons d’utiliser l’option *Gérer la bibliothèque pour moi* comme paramètre Gestion des bibliothèques, car cela facilite considérablement la mise à jour de la bibliothèque `AppMeasurement.js`.

1. Cochez la case pour activer **Utiliser Activity Map**.

   ![Activer le Activity Map d’utilisation](assets/track-clicked-component/analytic-track-click.png)

1. Sous **Général** > **Serveur de suivi**, entrez votre serveur de suivi, par ex. `tmd.sc.omtrdc.net`. Entrez votre serveur de suivi SSL si votre site prend en charge `https://`

   ![Entrer les serveurs de suivi](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Cliquez sur **Enregistrer** pour enregistrer les modifications.

## Ajouter une condition à la règle Page chargée

Ensuite, mettez à jour la règle **Page chargée** pour utiliser l&#39;élément de données **Type de ressource de composant** afin de vous assurer que la règle ne se déclenche que lorsque le événement `cmp:show` correspond à la **page**. D&#39;autres composants peuvent déclencher le événement `cmp:show`, par exemple le composant Carrousel le déclenche lorsque les diapositives changent. Il est donc important d’ajouter une condition pour cette règle.

1. Dans l’interface utilisateur de lancement, accédez à la règle **Page chargée** créée précédemment.
1. Sous **Conditions**, cliquez sur **Ajouter** pour ouvrir l&#39;Assistant **Configuration de condition**.
1. Pour **Type de condition**, sélectionnez **Comparaison des valeurs**.
1. Définissez la première valeur du champ de formulaire sur `%Component Resource Type%`. Vous pouvez utiliser l&#39;icône ![de l&#39;élément de données](assets/collect-data-analytics/cylinder-icon.png) pour sélectionner l&#39;élément de données **Type de ressource de composant**. Conservez la valeur `Equals` pour le comparateur.
1. Définissez la seconde valeur sur `wknd/components/page`.

   ![Configuration de condition pour la règle chargée de page](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Il est possible d’ajouter cette condition dans la fonction de code personnalisé qui écoute le événement `cmp:show` créé précédemment dans le didacticiel. Toutefois, l’ajout de cette variable dans l’interface utilisateur permet une plus grande visibilité aux utilisateurs supplémentaires qui pourraient avoir besoin d’apporter des modifications à la règle. De plus, nous utilisons notre élément de données !

1. Enregistrez les modifications.

## Définition des variables Analytics et déclenchement de la balise de Vue de page

Actuellement, la règle **Page chargée** génère simplement une instruction console. Ensuite, utilisez les éléments de données et l’extension Analytics pour définir les variables Analytics comme une **action** dans la règle **Page chargée**. Nous définirons également une action supplémentaire pour déclencher la **balise de Vue de page** et envoyer les données collectées à Adobe Analytics.

1. Dans la règle **Page chargée** **remove** l&#39;action **Core - Custom Code** (instructions de la console) :

   ![Supprimer l’action de code personnalisé](assets/collect-data-analytics/remove-console-statements.png)

1. Sous Actions, cliquez sur **Ajouter** pour ajouter une nouvelle action.
1. Définissez le type **Extension** sur **Adobe Analytics** et définissez **Type d&#39;action** sur **Définir des variables**.

   ![Définition de l’extension d’action sur Analytics et définition de variables](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Dans le panneau principal, sélectionnez un **eVar** disponible et définissez-le comme valeur de l’élément de données **Modèle de page**. Utilisez l’icône Éléments de données ![Icône Éléments de données](assets/collect-data-analytics/cylinder-icon.png) pour sélectionner l’élément **Modèle de page**.

   ![Définir comme modèle de page eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Faites défiler la page vers le bas, sous **Paramètres supplémentaires** définissez **Nom de page** sur l’élément de données **Nom de page** :

   ![Jeu de variables d&#39;Environnement de noms de page](assets/collect-data-analytics/page-name-env-variable-set.png)

   Enregistrez les modifications.

1. Ensuite, ajoutez une action supplémentaire à droite de **Adobe Analytics - Set Variables** en appuyant sur l&#39;icône **plus** :

   ![Ajouter une action de lancement supplémentaire](assets/collect-data-analytics/add-additional-launch-action.png)

1. Définissez le type **Extension** sur **Adobe Analytics** et définissez **Type d&#39;action** sur **Envoyer la balise**. Comme il s’agit d’une vue de page, laissez le suivi par défaut défini sur **`s.t()`**.

   ![Action Envoyer une balise Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Enregistrez les modifications. La règle **Page chargée** doit maintenant avoir la configuration suivante :

   ![Configuration du lancement final](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Écoute le  `cmp:show` événement.
   * **2.** Vérifiez que le événement a été déclenché par une page.
   * **3.** Définition des variables Analytics pour le  **nom de** page et le modèle de  **page**
   * **4.** Envoyer la balise de Vue de page Analytics
1. Enregistrez toutes les modifications et générez votre bibliothèque de lancement en faisant la promotion vers l’Environnement approprié.

## Validation de la balise de Vue de page et de l’appel Analytics

Maintenant que la règle **Page chargée** envoie la balise Analytics, vous devriez être en mesure d’afficher les variables de suivi Analytics à l’aide du débogueur Experience Platform.

1. Ouvrez le [site WKND](https://wknd.site/us/en.html) dans votre navigateur.
1. Cliquez sur l’icône Débogueur ![Icône Débogueur de plateforme d’expérience](assets/collect-data-analytics/experience-cloud-debugger.png) pour ouvrir le Débogueur Experience Platform.
1. Assurez-vous que le débogueur mappe la propriété Launch sur *votre* environnement de développement, comme décrit précédemment et que **la journalisation de la console** est cochée.
1. Ouvrez le menu Analytics et vérifiez que la suite de rapports est définie sur *votre* suite de rapports. Le nom de page doit également être renseigné :

   ![Débogueur d’onglets Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Faites défiler vers le bas et développez **Demandes réseau**. Vous devriez être en mesure de trouver l’**evar** définie pour le **modèle de page** :

   ![Evar et jeu de noms de page](assets/collect-data-analytics/evar-page-name-set.png)

1. Revenez au navigateur et ouvrez la console de développement. Cliquez sur **Carrousel** en haut de la page.

   ![Page du carrousel Clic publicitaire](assets/collect-data-analytics/click-carousel-page.png)

1. Observez dans la console du navigateur l’instruction console :

   ![Condition non remplie](assets/collect-data-analytics/condition-not-met.png)

   En effet, le carrousel déclenche un événement `cmp:show` *mais* en raison de notre vérification du **Type de ressource de composant**, aucun événement n&#39;est déclenché.

   >[!NOTE]
   >
   > Si vous ne voyez aucun journal de console, vérifiez que **Consignation de la console** est cochée sous **Lancement** dans le débogueur Experience Platform.

1. Accédez à une page d’article telle que [Australie occidentale](https://wknd.site/us/en/magazine/western-australia.html). Observez que le nom de page et le type de modèle changent.

## Félicitations ! 

Vous venez d&#39;utiliser la couche de données client et l&#39;Experience Platform Launch d&#39;Adobe pilotés par le événement pour collecter des données de page de données d&#39;un site AEM et les envoyer à Adobe Analytics.

### Étapes suivantes

Consultez le didacticiel suivant pour savoir comment utiliser la couche de données client Adobe pilotée par le événement pour [suivre les clics de composants spécifiques sur un site Adobe Experience Manager](track-clicked-component.md).
