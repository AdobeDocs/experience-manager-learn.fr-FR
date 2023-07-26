---
title: Intégration d’AEM Sites à Adobe Analytics avec l’extension de balises Adobe Analytics
description: Intégrez AEM Sites à Adobe Analytics, en utilisant la couche de données client d’Adobe orientée événement pour collecter des données sur l’activité des utilisateurs sur un site web créé avec Adobe Experience Manager. Découvrez comment utiliser les règles de balise pour écouter ces événements et envoyer des données à une suite de rapports Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="Intégration" type="positive"
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '2470'
ht-degree: 5%

---

# Intégration d’AEM Sites et d’Adobe Analytics

>[!NOTE]
>
>Adobe Experience Platform Launch a été rebaptisé en tant que suite de technologies de collecte de données dans Adobe Experience Platform. Plusieurs modifications terminologiques ont par conséquent été apportées à la documentation du produit. Reportez-vous aux [document](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) pour une référence consolidée des modifications terminologiques.


Découvrez comment intégrer AEM Sites et Adobe Analytics à l’extension de balises Adobe Analytics à l’aide des fonctionnalités intégrées de la [Adobe de la couche de données client avec les composants principaux AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr) pour collecter des données sur une page dans Adobe Experience Manager Sites. [Balises dans Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=fr) et le [Extension Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=fr) sont utilisées pour créer des règles pour envoyer des données de page à Adobe Analytics.

## Ce que vous allez construire {#what-build}

![Suivi des données de page](assets/collect-data-analytics/analytics-page-data-tracking.png)

Dans ce tutoriel, vous allez déclencher une règle de balise basée sur un événement de la couche de données client Adobe. Ajoutez également des conditions pour le moment où la règle doit être déclenchée, puis envoyez la variable **Nom de la page** et **Modèle de page** des valeurs d’une page AEM à Adobe Analytics.

### Objectifs {#objective}

1. Créez une règle pilotée par un événement dans la propriété de balise qui capture les modifications à partir de la couche de données.
1. Mappage des propriétés de couche de données de page aux éléments de données dans la propriété de balise
1. Collecte et envoi de données de page dans Adobe Analytics à l’aide de la balise de page vue

## Conditions préalables

Les éléments suivants sont requis :

* **Propriété de balise** dans Experience Platform
* **Adobe Analytics** identifiant de suite de rapports test/dev et serveur de suivi. Consultez la documentation suivante pour [création d’une suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Débogueur Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) extension de navigateur. Captures d’écran de ce tutoriel extraites du navigateur Chrome.
* (Facultatif) AEM Site avec la variable [Adobe de la couche de données client activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation). Ce tutoriel utilise le public face [WKND](https://wknd.site/us/en.html) mais vous êtes invité à utiliser votre propre site.

>[!NOTE]
>
> Besoin d’aide pour intégrer la propriété de balise et AEM site ? [Voir cette série vidéo](../experience-platform/data-collection/tags/overview.md).

## Changement d’environnement de balise pour le site WKND

Le [WKND](http://wknd.site/us/en.html) est un site public basé sur [un projet open source](https://github.com/adobe/aem-guides-wknd) conçu comme une référence et [tutoriel](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr) pour une mise en oeuvre AEM.

Au lieu de configurer un environnement AEM et d’installer la base de code WKND, vous pouvez utiliser le débogueur Experience Platform pour **switch** la live [Site WKND](http://wknd.site/us/en.html) to *your* propriété de balise. Cependant, vous pouvez utiliser votre propre site AEM s’il comporte déjà la variable [Adobe de la couche de données client activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation).

1. Connectez-vous à Experience Platform et [créer une propriété Tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (si ce n&#39;est déjà fait).
1. Assurez-vous qu’une balise initiale JavaScript [La bibliothèque a été créée](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) et converti en balise [environnement](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=fr).
1. Copiez le code incorporé JavaScript de l’environnement de balises dans lequel votre bibliothèque a été publiée.

   ![Copier le code incorporé de la propriété de balise](assets/collect-data-analytics/launch-environment-copy.png)

1. Dans votre navigateur, ouvrez un nouvel onglet et accédez à [Site WKND](http://wknd.site/us/en.html)
1. Ouvrez l’extension de navigateur du débogueur Experience Platform.

   ![Débogueur Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Accédez à **Balises Experience Platform** > **Configuration** et sous **Codes incorporés injectés** remplacer le code incorporé existant par *your* code incorporé copié à partir de l’étape 3.

   ![Remplacer le code incorporé](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Activer **Journalisation de la console** et **Verrouiller** Débogueur dans l’onglet WKND.

   ![Journalisation de la console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Vérifier la couche de données client Adobe sur le site WKND

Le [Projet de référence WKND](https://github.com/adobe/aem-guides-wknd) est créé avec AEM composants principaux et possède la variable [Adobe de la couche de données client activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation) par défaut. Vérifiez ensuite que la couche de données client Adobe est activée.

1. Accédez à [Site WKND](http://wknd.site/us/en.html).
1. Ouvrez les outils de développement du navigateur et accédez au **Console**. Exécutez la commande suivante :

   ```js
   adobeDataLayer.getState();
   ```

   Le code ci-dessus renvoie l’état actuel de la couche de données client Adobe.

   ![Adobe de l’état de la couche de données](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Développez la réponse et examinez la variable `page` entrée . Vous devriez voir un schéma de données comme suit :

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

   Pour envoyer des données de page à Adobe Analytics, utilisez les propriétés standard telles que `dc:title`, `xdm:language`, et `xdm:template` de la couche de données.

   Pour plus d’informations, voir [Schéma de page](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) dans les schémas de données des composants principaux.

   >[!NOTE]
   >
   > Si vous ne voyez pas le `adobeDataLayer` Objet JavaScript ? Assurez-vous que la variable [La couche de données client Adobe a été activée.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation) sur votre site.

## Créer une règle Page chargée

La couche de données client Adobe est une **event** couche de données pilotée. Lorsque la couche de données Page AEM est chargée, elle déclenche une `cmp:show` . Créez une règle qui est déclenchée lorsque la variable `cmp:show` est déclenché à partir de la couche de données de page.

1. Accédez à Experience Platform et à la propriété de balise intégrée au site AEM.
1. Accédez au **Règles** dans l’interface utilisateur de la propriété de balise, puis cliquez sur **Créer une règle**.

   ![Créer une règle](assets/collect-data-analytics/analytics-create-rule.png)

1. Attribuer un nom à la règle **Page chargée**.
1. Dans le **Événements** sous-section, cliquez sur **Ajouter** pour ouvrir le **Configuration d’événement** assistant.
1. Pour **Type d’événement** champ, sélectionnez **Code personnalisé**.

   ![Attribuez un nom à la règle et ajoutez l’événement de code personnalisé](assets/collect-data-analytics/custom-code-event.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.log("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
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

   Le fragment de code ci-dessus ajoute un écouteur d’événement par [publication d’une fonction](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) dans la couche de données. When `cmp:show` est déclenché. `pageShownEventHandler` est appelée. Dans cette fonction, quelques contrôles d’intégrité sont ajoutés et un nouveau `event` est construit avec la dernière [état de la couche de données](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) pour le composant qui a déclenché l’événement.

   Enfin, le `trigger(event)` est appelée. Le `trigger()` est un nom réservé dans la propriété tag et il **triggers** la règle. Le `event` est transmis en tant que paramètre qui, à son tour, est exposé par un autre nom réservé dans la propriété tag . Les éléments de données de la propriété de balise peuvent désormais référencer diverses propriétés à l’aide d’un fragment de code comme `event.component['someKey']`.

1. Enregistrez les modifications.
1. Suivant sous **Actions** click **Ajouter** pour ouvrir le **Configuration d’action** assistant.
1. Pour **Type d’action** champ, choisissez **Code personnalisé**.

   ![Type d’action Custom Code](assets/collect-data-analytics/action-custom-code.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   Le `event` est transmis à partir de `trigger()` appelée dans l’événement personnalisé. Ici, le `component` est la page active dérivée de la couche de données. `getState` dans l’événement personnalisé.

1. Enregistrez les modifications et exécutez une [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) dans la propriété tag pour promouvoir le code vers la propriété [environnement](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=fr) utilisé sur votre site AEM.

   >[!NOTE]
   >
   > Il peut s’avérer utile d’utiliser la variable [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) pour changer le code incorporé en **Développement** environnement.

1. Accédez à votre site AEM et ouvrez les outils de développement pour afficher la console. Actualisez la page. Vous devriez constater que les messages de la console ont été enregistrés :

![Messages de la console chargés en page](assets/collect-data-analytics/page-show-event-console.png)

## Création d’éléments de données

Créez ensuite plusieurs éléments de données pour capturer différentes valeurs de la couche de données client Adobe. Comme vous l’avez vu dans l’exercice précédent, il est possible d’accéder aux propriétés de la couche de données directement par le biais du code personnalisé. L’avantage de l’utilisation des éléments de données est qu’ils peuvent être réutilisés dans les règles de balises.

Les éléments de données sont mappés à la variable `@type`, `dc:title`, et `xdm:template` propriétés.

### Type de ressource de composant

1. Accédez à Experience Platform et à la propriété de balise intégrée au site AEM.
1. Accédez au **Éléments de données** et cliquez sur **Créer un élément de données**.
1. Pour le **Nom** , saisissez la **Type de ressource de composant**.
1. Pour le **Type d’élément de données** champ, sélectionnez **Code personnalisé**.

   ![Type de ressource de composant](assets/collect-data-analytics/component-resource-type-form.png)

1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Enregistrez les modifications.

   >[!NOTE]
   >
   > Rappelez-vous que la variable `event` est rendu disponible et défini sur la portée en fonction de l’événement qui a déclenché la variable **Règle** dans la propriété de balise. La valeur d’un élément de données n’est pas définie tant que l’élément de données n’est pas *référencé* dans une règle. Par conséquent, il est sans risque d’utiliser cet élément de données dans une règle comme la variable **Page chargée** règle créée à l’étape précédente *mais* ne serait pas utilisable en toute sécurité dans d’autres contextes.

### Nom de page

1. Cliquez sur **Ajouter un élément de données** button
1. Pour le **Nom** champ, entrer **Nom de la page**.
1. Pour le **Type d’élément de données** champ, sélectionnez **Code personnalisé**.
1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Enregistrez les modifications.

### Modèle de page

1. Cliquez sur le bouton **Ajouter un élément de données** button
1. Pour le **Nom** champ, entrer **Modèle de page**.
1. Pour le **Type d’élément de données** champ, sélectionnez **Code personnalisé**.
1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Enregistrez les modifications.

1. Vous devez maintenant avoir trois éléments de données dans votre règle :

   ![Éléments de données dans la règle](assets/collect-data-analytics/data-elements-page-rule.png)

## Ajout de l’extension Analytics

Ajoutez ensuite l’extension Analytics à votre propriété de balise pour envoyer des données dans une suite de rapports.

1. Accédez à Experience Platform et à la propriété de balise intégrée au site AEM.
1. Accédez à **Extensions** > **Catalogue**
1. Recherchez la variable **Adobe Analytics** extension et cliquez sur **Installer**

   ![Extension Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Sous **Gestion des bibliothèques** > **Suites de rapports**, saisissez les identifiants de suite de rapports à utiliser avec chaque environnement de balises.

   ![Saisie des identifiants de suite de rapports](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Vous pouvez utiliser une suite de rapports pour tous les environnements dans ce tutoriel, mais dans la vie réelle, vous souhaiterez utiliser des suites de rapports distinctes, comme illustré ci-dessous.

   >[!TIP]
   >
   >Il est recommandé d’utiliser la variable *Gérer la bibliothèque pour moi, option* en tant que paramètre de gestion des bibliothèques , car il facilite la conservation de la variable `AppMeasurement.js` bibliothèque à jour.

1. Cochez la case pour activer . **Utiliser le Activity Map**.

   ![Activer Use Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. Sous **Général** > **Serveur de suivi**, entrez votre serveur de suivi, par exemple : `tmd.sc.omtrdc.net`. Entrez votre serveur de suivi SSL si votre site prend en charge `https://`

   ![Renseignez les serveurs de tracking](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Cliquez sur **Enregistrer** pour enregistrer les modifications.

## Ajouter une condition à la règle Page chargée

Mettez ensuite à jour la variable **Page chargée** pour utiliser la règle **Type de ressource de composant** élément de données pour s’assurer que la règle ne se déclenche que lorsque la variable `cmp:show` est pour la variable **Page**. D’autres composants peuvent déclencher la variable `cmp:show` par exemple, le composant Carrousel le déclenche lorsque les diapositives changent. Il est donc important d’ajouter une condition pour cette règle.

1. Dans l’interface utilisateur de la propriété de balise, accédez au **Page chargée** règle créée précédemment.
1. Sous **Conditions** click **Ajouter** pour ouvrir le **Configuration de condition** assistant.
1. Pour **Type de condition** champ, sélectionnez **Value Comparison** .
1. Définissez la première valeur du champ de formulaire sur `%Component Resource Type%`. Vous pouvez utiliser l’icône d’élément de données ![icône d’élément de données](assets/collect-data-analytics/cylinder-icon.png) pour sélectionner la variable **Type de ressource de composant** élément de données. Laissez le comparateur défini sur `Equals`.
1. Définissez la seconde valeur sur `wknd/components/page`.

   ![Configuration de condition pour la règle chargée sur la page](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Il est possible d’ajouter cette condition dans la fonction de code personnalisé qui écoute la variable `cmp:show` créé plus tôt dans le tutoriel. Cependant, l’ajout de cette règle dans l’interface utilisateur offre une plus grande visibilité aux utilisateurs supplémentaires qui peuvent avoir besoin d’apporter des modifications à la règle. De plus, nous pouvons utiliser notre élément de données .

1. Enregistrez les modifications.

## Définition des variables Analytics et déclenchement de la balise Page vue

Actuellement, la variable **Page chargée** génère simplement une instruction console. Ensuite, utilisez les éléments de données et l’extension Analytics pour définir les variables Analytics en tant que **action** dans le **Page chargée** règle. Nous définissons également une action supplémentaire pour déclencher la variable **Balise de page vue** et envoyer les données collectées à Adobe Analytics.

1. Dans la règle Page chargée , **remove** la valeur **Core - Code personnalisé** action (les instructions de la console) :

   ![Suppression d’une action de code personnalisé](assets/collect-data-analytics/remove-console-statements.png)

1. Sous la sous-section Actions , cliquez sur **Ajouter** pour ajouter une nouvelle action.

1. Définissez la variable **Extension** saisir **Adobe Analytics** et définissez la variable **Type d’action** to  **Définition de variables**

   ![Définition de l’extension d’action sur Analytics Set Variables](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Dans le panneau principal, sélectionnez une **eVar** et défini comme valeur de l’élément de données **Modèle de page**. Utiliser l’icône Éléments de données ![Icône Éléments de données](assets/collect-data-analytics/cylinder-icon.png) pour sélectionner la variable **Modèle de page** élément .

   ![Définir comme modèle de page d’eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Faites défiler la page vers le bas, sous **Paramètres supplémentaires** set **Nom de la page** à l’élément de données **Nom de la page**:

   ![Jeu de variables d’environnement de nom de page](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Enregistrez les modifications.

1. Ajoutez ensuite une action supplémentaire à droite du **Adobe Analytics - Définition de variables** en appuyant sur le bouton **plus** icon :

   ![Ajout d’une action de règle de balise supplémentaire](assets/collect-data-analytics/add-additional-launch-action.png)

1. Définissez la variable **Extension** saisir **Adobe Analytics** et définissez la variable **Type d’action** to  **Envoyer la balise**. Puisque cette action est considérée comme une page vue, laissez le suivi par défaut défini sur **`s.t()`**.

   ![Action Envoyer la balise Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Enregistrez les modifications. Le **Page chargée** doit maintenant avoir la configuration suivante :

   ![Configuration finale de la règle de balise](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Écoute de la `cmp:show` .
   * **2.** Vérifiez que l’événement a été déclenché par une page.
   * **3.** Définition des variables Analytics pour **Nom de la page** et **Modèle de page**
   * **4.** Envoyer la balise de page vue Analytics

1. Enregistrez toutes les modifications et créez votre bibliothèque de balises, en effectuant la promotion vers l’environnement approprié.

## Validation de la balise de page vue et de l’appel Analytics

Maintenant que la variable **Page chargée** envoie la balise Analytics. Vous devriez pouvoir voir les variables de suivi Analytics à l’aide du débogueur Experience Platform.

1. Ouvrez le [Site WKND](https://wknd.site/us/en.html) dans votre navigateur.
1. Cliquez sur l’icône Debugger . ![Icône du débogueur Experience Platform](assets/collect-data-analytics/experience-cloud-debugger.png) pour ouvrir le débogueur Experience Platform.
1. Assurez-vous que le débogueur mappe la propriété de balise à *your* Environnement de développement, comme décrit précédemment et **Journalisation de la console** est cochée.
1. Ouvrez le menu Analytics et vérifiez que la suite de rapports est définie sur *your* suite de rapports. Le Nom de page doit également être renseigné :

   ![Débogueur de l’onglet Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Faire défiler vers le bas et développer **Requêtes réseau**. Vous devriez être en mesure de trouver la variable **evar** défini pour le **Modèle de page**:

   ![Evar et nom de page définis](assets/collect-data-analytics/evar-page-name-set.png)

1. Revenez au navigateur et ouvrez la console de développement. Cliquez sur les **Carrousel** en haut de la page.

   ![Clic sur la page du carrousel](assets/collect-data-analytics/click-carousel-page.png)

1. Observez l’instruction de la console dans la console du navigateur :

   ![Condition non remplie](assets/collect-data-analytics/condition-not-met.png)

   Cela est dû au fait que le carrousel déclenche une `cmp:show` event *mais* en raison de notre vérification de la fonction **Type de ressource de composant**, aucun événement n’est déclenché.

   >[!NOTE]
   >
   > Si vous ne voyez aucun journal de la console, assurez-vous que **Journalisation de la console** est coché sous **Balises Experience Platform** dans le débogueur Experience Platform.

1. Accédez à une page d’article telle que [Australie occidentale](https://wknd.site/us/en/magazine/western-australia.html). Observez que le nom de page et le type de modèle changent.

## Félicitations.

Vous venez d’utiliser la couche de données client et les balises Adobe basées sur un événement dans Experience Platform pour collecter les données de page de données d’un site AEM et les envoyer à Adobe Analytics.

### Étapes suivantes

Consultez le tutoriel suivant pour savoir comment utiliser la couche de données client Adobe orientée événement pour [suivi des clics de composants spécifiques sur un site Adobe Experience Manager ;](track-clicked-component.md).
