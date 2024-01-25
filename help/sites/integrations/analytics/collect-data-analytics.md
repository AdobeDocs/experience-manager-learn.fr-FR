---
title: Intégrer AEM Sites à Adobe Analytics avec l’extension de balises Adobe Analytics
description: Intégrez AEM Sites à Adobe Analytics en utilisant la couche de données client d’Adobe orientée événement pour collecter des données sur l’activité des utilisateurs et utilisatrices sur un site web créé avec Adobe Experience Manager. Découvrez comment utiliser les règles de balise pour écouter ces événements et envoyer des données à une suite de rapports Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="Intégration" type="positive"
doc-type: Tutorial
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
duration: 629
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '2307'
ht-degree: 100%

---

# Intégrer AEM Sites et Adobe Analytics

>[!NOTE]
>
>Adobe Experience Platform Launch est mort, vive les suites de collecte de données dans Adobe Experience Platform. Plusieurs modifications terminologiques ont par conséquent été apportées à la documentation du produit. Consultez ce [document](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?lang=fr) qui recense les modifications de la terminologie.


Découvrez comment intégrer AEM Sites et Adobe Analytics à l’extension de balises Adobe Analytics à l’aide des fonctionnalités intégrées de la [couche de données client d’Adobe avec les composants principaux AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr) pour collecter des données relatives à une page dans Adobe Experience Manager Sites. Les [balises dans Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=fr) et [l’extension Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=fr) sont utilisées pour créer des règles permettant d’envoyer des données de page à Adobe Analytics.

## Ce que vous allez créer {#what-build}

![Suivi des données de page](assets/collect-data-analytics/analytics-page-data-tracking.png)

Dans ce tutoriel, vous allez déclencher une règle de balise basée sur un événement de la couche de données client Adobe. Ajoutez également des conditions pour le moment où la règle doit être déclenchée, puis envoyez les valeurs **Nom de page** et **Modèle de page** d’une page AEM à Adobe Analytics.

### Objectifs {#objective}

1. Créez une règle pilotée par un événement dans la propriété de balise qui capture les modifications de la couche de données.
1. Mapper les propriétés de couche de données de page à des éléments de données dans la propriété de balise
1. Collecter et envoyer des données de page dans Adobe Analytics à l’aide de la balise de page vue

## Conditions préalables

Les éléments suivants sont requis :

* **Propriété de balise** dans Experience Platform
* Identifiant de suite de rapports test/dev et serveur de suivi **Adobe Analytics**. Consultez la documentation suivante sur la [création d’une suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=fr).
* Extension de navigateur [Débogueur Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=fr). Les captures d’écran de ce tutoriel sont extraites du navigateur Chrome.
* (Facultatif) AEM Site avec la [couche de données client Adobe activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation). Ce tutoriel utilise le site public [WKND](https://wknd.site/fr/fr.html), mais vous pouvez utiliser votre propre site.

>[!NOTE]
>
> Besoin d’aide pour intégrer la propriété de balise et un site AEM ? [Consultez cette série de vidéos](../experience-platform/data-collection/tags/overview.md).

## Changement d’environnement de balise pour le site WKND

[WKND](https://wknd.site/fr/fr.html) est un site public basé sur [un projet open source](https://github.com/adobe/aem-guides-wknd) conçu comme une référence et un [tutoriel](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr) pour une implémentation AEM.

Au lieu de configurer un environnement AEM et d’installer la base de code WKND, vous pouvez utiliser le débogueur Experience Platform pour **changer** le [site WKND](https://wknd.site/fr/fr.html) en direct pour *votre* propriété de balise. Cependant, vous pouvez utiliser votre propre site AEM s’il comporte déjà la [couche de données client Adobe activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation).

1. Connectez-vous à Experience Platform et [créez une propriété de balise](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=fr) (si ce n’est déjà fait).
1. Assurez-vous qu’une bibliothèque JavaScript de balises initiale [a été créée](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library?lang=fr) et convertie en [environnement](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=fr) de balises.
1. Copiez le code incorporé JavaScript de l’environnement de balises dans lequel votre bibliothèque a été publiée.

   ![Copier le code incorporé de la propriété de balise](assets/collect-data-analytics/launch-environment-copy.png)

1. Dans votre navigateur, ouvrez un nouvel onglet et accédez au [site WKND](https://wknd.site/fr/fr.html).
1. Ouvrez l’extension de navigateur du débogueur Experience Platform.

   ![Débogueur Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Accédez à **Balises Experience Platform** > **Configuration** et sous **Codes incorporés injectés**, remplacez le code incorporé existant par *votre* code incorporé copié à l’étape 3.

   ![Remplacer le code incorporé](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Il faut ensuite activer la **Journalisation de la console** et **Verrouiller** le débogueur dans l’onglet WKND.

   ![Journalisation de la console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Vérifier la couche de données client Adobe sur le site WKND

Le [projet de référence WKND](https://github.com/adobe/aem-guides-wknd) est créé avec les composants principaux AEM et possède la [couche de données client Adobe activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation) par défaut. Vérifiez ensuite que la couche de données client Adobe est activée.

1. Accédez au [site WKND](https://wknd.site/fr/fr.html).
1. Ouvrez les outils de développement du navigateur et accédez à la **Console**. Exécutez la commande suivante :

   ```js
   adobeDataLayer.getState();
   ```

   Le code ci-dessus renvoie le statut en cours de la couche de données client Adobe.

   ![État de la couche de données Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Développez la réponse et examinez l’entrée `page`. Un schéma de données similaire à celui-ci doit s’afficher :

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

   Pour plus d’informations, consultez le [schéma de page](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page?lang=fr) dans les schémas de données des composants principaux.

   >[!NOTE]
   >
   > Si vous ne voyez pas l’objet JavaScript `adobeDataLayer` : Assurez-vous que la [couche de données client Adobe a été activée](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation) sur votre site.

## Créer une règle Page chargée

La couche de données de la clientèle Adobe est une couche de données **pilotée par les événements**. Lorsque la couche de données de page AEM est chargée, elle déclenche un événement `cmp:show`. Créez une règle qui se déclenche lorsque l’événement `cmp:show` est déclenché à partir de la couche de données de page.

1. Accédez à Experience Platform et à la propriété de balise intégrée au site AEM.
1. Accédez à la section **Règles** dans l’interface utilisateur de la propriété de balise, puis cliquez sur **Créer une règle**.

   ![Création d’une règle.](assets/collect-data-analytics/analytics-create-rule.png)

1. Nommez la règle **Page chargée**.
1. Dans la sous-section **Événements**, cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration d’événement**.
1. Pour le champ **Type d’événement**, sélectionnez **Code personnalisé**.

   ![Nommez la règle et ajoutez l’événement de code personnalisé.](assets/collect-data-analytics/custom-code-event.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

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

   L’extrait de code ci-dessus ajoute un écouteur d’événement en [envoyant une fonction](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) dans la couche de données. Lorsque l’événement `cmp:show` est déclenché, la fonction `pageShownEventHandler` est appelée. Dans cette fonction, quelques contrôles d’intégrité sont ajoutés et un nouvel `event` est construit avec le dernier [état de la couche de données](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) pour le composant qui a déclenché l’événement.

   Enfin, la fonction `trigger(event)` est appelée. La fonction `trigger()` est un nom réservé dans la propriété de balise et elle **déclenche** la règle. L’objet `event` est transmis en tant que paramètre qui, à son tour, est exposé par un autre nom réservé dans la propriété de balise. Les éléments de données dans la propriété de balise peuvent désormais référencer diverses propriétés avec un extrait de code du type `event.component['someKey']`.

1. Enregistrez les modifications.
1. Ensuite, sous **Actions**, cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration de l’action**.
1. Pour le champ **Type d’action**, choisissez **Code personnalisé**.

   ![Type d’action Code personnalisé.](assets/collect-data-analytics/action-custom-code.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez l’extrait de code suivant :

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   L’objet `event` est transmis à partir de la méthode `trigger()` appelée dans l’événement personnalisé. Ici, le `component` est la page actuelle provenant de la couche de données `getState` dans l’événement personnalisé.

1. Enregistrez les modifications et exécutez une [création](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html?lang=fr) dans la propriété de balise pour promouvoir le code vers l’[environnement](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=fr) utilisé sur votre site AEM.

   >[!NOTE]
   >
   > Il peut s’avérer utile d’utiliser [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=fr) pour changer le code incorporé dans un environnement de **développement**.

1. Accédez à votre site AEM et ouvrez les outils de développement pour afficher la console. Actualisez la page. Vous devriez constater que les messages de la console ont été enregistrés :

![Messages de la console chargés dans la page.](assets/collect-data-analytics/page-show-event-console.png)

## Créer des éléments de données

Créez ensuite plusieurs éléments de données pour capturer différentes valeurs de la couche de données client Adobe. Comme vous l’avez vu dans l’exercice précédent, il est possible d’accéder aux propriétés de la couche de données directement par le biais du code personnalisé. L’avantage de l’utilisation des éléments de données est qu’ils peuvent être réutilisés dans les règles de balises.

Les éléments de données sont mappés aux propriétés `@type`, `dc:title` et `xdm:template`.

### Type de ressource du composant

1. Accédez à Experience Platform et à la propriété de balise intégrée au site AEM.
1. Accédez à la section **Éléments de données** et cliquez sur **Créer un élément de données**.
1. Pour le champ **Nom**, saisissez le **Type de ressource du composant**.
1. Pour le champ **Type d’élément de données**, sélectionnez **Code personnalisé**.

   ![Type de ressource du composant.](assets/collect-data-analytics/component-resource-type-form.png)

1. Cliquez sur le bouton **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Enregistrez les modifications.

   >[!NOTE]
   >
   > Rappelons que l’objet `event` devient disponible et est défini en fonction de l’événement qui a déclenché la **Règle** dans la propriété de balise. La valeur d’un élément de données n’est pas définie tant que l’élément de données n’est pas *référencé* dans une règle. Il est donc possible d’utiliser cet élément de données dans une règle telle que **Page chargée** créée à l’étape précédente *mais* il ne serait pas judicieux de l’utiliser dans d’autres contextes.

### Nom de page

1. Cliquez sur le bouton **Ajouter un élément de données**.
1. Pour le champ **Nom**, saisissez le **nom de la page**.
1. Pour le champ **Type d’élément de données**, sélectionnez **Code personnalisé**.
1. Cliquez sur le bouton **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Enregistrez les modifications.

### Modèle de page

1. Cliquez sur le bouton **Ajouter un élément de données**.
1. Pour le champ **Nom**, saisissez **Modèle de page**.
1. Pour le champ **Type d’élément de données**, sélectionnez **Code personnalisé**.
1. Cliquez sur le bouton **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Enregistrez les modifications.

1. Vous devez maintenant avoir trois éléments de données dans votre règle :

   ![Éléments de données dans la règle](assets/collect-data-analytics/data-elements-page-rule.png)

## Ajouter l’extension Analytics

Ajoutez ensuite l’extension Analytics à votre propriété de balise pour envoyer des données dans une suite de rapports.

1. Accédez à Experience Platform et à la propriété de balise intégrée au site AEM.
1. Accédez à **Extensions** > **Catalogue**.
1. Recherchez l’extension **Adobe Analytics** et cliquez sur **Installer**.

   ![Extension Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Sous **Gestion des bibliothèques** > **Suites de rapports**, saisissez les identifiants de suite de rapports à utiliser avec chaque environnement de balises.

   ![Saisie des identifiants de suite de rapports](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Vous pouvez utiliser une suite de rapports pour tous les environnements dans ce tutoriel, mais dans la vie réelle, vous souhaiterez utiliser des suites de rapports distinctes, comme illustré ci-dessous.

   >[!TIP]
   >
   >Il est recommandé d’utiliser l’*option Gérer la bibliothèque pour moi* en tant que paramètre de gestion des bibliothèques, car elle facilite grandement le maintien de la bibliothèque `AppMeasurement.js` à jour.

1. Cochez la case pour activer **Utiliser l’Activity Map**.

   ![Activer Utiliser l’Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. Sous **Général** > **Serveur de suivi**, entrez votre serveur de suivi, par exemple : `tmd.sc.omtrdc.net`. Entrer votre serveur de suivi SSL si votre site prend en charge `https://`.

   ![Renseigner les serveurs de suivi](assets/collect-data-analytics/analytics-config-trackingServer.png).

1. Cliquez sur **Enregistrer** pour enregistrer les modifications.

## Ajouter une condition à la règle Page chargée

Mettez ensuite à jour la règle **Page chargée** pour utiliser l’élément de données **Type de ressource de composant** pour vous assurer que la règle ne se déclenche que lorsque l’événement `cmp:show` est pour la **Page**. D’autres composants peuvent déclencher l’événement `cmp:show`, par exemple, le composant Carousel le déclenche lorsque les diapositives changent. Il est donc important d’ajouter une condition pour cette règle.

1. Dans l’interface utilisateur de la propriété de balise, accédez à la règle **Page chargée** créée précédemment.
1. Sous **Conditions**, cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration de condition**.
1. Dans le champ **Type de condition**, sélectionnez l’option **Comparaison de valeurs**.
1. Définissez la première valeur du champ de formulaire sur `%Component Resource Type%`. Vous pouvez utiliser l’icône d’élément de données ![data-element icon](assets/collect-data-analytics/cylinder-icon.png) pour sélectionner l’élément de données **Type de ressource de composant**. Laissez le comparateur défini sur `Equals`.
1. Définissez la deuxième valeur sur `wknd/components/page`.

   ![Configuration de condition pour la règle Page chargée](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Il est possible d’ajouter cette condition dans la fonction de code personnalisé qui écoute l’évènement `cmp:show` créé plus tôt dans le tutoriel. Toutefois, l’ajout de cette condition dans l’interface utilisateur offre une plus grande visibilité aux utilisateurs et utilisatrices supplémentaires qui peuvent avoir besoin d’apporter des modifications à la règle. De plus, nous pouvons utiliser notre élément de données.

1. Enregistrez les modifications.

## Définir les variables Analytics et déclencher la balise Page vue

Actuellement, la règle **Page chargée** génère simplement une instruction console. Ensuite, utilisez les éléments de données et l’extension Analytics pour définir les variables Analytics en tant qu’**action** dans la règle **Page chargée**. Nous définissons également une action supplémentaire pour déclencher la **balise Page vue** et envoyer les données collectées à Adobe Analytics.

1. Dans la règle Page chargée, **supprimez** l’action **Principal - Code personnalisé** (les instructions console) :

   ![Supprimer une action de code personnalisé](assets/collect-data-analytics/remove-console-statements.png)

1. Dans la sous-section Actions, cliquez sur **Ajouter** pour ajouter une nouvelle action.

1. Définissez le type **Extension** sur **Adobe Analytics** et le **Type d’action** sur **Définir des variables**.

   ![Définir l’extension d’action sur les variables définies Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Dans le panneau principal, sélectionnez une **eVar** disponible et définie comme valeur de l’élément de données **Modèle de page**. Utilisez l’icône Éléments de données ![Icône Éléments de données](assets/collect-data-analytics/cylinder-icon.png) pour sélectionner l’élément **Modèle de page**.

   ![Définir comme modèle de page eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Faites défiler la page vers le bas, sous **Paramètres supplémentaires**, définissez **Nom de la page** sur l’élément de données **Nom de la page** :

   ![Variable d’environnement Nom de la page définie](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Enregistrez les modifications.

1. Ajoutez ensuite une action supplémentaire à droite d’**Adobe Analytics - Définir des variables** en appuyant sur l’icône **plus** :

   ![Ajouter une action de règle de balise supplémentaire](assets/collect-data-analytics/add-additional-launch-action.png)

1. Définissez le type **Extension** sur **Adobe Analytics** et le **Type d’action** sur **Envoyer la balise**. Puisque cette action est considérée comme une page vue, laissez le suivi par défaut défini sur **`s.t()`**.

   ![Action Envoyer la balise Adobe Analytics.](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Enregistrez les modifications. La règle **Page chargée** doit maintenant avoir la configuration suivante :

   ![Configuration finale de la règle de balise](assets/collect-data-analytics/final-page-loaded-config.png).

   * **1.** Écoutez l’événement `cmp:show`.
   * **2.** Vérifiez que l’événement a été déclenché par une page.
   * **3.** Définissez des variables Analytics pour **Nom de la page** et **Modèle de page**.
   * **4.** Envoyez la balise Page vue Analytics.

1. Enregistrez toutes les modifications et créez votre bibliothèque de balises, en effectuant la promotion vers l’environnement approprié.

## Valider la balise Page vue et l’appel d’Analytics

Maintenant que la règle **Page chargée** envoie la balise Analytics, vous devriez pouvoir voir les variables de suivi d’Analytics à l’aide d’Experience Platform Debugger.

1. Ouvrez le [site WKND](https://wknd.site/fr/fr.html) dans votre navigateur.
1. Cliquez sur l’icône du Debugger ![icône d’Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) pour ouvrir l’Experience Platform Debugger.
1. Assurez-vous que le Debugger mappe la propriété de balise pour *votre* environnement de développement, comme décrit précédemment et que l’option **Journalisation de la console** est activée.
1. Ouvrez le menu Analytics et vérifiez que la suite de rapports est définie sur *votre* suite de rapports. Le nom de page doit également être renseigné :

   ![Débogueur de l’onglet Analytics.](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Faites défiler vers le bas et développez **Requêtes réseau**. Vous devriez être en mesure de trouver la valeur **evar** définie pour le **Modèle de page** :

   ![Evar et nom de page définis.](assets/collect-data-analytics/evar-page-name-set.png)

1. Revenez au navigateur et ouvrez la Developer Console. Cliquez sur le **carrousel** en haut de la page.

   ![Clic sur la page du carrousel.](assets/collect-data-analytics/click-carousel-page.png)

1. Observez l’instruction de console dans la console du navigateur :

   ![Condition non remplie.](assets/collect-data-analytics/condition-not-met.png)

   Cela est dû au fait que le carrousel déclenche bien un événement `cmp:show`, *mais* en raison de notre vérification du **type de ressource de composant**, aucun événement n’est déclenché.

   >[!NOTE]
   >
   > Si vous ne voyez aucun journal de console, assurez-vous que l’option **Journalisation de la console** est cochée sous les **balises Experience Platform** dans Experience Platform Debugger.

1. Accédez à une page d’article telle que [Western Australia](https://wknd.site/us/en/magazine/western-australia.html). Observez que le nom de page et le type de modèle changent.

## Félicitations.

Vous venez d’utiliser la couche de données client Adobe et les balises orientées événement dans Experience Platform pour collecter des données de page de données d’un site AEM et les envoyer à Adobe Analytics.

### Étapes suivantes

Consultez le tutoriel suivant pour savoir comment utiliser la couche de données client Adobe orientée événement pour [suivre les clics de composants spécifiques sur un site Adobe Experience Manager](track-clicked-component.md).
