---
title: Extension des microservices Asset Compute pour AEM en tant que Cloud Service
description: Ce didacticiel vous guide tout au long de la création d’un collaborateur simple Asset Compute qui crée un rendu d’actif en recadrant le fichier d’origine sur un cercle et applique un contraste et une luminosité configurables. Bien que le programme de travail lui-même soit de base, ce didacticiel l’utilise pour explorer la création, le développement et le déploiement d’un programme de travail Asset Compute personnalisé à utiliser avec AEM en tant que Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 1%

---


# Extensibilité des microservices de l&#39;Asset Compute

aem en tant que Cloud Service Asset Compute les microservices prennent en charge le développement et le déploiement de travailleurs personnalisés utilisés pour lire et manipuler les données binaires des ressources stockées dans AEM, le plus souvent, pour créer des rendus de ressources personnalisés.

Alors que dans AEM 6.x, les processus AEM de flux de travail personnalisés ont été utilisés pour lire, transformer et écrire des rendus de ressources, dans l&#39; en tant que travailleurs Cloud Service Asset Compute répondent à ce besoin.

## Ce que vous allez faire

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Ce didacticiel vous guide tout au long de la création d’un collaborateur simple Asset Compute qui crée un rendu d’actif en recadrant le fichier d’origine sur un cercle et applique un contraste et une luminosité configurables. Bien que le programme de travail lui-même soit de base, ce didacticiel l’utilise pour explorer la création, le développement et le déploiement d’un programme de travail Asset Compute personnalisé à utiliser avec AEM en tant que Cloud Service.

### Objectifs {#objective}

1. Fourniture et configuration des comptes et services nécessaires à la création et au déploiement d&#39;un intervenant Asset Compute
1. Création et configuration d’un projet Asset Compute
1. Développer un collaborateur de calcul de ressources am qui génère un rendu personnalisé
1. Ecrivez des tests pour et apprenez comment déboguer le collaborateur Asset Compute personnalisé
1. Déployez le travailleur Asset Compute et intégrez-le AEM en tant que service d’auteur Cloud Service via des Profils de traitement

## Configuration

Apprenez à vous préparer à étendre les effectifs de Asset Compute et à comprendre quels services et comptes doivent être configurés et configurés, ainsi que les logiciels installés localement pour le développement.

### Approvisionnement des comptes et des services{#accounts-and-services}

Les comptes et services suivants nécessitent la mise en service et l&#39;accès à pour compléter le tutoriel, AEM en tant qu&#39;environnement de développement Cloud Service ou programme Sandbox, l&#39;accès à l&#39;Adobe Project Firefly et l&#39;Enregistrement Microsoft Azure Blob.

+ [Fourniture de comptes et de services](./set-up/accounts-and-services.md)

### Environnement de développement local

Le développement local des projets Asset Compute requiert un ensemble d&#39;outils de développement spécifique, différent du développement AEM traditionnel, notamment : Microsoft Visual Studio Code, Docker Desktop, Node.js et modules npm pris en charge.

+ [Configuration de l&#39;environnement de développement local](./set-up/development-environment.md)

### Adobe Projet Firefly

Les projets Asset Compute sont des projets spécialement définis de Adobe Project Firefly et, en tant que tels, nécessitent l&#39;accès à Adobe Project Firefly dans la console de développement des Adobes pour les configurer et les déployer.

+ [Configurer la luciole du projet d&#39;Adobe](./set-up/firefly.md)

## Développer

Découvrez comment créer et configurer un projet Asset Compute, puis développer un intervenant personnalisé qui génère un rendu d’actif sur mesure.

### Créer un projet Asset Compute

Les projets Asset Compute, qui contiennent un ou plusieurs agents Asset Compute, sont générés à l’aide de l’interface de ligne de commande d’E/S interactive de l’Adobe. Les projets Asset Compute sont des projets spécialement structurés de projet Adobe Firefly, qui sont à leur tour des projets Node.js.

+ [Créer un projet Asset Compute](./develop/project.md)

### Configuration des variables d’environnement

Les variables d&#39;Environnement sont conservées dans le `.env` fichier pour le développement local et sont utilisées pour fournir des informations d&#39;identification d&#39;E/S d&#39;Adobe et des informations d&#39;enregistrement de cloud requises pour le développement local.

+ [Configuration des variables d’environnement](./develop/environment-variables.md)

### Configuration du fichier manifest.yml

Les projets Asset Compute contiennent des manifestes qui définissent tous les travailleurs Asset Compute contenus dans le projet, ainsi que les ressources dont ils disposent lorsqu&#39;ils sont déployés à Adobe I/O Runtime pour être exécutés.

+ [Configuration du fichier manifest.yml](./develop/manifest.md)

### Développer un travailleur

Le développement d’un intervenant Asset Compute est au coeur de l’extension des microservices Asset Compute, car le travailleur contient le code personnalisé qui génère, ou orchestre, la génération du rendu d’actif résultant.

+ [Développer un intervenant en calcul des ressources](./develop/worker.md)

### Utilisation de l&#39;outil de développement de calcul des ressources

L&#39;outil de développement de l&#39;informatique d&#39;actifs fournit un outil Web local pour le déploiement, l&#39;exécution et l&#39;aperçu des rendus générés par les travailleurs, ce qui permet le développement rapide et itératif des travailleurs de l&#39;informatique d&#39;actifs.

+ [Utilisation de l&#39;outil de développement de calcul des ressources](./develop/development-tool.md)

## Test et débogage

Découvrez comment tester les agents de traitement d’actifs personnalisés pour qu’ils aient confiance dans leur fonctionnement et déboguer les agents de traitement d’actifs pour comprendre et résoudre les problèmes liés à l’exécution du code personnalisé.

### Tester un collaborateur

Asset Compute fournit une structure de test pour la création de suites de tests pour les employés, en définissant des tests qui garantissent un comportement correct facile.

+ [Tester un collaborateur](./test-debug/test.md)

### Débogage d’un collaborateur

Les agents Asset Compute fournissent divers niveaux de débogage, de la sortie traditionnelle à l&#39;intégration avec `console.log(..)` VS Code __et__ wskdebug ____, ce qui permet aux développeurs de parcourir le code de travail tel qu&#39;il s&#39;exécute en temps réel.

+ [Débogage d’un collaborateur](./test-debug/debug.md)

## Déploiement

Découvrez comment intégrer des employés de Asset Compute personnalisés à l’AEM en tant que Cloud Service, en les déployant d’abord dans Adobe I/O Runtime, puis en appelant à partir d’AEM en tant qu’auteur Cloud Service via les Profils de traitement d’AEM Assets.

### Déploiement sur Adobe I/O Runtime

Les employés de Asset Compute doivent être déployés à Adobe I/O Runtime pour être utilisés avec AEM comme Cloud Service.

+ [Utilisation des Profils de traitement](./deploy/runtime.md)

### Intégrer les travailleurs au moyen de Profils de traitement AEM

Une fois déployés à Adobe I/O Runtime, les employés de Asset Compute peuvent être enregistrés en AEM en tant que Cloud Service via les Profils [de traitement des](../../assets/configuring/processing-profiles.md)ressources. Les Profils de traitement sont, à leur tour, appliqués aux dossiers de fichiers qui s’appliquent aux fichiers qu’ils contiennent.

+ [Intégration des Profils de traitement AEM](./deploy/processing-profiles.md)

## Avancé

Ces didacticiels abrégés abordent des cas d&#39;utilisation plus avancés en s&#39;appuyant sur les connaissances fondamentales établies dans les chapitres précédents.

+ [Développement d’un intervenant](./advanced/metadata.md) de métadonnées Asset Compute qui peut réécrire des métadonnées dans

## Codebase sur Github

Le code de base du tutoriel est disponible sur Github à l&#39;adresse suivante :

+ [adobe/aem-guides-wknd-asset-computing](https://github.com/adobe/aem-guides-wknd-asset-compute) @ branche principale

Le code source ne contient pas le ou les `.env` `config.json` fichiers requis. Ils doivent être ajoutés et configurés à l’aide de vos [comptes et de vos informations sur les services](#accounts-and-services) .

## Ressources supplémentaires

Vous trouverez ci-dessous diverses ressources d’Adobe qui fournissent des informations supplémentaires et des API et des SDK utiles pour le développement des employés de Asset Compute.

### Documentation

+ [Documentation du service Asset Compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Fichier Lisez-moi pour l&#39;outil de développement de calcul](https://github.com/adobe/asset-compute-devtool)
+ [Exemples de travailleurs de calcul des ressources](https://github.com/adobe/asset-compute-example-workers)

### API et SDK

+ [SDK Asset Compute](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [XMP de calcul des ressources](https://github.com/adobe/asset-compute-xmp#readme)
+ [Bibliothèque Adobe Cloud Blobstore Wrapper](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe de la bibliothèque Récupération de la nouvelle tentative](https://github.com/adobe/node-fetch-retry)
+ [Exemples de travailleurs de calcul des ressources](https://github.com/adobe/asset-compute-example-workers)
