---
title: Prise en main d’AEM sans affichage - Content Services
description: Tutoriel complet illustrant comment créer et exposer du contenu à l’aide d’AEM sans affichage.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 5aa32791-861a-48e3-913c-36028373b788
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 4%

---

# Prise en main d’AEM sans affichage - Content Services

AEM Content Services utilise les pages AEM traditionnelles pour composer des points de terminaison d’API REST sans interface utilisateur, et les composants d’AEM définissent, ou référencent, le contenu à exposer sur ces points de terminaison.

AEM Content Services permet les mêmes abstractions de contenu que celles utilisées pour la création de pages web dans AEM Sites pour définir le contenu et les schémas de ces API HTTP. L’utilisation des composants Pages d’AEM et AEM permet aux marketeurs de composer et de mettre à jour rapidement des API JSON flexibles pouvant alimenter n’importe quelle application.

## Tutoriel sur Content Services

Tutoriel complet illustrant comment créer et exposer du contenu à l’aide d’AEM et utilisé par une application mobile native, dans un scénario CMS sans interface.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

Ce tutoriel explique comment AEM Content Services peut être utilisé pour alimenter l’expérience d’une application mobile qui affiche des informations sur les événements (musique, performance, art, etc.). qui est géré par l’équipe WKND.

Ce tutoriel abordera les sujets suivants :

* Créer du contenu représentant un événement à l’aide de fragments de contenu
* Définition d’un point d’entrée Content Services AEM à l’aide des modèles et des pages d’AEM Sites qui exposent les données d’événement au format JSON
* Découvrez comment AEM composants principaux de la gestion du contenu web peuvent être utilisés pour permettre aux marketeurs de créer des points d’entrée JSON.
* Utilisation AEM JSON Content Services à partir d’une application mobile
   * L’utilisation d’Android est due au fait qu’il comporte un émulateur multiplateforme que tous les utilisateurs (Windows, macOS et Linux) de ce tutoriel peuvent utiliser pour exécuter l’application native.

## Projet GitHub

Le code source et les packages de contenu sont disponibles sur la page [AEM Guides - Projet GitHub WKND Mobile](https://github.com/adobe/aem-guides-wknd-mobile).

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez laisser une [Problème GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL et AEM Content Services

|  | API GraphQL AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Définition d’un schéma | Modèles de fragment de contenu structuré | Composants AEM |
| Contenu | Fragments de contenu | Composants AEM |
| Détection de contenu | Par requête GraphQL | Par AEM page |
| Format de diffusion | GraphQL JSON | AEM ComponentExporter JSON |
