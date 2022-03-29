---
title: Débogage AEM as a Cloud Service
description: sur l’infrastructure cloud en libre-service, évolutive, ce qui nécessite que les développeurs AEM comprennent comment comprendre et déboguer diverses facettes d’AEM as a Cloud Service, de la création et du déploiement à l’obtention des détails sur l’exécution des applications d’AEM.
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
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Débogage AEM as a Cloud Service

AEM as a Cloud Service est le moyen natif de tirer parti des applications AEM. AEM as a Cloud Service s’exécute sur une infrastructure cloud en libre-service, évolutive, ce qui nécessite AEM développeurs de comprendre comment comprendre et déboguer diverses facettes d’as a Cloud Service, de la création et du déploiement à l’obtention des détails sur l’exécution des applications.

## Journaux

Les journaux fournissent des détails sur le fonctionnement de votre application dans AEM as a Cloud Service, ainsi que des informations sur les problèmes liés aux déploiements.

[Débogage AEM as a Cloud Service à l’aide des journaux](./logs.md)

## Création et déploiement

Les pipelines d’Adobe Cloud Manager déploient AEM application par le biais d’une série d’étapes afin de déterminer la qualité et la viabilité du code lors de leur déploiement sur AEM as a Cloud Service. Chacune de ces étapes peut entraîner un échec. Il est donc important de comprendre comment déboguer les versions afin de déterminer la cause première de et comment résoudre les échecs.

[Débogage AEM version et déploiement as a Cloud Service](./build-and-deployment.md)

## Developer Console

Developer Console fournit une variété d’informations et d’introspections dans AEM environnements as a Cloud Service utiles pour comprendre comment votre application est reconnue par et fonctionne dans AEM as a Cloud Service.

[Débogage AEM as a Cloud Service avec Developer Console](./developer-console.md)

## Explorateur de référentiels

Repository Browser est un outil puissant qui offre une visibilité sur AEM entrepôt de données sous-jacent, ce qui permet de déboguer facilement AEM environnement as a Cloud Service. Repository Browser prend en charge une vue en lecture seule des ressources et des propriétés d’AEM dans les services de production, d’évaluation et de développement, ainsi que dans les services de création, de publication et d’aperçu.

[Débogage AEM as a Cloud Service avec l’explorateur de référentiel](./repository-browser.md)
