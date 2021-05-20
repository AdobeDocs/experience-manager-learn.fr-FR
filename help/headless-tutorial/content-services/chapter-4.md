---
title: Chapitre 4 - Définition des modèles Content Services - Content Services
seo-title: Prise en main d’AEM sans affichage - Chapitre 4 - Définition des modèles Content Services
description: Le chapitre 4 du tutoriel AEM sans affichage aborde le rôle d’AEM Modèles modifiables dans le contexte d’AEM Content Services. Les modèles modifiables sont utilisés pour définir la structure de contenu JSON AEM Content Services finira par exposer.
seo-description: Le chapitre 4 du tutoriel AEM sans affichage aborde le rôle d’AEM Modèles modifiables dans le contexte d’AEM Content Services. Les modèles modifiables sont utilisés pour définir la structure de contenu JSON AEM Content Services finira par exposer.
feature: Fragments de contenu, API
topic: Sans affichage, gestion de contenu
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 1%

---


# Chapitre 4 - Définition des modèles Content Services

Le chapitre 4 du tutoriel AEM sans affichage aborde le rôle d’AEM Modèles modifiables dans le contexte d’AEM Content Services. Les modèles modifiables sont utilisés pour définir la structure de contenu JSON AEM Content Services expose aux clients via la composition des composants AEM activés pour Content Services.

## Comprendre le rôle des modèles dans AEM Content Services

AEM les modèles modifiables sont utilisés pour définir les points de fin HTTP qui seront accessibles pour exposer le contenu de l’événement au format JSON.

Les modèles modifiables traditionnellement AEM sont utilisés pour définir des pages web, mais cette utilisation est simplement une convention. Les modèles modifiables peuvent être utilisés pour composer **n’importe quel** ensemble de contenu. mode d’accès à ce contenu : en tant que code HTML dans un navigateur, en tant que code JSON utilisé par JavaScript (AEM Éditeur SPA) ou une application mobile est une fonction de la manière dont cette page est demandée.

Dans AEM Content Services, des modèles modifiables sont utilisés pour définir la manière dont les données JSON sont exposées.

Pour l’application [!DNL WKND Mobile] , nous allons créer un modèle modifiable unique qui sera utilisé pour générer un point de terminaison API unique. Bien que cet exemple soit simple pour illustrer les concepts d’AEM sans affichage, vous pouvez créer plusieurs pages (ou points de fin) exposant chacun différents ensembles de contenu afin de créer une API plus complexe et mieux organisée.

## Présentation du point de terminaison de l’API

Pour comprendre comment composer notre point de terminaison d’API et comprendre quel contenu doit être exposé à notre application [!DNL WKND Mobile], nous allons revoir la conception.

![Décomposition de page de l’API d’événements](./assets/chapter-4/design-to-component-mapping.png)

Comme nous pouvons le constater, nous disposons de trois ensembles logiques de contenu à fournir à l’application mobile.

1. **Logo**
2. **Ligne de balise**
3. Liste des **Événements**

Pour ce faire, nous pouvons mettre en correspondance ces exigences avec les composants AEM (et, dans notre cas, AEM les composants principaux de la gestion de contenu web) afin d’exposer le contenu requis au format JSON.

1. Le **logo** sera affiché via un **composant d’image**
2. La **ligne de balise** sera affichée via un **composant Texte**
3. La liste des **Événements** sera affichée via un **composant Liste de fragments de contenu** qui, à son tour, référence un ensemble de fragments de contenu d’événement.

>[!NOTE]
>
>Pour prendre en charge l’exportation JSON de pages et de composants par AEM Content Service, les pages et les composants doivent **dériver d’AEM composants principaux WCM**.
>
>[Les ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) composants principaux de la gestion de contenu web d’AEM disposent d’une fonctionnalité intégrée pour prendre en charge un schéma JSON normalisé des pages et composants exportés. Tous les composants WKND Mobile utilisés dans ce tutoriel (Page, Image, Texte et Liste de fragments de contenu) sont dérivés des composants principaux de la gestion de contenu web d’AEM.

## Définition du modèle d’API d’événement

1. Accédez à **[!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles] >[!DNL WKND Mobile]**.

1. Créez le modèle **[!DNL Events API]** :

   1. Appuyez sur **[!UICONTROL Créer]** dans la barre d’actions supérieure.
   1. Sélectionnez le modèle **[!DNL WKND Mobile - Empty Page]**
   1. Appuyez sur **[!UICONTROL Suivant]** dans la barre d’actions supérieure.
   1. Saisissez **[!DNL Events API]** dans le champ [!UICONTROL Titre du modèle]
   1. Appuyez sur **[!UICONTROL Créer]** dans la barre d’actions supérieure.
   1. Appuyez sur **[!UICONTROL Ouvrir]** pour ouvrir le nouveau modèle à modifier.

1. Tout d’abord, nous autorisons les trois composants d’AEM identifiés dont nous avons besoin pour modéliser le contenu en modifiant la [!UICONTROL Stratégie] de la racine [!UICONTROL Conteneur de mises en page]. Vérifiez que le mode **[!UICONTROL Structure]** est principal, sélectionnez **[!DNL Layout Container \[Root\]]**, puis appuyez sur le bouton **[!UICONTROL Stratégie]**.
1. Sous **[!UICONTROL Propriétés] > [!UICONTROL Composants autorisés]** recherchez **[!DNL WKND Mobile]**. Autorisez les composants suivants du groupe de composants [!DNL WKND Mobile] afin qu’ils puissent être utilisés sur la page de l’API [!DNL Events].

   * **[!DNL WKND Mobile > Image]**

      * Le logo de l’application
   * **[!DNL WKND Mobile > Text]**

      * Texte d’introduction de l’application
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Liste des catégories d’événements disponibles pour affichage dans l’application.



1. Appuyez sur la coche **[!UICONTROL Terminé]** dans le coin supérieur droit lorsque vous avez terminé.
1. **** Actualisez la fenêtre du navigateur pour afficher la nouvelle liste  [!UICONTROL de composants ] autorisés dans le rail de gauche.
1. Dans l’outil de recherche de composants du rail de gauche, faites glisser les composants AEM suivants :
   1. **[!DNL Image]** pour le logo
   2. **[!DNL Text]** pour la ligne de balise
   3. **[!DNL Content Fragment List]** pour les événements
1. **Pour chacun des composants** ci-dessus, sélectionnez-les et appuyez sur le bouton de  **** déverrouillage.
1. Cependant, assurez-vous que le **conteneur de mises en page** est **verrouillé** pour empêcher l’ajout d’autres composants ou que ces trois composants ne soient pas supprimés.
1. Appuyez sur **[!UICONTROL Informations sur la page] > [!UICONTROL Afficher dans Admin]** pour revenir à la liste des modèles [!DNL WKND Mobile]. Sélectionnez le nouveau modèle **[!DNL Events API]** et appuyez sur **[!UICONTROL Activer]** dans la barre d’actions supérieure.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> Notez que les composants utilisés pour faire surface du contenu sont ajoutés au modèle lui-même et verrouillés. Cela permet aux auteurs de modifier les composants prédéfinis, mais pas d’ajouter ou de supprimer arbitrairement des composants, car la modification de l’API elle-même peut rompre les hypothèses autour de la structure JSON et interrompre les applications consommatrices. Toutes les API doivent être stables.

## Étapes suivantes

Vous pouvez éventuellement installer le module de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur l’auteur AEM via [AEM Gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 5 - Création de pages Content Services](./chapter-5.md)
