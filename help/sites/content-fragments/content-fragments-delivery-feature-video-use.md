---
title: Diffusion de fragments de contenu dans AEM
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: Les fragments de contenu, indépendamment de la mise en page, peuvent être utilisés directement dans AEM Sites avec les composants principaux ou peuvent être diffusés sans interface sur les canaux en aval.
seo-description: Content Fragments, independent of layout, can be used directly in AEM Sites with Core Components or can be delivered in a headless manner to downstream channels.
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: User
level: Beginner
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 6%

---

# Diffusion de fragments de contenu {#delivering-content-fragments}

Les fragments de contenu Adobe Experience Manager (AEM) sont des contenus éditoriaux basés sur du texte qui peuvent inclure certains éléments de données structurés associés, mais considérés comme du contenu pur sans informations de conception ou de mise en page. Les fragments de contenu sont généralement créés en tant que contenu indépendant du canal, destiné à être utilisé et réutilisé sur plusieurs canaux, ce qui à son tour encapsule le contenu dans une expérience spécifique au contexte.

Les fragments de contenu, indépendamment de la mise en page, peuvent être utilisés directement dans AEM Sites avec les composants principaux ou peuvent être diffusés sans interface sur les canaux en aval.

Cette série de vidéos présente les options de remise pour l’utilisation de fragments de contenu. Détails sur la définition et [création de fragments de contenu ici](content-fragments-feature-video-use.md).

1. Utilisation de fragments de contenu sur des pages web
2. Exposition de fragments de contenu au format JSON à l’aide d’AEM Content Services
3. Utilisation de l’API HTTP Assets

## Utilisation de fragments de contenu dans des pages web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Les fragments de contenu peuvent être utilisés sur les pages AEM Sites ou de manière similaire, dans les fragments d’expérience, à l’aide des composants principaux de la gestion de contenu web AEM [Composant Fragment de contenu](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=fr).

Les composants de fragment de contenu peuvent être stylisés à l’aide d’AEM système de style pour afficher le contenu suivant les besoins.

## Exposition de fragments de contenu au format JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services facilite la création d’AEM points de terminaison HTTP basés sur une page qui convertissent le contenu dans un format JSON normalisé.

La vidéo ci-dessus utilise la méthode [Composant de fragment de contenu](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) pour exposer des fragments de contenu individuels. Le [Composant de liste de fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) est un nouveau composant qui permet à un auteur de définir une requête qui renseignera dynamiquement la page avec une liste de fragments de contenu. Le composant Liste de fragments de contenu est préférable lorsque plusieurs fragments de contenu doivent être exposés.

*Exemple de payload JSON du point d’entrée Content Services :*\
**[athlètes.json](assets/athletes.json)**

## Utilisation de l’API HTTP Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

La première version d’AEM 6.5 est une prise en charge améliorée des fragments de contenu avec l’API HTTP Assets. Cela permet aux développeurs d’effectuer facilement des opérations CRUD (Create, Read, Update et Delete) sur des fragments de contenu.

*Exemples de requêtes POSTMAN :*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Quel mode de diffusion utiliser

### Canal web

La méthode de diffusion d’un fragment de contenu via un canal web est simple en utilisant le composant Fragment de contenu avec AEM Sites.

### Découplé

Il existe deux options pour exposer le fragment de contenu au format JSON afin de prendre en charge un canal tiers dans un cas d’utilisation sans affichage :

1. Utilisez les pages Content Services AEM et de l’API Proxy (#2 vidéo) lorsque le cas d’utilisation Principal est de fournir des fragments de contenu à utiliser par un canal tiers (lecture seule). La structure Content Services offre davantage de flexibilité et d’options quant aux données exposées. Les développeurs peuvent également étendre la structure Content Services pour augmenter et/ou enrichir les données.

2. Utilisez l’API HTTP Assets (Video #3) lorsque le canal tiers doit modifier et/ou mettre à jour les fragments de contenu. Un cas d’utilisation type est l’ingestion de contenu tiers dans un environnement de création AEM.

## Ressources supplémentaires {#additional-resources}

* [Création de fragments de contenu](content-fragments-feature-video-use.md)
* [AEM composants principaux WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [AEM composant de fragment de contenu WCM principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

Pour télécharger et installer le package ci-dessous sur une instance AEM 6.4+ pour l’état final de la série vidéo :\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
