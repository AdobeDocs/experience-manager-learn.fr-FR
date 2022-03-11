---
title: Chapitre 6 - Exposition du contenu sur AEM Publish as JSON - Content Services
description: Le chapitre 6 du tutoriel AEM sans affichage vous assure que tous les modules, configurations et contenus nécessaires se trouvent sur AEM Publish pour permettre la consommation à partir de l’application mobile.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 2%

---

# Chapitre 6 - Exposition du contenu sur la publication AEM pour diffusion

Le chapitre 6 du tutoriel AEM sans affichage vous assure que tous les modules, configurations et contenus nécessaires se trouvent sur AEM Publish pour permettre leur consommation par l’application mobile.

## Publication de contenu pour AEM Content Services

La configuration et le contenu créés pour piloter les événements via AEM Content Services doivent être publiés sur AEM Publish afin que l’application mobile puisse y accéder.

Comme AEM Content Services est basé sur Configuration (modèles de fragment de contenu, modèles modifiables), Ressources (fragments de contenu, images) et Pages, tous ces éléments disposent automatiquement de fonctionnalités AEM gestion de contenu, notamment :

* Processus de révision et de traitement
* et activation/désactivation pour la publication et l’extraction de contenu à partir des points d’entrée AEM Content Services de la publication AEM

1. Assurez-vous que la variable **[!DNL WKND Mobile]Packages d’applications**, répertoriés dans [Chapitre 1](./chapter-1.md#wknd-mobile-application-packages), sont installés sur **Publication AEM** using [!UICONTROL Gestionnaire de modules].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publiez le **[!DNL WKND Mobile Events API]Modèle modifiable**
   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles] >[!DNL WKND Mobile]**
   1. Sélectionnez la **[!DNL Event API]** modèle
   1. Appuyer **[!UICONTROL Publier]** dans la barre d’actions supérieure
   1. Publiez le **modèle** et **toutes les références** (stratégies de contenu, mappages de stratégies de contenu et modèles)

1. Publiez le **[!DNL WKND Mobile Events]fragments de contenu**.

   Notez que cela est nécessaire, car l’API Events utilise le composant Content Fragment List, qui ne fait pas spécifiquement référence aux fragments de contenu.

   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Ressources] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Sélectionnez l’ensemble des **[!DNL Event]** fragments de contenu
   1. Appuyez sur le bouton **[!UICONTROL Gérer la publication]** dans la barre d’actions supérieure
   1. Laissez la valeur par défaut **Publier** action en l’état, appuyez sur **[!UICONTROL Suivant]** dans la barre d’actions supérieure
   1. Sélectionner **all** fragments de contenu
   1. Appuyer **[!UICONTROL Publier]** dans la barre d’actions supérieure
      * *Le [!DNL Events] Le modèle de fragment de contenu et les images d’événement de référence sont automatiquement publiés avec les fragments de contenu.*

1. Publiez le **[!DNL Events API]page**.
   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Sélectionnez la **[!DNL Events]** page
   1. Appuyez sur le bouton **[!UICONTROL Gérer la publication]** dans la barre d’actions supérieure
   1. Laissez la valeur par défaut **Publier** action en l’état, appuyez sur **[!UICONTROL Suivant]** dans la barre d’actions supérieure
   1. Sélectionnez la **[!DNL Events]** page
   1. Appuyer **[!DNL Publish]** dans la barre d’actions supérieure

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Vérification de la publication AEM

1. Dans un nouveau navigateur Web, assurez-vous d’être déconnecté d’AEM Publish et demandez les URL suivantes (en les remplaçant par `http://localhost:4503` pour tout hôte (hôte : port sur lequel AEM Publish est exécuté).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Ces requêtes doivent renvoyer la même réponse JSON que lors de la révision des points de terminaison AEM Author correspondants. Dans le cas contraire, vérifiez que toutes les publications ont réussi (vérifiez les files d’attente de réplication), la variable [!DNL WKND Mobile] `ui.apps` Le module est installé sur AEM Publish et passez en revue les `error.log` pour AEM Publish.

## Étape suivante

Il n’y a aucun package supplémentaire à installer. Assurez-vous que le contenu et la configuration décrits dans cette section sont publiés sur AEM Publish, sinon les chapitres suivants ne fonctionneront pas.

* [Chapitre 7 - Consommation AEM Content Services à partir d’une application mobile](./chapter-7.md)
