---
title: Prise en main d’AEM Headless - Content Services
description: Tutoriel complet illustrant comment créer et exposer du contenu à l’aide d’AEM Headless.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 322
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 100%

---

# Prise en main d’AEM Headless - Content Services

AEM Content Services utilise les pages AEM traditionnelles pour composer des points d’entrée d’API REST découplés. Les composants AEM définissent, ou référencent, le contenu à exposer sur ces points d’entrée.

AEM Content Services permet les mêmes abstractions de contenu que celles utilisées pour la création de pages web dans AEM Sites pour définir le contenu et les schémas de ces API HTTP. L’utilisation des Pages AEM et des Composants AEM permet aux personnes spécialisées dans le marketing de composer et de mettre à jour rapidement des API JSON flexibles pouvant alimenter n’importe quelle application.

## Tutoriel sur Content Services

Tutoriel complet illustrant comment créer et exposer du contenu à l’aide d’AEM et utilisé par une application mobile native, dans un scénario CMS découplé.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

Ce tutoriel explique comment AEM Content Services peut être utilisé pour alimenter l’expérience d’une application mobile qui affiche des informations sur les événements (musique, performance, art, etc.) et est élaboré par l’équipe WKND.

Ce tutoriel aborde les sujets suivants :

* Créer du contenu représentant un événement à l’aide de fragments de contenu.
* Définir des points d’entrée AEM Content Services à l’aide des modèles et des pages d’AEM Sites qui exposent les données d’événement au format JSON.
* Découvrir comment les composants principaux de gestion de contenu web AEM peuvent être utilisés pour permettre aux personnes spécialisées dans le marketing de créer des points d’entrée JSON.
* Utiliser le format JSON d’AEM Content Services à partir d’une application mobile.
   * L’utilisation d’Android est due à la présence d’un émulateur sur plusieurs plateformes, que tous les utilisateurs et utilisatrices (Windows, macOS et Linux) de ce tutoriel peuvent utiliser pour exécuter l’application native.

## Projet GitHub

Le code source et les packages de contenu sont disponibles sur la page [AEM Guides - Projet GitHub WKND Mobile](https://github.com/adobe/aem-guides-wknd-mobile).

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez consigner un [Problème GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL et AEM Content Services

|                                | API AEM GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Définition d’un schéma | Modèles structurés de fragment de contenu | Composants AEM |
| Contenu | Fragments de contenu | Composants AEM |
| Détection de contenu | Par requête GraphQL | Par Page AEM |
| Format de diffusion | JSON GraphQL | Exporteur JSON de composant AEM |
