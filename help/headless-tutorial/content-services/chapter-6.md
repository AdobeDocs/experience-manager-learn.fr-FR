---
title: Chapitre 6 - Présentation du contenu sur AEM Publish en tant que JSON - Content Services
description: Le chapitre 6 du didacticiel AEM sans en-tête traite de la manière dont tous les packages, configurations et contenus nécessaires sont intégrés à AEM Publish pour permettre la consommation à partir de l’application mobile.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 2%

---


# Chapitre 6 - Exposition de contenu sur AEM Publish pour Diffusion

Le chapitre 6 du didacticiel AEM sans en-tête traite de la manière dont tous les packages, configurations et contenus nécessaires sont intégrés à AEM Publish pour permettre la consommation par l’application mobile.

## Publication de contenu pour AEM Content Services

La configuration et le contenu créés pour diriger les Événements via AEM Content Services doivent être publiés dans AEM Publish afin que l’application mobile puisse y accéder.

Comme AEM Content Services est créé à partir de Configuration (modèles de fragments de contenu, modèles modifiables), de Ressources (fragments de contenu, images) et de Pages, toutes ces pièces bénéficient automatiquement des fonctionnalités d’AEM gestion de contenu, notamment :

* Processus de révision et de traitement
* et activation/désactivation pour la publication et l’extraction de contenu des points de terminaison AEM Content Services d’AEM Publish

1. Assurez-vous que les **[!DNL WKND Mobile]packages d’applications** répertoriés dans [Chapitre 1](./chapter-1.md#wknd-mobile-application-packages) sont installés sur **AEM Publish** à l’aide de [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publier le **[!DNL WKND Mobile Events API]modèle modifiable**
   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles] >[!DNL WKND Mobile]**
   1. Sélectionnez le modèle **[!DNL Event API]**
   1. Appuyez sur **[!UICONTROL Publier]** dans la barre d’actions supérieure.
   1. Publiez le **modèle** et **toutes les références** (stratégies de contenu, mappages de stratégies de contenu et modèles).

1. Publiez les fragments de contenu **[!DNL WKND Mobile Events]**.

Notez que cela est nécessaire car l’API Événements utilise le composant Liste Fragment de contenu, qui ne fait pas spécifiquement référence aux fragments de contenu.
1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Actifs] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Sélectionnez tous les fragments de contenu **[!DNL Event]**.
1. Appuyez sur **[!UICONTROL Gérer la publication]** dans la barre d’actions supérieure.
1. Si vous laissez l&#39;action **Publier** par défaut en l&#39;état, appuyez sur **[!UICONTROL Suivant]** dans la barre d&#39;actions supérieure.
1. Sélectionnez **tous** fragments de contenu.
1. Appuyez sur **[!UICONTROL Publier]** dans la barre d’actions supérieure.
* *Le [!DNL Events] modèle de fragment de contenu et les images de Événement de référence seront automatiquement publiés avec les fragments de contenu.*

1. Publiez la page **[!DNL Events API]**.
   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Sélectionnez la page **[!DNL Events]**
   1. Appuyez sur **[!UICONTROL Gérer la publication]** dans la barre d’actions supérieure.
   1. En laissant l’action **Publier** par défaut telle quelle, appuyez sur **[!UICONTROL Suivant]** dans la barre d’actions supérieure.
   1. Sélectionnez la page **[!DNL Events]**
   1. Appuyez sur **[!DNL Publish]** dans la barre d’actions supérieure.

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Vérification de la publication AEM

1. Dans un nouveau navigateur Web, vérifiez que vous êtes déconnecté d’AEM Publish et demandez les URL suivantes (en remplaçant `http://localhost:4503` par tout hôte : port sur lequel AEM Publish est exécuté).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Ces requêtes doivent renvoyer la même réponse JSON que lors de la révision des points de terminaison AEM Author correspondants. Dans le cas contraire, assurez-vous que toutes les publications ont réussi (vérifiez les files d’attente de réplication), le package [!DNL WKND Mobile] `ui.apps` est installé sur AEM Publish et vérifiez le `error.log` pour AEM Publish.

## Étape suivante

Il n&#39;y a aucun pack supplémentaire à installer. Assurez-vous que le contenu et la configuration décrits dans cette section sont publiés dans AEM Publish, sinon les chapitres suivants ne fonctionneront pas.

* [Chapitre 7 - Consommation de AEM Content Services à partir d&#39;une application mobile](./chapter-7.md)
