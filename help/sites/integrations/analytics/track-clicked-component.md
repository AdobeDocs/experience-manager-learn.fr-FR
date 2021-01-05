---
title: Suivi du composant cliqué avec Adobe Analytics
description: Utilisez la couche Données client Adobe pilotée par le événement pour effectuer le suivi des clics de composants spécifiques sur un site Adobe Experience Manager. Découvrez comment utiliser des règles en Experience Platform Launch pour écouter ces événements et envoyer des données à un Adobe Analytics avec une balise de lien de suivi.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 64c167ec1d625fdd8be1bc56f7f5e59460b8fed3
workflow-type: tm+mt
source-wordcount: '1831'
ht-degree: 5%

---


# Suivi du composant cliqué avec Adobe Analytics

Utilisez la couche de données client [Adobe pilotée par le événement avec les composants principaux AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/data-layer/overview.html) pour effectuer le suivi des clics de composants spécifiques sur un site Adobe Experience Manager. Découvrez comment utiliser des règles dans Experience Platform Launch pour écouter les événements de clics, filtrer par composant et envoyer les données à une instance Adobe Analytics avec une balise de lien de suivi.

## Ce que vous allez créer

L&#39;équipe marketing de WKND souhaite identifier les boutons d&#39;appel à l&#39;action (CTA) qui génèrent les meilleurs résultats sur la page d&#39;accueil. Dans ce tutoriel, nous allons ajouter une nouvelle règle dans l&#39;Experience Platform Launch qui écoute les événements `cmp:click` de **Teaser** et **Button** composants et envoie l&#39;identifiant du composant et un nouveau événement à Adobe Analytics avec la balise de lien de suivi.

![Eléments de suivi des clics](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objectifs {#objective}

1. Créez une règle pilotée par événement dans Launch en fonction du événement `cmp:click`.
1. Filtrez les différents événements par type de ressource de composant.
1. Définissez l’identifiant du composant sur lequel l’utilisateur a cliqué et envoyez l’événement Adobe Analytics avec la balise de lien de suivi.

## Conditions préalables

Ce didacticiel est la suite de [Collecte de données de page avec Adobe Analytics](./collect-data-analytics.md) et suppose que vous disposez des éléments suivants :

* Une **propriété de lancement** avec l&#39;extension [Adobe Analytics](https://docs.adobe.com/content/help/fr-FR/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) activée
* **Adobe** Analytics/dev report suite ID et serveur de suivi. Consultez la documentation suivante pour [créer une nouvelle suite de rapports](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Extension ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) de navigateur Experience Platform Debuggerbrowser configurée avec votre propriété Launch chargée sur  [https://wknd.site/us/en.](https://wknd.site/us/en.html) htmlun site AEM avec la couche de données d&#39;Adobe activée.

## Inspect the Button and Teaser Schéma

Avant de créer des règles dans Lancement, il est utile de vérifier le [schéma du bouton et du teaser](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) et de les inspecter dans l’implémentation de la couche de données.

1. Accédez à [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Ouvrez les outils de développement du navigateur et accédez à la **Console**. Exécutez la commande suivante :

   ```js
   adobeDataLayer.getState();
   ```

   Cette opération renvoie l&#39;état actuel de la couche de données du client Adobe.

   ![Etat de la couche de données via la console du navigateur](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Développez la réponse et recherchez les entrées précédées de l’entrée `button-` et `teaser-xyz-cta`. Un schéma de données doit s’afficher comme suit :

   Schéma de bouton :

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Schéma du teaser :

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Elles sont basées sur le Schéma [Composant/Élément de Conteneur](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item). La règle que nous allons créer dans Lancement utilisera ce schéma.

## Créer une règle de clic CTA

La couche de données client de l&#39;Adobe est une couche de données pilotée **événement**. Lorsque l&#39;utilisateur clique sur un composant principal, un événement `cmp:click` est distribué via la couche de données. Créez ensuite une règle pour écouter le événement `cmp:click`.

1. Accédez à l&#39;Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez à la section **Règles** de l’interface utilisateur de lancement, puis cliquez sur **Ajouter la règle**.
1. Attribuez un nom à la règle **CTA cliqué**.
1. Cliquez sur **Événements** > **Ajouter** pour ouvrir l&#39;Assistant **Configuration de Événement**.
1. Sous **Type d&#39;événement** sélectionnez **Code personnalisé**.

   ![Nommer la règle cliquée par le CTA et ajouter le événement de code personnalisé](assets/track-clicked-component/custom-code-event.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   Le fragment de code ci-dessus ajoute un écouteur de événement en [poussant une fonction](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) dans la couche de données. Lorsque le événement `cmp:click` est déclenché, la fonction `componentClickedHandler` est appelée. Dans cette fonction, quelques vérifications d&#39;intégrité sont ajoutées et un nouvel objet `event` est construit avec l&#39;état [le plus récent de la couche de données](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) pour le composant qui a déclenché le événement.

   Ensuite, `trigger(event)` est appelé. `trigger()` est un nom réservé dans Lancement et déclenche la règle de lancement. Nous transmettons l&#39;objet `event` en tant que paramètre qui sera à son tour exposé par un autre nom réservé dans Lancement nommé `event`. Les éléments de données du lancement peuvent désormais référencer diverses propriétés telles que : `event.component['someKey']`.

1. Enregistrez les modifications.
1. Ensuite, sous **Actions**, cliquez sur **Ajouter** pour ouvrir l&#39;Assistant **Configuration de l&#39;action**.
1. Sous **Type d&#39;action**, choisissez **Code personnalisé**.

   ![Type d’action Code personnalisé](assets/track-clicked-component/action-custom-code.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   L&#39;objet `event` est transmis à partir de la méthode `trigger()` appelée dans le événement personnalisé. `component` est l’état actuel du composant dérivé de la couche de données  `getState` qui a déclenché le clic.

1. Enregistrez les modifications et exécutez une [build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) dans Lancement pour promouvoir le code dans l&#39;environnement [](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) utilisé sur votre site AEM.

   >[!NOTE]
   >
   > Il peut s’avérer très utile d’utiliser le [débogueur de Adobe Experience Platform](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) pour passer le code incorporé à un environnement **Développement**.

1. Accédez au [site WKND](https://wknd.site/us/en.html) et ouvrez les outils de développement pour vue à la console. Sélectionnez **Conserver le journal**.

1. Cliquez sur l&#39;un des boutons **Teaser** ou **Bouton** CTA pour accéder à une autre page.

   ![Bouton CTA pour cliquer](assets/track-clicked-component/cta-button-to-click.png)

1. Observez dans la console du développeur que la règle **CTA cliquée** a été déclenchée :

   ![Bouton CTA cliqué](assets/track-clicked-component/cta-button-clicked-log.png)

## Créer des éléments de données

Créez ensuite un élément de données pour capturer l’ID et le titre du composant sur lequel l’utilisateur a cliqué. Rappelez-vous dans l&#39;exercice précédent que la sortie de `event.path` était semblable à `component.button-b6562c963d` et que la valeur de `event.component['dc:title']` ressemblait à &quot;Vues Trips&quot;.

### ID de composant

1. Accédez à l&#39;Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez à la section **Éléments de données** et cliquez sur **Ajouter un nouvel élément de données**.
1. Pour **Nom**, saisissez **ID de composant**.
1. Pour **Type d’élément de données**, sélectionnez **Code personnalisé**.

   ![Formulaire d’élément de données d’ID de composant](assets/track-clicked-component/component-id-data-element.png)

1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Enregistrez les modifications.

   >[!NOTE]
   >
   > Rappelez-vous que l’objet `event` est rendu disponible et que sa portée est définie en fonction du événement qui a déclenché la **règle** dans le lancement. La valeur d’un élément de données n’est pas définie tant que l’élément de données n’est pas *référencé* dans une règle. Par conséquent, il est sans risque d’utiliser cet élément de données dans une règle telle que la règle **CTA cliqué** créée dans l’exercice précédent *mais* ne serait pas utilisable en toute sécurité dans d’autres contextes.

### Libellé du composant 

1. Accédez à la section **Éléments de données** et cliquez sur **Ajouter un nouvel élément de données**.
1. Pour **Nom**, saisissez **Titre du composant**.
1. Pour **Type d’élément de données**, sélectionnez **Code personnalisé**.
1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Enregistrez les modifications.

## Ajouter une condition à la règle de clic CTA

Ensuite, mettez à jour la règle **CTA cliqué** pour vous assurer que la règle se déclenche uniquement lorsque le événement `cmp:click` est déclenché pour un **Teaser** ou un **bouton**. Comme le CTA du Teaser est considéré comme un objet distinct dans la couche de données, il est important de vérifier le parent pour vérifier qu&#39;il provient d&#39;un Teaser.

1. Dans l’interface utilisateur de lancement, accédez à la règle **CTA cliquée** créée précédemment.
1. Sous **Conditions**, cliquez sur **Ajouter** pour ouvrir l&#39;Assistant **Configuration de condition**.
1. Pour **Type de condition**, sélectionnez **Code personnalisé**.

   ![Code personnalisé Condition de clic CTA](assets/track-clicked-component/custom-code-condition.png)

1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   Le code ci-dessus vérifie d&#39;abord si le type de ressource provient d&#39;un **bouton**, puis s&#39;il provient d&#39;une DEC dans un **Teaser**.

1. Enregistrez les modifications.

## Définition des variables Analytics et déclenchement de la balise de lien de suivi

Actuellement, la règle **CTA cliqué** génère simplement une instruction console. Ensuite, utilisez les éléments de données et l’extension Analytics pour définir les variables Analytics sous la forme **action**. Nous définirons également une action supplémentaire pour déclencher le **lien de suivi** et envoyer les données collectées à Adobe Analytics.

1. Dans la règle **CTA cliqué** **remove** l&#39;action **Core - Custom Code** (instructions de la console) :

   ![Supprimer l’action de code personnalisé](assets/track-clicked-component/remove-console-statements.png)

1. Sous Actions, cliquez sur **Ajouter** pour ajouter une nouvelle action.
1. Définissez le type **Extension** sur **Adobe Analytics** et définissez **Type d&#39;action** sur **Définir des variables**.

1. Définissez les valeurs suivantes pour **eVars**, **Props** et **Événements** :

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![Définir la prop et les événements de l’eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Ici, `%Component ID%` est utilisé, car il garantit un identifiant unique pour la LTC sur laquelle l&#39;utilisateur a cliqué. L’utilisation de `%Component ID%` présente un inconvénient potentiel : le rapport Analytics contient des valeurs du type `button-2e6d32893a`. L&#39;utilisation de `%Component Title%` donnerait un nom plus convivial à l&#39;homme, mais la valeur peut ne pas être unique.

1. Ensuite, ajoutez une action supplémentaire à droite de **Adobe Analytics - Set Variables** en appuyant sur l&#39;icône **plus** :

   ![Ajouter une action de lancement supplémentaire](assets/track-clicked-component/add-additional-launch-action.png)

1. Définissez le type **Extension** sur **Adobe Analytics** et définissez **Type d&#39;action** sur **Envoyer la balise**.
1. Sous **Suivi**, définissez le bouton radio sur **`s.tl()`**.
1. Pour **Type de lien** choisissez **Lien personnalisé** et pour **Nom de lien** définissez la valeur sur : **`%Component Title%: CTA Clicked`** :

   ![Configuration de la balise Send Link](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Cette opération combine la variable dynamique de l’élément de données **Component Title** et la chaîne statique **CTA Click**.

1. Enregistrez les modifications. La règle **CTA cliqué** doit maintenant avoir la configuration suivante :

   ![Configuration du lancement final](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Écoute le  `cmp:click` événement.
   * **2.** Vérifiez que le événement a été déclenché par un  **** Teaser  **Buttonor**.
   * **3.** Définissez les variables Analytics pour effectuer le suivi de l’ **ID de** composant en tant qu’ **eVar**,  **prop** et un  **événement.**
   * **4.** Envoyez la balise de lien de suivi Analytics (et ne la  **** traitez pas comme une vue de page).

1. Enregistrez toutes les modifications et générez votre bibliothèque de lancement en faisant la promotion vers l’Environnement approprié.

## Validation de la balise de lien de suivi et de l’appel Analytics

Maintenant que la règle **CTA cliqué** envoie la balise Analytics, vous devriez être en mesure d’afficher les variables de suivi Analytics à l’aide du débogueur Experience Platform.

1. Ouvrez le [site WKND](https://wknd.site/us/en.html) dans votre navigateur.
1. Cliquez sur l’icône Débogueur ![Icône Débogueur de plateforme d’expérience](assets/track-clicked-component/experience-cloud-debugger.png) pour ouvrir le Débogueur Experience Platform.
1. Assurez-vous que le débogueur mappe la propriété Launch sur *votre* environnement de développement, comme décrit précédemment et que **la journalisation de la console** est cochée.
1. Ouvrez le menu Analytics et vérifiez que la suite de rapports est définie sur *votre* suite de rapports.

   ![Débogueur d’onglets Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. Dans le navigateur, cliquez sur l&#39;un des boutons **Teaser** ou **Bouton** CTA pour accéder à une autre page.

   ![Bouton CTA pour cliquer](assets/track-clicked-component/cta-button-to-click.png)

1. Revenez au débogueur Experience Platform, faites défiler la page vers le bas et développez **Demandes réseau** > *Votre suite de rapports*. Vous devriez être en mesure de trouver le jeu **eVar**, **prop** et **événement**.

   ![Événements, eVar et prop Analytics suivis sur un clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Revenez au navigateur et ouvrez la console de développement. Accédez au pied de page du site et cliquez sur l’un des liens de navigation :

   ![Cliquez sur le lien Navigation dans le pied de page.](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observez dans la console du navigateur que le message *&quot;Code personnalisé&quot; pour la règle &quot;CTA cliqué&quot; n&#39;a pas été satisfait*.

   Cela est dû au fait que le composant Navigation déclenche un événement `cmp:click` *mais* en raison de notre vérification de la valeur par rapport au type de ressource, aucune action n&#39;est entreprise.

   >[!NOTE]
   >
   > Si vous ne voyez aucun journal de console, vérifiez que **Consignation de la console** est cochée sous **Lancement** dans le débogueur Experience Platform.

## Félicitations ! 

Vous venez d&#39;utiliser la couche de données client Adobe et l&#39;Experience Platform Launch orientés événement pour effectuer le suivi des clics de composants spécifiques sur un site Adobe Experience Manager.