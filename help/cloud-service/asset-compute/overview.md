---
title: Extensibilité des microservices d’Asset Compute pour AEM as a Cloud Service
description: Ce tutoriel décrit comment créer un programme de travail simple d’Asset Compute qui crée un rendu de ressource en réduisant la ressource originale à un cercle et en appliquant un contraste et une luminosité configurables. Ce tutoriel utilise le programme de travail lui-même pour explorer la création, le développement et le déploiement d’un programme de travail personnalisé d’Asset Compute à utiliser avec AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 333
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 100%

---

# Extensibilité des microservices d’Asset Compute

Les microservices d’Asset Compute d’AEM as a Cloud Service prennent en charge le développement et le déploiement de programmes de travail personnalisés dans la lecture et la manipulation des données binaires des ressources stockées dans AEM, le plus souvent pour créer des rendus de ressources personnalisés.

Alors que dans AEM 6.x, les processus de workflow personnalisés d’AEM étaient utilisés pour lire, transformer et écrire des rendus de ressources, tout cela est effectué par les programmes de travail d’Asset Compute dans AEM as a Cloud Service.

## Ce que vous devez faire

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Ce tutoriel décrit comment créer un programme de travail simple d’Asset Compute qui crée un rendu de ressource en réduisant la ressource originale à un cercle et en appliquant un contraste et une luminosité configurables. Ce tutoriel utilise le programme de travail lui-même pour explorer la création, le développement et le déploiement d’un programme de travail personnalisé d’Asset Compute à utiliser avec AEM as a Cloud Service.

### Objectifs {#objective}

1. Fournir et configurer les comptes et services nécessaires pour construire et déployer un programme de travail d’Asset Compute
1. Créer et configurer un projet Asset Compute
1. Développer un programme de travail d’Asset Compute qui génère un rendu personnalisé
1. Rédiger des tests et apprendre à déboguer le programme de travail d’Asset Compute personnalisé.
1. Déployer le programme de travail d’Asset Compute et l’intégrer au service de création d’AEM as a Cloud Service via les profils de traitement

## Configuration

Découvrez comment vous préparer correctement à étendre les secondaires Asset Compute. Apprenez également à identifier les services et comptes qui doivent être configurés et approvisionnés, et les logiciels qui doivent être installés localement en vue du développement.

### Approvisionnement des comptes et des services{#accounts-and-services}

Les comptes et services suivants nécessitent un approvisionnement et un accès pour compléter le tutoriel : environnement de développement ou programme de sandbox d’AEM as a Cloud Service, accès à App Builder et à Microsoft Azure Blob Storage.

+ [Approvisionnement de comptes et de services](./set-up/accounts-and-services.md)

### Environnement de développement local

Le développement local de projets Asset Compute nécessite un ensemble d’outils de développement spécifique, différent de celui requis pour le développement traditionnel avec AEM, comme par exemple : Microsoft Visual Studio Code, Docker Desktop, Node.js et la prise en charge des modules npm.

+ [Configurer l’environnement de développement local](./set-up/development-environment.md)

### App Builder

Les projets Asset Compute sont des projets App Builder définis spécialement et, à ce titre, ils nécessitent un accès à App Builder dans l’Adobe Developer Console pour être configurés et déployés.

+ [Configurer App Builder](./set-up/app-builder.md)

## Développer

Découvrez comment créer et configurer un projet Asset Compute, puis développer un programme de travail personnalisé qui génère un rendu de ressource personnalisé.

### Créer un nouveau projet Asset Compute

Les projets Asset Compute, qui contiennent un ou plusieurs programmes de travail d’Asset Compute, sont générés à l’aide de l’interface de ligne de commande interactive d’Adobe I/O CLI. Les projets Asset Compute sont des projets App Builder spécialement structurés, qui sont aussi des projets Node.js.

+ [Créer un nouveau projet Asset Compute](./develop/project.md)

### Configurer les variables d’environnement

Les variables d’environnement sont conservées dans le fichier `.env` pour le développement local et sont utilisées pour fournir des informations d’identification d’Adobe I/O et de l’espace de stockage dans le cloud requises pour le développement local.

+ [Configurer les variables d’environnement](./develop/environment-variables.md)

### Configurer le fichier manifest.yml

Les projets Asset Compute contiennent des manifestes qui définissent tous les programmes de travail d’Assets Compute contenus dans le projet, ainsi que les ressources dont ils disposent lorsqu’ils sont déployés vers Adobe I/O Runtime pour exécution.

+ [Configurer le fichier manifest.yml](./develop/manifest.md)

### Développer un programme de travail

Le développement d’un programme de travail d’Asset Compute est essentiel pour étendre les microservices d’Asset Compute, car le programme de travail comporte le code personnalisé qui génère, ou qui structure, la génération du rendu de la ressource correspondante.

+ [Développer un programme de travail d’Asset Compute](./develop/worker.md)

### Utiliser l’outil de développement d’Assets Compute

L’outil de développement d’Asset Compute fournit un outil web local pour le déploiement, l’exécution et la prévisualisation des rendus générés par les programmes de travail. Cela permet de développer rapidement et de manière itérative des programmes de travail d’Asset Compute.

+ [Utiliser l’outil de développement d’Assets Compute](./develop/development-tool.md)

## Tester et déboguer

Apprenez à tester les programmes de travail personnalisés d’Asset Compute pour vous assurer qu’ils fonctionnent correctement, et à les déboguer pour mieux comprendre et résoudre les problèmes d’exécution du code personnalisé.

### Tester un programme de travail

Asset Compute fournit un cadre de test qui permet de créer des suites de test pour les programmes de travail, ce qui facilite la définition de tests assurant un bon fonctionnement.

+ [Tester un programme de travail](./test-debug/test.md)

### Déboguer un programme de travail

Les workers d’Asset Compute fournissent divers niveaux de débogage, de la traditionnelle sortie `console.log(..)` aux intégrations à __VS Code__ et à __wskdebug__, ce qui permet aux développeurs et développeuses de parcourir le code du programme de travail tel qu’il s’exécute en temps réel.

+ [Déboguer un programme de travail](./test-debug/debug.md)

## Déployer

Découvrez comment intégrer des programmes de travail personnalisés d’Asset Compute à AEM as a Cloud Service, en les déployant d’abord vers Adobe I/O Runtime, puis en les invoquant depuis le service de création d’AEM as a Cloud Service via les profils de traitement d’AEM Assets.

### Déployer sur Adobe I/O Runtime

Les programmes de travail d’Asset Compute doivent être déployés sur Adobe I/O Runtime pour être utilisés avec AEM as a Cloud Service.

+ [Utiliser des profils de traitement](./deploy/runtime.md)

### Intégrer des programmes de travail via des profils de traitement AEM

Une fois déployés dans Adobe I/O Runtime, les programmes de travail d’Assets Compute peuvent être enregistrés dans AEM as a Cloud Service via des [profils de traitement Assets](../../assets/configuring/processing-profiles.md). Les profils de traitement sont ensuite appliqués aux dossiers des ressources s’appliquant aux ressources qu’ils contiennent.

+ [Intégrer aux profils de traitement AEM](./deploy/processing-profiles.md)

## Avancé

Ces tutoriels abrégés abordent d’autres cas d’utilisation avancés en s’appuyant sur des connaissances fondamentales établies dans les chapitres précédents.

+ [Développer un programme de travail de métadonnées d’Asset Compute](./advanced/metadata.md) qui peut réécrire des métadonnées dans

## la base de code sur Github.

La base de code du tutoriel est disponible sur Github à l’adresse :

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ branche principale.

Le code source ne contient pas les fichiers `.env` ou `config.json` requis. Ils doivent être ajoutés et configurés à l’aide de vos informations de [comptes et services](#accounts-and-services).

## Ressources supplémentaires

Vous trouverez ci-dessous diverses ressources d’Adobe qui fournissent des informations supplémentaires, ainsi que des API et des SDK utiles pour le développement des programmes de travail d’Asset Compute.

### Documentation

+ [Documentation Asset Compute Service](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=fr)
+ [Fichier Lisez-moi de l’outil de développement d’Asset Compute](https://github.com/adobe/asset-compute-devtool)
+ [Exemples de programmes de travail d’Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### API et SDK

+ [SDK d’Asset Compute](https://github.com/adobe/asset-compute-sdk)
   + [Éléments courants d’Asset Compute](https://github.com/adobe/asset-compute-commons)
   + [XMP d’Asset Compute](https://github.com/adobe/asset-compute-xmp#readme)
+ [Bibliothèque Blobstore Wrapper d’Adobe Cloud](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Bibliothèque Node Fetch Retry d’Adobe](https://github.com/adobe/node-fetch-retry)
+ [Exemples de programmes de travail d’Asset Compute](https://github.com/adobe/asset-compute-example-workers)
