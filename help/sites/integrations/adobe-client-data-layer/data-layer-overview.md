---
title: Utilisation de la couche de données client Adobe avec les composants principaux AEM
description: La couche de données client Adobe introduit une méthode standard pour collecter et stocker des données sur une expérience visiteur sur une page web, puis faciliter l’accès à ces données. La couche de données client Adobe est indépendante de la plate-forme, mais elle est entièrement intégrée aux composants principaux pour l’utilisation avec AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 8%

---

# Utilisation de la couche de données client Adobe avec les composants principaux AEM {#overview}

La couche de données client Adobe introduit une méthode standard pour collecter et stocker des données sur une expérience visiteur sur une page web, puis faciliter l’accès à ces données. La couche de données client Adobe est indépendante de la plate-forme, mais elle est entièrement intégrée aux composants principaux pour l’utilisation avec AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Souhaitez-vous activer la couche de données client Adobe sur votre site AEM ? [Voir les instructions ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Exploration de la couche de données

Vous pouvez vous faire une idée des fonctionnalités intégrées de la couche de données client Adobe en utilisant simplement les outils de développement de votre navigateur et les outils en ligne. [Site de référence WKND](https://wknd.site/).

>[!NOTE]
>
> Captures d’écran ci-dessous extraites du navigateur Chrome.

1. Accédez à [https://wknd.site](https://wknd.site)
1. Ouvrez vos outils de développement et saisissez la commande suivante dans le **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect la réponse pour afficher l’état actuel de la couche de données sur un site AEM. Vous devriez voir des informations sur la page et les composants individuels.

   ![Réponse de la couche de données d’Adobe](assets/data-layer-state-response.png)

1. Envoyez un objet de données à la couche de données en saisissant les éléments suivants dans la console :

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

1. Exécutez la commande `adobeDataLayer.getState()` de nouveau et recherchez l’entrée pour `training-data`.
1. Ajoutez ensuite un paramètre de chemin pour renvoyer uniquement l’état spécifique d’un composant :

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Renvoie une seule entrée de données de composant](assets/return-just-single-component.png)

## Utilisation des événements

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

   Le code ci-dessus inspectera la variable `event` et utilisez l’objet `adobeDataLayer.getState` pour obtenir l’état actuel de l’objet qui a déclenché l’événement. La méthode d’assistance inspecte alors le `filter` et uniquement si la variable active `dataObject` correspond au filtre qui sera renvoyé.

   >[!CAUTION]
   >
   > C&#39;est important **not** pour actualiser le navigateur tout au long de cet exercice, sinon le code JavaScript de la console est perdu.

1. Ensuite, saisissez un gestionnaire d’événements appelé lors d’une **Teaser** s’affiche dans une **Carrousel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Le `teaserShownHandler` appellera la variable `getDataObjectHelper` et transmettre un filtre de `wknd/components/teaser` comme la propriété `@type` pour filtrer les événements déclenchés par d’autres composants.

1. Ensuite, placez un écouteur d’événement sur la couche de données pour écouter le rapport `cmp:show` .

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   Le `cmp:show` est déclenché par de nombreux composants différents, comme lorsqu’une nouvelle diapositive s’affiche dans la variable **Carrousel** ou lorsqu’un nouvel onglet est sélectionné dans le **Onglet** composant.

1. Sur la page, faites basculer les diapositives du carrousel et observez les instructions de la console :

   ![Activer/désactiver le carrousel et afficher l’écouteur d’événement](assets/teaser-console-slides.png)

1. Supprimez l’écouteur d’événement de la couche de données pour arrêter l’écoute de la variable `cmp:show` event:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Revenez à la page et faites basculer les diapositives du carrousel. Notez qu’aucune autre instruction n’est consignée et que l’événement n’est pas écouté.

1. Créez ensuite un gestionnaire d’événements appelé lorsque l’événement de page affichée est déclenché :

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Notez que le type de ressource `wknd/components/page` est utilisé pour filtrer l’événement.

1. Ensuite, placez un écouteur d’événement sur la couche de données pour écouter le rapport `cmp:show` , en appelant la fonction `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Une instruction console doit immédiatement être déclenchée avec les données de page :

   ![Données d’affichage de page](assets/page-show-console-data.png)

   Le `cmp:show` pour la page est déclenché à chaque chargement de page tout en haut de la page. Vous pouvez demander pourquoi le gestionnaire d’événements a-t-il été déclenché alors que la page a clairement déjà été chargée ?

   Il s’agit de l’une des fonctionnalités uniques de la couche de données client Adobe, en ce sens que vous pouvez enregistrer les écouteurs d’événement. **before** ou **after** la couche de données a été initialisée. Il s’agit d’une fonctionnalité essentielle pour éviter les conditions de concurrence.

   La couche de données conserve un tableau de file d’attente de tous les événements qui se sont produits en séquence. Par défaut, la couche de données déclenche des rappels d’événement pour les événements qui se sont produits dans la variable **previous** ainsi que les événements dans la variable **futur**. Il est possible de filtrer les événements juste au-delà ou à l’avenir. [Vous trouverez plus d’informations dans la documentation](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Étapes suivantes

Consultez le tutoriel suivant pour savoir comment utiliser la couche de données client Adobe orientée événement pour [collecter des données de page et les envoyer à Adobe Analytics ;](../analytics/collect-data-analytics.md).

Ou apprenez à [Personnalisation de la couche de données client Adobe avec des composants AEM](./data-layer-customize.md)


## Ressources supplémentaires {#additional-resources}

* [Documentation sur la couche de données client Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Utilisation de la couche de données client Adobe et de la documentation des composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
