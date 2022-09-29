---
title: Suivi des composants cliqués avec Adobe Analytics
description: Utilisez la couche de données client Adobe orientée événement pour effectuer le suivi des clics sur des composants spécifiques d’un site Adobe Experience Manager. Découvrez comment utiliser des règles dans Experience Platform Launch pour écouter ces événements et envoyer des données à une Adobe Analytics avec une balise de lien de suivi.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1806'
ht-degree: 4%

---

# Suivi des composants cliqués avec Adobe Analytics

Utilisation de l’événement [Adobe de la couche de données client avec les composants principaux AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr) pour effectuer le suivi des clics sur des composants spécifiques d’un site Adobe Experience Manager. Découvrez comment utiliser des règles dans Experience Platform Launch pour écouter les événements de clics, filtrer par composant et envoyer les données à une instance Adobe Analytics avec une balise de lien de suivi.

## Ce que vous allez créer

L’équipe marketing WKND souhaite déterminer les boutons CTA (Appel à l’action) les plus performants sur la page d’accueil. Dans ce tutoriel, nous allons ajouter une nouvelle règle dans Experience Platform Launch qui écoute les `cmp:click` des événements de **Teaser** et **Bouton** composants et envoyez l’identifiant du composant et un nouvel événement à Adobe Analytics à côté de la balise de lien de suivi.

![Ce que vous allez créer pour les clics de suivi](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objectifs {#objective}

1. Créez une règle basée sur un événement dans Launch. `cmp:click` .
1. Filtrez les différents événements par type de ressource de composant.
1. Définissez l’identifiant du composant sur lequel l’utilisateur a cliqué et envoyez l’événement Adobe Analytics avec la balise de lien de suivi.

## Prérequis

Ce tutoriel est une suite [Collecte de données de page avec Adobe Analytics](./collect-data-analytics.md) et suppose que vous avez :

* A **Propriété Launch** avec le [Extension Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html?lang=fr) enabled
* **Adobe Analytics** identifiant de suite de rapports test/dev et serveur de suivi. Consultez la documentation suivante pour [création d’une suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Débogueur Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) extension de navigateur configurée avec votre propriété Launch chargée sur [https://wknd.site/us/en.html](https://wknd.site/us/en.html) ou un site AEM avec la couche de données d’Adobe activée.

## Inspect du schéma de bouton et de teaser

Avant d’établir des règles dans Launch, il est utile de consulter la section [schéma du bouton et du teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) et inspectez-les dans l’implémentation de la couche de données.

1. Accédez à [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Ouvrez les outils de développement du navigateur et accédez au **Console**. Exécutez la commande suivante :

   ```js
   adobeDataLayer.getState();
   ```

   Cette opération renvoie l’état actuel de la couche de données client Adobe.

   ![État de la couche de données via la console du navigateur](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Développez la réponse et recherchez les entrées affectées du préfixe `button-` et  `teaser-xyz-cta` entrée . Vous devriez voir un schéma de données comme suit :

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

   Ils reposent sur la variable [Schéma d’élément de composant/conteneur](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). La règle que nous allons créer dans Launch utilisera ce schéma.

## Création d’une règle CTA sélectionnée

La couche de données client Adobe est une **event** couche de données pilotée. Lorsque vous cliquez sur un composant principal . `cmp:click` est distribué via la couche de données. Créez ensuite une règle pour écouter le `cmp:click` .

1. Accédez à Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez au **Règles** dans l’interface utilisateur de Launch, puis cliquez sur **Ajouter une règle**.
1. Attribuer un nom à la règle **CTA cliqué**.
1. Cliquez sur **Événements** > **Ajouter** pour ouvrir le **Configuration d’événement** assistant.
1. Sous **Type d’événement** select **Code personnalisé**.

   ![Nommez la règle CTAS cliqué et ajoutez l’événement de code personnalisé](assets/track-clicked-component/custom-code-event.png)

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

   Le fragment de code ci-dessus ajoute un écouteur d’événement par [publication d’une fonction](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) dans la couche de données. Lorsque la variable `cmp:click` est déclenché. `componentClickedHandler` est appelée. Dans cette fonction, quelques contrôles d’intégrité sont ajoutés et un nouveau `event` est créé avec la dernière [état de la couche de données](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) pour le composant qui a déclenché l’événement.

   Après cela `trigger(event)` est appelée. `trigger()` est un nom réservé dans Launch et &quot;déclenche&quot; la règle Launch. Nous passons la `event` comme paramètre qui, à son tour, est exposé par un autre nom réservé dans Launch nommé `event`. Les éléments de données de Launch peuvent désormais référencer diverses propriétés comme suit : `event.component['someKey']`.

1. Enregistrez les modifications.
1. Suivant sous **Actions** click **Ajouter** pour ouvrir le **Configuration d’action** assistant.
1. Sous **Type d’action** select **Code personnalisé**.

   ![Type d’action Custom Code](assets/track-clicked-component/action-custom-code.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez le fragment de code suivant :

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   Le `event` est transmis à partir de `trigger()` appelée dans l’événement personnalisé. `component` est l’état actuel du composant dérivé de la couche de données. `getState` qui a déclenché le clic.

1. Enregistrez les modifications et exécutez une [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) dans Launch pour convertir le code en [environnement](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) utilisé sur votre site AEM.

   >[!NOTE]
   >
   > Il peut s’avérer très utile d’utiliser la variable [Débogueur Adobe Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) pour changer le code incorporé en **Développement** environnement.

1. Accédez au [Site WKND](https://wknd.site/us/en.html) et ouvrez les outils de développement pour afficher la console. Sélectionner **Conserver le journal**.

1. Cliquez sur l’une des **Teaser** ou **Bouton** Boutons CTA pour accéder à une autre page.

   ![Bouton CTA à cliquer](assets/track-clicked-component/cta-button-to-click.png)

1. Observez dans la console de développement que la variable **CTA cliqué** a été déclenchée :

   ![Bouton CTA cliqué](assets/track-clicked-component/cta-button-clicked-log.png)

## Création d’éléments de données

Créez ensuite un élément de données pour capturer l’identifiant et le titre du composant sur lequel l’utilisateur a cliqué. Rappelez dans l’exercice précédent le résultat de `event.path` était similaire à `component.button-b6562c963d` et la valeur de `event.component['dc:title']` C’était un peu comme &quot;View Trips&quot;.

### Identifiant du composant

1. Accédez à Experience Platform Launch et à la propriété Web intégrée au site AEM.
1. Accédez au **Éléments de données** et cliquez sur **Ajouter un élément de données**.
1. Pour **Nom** enter **Identifiant du composant**.
1. Pour **Type d’élément de données** select **Code personnalisé**.

   ![Formulaire d’élément de données d’identifiant de composant](assets/track-clicked-component/component-id-data-element.png)

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
   > Rappelez-vous que la variable `event` est rendu disponible et défini sur la portée en fonction de l’événement qui a déclenché la variable **Règle** dans Launch. La valeur d’un élément de données n’est pas définie tant que l’élément de données n’est pas *référencé* dans une règle. Par conséquent, il est sans risque d’utiliser cet élément de données à l’intérieur d’une règle comme la variable **CTA cliqué** règle créée dans l’exercice précédent *mais* ne serait pas utilisable en toute sécurité dans d’autres contextes.

### Libellé du composant 

1. Accédez au **Éléments de données** et cliquez sur **Ajouter un élément de données**.
1. Pour **Nom** enter **Titre du composant**.
1. Pour **Type d’élément de données** select **Code personnalisé**.
1. Cliquez sur **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Enregistrez les modifications.

## Ajouter une condition à la règle Cliqué CTA

Mettez ensuite à jour la variable **CTA cliqué** pour s’assurer que la règle ne se déclenche que lorsque la variable `cmp:click` est déclenché pour un événement **Teaser** ou **Bouton**. Étant donné que le CTA du teaser est considéré comme un objet distinct dans la couche de données, il est important de vérifier le parent pour vérifier qu’il provient d’un teaser.

1. Dans l’interface utilisateur de Launch, accédez au **CTA cliqué** règle créée précédemment.
1. Sous **Conditions** click **Ajouter** pour ouvrir le **Configuration de condition** assistant.
1. Pour **Type de condition** select **Code personnalisé**.

   ![Code personnalisé de condition cliquée CTA](assets/track-clicked-component/custom-code-condition.png)

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

   Le code ci-dessus vérifie d’abord si le type de ressource provient d’une **Bouton** puis vérifie si le type de ressource provient d’une CTA au sein d’une **Teaser**.

1. Enregistrez les modifications.

## Définition des variables Analytics et déclenchement de la balise Lien de suivi

Actuellement, la variable **CTA cliqué** génère simplement une instruction console. Ensuite, utilisez les éléments de données et l’extension Analytics pour définir les variables Analytics en tant que **action**. Nous définirons également une action supplémentaire pour déclencher la variable **Suivi du lien** et envoyer les données collectées à Adobe Analytics.

1. Dans le **CTA cliqué** règle **remove** la valeur **Core - Code personnalisé** action (les instructions de la console) :

   ![Suppression d’une action de code personnalisé](assets/track-clicked-component/remove-console-statements.png)

1. Sous Actions, cliquez sur **Ajouter** pour ajouter une nouvelle action.
1. Définissez la variable **Extension** saisir **Adobe Analytics** et définissez la variable **Type d’action** to  **Définition de variables**.

1. Définissez les valeurs suivantes pour **eVars**, **Propriétés**, et **Événements**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Définition des props et événements d’eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Ici `%Component ID%` est utilisé, car il garantit un identifiant unique pour l’CTA sur lequel l’utilisateur a cliqué. Un inconvénient potentiel de l’utilisation de `%Component ID%` est que le rapport Analytics contiendra des valeurs telles que `button-2e6d32893a`. Utilisation `%Component Title%` donnerait un nom plus humain, mais la valeur pourrait ne pas être unique.

1. Ajoutez ensuite une action supplémentaire à droite du **Adobe Analytics - Définition de variables** en appuyant sur le bouton **plus** icon :

   ![Ajout d’une action de lancement supplémentaire](assets/track-clicked-component/add-additional-launch-action.png)

1. Définissez la variable **Extension** saisir **Adobe Analytics** et définissez la variable **Type d’action** to  **Envoyer la balise**.
1. Sous **Tracking** Définir le bouton radio sur **`s.tl()`**.
1. Pour **Type de lien** select **Lien personnalisé** et **Nom du lien** définissez la valeur sur : **`%Component Title%: CTA Clicked`**:

   ![Configuration de la balise Send Link](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Cela combinera la variable dynamique de l’élément de données. **Titre du composant** et la chaîne statique **CTA cliqué**.

1. Enregistrez les modifications. Le **CTA cliqué** doit maintenant avoir la configuration suivante :

   ![Configuration finale du lancement](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Écoute de la `cmp:click` .
   * **2.** Vérifiez que l’événement a été déclenché par une **Bouton** ou **Teaser**.
   * **3.** Définissez des variables Analytics pour pour effectuer le suivi de la variable **Identifiant du composant** as a **eVar**, **prop** et un **event**.
   * **4.** Envoyez la balise de lien de suivi Analytics (et procédez comme suit : **not** le traiter comme une page vue).

1. Enregistrez toutes les modifications et créez votre bibliothèque Launch, en effectuant la promotion vers l’environnement approprié.

## Validation de la balise de lien de suivi et de l’appel Analytics

Maintenant que la variable **CTA cliqué** envoie la balise Analytics. Vous devriez pouvoir voir les variables de suivi Analytics à l’aide du débogueur Experience Platform.

1. Ouvrez le [Site WKND](https://wknd.site/us/en.html) dans votre navigateur.
1. Cliquez sur l’icône Debugger . ![Icône du débogueur Experience Platform](assets/track-clicked-component/experience-cloud-debugger.png) pour ouvrir le débogueur Experience Platform.
1. Assurez-vous que le débogueur mappe la propriété Launch à *your* Environnement de développement, comme décrit précédemment et **Journalisation de la console** est cochée.
1. Ouvrez le menu Analytics et vérifiez que la suite de rapports est définie sur *your* suite de rapports.

   ![Débogueur de l’onglet Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. Dans le navigateur, cliquez sur l’une des **Teaser** ou **Bouton** Boutons CTA pour accéder à une autre page.

   ![Bouton CTA à cliquer](assets/track-clicked-component/cta-button-to-click.png)

1. Revenez au débogueur Experience Platform, faites défiler l’écran vers le bas et développez . **Requêtes réseau** > *Votre suite de rapports*. Vous devriez être en mesure de trouver la variable **eVar**, **prop**, et **event** définie.

   ![Événements Analytics, evar et prop suivis lors du clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Revenez au navigateur et ouvrez la console de développement. Accédez au pied de page du site et cliquez sur l’un des liens de navigation :

   ![Cliquez sur le lien Navigation dans le pied de page.](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observez le message dans la console du navigateur. *&quot;Code personnalisé&quot; pour la règle &quot;CTA cliqué&quot; n’a pas été respectée*.

   En effet, le composant Navigation ne déclenche pas de `cmp:click` event *mais* en raison de la vérification du par rapport au type de ressource, aucune action n’est effectuée.

   >[!NOTE]
   >
   > Si vous ne voyez aucun journal de la console, assurez-vous que **Journalisation de la console** est coché sous **Launch** dans le débogueur Experience Platform.

## Félicitations !

Vous venez d’utiliser la couche de données client et l’Experience Platform Launch Adobe pilotés par les événements pour effectuer le suivi des clics sur des composants spécifiques d’un site Adobe Experience Manager.
