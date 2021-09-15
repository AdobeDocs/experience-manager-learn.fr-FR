---
title: Débogage d’AEM en tant que Cloud Service
description: sur l’infrastructure cloud en libre-service, évolutive, ce qui nécessite que les développeurs d’AEM comprennent comment comprendre et déboguer diverses facettes d’AEM en tant que Cloud Service, de la création et du déploiement à l’obtention des détails sur l’exécution des applications d’AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---

# Débogage d’AEM en tant que Cloud Service

AEM en tant que Cloud Service est le moyen natif de tirer parti des applications AEM. AEM as a Cloud Service s’exécute sur une infrastructure cloud évolutive en libre-service, ce qui nécessite AEM développeurs de comprendre comment comprendre et déboguer différentes facettes d’en tant que Cloud Service, de la création et du déploiement à l’obtention des détails sur l’exécution des applications.

## Journaux

Les journaux fournissent des détails sur le fonctionnement de votre application dans AEM en tant que Cloud Service, ainsi que des informations sur les problèmes liés aux déploiements.

[Débogage d’AEM en tant que Cloud Service à l’aide des journaux](./logs.md)

## Création et déploiement

Les pipelines d’Adobe Cloud Manager déploient AEM application par le biais d’une série d’étapes afin de déterminer la qualité et la viabilité du code lorsqu’ils sont déployés sur AEM en tant que Cloud Service. Chacune de ces étapes peut entraîner un échec. Il est donc important de comprendre comment déboguer les versions afin de déterminer la cause première de et comment résoudre les échecs.

[Débogage d’AEM en tant que version et déploiement de Cloud Service](./build-and-deployment.md)

## Developer Console

Developer Console fournit diverses informations et introspections dans AEM en tant qu’environnements de Cloud Service utiles pour comprendre comment votre application est reconnue par et fonctionne dans AEM en tant que Cloud Service.

[Débogage d’AEM en tant que Cloud Service avec Developer Console](./developer-console.md)

## CRXDE Lite

CRXDE Lite est un outil classique, mais puissant, pour le débogage d’AEM en tant qu’environnements de développement de Cloud Service. CRXDE Lite fournit une suite de fonctionnalités qui aide le débogage à ne pas inspecter toutes les ressources et propriétés, à manipuler les parties modifiables du JCR, à examiner les autorisations et à évaluer les requêtes.

[Débogage d’AEM en tant que Cloud Service avec CRXDE Lite](./crxde-lite.md)
