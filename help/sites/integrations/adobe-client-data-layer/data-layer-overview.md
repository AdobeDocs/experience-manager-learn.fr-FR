---
title: Utiliser la couche de données client Adobe avec les composants principaux d’AEM
description: La couche de données client Adobe introduit une méthode standard pour collecter et stocker des données sur une expérience de visite sur une page web, puis faciliter l’accès à ces données. La couche de données client Adobe est indépendante de la plateforme, mais elle est entièrement intégrée aux composants principaux pour l’utilisation avec AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
jira: KT-6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
doc-type: Tutorial
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
duration: 816
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 100%

---

# Utiliser la couche de données client Adobe avec les composants principaux d’AEM {#overview}

La couche de données client Adobe introduit une méthode standard pour collecter et stocker des données sur une expérience de visite sur une page web, puis faciliter l’accès à ces données. La couche de données client Adobe est indépendante de la plateforme, mais elle est entièrement intégrée aux composants principaux pour l’utilisation avec AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Souhaitez-vous activer la couche de données client Adobe sur votre site AEM ? [Voir les instructions ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr#installation-activation).

## Explorer la couche de données

Vous pouvez vous faire une idée des fonctionnalités intégrées de la couche de données client Adobe en utilisant simplement les outils de développement de votre navigateur et le [site de référence WKND](https://wknd.site/fr/fr.html) en ligne.

>[!NOTE]
>
> Les captures d’écran ci-dessous sont extraites du navigateur Chrome.

1. Accédez à [https://wknd.site/us/en.html](https://wknd.site/fr/fr.html).
1. Ouvrez vos outils de développement et saisissez la commande suivante dans la **Console** :

   ```js
   window.adobeDataLayer.getState();
   ```

   Pour afficher l’état actuel de la couche de données sur un site AEM, examinez la réponse. Vous devriez voir des informations sur la page et les composants individuels.

   ![Réponse de la couche de données Adobe.](assets/data-layer-state-response.png)

1. Envoyez un objet de données à la couche de données en saisissant les éléments suivants dans la console :

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
1. Ajoutez ensuite un paramètre de chemin d’accès pour renvoyer uniquement l’état spécifique d’un composant :

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Renvoi d’une seule entrée de données de composant.](assets/return-just-single-component.png)

## Travailler avec les événements

Il est recommandé de déclencher les codes personnalisés basés sur un événement à partir de la couche de données. Explorez ensuite l’enregistrement et l’écoute de différents événements.

1. Saisissez la méthode d’assistance suivante dans votre console :

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

   Le code ci-dessus inspecte l’objet `event` et utilise la méthode `adobeDataLayer.getState` pour obtenir l’état actuel de l’objet qui a déclenché l’événement. Ensuite, la méthode d’assistance examine le `filter` qui n’est renvoyé que si le `dataObject` actif répond aux critères de filtre.

   >[!CAUTION]
   >
   > Il est important de **ne pas** actualiser le navigateur tout au long de cet exercice, car le code JavaScript de la console risque de se perdre.

1. Saisissez ensuite un gestionnaire d’événements qui est appelé lorsqu’un composant **Teaser** est affiché dans un **Carrousel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   La fonction `teaserShownHandler` appelle la fonction `getDataObjectHelper` et transmet un filtre de `wknd/components/teaser` en tant que `@type` pour filtrer les événements déclenchés par d’autres composants.

1. Ensuite, placez un écouteur d’événement sur la couche de données pour écouter l’événement `cmp:show`.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   L’événement `cmp:show` est déclenché par de nombreux composants, par exemple lorsqu’une nouvelle diapositive est affichée dans le **Carrousel** ou lorsqu’un nouvel onglet est sélectionné dans le composant **Onglet**.

1. Sur la page, faites basculer les diapositives du carrousel et observez les instructions de la console :

   ![Activation du carrousel à l’aide du bouton (bascule) et affichage de l’écouteur d’événement.](assets/teaser-console-slides.png)

1. Pour arrêter d’écouter l’événement `cmp:show`, supprimez l’écouteur d’événement de la couche de données.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Revenez à la page et faites basculer les diapositives du carrousel. Notez qu’aucune autre instruction n’est consignée et que l’événement n’est pas écouté.

1. Créez ensuite un gestionnaire d’événements appelé lorsque l’événement de page affichée est déclenché :

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

1. Ensuite, placez un écouteur d’événement sur la couche de données pour écouter l’événement `cmp:show`, ce qui appelle le `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Une instruction de console doit immédiatement être déclenchée avec les données de page :

   ![Données d’affichage de page.](assets/page-show-console-data.png)

   L’événement `cmp:show` de la page est déclenché à chaque chargement de page, tout en haut de celle-ci. Vous pouvez vous demander : pourquoi le gestionnaire d’événements a-t-il été déclenché alors que la page a visiblement déjà été chargée ?

   L’une des fonctionnalités uniques de la couche de données client Adobe est que vous pouvez enregistrer des écouteurs d’événement **avant** ou **après** que la couche de données a été initialisée, ce qui permet d’éviter les conditions de concurrence.

   La couche de données conserve un tableau de file d’attente de tous les événements qui se sont produits en séquence. Par défaut, la couche de données déclenche des rappels d’événements pour les événements qui se sont produits dans le **passé**, ainsi que pour ceux qui se produiront **à l’avenir**. Il est possible de filtrer les événements du passé ou du futur. [Vous trouverez plus d’informations à ce sujet dans la documentation](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Étapes suivantes

Deux options s’offrent à vous pour continuer à apprendre. La première consiste à consulter le tutoriel [Collecter des données de page et les envoyer à Adobe Analytics](../analytics/collect-data-analytics.md), qui présente l’utilisation de la couche de données client Adobe. La deuxième option consiste à apprendre à [personnaliser la couche de données client Adobe avec des composants AEM](./data-layer-customize.md).


## Ressources supplémentaires {#additional-resources}

* [Documentation sur la couche de données client Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Documentation sur l’utilisation de la couche de données client Adobe et des composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr)
