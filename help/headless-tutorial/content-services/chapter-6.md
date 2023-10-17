---
title: 'Chapitre 6 : exposer le contenu sur une instance de publication AEM en tant que JSON - Content Services'
description: Dans le chapitre 6 du tutoriel AEM Headless, vous allez découvrir comment installer tous les packages, configurations et contenu nécessaires sur l’instance de publication AEM, afin de permettre leur consultation à partir de l’application mobile.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '465'
ht-degree: 100%

---

# Chapitre 6 : exposer le contenu sur l’instance de publication AEM pour sa diffusion

Dans le chapitre 6 du tutoriel AEM Headless, vous allez découvrir comment installer tous les packages, configurations et contenu nécessaires sur l’instance de publication AEM, afin de permettre leur consultation à partir de l’application mobile.

## Publier du contenu pour AEM Content Services

La configuration et le contenu créés pour piloter les événements via AEM Content Services doivent être publiés sur l’instance de publication AEM afin que l’application mobile puisse y accéder.

Comme AEM Content Services repose sur des éléments de configuration (modèles de fragment de contenu, modèles modifiables), des ressources (fragments de contenu, images) et des pages, ces derniers bénéficient automatiquement des fonctionnalités de gestion de contenu d’AEM, notamment :

* Workflow de révision et de traitement
* et activation et désactivation pour la publication et l’extraction de contenu à partir des points de terminaison AEM Content Services de l’instance de publication AEM

1. Assurez-vous que les packages de l’application **[!DNL WKND Mobile]**, répertoriés au [Chapitre 1](./chapter-1.md#wknd-mobile-application-packages), sont installés sur l’**instance de publication AEM** à l’aide du [!UICONTROL Gestionnaire de modules].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publiez le modèle modifiable **[!DNL WKND Mobile Events API]**
   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles] >[!DNL WKND Mobile]**
   1. Sélectionnez le modèle **[!DNL Event API]**
   1. Appuyez sur **[!UICONTROL Publier]** dans la barre d’actions supérieure
   1. Publiez le **modèle** et **toutes les références** (politiques de contenu, mappages de politiques de contenu et modèles)

1. Publiez les fragments de contenu **[!DNL WKND Mobile Events]**.

   Cette opération est nécessaire, car l’API Events utilise le composant Liste des fragments de contenu, qui ne fait pas spécifiquement référence aux fragments de contenu.

   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Ressources] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Sélectionnez tous les fragments de contenu **[!DNL Event]**
   1. Appuyez sur le bouton **[!UICONTROL Gérer la publication]** dans la barre d’actions supérieure
   1. Laissez l’action par défaut **Publier** en l’état, puis appuyez sur **[!UICONTROL Suivant]** dans la barre d’actions supérieure
   1. Sélectionnez **tous** les fragments de contenu
   1. Appuyez sur **[!UICONTROL Publier]** dans la barre d’actions supérieure
      * *Le modèle de fragment de contenu [!DNL Events] et les images d’événements de référence seront automatiquement publiés avec les fragments de contenu.*

1. Publiez la page **[!DNL Events API]**
   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Sélectionnez la page **[!DNL Events]**
   1. Appuyez sur **[!UICONTROL Gérer la publication]** dans la barre d’actions supérieure
   1. Laissez l’action par défaut **Publier**, puis appuyez sur **[!UICONTROL Suivant]** dans la barre d’actions supérieure
   1. Sélectionnez la page **[!DNL Events]**
   1. Appuyez sur **[!DNL Publish]** dans la barre d’actions supérieure.

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Vérifier l’instance de publication AEM

1. Dans un nouveau navigateur web, vérifiez la déconnexion de l’instance de publication AEM, puis demandez les URL suivantes (en remplaçant `http://localhost:4503` par l’hôte et le port sur lesquels l’instance de publication AEM est exécutée).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   Ces requêtes doivent renvoyer la même réponse JSON que celle obtenue lors de la révision des points d’entrée de l’instance de création AEM correspondante. Dans le cas contraire, vérifiez que toutes les publications ont réussi (vérifiez les files d’attente de réplication), que le package [!DNL WKND Mobile] `ui.apps` est installé sur l’instance de publication AEM et consultez le fichier `error.log` de l’instance de publication AEM.

## Étape suivante

Aucun package supplémentaire ne doit être installé. Avant de passer au chapitre suivant, assurez-vous que le contenu et la configuration décrits dans cette section sont publiés sur l’instance de publication AEM.

* [Chapitre 7 : consulter AEM Content Services à partir d’une application mobile](./chapter-7.md)
