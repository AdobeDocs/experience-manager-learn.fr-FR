---
title: Extension des microservices d'Asset compute pour AEM en tant que Cloud Service
description: Ce didacticiel vous guide tout au long de la création d’un simple collaborateur d’Assets compute qui crée un rendu de fichier en recadrant le fichier d’origine sur un cercle et applique un contraste et une luminosité configurables. Bien que le programme de travail lui-même soit de base, ce didacticiel l’utilise pour explorer la création, le développement et le déploiement d’un Asset compute de travail personnalisé à utiliser avec AEM en tant que Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 3%

---


# Extensibilité des microservices d&#39;Asset compute

AEM en tant que microservices de Asset compute Cloud Service prennent en charge le développement et le déploiement de travailleurs personnalisés utilisés pour lire et manipuler les données binaires des ressources stockées dans AEM, le plus souvent, pour créer des rendus de ressources personnalisés.

Alors que dans AEM 6.x, les processus AEM de flux de travail personnalisés ont été utilisés pour lire, transformer et écrire en retour des rendus de ressources, dans l&#39;, en tant que travailleurs d&#39;Asset compute Cloud Service répondent à ce besoin.

## Ce que vous allez faire

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Ce didacticiel vous guide tout au long de la création d’un simple collaborateur d’Assets compute qui crée un rendu de fichier en recadrant le fichier d’origine sur un cercle et applique un contraste et une luminosité configurables. Bien que le programme de travail lui-même soit de base, ce didacticiel l’utilise pour explorer la création, le développement et le déploiement d’un Asset compute de travail personnalisé à utiliser avec AEM en tant que Cloud Service.

### Objectifs {#objective}

1. Fourniture et configuration des comptes et services nécessaires à la création et au déploiement d&#39;un travailleur d&#39;Asset compute
1. Création et configuration d’un projet d’Asset compute
1. Développer un collaborateur d’Assets compute am qui génère un rendu personnalisé
1. Ecrivez des tests pour et apprenez comment déboguer l&#39;agent d&#39;Asset compute personnalisé.
1. Déployez le travailleur d’Asset compute et intégrez-le AEM en tant que service d’auteur Cloud Service via les Profils de traitement

## Configuration

Apprenez à vous préparer correctement à l&#39;extension du personnel d&#39;Asset compute et à comprendre quels services et comptes doivent être configurés et configurés, et quels logiciels doivent être installés localement pour le développement.

### Approvisionnement des comptes et des services{#accounts-and-services}

Les comptes et services suivants nécessitent la mise en service et l&#39;accès à pour compléter le tutoriel, AEM en tant qu&#39;environnement de développement Cloud Service ou programme Sandbox, l&#39;accès à l&#39;Adobe Project Firefly et l&#39;Enregistrement Microsoft Azure Blob.

+ [Fourniture de comptes et de services](./set-up/accounts-and-services.md)

### Environnement de développement local

Le développement local des projets d&#39;Asset compute nécessite un ensemble d&#39;outils spécifiques de développement, différent du développement AEM traditionnel, notamment : Microsoft Visual Studio Code, Docker Desktop, Node.js et modules npm pris en charge.

+ [Configuration de l&#39;environnement de développement local](./set-up/development-environment.md)

### Adobe Projet Firefly

Les projets d&#39;Asset compute sont des projets de lucioles d&#39;Adobe spécialement définis et, en tant que tels, nécessitent l&#39;accès à la luciole de projet d&#39;Adobe dans la console de développement des Adobes pour les configurer et les déployer.

+ [Configurer la luciole du projet d&#39;Adobe](./set-up/firefly.md)

## Développer

Découvrez comment créer et configurer un projet d’Asset compute, puis développer un intervenant personnalisé qui génère un rendu d’actif sur mesure.

### Création d’un projet d’Asset compute

Les projets d’Asset compute, qui contiennent un ou plusieurs travailleurs de l’Asset compute, sont générés à l’aide de l’interface de ligne de commande interactive de l’Adobe I/O. Les projets d&#39;Asset compute sont des projets de lucioles d&#39;Adobe spécialement structurés, qui sont à leur tour des projets Node.js.

+ [Création d’un projet d’Asset compute](./develop/project.md)

### Configuration des variables d’environnement

Les variables d&#39;Environnement sont conservées dans le fichier `.env` pour le développement local et sont utilisées pour fournir les informations d&#39;identification d&#39;Adobe I/O et les informations d&#39;identification d&#39;enregistrement cloud requises pour le développement local.

+ [Configuration des variables d’environnement](./develop/environment-variables.md)

### Configuration du fichier manifest.yml

Les projets d&#39;Asset compute contiennent des manifestes qui définissent tous les travailleurs de l&#39;Asset compute contenus dans le projet, ainsi que les ressources dont ils disposent lorsqu&#39;ils sont déployés à Adobe I/O Runtime pour exécution.

+ [Configuration du fichier manifest.yml](./develop/manifest.md)

### Développer un travailleur

Le développement d’un travailleur d’Asset compute constitue le coeur de l’extension des microservices d’Asset compute, dans la mesure où le travailleur contient le code personnalisé qui génère, ou orchestre, la génération du rendu d’actif résultant.

+ [Développer un Asset compute](./develop/worker.md)

### Utilisation de l&#39;outil de développement d&#39;Assets compute

L&#39;outil de développement d&#39;Assets compute fournit un outil Web local pour le déploiement, l&#39;exécution et l&#39;aperçu des rendus générés par les travailleurs, ce qui permet le développement rapide et itératif des travailleurs d&#39;Asset compute.

+ [Utilisation de l&#39;outil de développement d&#39;Assets compute](./develop/development-tool.md)

## Test et débogage

Découvrez comment tester les agents d&#39;Asset compute personnalisés pour être sûrs de leur fonctionnement et déboguer les agents d&#39;Asset compute pour comprendre et résoudre les problèmes liés à l&#39;exécution du code personnalisé.

### Tester un collaborateur

L’Asset compute fournit une structure de test pour la création de suites de tests pour les travailleurs, en définissant des tests qui garantissent un comportement correct facile.

+ [Tester un collaborateur](./test-debug/test.md)

### Débogage d’un collaborateur

Les travailleurs d&#39;Asset compute fournissent divers niveaux de débogage, de la sortie traditionnelle `console.log(..)` à l&#39;intégration avec __VS Code__ et __wskdebug__, ce qui permet aux développeurs de parcourir le code de travail tel qu&#39;il s&#39;exécute en temps réel.

+ [Débogage d’un collaborateur](./test-debug/debug.md)

## Déploiement

Découvrez comment intégrer des travailleurs d’Asset compute personnalisés avec AEM en tant que Cloud Service, en les déployant d’abord à Adobe I/O Runtime, puis en appelant à partir d’AEM en tant qu’auteur Cloud Service via les Profils de traitement d’AEM Assets.

### Déploiement sur Adobe I/O Runtime

Les employés d&#39;Asset compute doivent être déployés à Adobe I/O Runtime pour être utilisés avec l&#39;AEM comme Cloud Service.

+ [Utilisation des Profils de traitement](./deploy/runtime.md)

### Intégrer les travailleurs au moyen de Profils de traitement AEM

Une fois déployés à Adobe I/O Runtime, les travailleurs de l&#39;Asset compute peuvent être enregistrés en tant que Cloud Service dans l&#39;AEM par l&#39;intermédiaire de [Profils de traitement des actifs](../../assets/configuring/processing-profiles.md). Les Profils de traitement sont, à leur tour, appliqués aux dossiers de fichiers qui s’appliquent aux fichiers qu’ils contiennent.

+ [Intégration des Profils de traitement AEM](./deploy/processing-profiles.md)

## Avancé

Ces didacticiels abrégés abordent des cas d&#39;utilisation plus avancés en s&#39;appuyant sur les connaissances fondamentales établies dans les chapitres précédents.

+ [Développement d’un ](./advanced/metadata.md) travail de métadonnées d’Asset compute capable de réécrire les métadonnées dans

## Codebase sur Github

Le code de base du tutoriel est disponible sur Github à l&#39;adresse suivante :

+ [branche principale adobe/aem-guides-wknd-asset-computing](https://github.com/adobe/aem-guides-wknd-asset-compute) @

Le code source ne contient pas les fichiers `.env` ou `config.json` requis. Ils doivent être ajoutés et configurés à l&#39;aide de vos [comptes et services](#accounts-and-services) informations.

## Ressources supplémentaires

Vous trouverez ci-dessous diverses ressources d&#39;Adobe qui fournissent des informations complémentaires et des API et SDK utiles pour le développement de travailleurs d&#39;Asset compute.

### Documentation

+ [Documentation Asset compute Service](https://docs.adobe.com/content/help/fr-FR/asset-compute/using/extend/understand-extensibility.html)
+ [Outil de développement d&#39;Asset compute Lisez-moi](https://github.com/adobe/asset-compute-devtool)
+ [Exemples d&#39;Assets compute](https://github.com/adobe/asset-compute-example-workers)

### API et SDK

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Bibliothèque Adobe Cloud Blobstore Wrapper](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe de la bibliothèque Récupération de la nouvelle tentative](https://github.com/adobe/node-fetch-retry)
+ [Exemples d&#39;Assets compute](https://github.com/adobe/asset-compute-example-workers)
