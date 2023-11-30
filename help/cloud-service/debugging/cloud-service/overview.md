---
title: Débogage d’AEM as a Cloud Service
description: sur une infrastructure cloud en libre-service et évolutive, ce qui nécessite que les développeurs et développeuses AEM puissent comprendre et déboguer divers aspects d’AEM as a Cloud Service, depuis la version jusqu’au déploiement, en passant par l’obtention de détails sur l’exécution des applications AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 100%

---

# Déboguer AEM as a Cloud Service

AEM as a Cloud Service est le moyen natif de tirer parti des applications AEM dans le cloud. AEM as a Cloud Service fonctionne sur une infrastructure cloud en libre-service et évolutive, ce qui nécessite que les développeurs et développeuses AEM puissent comprendre et déboguer divers aspects d’AEM as a Cloud Service, depuis la version jusqu’au déploiement, en passant par l’obtention de détails sur l’exécution des applications AEM.

## Journaux

Les journaux fournissent des détails sur le fonctionnement de votre application dans AEM as a Cloud Service, ainsi que des informations sur les problèmes liés aux déploiements.

[Déboguer AEM as a Cloud Service à l’aide de journaux](./logs.md)

## Version et déploiement

Les pipelines d’Adobe Cloud Manager déploient l’application AEM par le biais d’une série d’étapes permettant de déterminer la qualité et la viabilité du code lors de son déploiement sur AEM as a Cloud Service. Chacune de ces étapes peut entraîner un échec. Il est donc important de comprendre comment déboguer les versions afin de déterminer la cause première de ces échecs et de pouvoir les résoudre.

[Déboguer la version et le déploiement d’AEM as a Cloud Service](./build-and-deployment.md)

## Developer Console

Developer Console fournit tout un éventail d’informations et d’introspections dans les environnements d’AEM as a Cloud Service, qui permettent de comprendre comment votre application est reconnue par AEM as a Cloud Service et comment elle fonctionne en son sein.

[Déboguer AEM as a Cloud Service avec Developer Console](./developer-console.md)

## Explorateur de référentiels

L’explorateur de référentiels est un outil puissant qui offre une visibilité sur l’entrepôt de données AEM sous-jacent, ce qui permet de déboguer facilement l’environnement d’AEM as a Cloud Service. L’explorateur de référentiels prend en charge une vue en lecture seule des ressources et des propriétés d’AEM pour les services de production, d’évaluation et de développement, ainsi que pour les services de création, de publication et de prévisualisation.

[Déboguer AEM as a Cloud Service avec l’explorateur de référentiels](./repository-browser.md)
