---
title: Suivre les éléments cliqués avec Adobe Analytics
description: Utilisez la couche de données client Adobe orientée sur les événements pour effectuer le suivi des clics effectués sur des composants spécifiques sur un site Adobe Experience Manager. Découvrez comment utiliser les règles de balise pour écouter ces événements et envoyer des données à une suite de rapports Adobe Analytics à l’aide d’une balise de lien de suivi.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
badgeIntegration: label="Intégration" type="positive"
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: ht
source-wordcount: '1886'
ht-degree: 100%

---

# Suivre les éléments cliqués avec Adobe Analytics

>[!NOTE]
>
>Adobe Experience Platform Launch est mort, vive les suites de collecte de données dans Adobe Experience Platform. Plusieurs modifications terminologiques ont par conséquent été apportées à la documentation du produit. Consultez ce [document](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?lang=fr) qui recense les modifications de la terminologie.

Utilisez la [Couche de données client Adobe orientée sur les événements avec les composants principaux d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr) pour effectuer le suivi des clics effectués sur des composants spécifiques sur un site Adobe Experience Manager. Découvrez comment utiliser des règles dans les propriétés de balise pour écouter les événements de clics, filtrer par composant et envoyer les données à une instance Adobe Analytics avec une balise de lien de suivi.

## Ce que vous allez créer {#what-build}

L’équipe marketing de WKND souhaite savoir quels boutons `Call to Action (CTA)` sont les plus performants sur la page d’accueil. Dans ce tutoriel, une règle est ajoutée à la propriété de balise qui écoute les événements `cmp:click` des composants **Teaser** et **Bouton**. Envoyez ensuite l’identifiant du composant et un nouvel événement à Adobe Analytics, avec la balise de lien de suivi.

![Ce que vous allez créer pour les suivis de clics](assets/track-clicked-component/final-click-tracking-cta-analytics.png).

### Objectifs {#objective}

1. Créez une règle basée sur les événements dans la propriété de balise qui capture l’événement `cmp:click`.
1. Filtrez les différents événements par type de ressource de composant.
1. Définissez l’identifiant du composant et envoyez un événement à Adobe Analytics avec la balise de lien de suivi.

## Conditions préalables

Avant de suivre ce tutoriel, qui est la suite de [Collecter les données de page avec Adobe Analytics](./collect-data-analytics.md), assurez-vous d’avoir configuré les éléments suivants :

* Une **propriété de balise** avec l’[extension Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=fr) activée.
* Un identifiant de la suite de rapports de test/développement **Adobe Analytics** et un serveur de suivi. Consultez la documentation suivante pour [créer une suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=fr).
* L’extension de navigateur [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=fr) configurée avec votre propriété de balise chargée sur le [site WKND](https://wknd.site/fr/fr.html) ou un site AEM avec la couche de données Adobe activée.

## Examiner le schéma du bouton et du teaser

Avant de créer des règles dans la propriété de balise, consultez le [schéma du bouton et du teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#item) et examinez-les dans l’implémentation de la couche de données.

1. Accédez à la [page d’accueil WKND](https://wknd.site/fr/fr.html).
1. Ouvrez les outils de développement du navigateur et accédez à la **console**. Exécutez la commande suivante :

   ```js
   adobeDataLayer.getState();
   ```

   Le code ci-dessus renvoie le statut en cours de la couche de données client Adobe.

   ![Statut de la couche de données via la console du navigateur](assets/track-clicked-component/adobe-data-layer-state-browser.png).

1. Développez la réponse et recherchez les entrées affectées du préfixe `button-` et l’entrée `teaser-xyz-cta`. Un schéma de données similaire à celui-ci doit s’afficher :

   Schéma de bouton :

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Schéma du teaser :

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Les données ci-dessus sont basées sur le [Schéma d’élément Composant/Conteneur](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#item). La nouvelle règle de balise utilise ce schéma.

## Créer une règle CTA sur laquelle un utilisateur ou une utilisatrice a cliqué

La couche de données client Adobe est basée sur les **événements**. Lorsque l’on clique sur un composant de base, un événement `cmp:click` est distribué via la couche de données. Pour écouter l’événement `cmp:click`, créons une règle.

1. Accédez à Experience Platform et à la propriété de balise intégrée au site AEM.
1. Accédez à la section **Règles** dans l’interface utilisateur de la propriété de balise, puis cliquez sur **Ajouter une règle**.
1. Nommez la règle **CTA Clicked** (CTA cliqué).
1. Cliquez sur **Événements** > **Ajouter** pour ouvrir l’assistant **Configuration d’événement**.
1. Pour le champ **Type d’événement**, sélectionnez **Code personnalisé**.

   ![Nommage de la règle CTA Clicked (CTA cliqué) et ajout de l’événement de code personnalisé.](assets/track-clicked-component/custom-code-event.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez l’extrait de code suivant :

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
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

   L’extrait de code ci-dessus ajoute un écouteur d’événement en [envoyant une fonction](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) dans la couche de données. Lorsque l’événement `cmp:click` est déclenché, la fonction `componentClickedHandler` est appelée. Dans cette fonction, quelques contrôles d’intégrité sont ajoutés et un nouvel objet `event` est construit avec le dernier [état de la couche de données](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) pour le composant qui a déclenché l’événement.

   Enfin, la fonction `trigger(event)` est appelée. La fonction `trigger()` est un nom réservé dans la propriété de balise et elle **déclenche** la règle. L’objet `event` est transmis en tant que paramètre qui, à son tour, est exposé par un autre nom réservé dans la propriété de balise. Les éléments de données dans la propriété de balise peuvent désormais référencer diverses propriétés avec un extrait de code du type `event.component['someKey']`.

1. Enregistrez les modifications.
1. Ensuite, sous **Actions**, cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration de l’action**.
1. Pour le champ **Type d’action**, choisissez **Code personnalisé**.

   ![Type d’action Code personnalisé.](assets/track-clicked-component/action-custom-code.png)

1. Cliquez sur **Ouvrir l’éditeur** dans le panneau principal et saisissez l’extrait de code suivant :

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   L’objet `event` est transmis à partir de la méthode `trigger()` appelée dans l’événement personnalisé. L’objet `component` correspond à l’état actuel du composant dérivé de la méthode `getState()` de la couche de données et est l’élément qui a déclenché le clic.

1. Enregistrez les modifications et exécutez une [création](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html?lang=fr) dans la propriété de balise pour promouvoir le code vers l’[environnement](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=fr) utilisé sur votre site AEM.

   >[!NOTE]
   >
   > Il peut s’avérer utile d’utiliser [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=fr) pour changer le code incorporé dans un environnement de **développement**.

1. Accédez au [site WKND](https://wknd.site/fr/fr.html) et ouvrez les outils de développement pour afficher la console. Sélectionnez également la case à cocher **Conserver le journal**.

1. Cliquez sur l’un des boutons CTA **Teaser** ou **Bouton** pour accéder à une autre page.

   ![Bouton CTA à cliquer.](assets/track-clicked-component/cta-button-to-click.png)

1. Dans la Developer Console, on peut voir que la règle **CTA Clicked** (CTA cliqué) a été déclenchée :

   ![Bouton CTA cliqué.](assets/track-clicked-component/cta-button-clicked-log.png)

## Créer des éléments de données

Créez ensuite des éléments de données pour capturer l’identifiant de composant et le titre sur lequel l’utilisateur ou l’utilisatrice a cliqué. Rappelez-vous que dans l’exercice précédent, la sortie de `event.path` était comparable à `component.button-b6562c963d` et la valeur de `event.component['dc:title']` ressemblait à « View Trips ».

### ID de composant

1. Accédez à Experience Platform et à la propriété de balise intégrée au site AEM.
1. Accédez à la section **Éléments de données** et cliquez sur **Ajouter un élément de données**.
1. Dans le champ **Nom**, entrez l’**identifiant du composant**.
1. Dans le champ **Type d’élément de données**, sélectionnez **Code personnalisé**.

   ![Formulaire d’élément de données d’identifiant de composant.](assets/track-clicked-component/component-id-data-element.png)

1. Cliquez sur le bouton **Ouvrir l’éditeur** et saisissez ce qui suit dans l’éditeur de code personnalisé :

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. Enregistrez les modifications.

   >[!NOTE]
   >
   > Rappelons que l’objet `event` devient disponible et est défini en fonction de l’événement qui a déclenché la **Règle** dans la propriété de balise. La valeur d’un élément de données n’est pas définie tant que l’élément de données n’est pas *référencé* dans une règle. Il est donc possible d’utiliser cet élément de données dans une règle telle que **Chargement de page** créée à l’étape précédente *mais* il ne serait pas judicieux de l’utiliser dans d’autres contextes.


### Titre du composant

1. Accédez à la section **Éléments de données** et cliquez sur **Ajouter un nouvel élément de données**.
1. Dans le champ **Nom**, indiquez le **Titre du composant**.
1. Dans le champ **Type d’élément de données**, sélectionnez **Code personnalisé**.
1. Cliquez sur le bouton **Ouvrir l’éditeur** et saisissez le code suivant dans l’éditeur de code personnalisé :

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Enregistrez les modifications.

## Ajouter une condition à la règle CTA Clicked (CTA cliqué)

Mettez ensuite à jour la règle **CTA Clicked** (CTA cliqué) pour vous assurer que la règle ne se déclenche que si l’événement `cmp:click` se produit pour un **Teaser** ou un **Bouton**. Comme le bouton CTA Teaser est considéré comme un objet distinct dans la couche de données, il est important de vérifier le parent pour s’assurer qu’il provient d’un teaser.

1. Dans l’interface utilisateur de la propriété de balise, accédez à la règle **CTA Clicked** (CTA cliqué) créée précédemment.
1. Sous **Conditions**, cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration de condition**.
1. Dans le champ **Type de condition**, sélectionnez **Code personnalisé**.

   ![Code personnalisé de condition pour la règle CTA Clicked (CTA cliqué)](assets/track-clicked-component/custom-code-condition.png).

1. Cliquez sur **Ouvrir l’éditeur** et saisissez le code suivant dans l’éditeur de code personnalisé :

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

   Le code ci-dessus vérifie d’abord si le type de ressource provient d’un **Bouton** ou si le type de ressource provient d’un appel à l’action (CTA) présent sur un **Teaser**.

1. Enregistrez les modifications.

## Définir les variables Analytics et déclencher la balise de lien de suivi

Actuellement, la règle **CTA Clicked** (CTA cliqué) génère simplement une instruction console. Utilisez ensuite les éléments de données et l’extension Analytics pour définir les variables Analytics en tant qu’**action**. Définissons également une action supplémentaire pour déclencher la balise **Lien de suivi** et envoyer les données collectées à Adobe Analytics.

1. Dans la règle **CTA Clicked** (CTA cliqué), **supprimez** l’action **Principal - Code personnalisé** (les instructions console) :

   ![Suppression d’une action de code personnalisé](assets/track-clicked-component/remove-console-statements.png).

1. Sous Actions, cliquez sur **Ajouter** pour créer une action.
1. Définissez le type **Extension** sur **Adobe Analytics** et le **Type d’action** sur  **Définir des variables**.

1. Définissez les valeurs suivantes pour les **eVars**, **Props** et **Événements** :

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Définition des eVars, props et événements](assets/track-clicked-component/set-evar-prop-event.png).

   >[!NOTE]
   >
   > `%Component ID%` est utilisé ici, car il garantit un identifiant unique pour le bouton CTA sur lequel l’utilisateur ou l’utilisatrice a cliqué. Un inconvénient potentiel de l’utilisation de `%Component ID%` est que le rapport Analytics contient des valeurs telles que `button-2e6d32893a`. En utilisant le `%Component Title%`, le nom sonnerait plus humain, mais la valeur risque de ne pas être unique.

1. Ajoutez ensuite une action supplémentaire à droite d’**Adobe Analytics - Définir des variables** en appuyant sur l’icône **plus** :

   ![Ajout d’une action supplémentaire à la règle de balise](assets/track-clicked-component/add-additional-launch-action.png).

1. Définissez le type **Extension** sur **Adobe Analytics** et le **Type d’action** sur **Envoyer la balise**.
1. Sous **Suivi**, définissez le bouton radio sur **`s.tl()`**.
1. Dans le champ **Type de lien**, choisissez **Lien personnalisé** et dans **Nom du lien**, définissez la valeur sur **`%Component Title%: CTA Clicked`** :

   ![Configuration de la balise Envoyer le lien](assets/track-clicked-component/analytics-send-beacon-link-track.png).

   La configuration ci-dessus combine la variable dynamique de l’élément de données **Titre du composant** et la chaîne statique **CTA cliqué**.

1. Enregistrez les modifications. La règle **CTA cliqué** doit maintenant avoir la configuration suivante :

   ![Configuration finale de la règle de balise](assets/track-clicked-component/final-page-loaded-config.png).

   * **1.** Écoutez l’événement `cmp:click`.
   * **2.** Vérifiez que l’événement a été déclenché par un **Bouton** ou un **Teaser**.
   * **3.** Définissez les variables Analytics pour suivre l’**Identifiant du composant** en tant qu’**eVar**, **prop** et **événement**.
   * **4.** Envoyez la balise de lien de suivi Analytics (et ne la considérez **pas** comme une page vue).

1. Enregistrez toutes les modifications et créez votre bibliothèque de balises, en effectuant la promotion vers l’environnement approprié.

## Valider la balise du lien de suivi et l’appel d’Analytics

Maintenant que la règle **CTA cliqué** envoie la balise Analytics. Vous devriez pouvoir voir les variables de suivi d’Analytics à l’aide du Debugger Experience Platform.

1. Ouvrez le [Site WKND](https://wknd.site/fr/fr.html) dans votre navigateur.
1. Cliquez sur l’![icône du Debugger Experience Platform](assets/track-clicked-component/experience-cloud-debugger.png) pour ouvrir le Debugger Experience Platform.
1. Assurez-vous que le Debugger mappe la propriété de balise pour *votre* environnement de développement, comme décrit précédemment et que l’option **Journalisation de la console** est activée.
1. Ouvrez le menu Analytics et vérifiez que la suite de rapports est définie sur *votre* suite de rapports.

   ![Debugger de l’onglet Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. Dans le navigateur, cliquez sur l’un des boutons CTA **Teaser** ou **Bouton** pour accéder à une autre page.

   ![Bouton CTA sur lequel cliquer](assets/track-clicked-component/cta-button-to-click.png)

1. Revenez au Debugger Experience Platform, faites défiler l’écran vers le bas et développez **Requêtes réseau** > *Votre suite de rapports*. Vous devriez être en mesure de trouver les ensembles **eVar**, **prop** et **event**.

   ![Événements Analytics, evar et prop suivis lors du clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Revenez au navigateur et ouvrez la Developer Console. Accédez au pied de page du site et cliquez sur l’un des liens de navigation :

   ![Cliquez sur le lien Navigation dans le pied de page.](assets/track-clicked-component/click-navigation-link-footer.png)

1. Lisez le message dans la console du navigateur *« Code personnalisé » pour la règle « CTA cliqué » n’a pas été respecté*.

   Le message ci-dessus est dû au fait que le composant Navigation déclenche un `cmp:click`événement *, mais* en raison de la [Condition à la règle](#add-a-condition-to-the-cta-clicked-rule) qui vérifie le type de ressource, aucune action n’est effectuée.

   >[!NOTE]
   >
   > Si vous ne voyez aucun journal dans la console, assurez-vous que l’option **Journalisation de la console** est activée sous **Balises Experience Platform** dans le Debugger Experience Platform.

## Félicitations.

Vous venez d’utiliser la couche de données client et la balise Adobe pilotée par l’événement dans Experience Platform pour effectuer le suivi des clics sur des composants spécifiques d’un site AEM.
