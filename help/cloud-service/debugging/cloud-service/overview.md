---
title: Débogage de l’AEM en tant que Cloud Service
description: sur une infrastructure de cloud libre-service, évolutive et qui requiert des développeurs AEM de comprendre comment comprendre et déboguer les différentes facettes de l'AEM en tant que Cloud Service, de la génération et du déploiement à l'obtention des détails sur l'exécution des applications AEM.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 2%

---


# Débogage de l’AEM en tant que Cloud Service

L&#39;AEM en tant que Cloud Service est le moyen natif du cloud de tirer parti des applications AEM. AEM en tant que Cloud Service s&#39;exécute sur une infrastructure de cloud en libre-service, évolutive, ce qui nécessite que AEM développeurs comprennent comment comprendre et déboguer les différentes facettes de l&#39;Cloud Service, de la création et du déploiement à l&#39;obtention des détails sur l&#39;exécution des applications .

## Journaux

Les journaux fournissent des informations détaillées sur le fonctionnement de votre application en tant que Cloud Service AEM, ainsi que des informations sur les problèmes liés aux déploiements.

[Débogage de l’AEM en tant que Cloud Service à l’aide de journaux](./logs.md)

## Création et déploiement

Les pipelines Adobe Cloud Manager déploient l’application AEM par le biais d’une série d’étapes visant à déterminer la qualité et la viabilité du code lorsqu’ils sont déployés sur AEM en tant que Cloud Service. Chacune de ces étapes peut entraîner un échec, ce qui rend important de comprendre comment déboguer les compilations afin de déterminer la cause première de et comment résoudre les échecs.

[Débogage des AEM en tant que génération et déploiement Cloud Service](./build-and-deployment.md)

## Developer Console

La console de développement fournit une variété d&#39;informations et d&#39;introspections dans AEM en tant qu&#39;environnements Cloud Service utiles pour comprendre comment votre application est reconnue par AEM en tant que Cloud Service et comment elle fonctionne.

[Débogage de l’AEM en tant que Cloud Service avec la Console développeur](./developer-console.md)

## CRXDE Lite

Le CRXDE Lite est un outil classique mais puissant pour déboguer les AEM en tant qu&#39;environnements de développement Cloud Service. CRXDE Lite fournit une suite de fonctionnalités qui aide le débogage à éviter d’inspecter toutes les ressources et propriétés, de manipuler les parties mutables du JCR, d’étudier les autorisations et d’évaluer les requêtes.

[Débogage des AEM en tant que Cloud Service avec CRXDE Lite](./crxde-lite.md)
