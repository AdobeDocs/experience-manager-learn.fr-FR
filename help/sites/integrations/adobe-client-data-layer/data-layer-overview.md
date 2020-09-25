---
title: Utilisation de la couche de données du client Adobe avec les composants principaux AEM
description: La couche de données du client Adobe introduit une méthode standard pour collecter et stocker des données sur une expérience visiteuse sur une page Web, puis faciliter l'accès à ces données. La couche de données client Adobe est indépendante de la plate-forme, mais elle est entièrement intégrée aux composants principaux pour l’utilisation avec AEM.
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
translation-type: tm+mt
source-git-commit: e13a5171fbeb9e1eb5f78d1c691bc8b4b896a998
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 6%

---


# Using the Adobe Client Data Layer with AEM Core Components {#overview}

La couche de données du client Adobe introduit une méthode standard pour collecter et stocker des données sur une expérience visiteuse sur une page Web, puis faciliter l&#39;accès à ces données. La couche de données client Adobe est indépendante de la plate-forme, mais elle est entièrement intégrée aux composants principaux pour l’utilisation avec AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Souhaitez-vous activer la couche de données du client Adobe sur votre site AEM ? [Voir les instructions ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Explorer la couche de données

Vous pouvez vous faire une idée de la fonctionnalité intégrée de la couche de données du client Adobe en utilisant les outils de développement de votre navigateur et le site [de référence](https://wknd.site/)WKND actif.

>[!NOTE]
>
> Captures d’écran ci-dessous extraites du navigateur Chrome.

1. Accédez à [https://wknd.site](https://wknd.site)
1. Ouvrez vos outils de développement et saisissez la commande suivante dans la **console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   inspect la réponse pour afficher l’état actuel de la couche de données sur un site AEM. Vous devriez voir des informations sur la page et les composants individuels.

   ![Réponse de la couche de données d&#39;Adobe](assets/data-layer-state-response.png)

1. Poussez un objet de données vers la couche de données en saisissant les éléments suivants dans la console :

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. Exécutez à nouveau la commande `adobeDataLayer.getState()` et recherchez l&#39;entrée pour `training-data`.
1. Ajoutez ensuite un paramètre de chemin pour renvoyer uniquement l’état spécifique d’un composant :

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Ne retourner qu&#39;une seule entrée de données de composant](assets/return-just-single-component.png)

## Utilisation de Événements

Il est recommandé de déclencher tout code personnalisé basé sur un événement de la couche de données. Ensuite, explorez l’enregistrement et l’écoute de différents événements.

1. Saisissez la méthode d’assistance suivante dans votre console :

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   Le code ci-dessus inspecte l&#39; `event` objet et utilise la `adobeDataLayer.getState` méthode pour obtenir l&#39;état actuel de l&#39;objet qui a déclenché le événement. La méthode de l&#39;aide inspectera alors les `filter` critères et elle ne sera renvoyée que si le filtre `dataObject` satisfait au filtre.

   >[!CAUTION]
   >
   > Il sera important de **ne pas** actualiser le navigateur tout au long de cet exercice, sinon le code JavaScript de la console sera perdu.

1. Ensuite, saisissez un gestionnaire de événements qui sera appelé lorsqu’un composant **Teaser** s’affiche dans un **carrousel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Le `teaserShownHandler` système appelle la `getDataObjectHelper` méthode et transmet un filtre de `wknd/components/teaser` comme le pour `@type` filtrer les événements déclenchés par d&#39;autres composants.

1. Ensuite, poussez un écouteur de événement sur la couche de données pour écouter le `cmp:show` événement.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   Le `cmp:show` événement est déclenché par de nombreux composants différents, comme lorsqu&#39;une nouvelle diapositive s&#39;affiche dans le **carrousel** ou lorsqu&#39;un nouvel onglet est sélectionné dans le composant **Tabulation** .

1. Sur la page, basculez les diapositives du carrousel et observez les instructions de la console :

   ![Basculer vers le carrousel et afficher l’écouteur de événement](assets/teaser-console-slides.png)

1. Supprimez l’écouteur de événement de la couche de données pour arrêter l’écoute du `cmp:show` événement :

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Revenez à la page et faites basculer les diapositives du carrousel. Observez que plus aucune déclaration n&#39;est consignée et que le événement n&#39;est pas écouté.

1. Ensuite, saisissez un gestionnaire de événements qui sera appelé lorsque le événement de la page affichée est déclenché :

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Notez que le type de ressource `wknd/components/page` est utilisé pour filtrer le événement.

1. Ensuite, poussez un écouteur de événement sur la couche de données pour écouter le `cmp:show` événement, en appelant le `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Une instruction de console doit immédiatement s&#39;afficher déclenchée avec les données de page :

   ![Données d’affichage de page](assets/page-show-console-data.png)

   Le `cmp:show` événement de la page est déclenché à chaque chargement de page tout en haut de la page. Vous pouvez demander pourquoi le gestionnaire de événements a-t-il été déclenché alors que la page a clairement déjà été chargée ?

   Il s&#39;agit de l&#39;une des caractéristiques uniques de la couche de données du client Adobe, en ce sens que vous pouvez enregistrer les écouteurs de événement **avant** ou **après** l&#39;initialisation de la couche de données. Il s&#39;agit là d&#39;une fonction essentielle pour éviter les conditions de concurrence.

   La couche de données conserve un tableau de files d&#39;attente de tous les événements qui se sont produits dans l&#39;ordre. La couche de données déclenche par défaut des rappels de événement pour les événements qui se sont produits dans le **passé** ainsi que pour les événements dans le **futur**. Il est possible de filtrer les événements au-delà ou à l&#39;avenir. [Vous trouverez plus d&#39;informations dans la documentation](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Étapes suivantes

Consultez le didacticiel suivant pour savoir comment utiliser la couche de données client Adobe orientée événement pour [collecter des données de page et les envoyer à Adobe Analytics](../analytics/collect-data-analytics.md).


## Ressources supplémentaires {#additional-resources}

* [Documentation de la couche de données du client Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Utilisation de la couche de données du client Adobe et de la documentation des composants principaux](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
