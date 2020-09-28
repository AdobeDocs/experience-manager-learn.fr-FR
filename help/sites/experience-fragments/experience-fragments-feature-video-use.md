---
title: Utilisation de fragments d’expérience AEM
description: Les fragments d’expérience permettent aux auteurs de contenu de réutiliser le contenu sur plusieurs canaux, y compris les pages de sites et les systèmes tiers.
sub-product: sites, content-services
feature: experience-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 971
translation-type: tm+mt
source-git-commit: 3b5dd583a458393a41dbce1d8eeb0095a22db734
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# Utilisation de fragments d’expérience{#using-aem-experience-fragments}

Les fragments d’expérience sont une fonctionnalité de Adobe Experience Manager (AEM), introduite pour la première fois dans AEM 6.3. Les fragments d’expérience permettent aux auteurs de contenu de réutiliser du contenu sur plusieurs canaux, y compris les pages de sites et les systèmes tiers.

## Présentation des fragments d’expérience

>[!VIDEO](https://video.tv.adobe.com/v/17028/?quality=9&learn=on)

Un fragment d’expérience est un ensemble de contenu qui regroupe des formulaires et une expérience qui devraient avoir un sens en soi.

Avec les fragments d’expérience, les marketeurs peuvent :

* Réutilisation d’une expérience sur plusieurs canaux (canaux détenus et points de contact tiers)
* Créer des variations d’une expérience pour des cas d’utilisation spécifiques
* Maintenir les variations synchronisées avec l’utilisation de Live Copy
* Publication sur les réseaux sociaux d’expériences sur Facebook et Pinterest prêt à l’emploi

## Création de blocs avec des fragments d’expérience

Les blocs de création sont une nouvelle amélioration ajoutée au fragment d’expérience dans AEM 6.4+. Il permet aux auteurs de contenu de créer un bloc de création composé de composants qui peuvent être réutilisés pour créer du contenu à travers différentes variations et entre différents modèles.

>[!VIDEO](https://video.tv.adobe.com/v/21289/?quality=9&learn=on)

>[!NOTE]
>
> Le composant de bloc de création doit être ajouté à ses stratégies pour les modèles modifiables utilisés pour créer des fragments d’expérience.

* La création d’un bloc de création permet aux auteurs de contenu de réutiliser facilement le contenu selon différentes variantes.
* La modification du bloc de création de la copie originale doit automatiquement déployer les modifications apportées à ses références sans annuler l’héritage ou toute modification de la mise en page.
* Les auteurs de contenu peuvent facilement renommer un bloc de construction existant ou le supprimer.
* La suppression d’un bloc fonctionnel d’un fragment d’expérience ne supprime pas ses références.

## Fragments d’expérience Recherche de texte intégral

aem 6.5 prend désormais en charge les fonctionnalités de recherche de texte intégral pour les fragments d’expérience.

>[!VIDEO](https://video.tv.adobe.com/v/27720/?quality=9&learn=on)

* **Les auteurs** de contenu (recherche interne) peuvent désormais rechercher une partie de texte dans un fragment d’expérience et le résultat inclut les fragments d’expérience contenant le texte ainsi que la page qui référence le fragment d’expérience.
* **Les utilisateurs** du site (recherche externe) peuvent désormais effectuer une recherche de texte intégral à l’aide du composant de recherche, et le résultat inclut les pages du site qui référencent le fragment d’expérience contenant le mot-clé de recherche.
