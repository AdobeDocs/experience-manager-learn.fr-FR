---
title: Extensibilité des microservices d’Asset compute pour AEM as a Cloud Service
description: Ce tutoriel décrit la création d’un objet Worker d’Assets compute simple qui crée un rendu de ressource en recadrant la ressource d’origine sur un cercle et en appliquant un contraste et une luminosité configurables. Bien que le programme de travail lui-même soit de base, ce tutoriel l’utilise pour explorer la création, le développement et le déploiement d’un programme de travail d’Asset compute personnalisé à utiliser avec AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 3%

---

# Extensibilité des microservices d’Asset compute

AEM en tant que microservices d’Asset compute Cloud Service prennent en charge le développement et le déploiement de programmes de travail personnalisés utilisés pour lire et manipuler les données binaires des ressources stockées dans AEM, le plus souvent, pour créer des rendus de ressources personnalisés.

Alors que dans AEM 6.x, les processus AEM personnalisés étaient utilisés pour lire, transformer et écrire des rendus de ressources, dans les Assets compute as a Cloud Service, les processus répondent à ce besoin.

## Ce que vous allez faire

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Ce tutoriel décrit la création d’un objet Worker d’Assets compute simple qui crée un rendu de ressource en recadrant la ressource d’origine sur un cercle et en appliquant un contraste et une luminosité configurables. Bien que le programme de travail lui-même soit de base, ce tutoriel l’utilise pour explorer la création, le développement et le déploiement d’un programme de travail d’Asset compute personnalisé à utiliser avec AEM as a Cloud Service.

### Objectifs {#objective}

1. Configuration et configuration des comptes et services nécessaires pour créer et déployer un Asset compute Worker
1. Création et configuration d’un projet Asset compute
1. Développer un objet Worker d’Assets compute qui génère un rendu personnalisé
1. Rédigez des tests pour et apprenez à déboguer le programme de travail d’Asset compute personnalisé.
1. Déployez Asset compute Worker et intégrez-le AEM service Auteur as a Cloud Service via les profils de traitement

## Configuration

Découvrez comment vous préparer correctement à étendre les employés d’Asset compute et comprendre quels services et comptes doivent être configurés et configurés, ainsi que quels logiciels installés localement en vue du développement.

### Approvisionnement des comptes et des services{#accounts-and-services}

Les comptes et services suivants nécessitent la mise en service et l’accès à pour suivre le tutoriel, AEM l’environnement de développement as a Cloud Service ou le programme Sandbox, l’accès à App Builder et au stockage Azure Blob Microsoft.

+ [Configuration de comptes et de services](./set-up/accounts-and-services.md)

### Environnement de développement local

Le développement local de projets d’Asset compute nécessite un ensemble d’outils de développement spécifique, différent du développement AEM traditionnel, notamment : Microsoft Visual Studio Code, Docker Desktop, Node.js et prise en charge des modules npm.

+ [Configuration de l’environnement de développement local](./set-up/development-environment.md)

### App Builder

Les projets Asset compute sont des projets App Builder spécialement définis et, en tant que tels, nécessitent l’accès à App Builder dans la console Adobe Developer pour les configurer et les déployer.

+ [Configuration d’App Builder](./set-up/app-builder.md)

## Développer

Découvrez comment créer et configurer un projet Asset compute, puis développer un programme de travail personnalisé qui génère un rendu de ressource personnalisé.

### Création d’un projet Asset compute

Les projets d’Asset compute, qui contiennent un ou plusieurs objets Worker d’Asset compute, sont générés à l’aide de l’interface de ligne de commande de l’Adobe I/O interactif. Les projets Asset compute sont des projets App Builder spécialement structurés, qui sont à leur tour des projets Node.js.

+ [Création d’un projet Asset compute](./develop/project.md)

### Configuration des variables d’environnement

Les variables d’environnement sont conservées dans la variable `.env` pour le développement local et sont utilisés pour fournir des informations d’identification d’Adobe I/O et des informations d’identification de l’espace de stockage dans le cloud requises pour le développement local.

+ [Configuration des variables d’environnement ](./develop/environment-variables.md)

### Configuration du fichier manifest.yml

Les projets Asset compute contiennent des manifestes qui définissent tous les Assets compute employés contenus dans le projet, ainsi que les ressources dont ils disposent lorsqu’ils sont déployés vers Adobe I/O Runtime pour exécution.

+ [Configuration du fichier manifest.yml](./develop/manifest.md)

### Développement d’un worker

Le développement d’un objet Worker d’Asset compute constitue le coeur de l’extension des microservices d’Asset compute, dans la mesure où il contient le code personnalisé qui génère, ou orchestre, la génération du rendu de ressource résultant.

+ [Développement d’un Asset compute Worker](./develop/worker.md)

### Utilisation de l’outil de développement d’Assets compute

L’outil de développement d’Asset compute fournit un outil web local pour le déploiement, l’exécution et la prévisualisation des rendus générés par le programme de travail, ce qui permet le développement rapide et itératif des Assets compute Worker.

+ [Utilisation de l’outil de développement d’Assets compute](./develop/development-tool.md)

## Test et débogage

Découvrez comment tester les agents d’Asset compute personnalisés pour qu’ils soient confiants dans leur fonctionnement et déboguer les agents d’Asset compute pour comprendre et résoudre les problèmes liés à l’exécution du code personnalisé.

### Tester un programme de travail

asset compute fournit une structure de test pour la création de suites de tests pour les programmes de travail, en définissant des tests qui garantissent un comportement correct facile.

+ [Tester un programme de travail](./test-debug/test.md)

### Débogage d’un programme de travail

Les travailleurs de l’Asset compute fournissent divers niveaux de débogage à partir des `console.log(..)` sortie, pour les intégrations avec __Code VS__ et  __wskdebug__, ce qui permet aux développeurs de parcourir le code de travail tel qu’il s’exécute en temps réel.

+ [Débogage d’un programme de travail](./test-debug/debug.md)

## Déploiement dʼ

Découvrez comment intégrer des objets Worker Asset compute personnalisés avec AEM as a Cloud Service, en les déployant d’abord vers Adobe I/O Runtime, puis en appelant à partir d’AEM’auteur as a Cloud Service via les profils de traitement d’AEM Assets.

### Déploiement sur Adobe I/O Runtime

Les employés d’Asset compute doivent être déployés sur Adobe I/O Runtime pour être utilisés avec AEM as a Cloud Service.

+ [Utilisation des profils de traitement](./deploy/runtime.md)

### Intégrer des objets Worker via des profils de traitement AEM

Une fois déployé dans Adobe I/O Runtime, les Assets compute peuvent être enregistrés dans AEM as a Cloud Service via [Profils de traitement des ressources](../../assets/configuring/processing-profiles.md). Les profils de traitement sont, à leur tour, appliqués aux dossiers de ressources s’appliquant aux ressources qu’ils contiennent.

+ [Intégration aux profils de traitement AEM](./deploy/processing-profiles.md)

## Avancé

Ces tutoriels abrégés abordent des cas d’utilisation plus avancés en s’appuyant sur des connaissances fondamentales établies dans les chapitres précédents.

+ [Développement d’un objet Worker de métadonnées d’Asset compute](./advanced/metadata.md) qui peut réécrire des métadonnées dans le

## Codebase sur Github

Le code base du tutoriel est disponible sur Github à l’adresse :

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ branche principale

Le code source ne contient pas les `.env` ou `config.json` fichiers . Ils doivent être ajoutés et configurés à l’aide de la fonction [comptes et services](#accounts-and-services) informations.

## Ressources supplémentaires

Vous trouverez ci-dessous diverses ressources d’Adobe qui fournissent des informations supplémentaires ainsi que des API et des SDK utiles pour le développement des employés d’Asset compute.

### Documentation

+ [Documentation d’Asset compute Service](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=fr)
+ [Fichier Lisez-moi de l’outil de développement Asset compute](https://github.com/adobe/asset-compute-devtool)
+ [Exemples de traitement d’Asset compute](https://github.com/adobe/asset-compute-example-workers)

### API et SDK

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Bibliothèque Adobe Cloud Blobstore Wrapper](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe de la bibliothèque de reprise de récupération des noeuds](https://github.com/adobe/node-fetch-retry)
+ [Exemples de traitement d’Asset compute](https://github.com/adobe/asset-compute-example-workers)
