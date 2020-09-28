---
title: Prise en main AEM sans en-tête - Chapitre 6 - Exposition du contenu sur AEM Publish en tant que JSON
description: Le chapitre 6 du didacticiel AEM sans en-tête traite de la manière dont tous les packages, configurations et contenus nécessaires sont intégrés à AEM Publish pour permettre la consommation à partir de l’application mobile.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 2%

---


# Chapitre 6 - Exposition de contenu sur AEM Publish pour Diffusion

Le chapitre 6 du didacticiel AEM sans en-tête traite de la manière dont tous les packages, configurations et contenus nécessaires sont intégrés à AEM Publish pour permettre la consommation par l’application mobile.

## Publication de contenu pour AEM Content Services

La configuration et le contenu créés pour diriger les Événements via AEM Content Services doivent être publiés dans AEM Publish afin que l’application mobile puisse y accéder.

Comme AEM Content Services est créé à partir de Configuration (modèles de fragments de contenu, modèles modifiables), de Ressources (fragments de contenu, images) et de Pages, toutes ces pièces bénéficient automatiquement des fonctionnalités d’AEM gestion de contenu, notamment :

* Processus de révision et de traitement
* et activation/désactivation pour la publication et l’extraction de contenu des points de terminaison AEM Content Services d’AEM Publish

1. Assurez-vous que les packages **[!DNL WKND Mobile]** d’applications répertoriés dans le [chapitre 1](./chapter-1.md#wknd-mobile-application-packages)sont installés sur **AEM Publish** à l’aide de [!UICONTROL Package Manager.]
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publication du modèle **[!DNL WKND Mobile Events API]modifiable**
   1. Navigate to **[!UICONTROL AEM]>[!UICONTROL Tools]>[!UICONTROL General]>[!UICONTROL Templates]>[!DNL WKND Mobile]**
   1. Select the **[!DNL Event API]** template
   1. Appuyez sur **[!UICONTROL Publier]** dans la barre d’actions supérieure.
   1. Publiez le **modèle** et **toutes les références** (stratégies de contenu, mappages de stratégies de contenu et modèles).

1. Publiez les fragments **[!DNL WKND Mobile Events]de** contenu.

Notez que cela est nécessaire car l’API Événements utilise le composant Liste Fragment de contenu, qui ne fait pas spécifiquement référence aux fragments de contenu.
1. Accédez à **[!UICONTROL AEM]>[!UICONTROL Ressources]>[!UICONTROL Fichiers]>[!DNL WKND Mobile]>  >  1. Sélectionnez tous les fragments de contenu 1. Appuyez sur le  Gérer les publications dans la barre d&#39;actions supérieure1. Quittez l&#39;action Publier par défaut telle quelle, appuyez sur Suivant dans la barre d&#39;actions supérieure1. Sélectionnez  tous les fragments de contenu 1. Appuyez sur Publier.  dans la barre d&#39;action supérieure* Le modèle de fragment de contenu de  et les images de Événement référencées sont automatiquement publiés avec les fragments de contenu.[!DNL English][!DNL Events]****[!DNL Event]***********************[!DNL Events]*

1. Publish the **[!DNL Events API]page**.
   1. Accédez à **[!UICONTROL AEM]>[!UICONTROL Sites]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**
   1. Sélectionner la **[!DNL Events]** page
   1. Appuyez sur **[!UICONTROL Gérer la publication]** dans la barre d’actions supérieure.
   1. En laissant l’action **Publier** par défaut telle quelle, appuyez sur **[!UICONTROL Suivant]** dans la barre d’actions supérieure.
   1. Sélectionner la **[!DNL Events]** page
   1. Appuyez sur **[!DNL Publish]** la barre d’action supérieure.

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Vérification de la publication AEM

1. Dans un nouveau navigateur Web, vérifiez que vous êtes déconnecté d’AEM Publish et demandez les URL suivantes (en remplacement `http://localhost:4503` de tout hôte : port sur lequel AEM Publish est exécuté).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Ces requêtes doivent renvoyer la même réponse JSON que lors de la révision des points de terminaison AEM Author correspondants. Dans le cas contraire, assurez-vous que toutes les publications ont réussi (vérifiez les files d’attente de réplication), le [!DNL WKND Mobile]`ui.apps` package est installé sur AEM Publish et vérifiez le `error.log` pour AEM Publish.

## Étape suivante

Il n&#39;y a aucun pack supplémentaire à installer. Assurez-vous que le contenu et la configuration décrits dans cette section sont publiés dans AEM Publish, sinon les chapitres suivants ne fonctionneront pas.

* [Chapitre 7 - Consommation de AEM Content Services à partir d&#39;une application mobile](./chapter-7.md)
