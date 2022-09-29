---
title: Chapitre 4 - Définition des modèles Content Services - Content Services
description: Le chapitre 4 du tutoriel AEM sans affichage aborde le rôle d’AEM Modèles modifiables dans le contexte d’AEM Content Services. Les modèles modifiables sont utilisés pour définir la structure de contenu JSON AEM Content Services exposer en fin de compte.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 2%

---

# Chapitre 4 - Définition des modèles Content Services

Le chapitre 4 du tutoriel AEM sans affichage aborde le rôle d’AEM Modèles modifiables dans le contexte d’AEM Content Services. Les modèles modifiables sont utilisés pour définir la structure de contenu JSON AEM Content Services expose aux clients via la composition des composants AEM activés pour Content Services.

## Comprendre le rôle des modèles dans AEM Content Services

AEM les modèles modifiables sont utilisés pour définir les points de fin HTTP accessibles pour exposer le contenu de l’événement au format JSON.

Les modèles modifiables traditionnellement AEM sont utilisés pour définir des pages web, mais cette utilisation est simplement une convention. Les modèles modifiables peuvent être utilisés pour composer des **any** ensemble de contenu ; mode d’accès à ce contenu : comme HTML dans un navigateur, comme JSON utilisé par JavaScript (AEM Éditeur SPA) ou une application mobile est une fonction de la manière dont cette page est demandée.

Dans AEM Content Services, des modèles modifiables sont utilisés pour définir la manière dont les données JSON sont exposées.

Pour le [!DNL WKND Mobile] Application, nous allons créer un modèle modifiable unique utilisé pour générer un point de terminaison API unique. Bien que cet exemple soit simple pour illustrer les concepts d’AEM sans affichage, vous pouvez créer plusieurs pages (ou points de fin) exposant chacun différents ensembles de contenu afin de créer une API plus complexe et mieux organisée.

## Présentation du point de terminaison de l’API

Pour comprendre comment composer notre point de terminaison API et comprendre quel contenu doit être exposé à notre [!DNL WKND Mobile] Applis, nous allons revoir la conception.

![Décomposition de page de l’API d’événements](./assets/chapter-4/design-to-component-mapping.png)

Comme nous pouvons le constater, nous disposons de trois ensembles logiques de contenu à fournir à l’application mobile.

1. Le **Logo**
2. Le **Ligne de balise**
3. La liste des **Événements**

Pour ce faire, nous pouvons mettre en correspondance ces exigences avec les composants AEM (et, dans notre cas, AEM les composants principaux de la gestion de contenu web) afin d’exposer le contenu requis au format JSON.

1. Le **Logo** est affiché via un **Composant d’image**
2. Le **Ligne de balise** est affiché via un **Composant textuel**
3. La liste des **Événements** est affiché via un **Composant Liste de fragments de contenu** qui, à son tour, fait référence à un ensemble de fragments de contenu d’événement.

>[!NOTE]
>
>Pour prendre en charge AEM exportation JSON de pages et de composants par le service de contenu, les pages et les composants doivent **dérive des composants principaux de la gestion de contenu web AEM**.
>
>[AEM composants principaux WCM](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) disposent d’une fonctionnalité intégrée pour prendre en charge un schéma JSON normalisé des pages et composants exportés. Tous les composants WKND Mobile utilisés dans ce tutoriel (Page, Image, Texte et Liste de fragments de contenu) sont dérivés des composants principaux de la gestion de contenu web d’AEM.

## Définition du modèle d’API d’événement

1. Accédez à **[!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles] >[!DNL WKND Mobile]**.

1. Créez le **[!DNL Events API]** modèle :

   1. Appuyer **[!UICONTROL Créer]** dans la barre d’actions supérieure
   1. Sélectionnez la **[!DNL WKND Mobile - Empty Page]** modèle
   1. Appuyer **[!UICONTROL Suivant]** dans la barre d’actions supérieure
   1. Entrée **[!DNL Events API]** dans le [!UICONTROL Titre du modèle] field
   1. Appuyer **[!UICONTROL Créer]** dans la barre d’actions supérieure
   1. Appuyer **[!UICONTROL Ouvrir]** ouvrir le nouveau modèle à modifier ;

1. Tout d’abord, nous autorisons les trois composants AEM identifiés dont nous avons besoin pour modéliser le contenu en modifiant le [!UICONTROL Stratégie] de la racine [!UICONTROL Conteneur de mises en page]. Assurez-vous que la variable **[!UICONTROL Structure]** est principal, sélectionnez l’option **[!DNL Layout Container \[Root\]]**, puis appuyez sur le bouton **[!UICONTROL Stratégie]** bouton .
1. Sous **[!UICONTROL Propriétés] > [!UICONTROL Composants autorisés]** rechercher **[!DNL WKND Mobile]**. Autorisez les composants suivants de la [!DNL WKND Mobile] groupe de composants afin qu’ils puissent être utilisés sur la variable [!DNL Events] page API.

   * **[!DNL WKND Mobile > Image]**

      * Le logo de l’application
   * **[!DNL WKND Mobile > Text]**

      * Texte d’introduction de l’application
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Liste des catégories d’événements disponibles pour affichage dans l’application.



1. Appuyez sur le bouton **[!UICONTROL Terminé]** cochez la case en haut à droite lorsque vous avez terminé.
1. **Actualiser** dans la fenêtre du navigateur pour afficher [!UICONTROL Composants autorisés] dans le rail de gauche.
1. Dans l’outil de recherche de composants du rail de gauche, faites glisser les composants AEM suivants :
   1. **[!DNL Image]** pour le logo
   2. **[!DNL Text]** pour la ligne de balise
   3. **[!DNL Content Fragment List]** pour les événements
1. **Pour chacun des composants ci-dessus**, sélectionnez-les et appuyez sur la touche **déverrouiller** bouton .
1. Cependant, assurez-vous que la variable **conteneur de mises en page** is **verrouillé** pour empêcher l’ajout d’autres composants ou la suppression de ces trois composants.
1. Appuyer **[!UICONTROL Informations sur la page] > [!UICONTROL Afficher dans Admin]** pour revenir au [!DNL WKND Mobile] la liste des modèles. Sélectionnez la **[!DNL Events API]** modèle et appuyez sur **[!UICONTROL Activer]** dans la barre d’actions supérieure.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> Notez que les composants utilisés pour faire surface du contenu sont ajoutés au modèle lui-même et verrouillés. Cela permet aux auteurs de modifier les composants prédéfinis, mais pas d’ajouter ou de supprimer arbitrairement des composants, car la modification de l’API elle-même peut rompre les hypothèses autour de la structure JSON et interrompre les applications consommatrices. Toutes les API doivent être stables.

## Étapes suivantes

Si vous le souhaitez, vous pouvez installer la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) module de contenu sur l’auteur AEM via [AEM Gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 5 - Création de pages Content Services](./chapter-5.md)
