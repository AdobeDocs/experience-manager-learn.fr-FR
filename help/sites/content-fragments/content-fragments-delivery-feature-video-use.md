---
title: Diffusion de fragments de contenu en AEM
seo-title: Diffusion de fragments de contenu dans Adobe Experience Manager
description: Les fragments de contenu, indépendamment de la mise en page, peuvent être utilisés directement en AEM Sites avec les composants principaux ou peuvent être distribués sans encombre aux canaux en aval.
seo-description: Les fragments de contenu, indépendamment de la mise en page, peuvent être utilisés directement en AEM Sites avec les composants principaux ou peuvent être distribués sans encombre aux canaux en aval.
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---


# Diffusion de fragments de contenu {#delivering-content-fragments}

Les fragments de contenu Adobe Experience Manager (AEM) sont des contenus éditoriaux textuels qui peuvent inclure certains éléments de données structurés associés mais considérés comme du contenu pur sans informations de conception ou de mise en page. Les fragments de contenu sont généralement créés en tant que contenu indépendant du canal, destiné à être utilisé et réutilisé sur plusieurs canaux, ce qui à son tour encapsule le contenu dans un contexte spécifique.

Les fragments de contenu, indépendamment de la mise en page, peuvent être utilisés directement en AEM Sites avec les composants principaux ou peuvent être distribués sans encombre aux canaux en aval.

Cette série de vidéos présente les options de diffusion d’utilisation des fragments de contenu. Vous trouverez des détails sur la définition et la création de [fragments de contenu ici](content-fragments-feature-video-use.md).

1. Utilisation de fragments de contenu sur des pages Web
2. Exposition de fragments de contenu au format JSON à l’aide de AEM Content Services
3. Utilisation de l’API HTTP Assets

## Utilisation de fragments de contenu dans des pages Web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Les fragments de contenu peuvent être utilisés sur les pages AEM Sites ou, de la même manière, sur les fragments d’expérience, à l’aide du composant [Fragment de contenu](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/content-fragment-component.html) des composants principaux de la gestion de contenu Web  de l’AEM.

Les composants Fragment de contenu peuvent être mis en forme à l’aide de AEM Style System pour afficher le contenu selon les besoins.

## Présentation des fragments de contenu en tant que JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services facilite la création d’AEM points de terminaison HTTP basés sur les pages qui renvoient le contenu dans un format JSON normalisé.

La vidéo ci-dessus utilise le [composant de fragment de contenu](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html) pour exposer des fragments de contenu individuels. Le [composant de Liste de fragment de contenu](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) est un nouveau composant qui permet à un auteur de définir une requête qui renseigne dynamiquement la page avec une liste de fragments de contenu. Le composant Liste de fragments de contenu est préférable lorsque plusieurs fragments de contenu doivent être exposés.

*Exemple de charge utile JSON de point de terminaison Content Services :*\
**[athlètes.json](assets/athletes.json)**

## Utilisation de l’API HTTP Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

Pour la première fois dans AEM 6.5, la prise en charge des fragments de contenu est améliorée avec l’API HTTP Assets. Cela permet aux développeurs d’exécuter facilement des opérations de création, de lecture, de mise à jour et de suppression (CRUD) sur des fragments de contenu.

*Exemple de requête POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Méthode de diffusion à utiliser

### Canal web

La méthode de diffusion d’un fragment de contenu via un canal Web est simple en utilisant le composant Fragment de contenu avec AEM Sites.

### Sans tête

Il existe deux options pour exposer le fragment de contenu au format JSON afin de prendre en charge un canal tiers dans un cas d’utilisation sans en-tête :

1. Utilisez AEM Content Services et les pages d’API proxy (vidéo n° 2) lorsque le cas d’utilisation Principal consiste à diffuser des fragments de contenu à des fins de consommation (lecture seule) par un canal tiers. La structure Content Services offre davantage de flexibilité et d’options quant aux données à exposer. Les développeurs peuvent également étendre la structure Content Services pour enrichir et/ou enrichir les données.

2. Utilisez l’API HTTP Assets (vidéo n° 3) lorsque le canal tiers doit modifier et/ou mettre à jour des fragments de contenu. Un cas d’utilisation typique est l’importation de contenu tiers sur un environnement d’auteur AEM.

## Ressources supplémentaires {#additional-resources}

* [Création de fragments de contenu](content-fragments-feature-video-use.md)
* [Composants principaux WCM AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)
* [Composant de fragment de contenu principal de gestion de contenu Web AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

Pour télécharger et installer le package ci-dessous sur une instance AEM 6.4+ pour l’état final à partir de la série vidéo :\
**[aem_demo_fluide-experience-content-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
