---
title: Chapitre 4 - Définir des modèles Content Services - Content Services
description: Le chapitre 4 du tutoriel sur AEM Headless aborde le rôle des modèles modifiables d’AEM dans le contexte d’AEM Content Services. Les modèles modifiables sont utilisés pour définir la structure de contenu JSON exposée par AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 100%

---

# Chapitre 4 - Définitr des modèles Content Services

Le chapitre 4 du tutoriel sur AEM Headless aborde le rôle des modèles modifiables d’AEM dans le contexte d’AEM Content Services. Les modèles modifiables sont utilisés pour définir la structure de contenu JSON qu’AEM Content Services expose aux clients et clientes via la composition des composants AEM activés pour Content Services.

## Comprendre le rôle des modèles dans AEM Content Services

Les modèles modifiables AEM sont utilisés pour définir les points d’entrée HTTP accessibles pour exposer le contenu de l’événement au format JSON.

En général, les modèles modifiables AEM sont utilisés pour définir des pages web, mais cette utilisation est une simple convention. Les modèles modifiables peuvent être utilisés pour composer **tout** ensemble de contenu et mode d’accès de contenu : en tant que HTML dans un navigateur ou que JSON utilisé par JavaScript (éditeur de SPA AEM), ou encore en tant qu’application mobile. Cela dépend de la manière dont cette page est demandée.

Dans AEM Content Services, des modèles modifiables sont utilisés pour définir la manière dont les données JSON sont exposées.

Pour l’application [!DNL WKND Mobile], nous allons créer un modèle modifiable unique utilisé pour générer un point d’entrée API unique. Bien que cet exemple simple permette d’illustrer les concepts d’AEM Headless, vous pouvez créer plusieurs pages (ou points d’entrée) exposant chaque ensemble de contenu afin de créer une API plus complexe et mieux organisée.

## Comprendre le point d’entrée de l’API

Pour comprendre comment composer notre point d’entrée d’API et comprendre quel contenu doit être exposé à notre application [!DNL WKND Mobile], nous allons revoir la conception.

![Décomposition de page de l’API d’événements.](./assets/chapter-4/design-to-component-mapping.png)

Comme nous pouvons le constater, nous disposons de trois ensembles logiques de contenu à fournir à l’application mobile.

1. Le **Logo**.
2. Le **Ligne de balise**.
3. La liste des **Événements**.

Pour ce faire, nous pouvons mettre en correspondance ces exigences avec les composants AEM (et, dans notre cas, les composants AEM principaux de gestion de contenu web) afin d’exposer le contenu requis au format JSON.

1. Le **Logo** est surfacé via un **Composant d’image**.
2. La **Ligne de balise** est surfacée via un **Composant de texte**.
3. La liste des **Événements** est surfacée via un **Composant Liste de fragments de contenu** qui, à son tour, fait référence à un ensemble de fragments de contenu d’événement.

>[!NOTE]
>
>Pour prendre en charge l’exportation JSON des pages et composants par AEM Content Services, ces derniers doivent **dériver des composants principaux AEM de la gestion de contenu Web**.
>
>Les [Composants principaux AEM de gestion de contenu web](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) disposent d’une fonctionnalité intégrée pour prendre en charge un schéma JSON normalisé des pages et composants exportés. Tous les composants mobiles WKND utilisés dans ce tutoriel (Page, Image, Texte et Liste de fragments de contenu) sont dérivés des composants principaux de gestion de contenu web d’AEM.

## Définir le modèle d’API d’événement

1. Accédez à **[!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles] >[!DNL WKND Mobile]**.

1. Créez le modèle **[!DNL Events API]** :

   1. Appuyez sur **[!UICONTROL Créer]** dans la barre d’actions supérieure.
   1. Sélectionnez le modèle **[!DNL WKND Mobile - Empty Page]**.
   1. Appuyez sur **[!UICONTROL Suivant]** dans la barre d’actions supérieure.
   1. Entrez **[!DNL Events API]** dans le champ [!UICONTROL Titre du modèle].
   1. Appuyez sur **[!UICONTROL Créer]** dans la barre d’actions supérieure.
   1. Appuyez sur **[!UICONTROL Ouvrir]** pour ouvrir le nouveau modèle à modifier.

1. Tout d’abord, nous autorisons les trois composants AEM identifiés dont nous avons besoin pour modéliser le contenu en modifiant la [!UICONTROL Stratégie] du [!UICONTROL Conteneur de disposition] racine. Assurez-vous que le mode **[!UICONTROL Structure]** est actif, sélectionnez **[!DNL Layout Container \[Root\]]**, puis appuyez sur le bouton **[!UICONTROL Stratégie]**.
1. Sous **[!UICONTROL Propriétés] > [!UICONTROL Composants autorisés]**, recherchez **[!DNL WKND Mobile]**. Autorisez les composants suivants du groupe de composants [!DNL WKND Mobile], afin qu’ils puissent être utilisés sur la page d’API [!DNL Events].

   * **[!DNL WKND Mobile > Image]**

      * Logo de l’application.
   * **[!DNL WKND Mobile > Text]**

      * Texte d’introduction de l’application.
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Liste des catégories d’événement disponibles pour affichage dans l’application.



1. Cochez la case **[!UICONTROL Terminé]** en haut à droite lorsque vous avez terminé.
1. **Actualisez** la fenêtre du navigateur pour afficher la liste des nouveaux [!UICONTROL Composants autorisés] dans le rail de gauche.
1. Dans l’outil de recherche de composants du rail de gauche, faites glisser les composants AEM suivants :
   1. **[!DNL Image]** pour le Logo.
   2. **[!DNL Text]** pour la Ligne de balise.
   3. **[!DNL Content Fragment List]** pour les événements.
1. **Pour chacun des composants ci-dessus**, sélectionnez-les et appuyez sur le bouton **Déverrouiller**.
1. Cependant, assurez-vous que le **conteneur de disposition** est **verrouillé** pour empêcher l’ajout d’autres composants ou la suppression de ces trois composants.
1. Appuyez sur **[!UICONTROL Informations sur la page] > [!UICONTROL Afficher dans Admin]** pour revenir à la liste des modèles [!DNL WKND Mobile]. Sélectionnez le modèle **[!DNL Events API]** nouvellement créé et appuyez sur **[!UICONTROL Activer]** dans la barre d’actions supérieure.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> Notez que les composants utilisés pour surfacer le contenu sont ajoutés au modèle lui-même et verrouillés. Cela permet aux auteurs et autrices de modifier les composants prédéfinis, mais pas d’ajouter ou de supprimer arbitrairement des composants, car la modification de l’API elle-même peut rompre les hypothèses autour de la structure JSON et interrompre les applications consommatrices. Toutes les API doivent être stables.

## Étapes suivantes

Si vous le souhaitez, vous pouvez installer le package de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur l’instance de création AEM via le [Gestionnaire de packages AEM](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 5 - Créer des pages Content Services](./chapter-5.md)
