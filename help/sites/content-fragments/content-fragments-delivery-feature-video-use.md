---
title: Diffuser des fragments de contenu dans AEM
description: Les fragments de contenu, indépendamment de la disposition, peuvent être utilisés directement dans AEM Sites avec les composants principaux ou peuvent être diffusés de façon découplée sur les canaux en aval.
feature: Content Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 913
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 100%

---

# Diffuser des fragments de contenu {#delivering-content-fragments}

Les fragments de contenu Adobe Experience Manager (AEM) sont des contenus éditoriaux basés sur du texte qui peuvent inclure certains éléments de données structurées associés, mais sont considérés comme du contenu pur sans informations de conception ou de disposition. Les fragments de contenu sont généralement créés en tant que contenu indépendant du canal, destiné à être utilisé et réutilisé sur plusieurs canaux, ce qui à son tour encapsule le contenu dans une expérience spécifique au contexte.

Les fragments de contenu, indépendamment de la disposition, peuvent être utilisés directement dans AEM Sites avec les composants principaux ou peuvent être diffusés de façon découplée sur les canaux en aval.

Cette série de vidéos présente les options de diffusion pour l’utilisation de fragments de contenu. Vous trouverez plus d’informations sur la définition et la [création de fragments de contenu ici](content-fragments-feature-video-use.md).

1. Utiliser des fragments de contenu sur des pages web
2. Exposer des fragments de contenu au format JSON à l’aide d’AEM Content Services
3. Utiliser l’API HTTP Assets

## Utiliser des fragments de contenu dans les pages web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

Les fragments de contenu peuvent être utilisés sur les pages AEM Sites ou, de manière similaire, dans les fragments d’expérience, à l’aide des [Composant de fragment de contenu](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=fr) des composants principaux de la gestion de contenu web AEM.

Les composants de fragment de contenu peuvent être stylisés à l’aide du système de style d’AEM pour afficher le contenu suivant les besoins.

## Exposer des fragments de contenu au format JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services facilite la création de points d’entrée HTTP basés sur une page AEM qui convertissent le contenu dans un format JSON normalisé.

La vidéo ci-dessus utilise le [Composant de fragment de contenu](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=fr) pour exposer des fragments de contenu individuels. Le [Composant Liste de fragment de contenu](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/content-fragment-list.html?lang=fr) est un nouveau composant qui permet à un auteur ou à une autrice de définir une requête qui renseignera dynamiquement la page avec une liste de fragments de contenu. Le composant Liste de fragments de contenu est préférable lorsque plusieurs fragments de contenu doivent être exposés.

*Exemple de payload JSON du point d’entrée Content Services :*\
**[athletes.json](assets/athletes.json)**

## Utiliser l’API HTTP Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

AEM 6.5 comprend la première version d’une prise en charge améliorée des fragments de contenu avec l’API HTTP Assets. Cela permet aux développeurs et aux développeuses d’effectuer facilement des opérations CRUD (création, lecture, mise à jour et suppression) sur des fragments de contenu.

*Exemples de requêtes POSTMAN :*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Quelle méthode de diffusion utiliser

### Canal web

La méthode de diffusion d’un fragment de contenu via un canal web est simple en utilisant le composant de fragment de contenu avec AEM Sites.

### Découplé

Il existe deux options pour exposer le fragment de contenu au format JSON afin de prendre en charge un canal tiers dans un cas d’utilisation découplé :

1. Utilisez les pages de l’API Proxy et AEM Content Services (vidéo #2) lorsque le cas d’utilisation principal est de fournir des fragments de contenu à des fins de consommation (lecture seule) par un canal tiers. Le cadre Content Services offre davantage de flexibilité et d’options quant aux données exposées. Les développeurs et les développeuses peuvent également étendre le framework Content Services pour augmenter et/ou enrichir les données.

2. Utilisez l’API HTTP Assets (vidéo #3) lorsque le canal tiers doit modifier et/ou mettre à jour les fragments de contenu. Le scénario d’utilisation le plus courant est l’ingestion de contenu tiers dans un environnement de création AEM.

## Ressources supplémentaires {#additional-resources}

* [Créer des fragments de contenu](content-fragments-feature-video-use.md)
* [Composants principaux de la gestion de contenu web d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [Composant principal des fragments de contenu AEM WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=fr)

Pour télécharger et installer le package ci-dessous sur une instance AEM 6.4 ou ultérieure pour l’état final de la série vidéo :\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
