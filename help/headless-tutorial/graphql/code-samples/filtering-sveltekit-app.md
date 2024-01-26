---
title: Application SvelteKit simple
description: Application SvelteKit simple qui affiche des WKND Adventures modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 27
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 100%

---

# Filtrer l’application SvelteKit

Découvrez les possibilités offertes par les API GraphQL d’AEM Headless pour répertorier les données à l’aide d’une application [SvelteKit](https://kit.svelte.dev/). Cette application SvelteKit crée une liste des WKND Adventures, qui peuvent être sélectionnées pour en afficher les détails.

Ce code illustre l’utilisation du [client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) d’Adobe pour appeler les requêtes GraphQL persistantes à partir de SvelteKit. Cette application utilise la requête persistante `wknd-shared/adventures-all` pour collecter toutes les Adventures et obtenir une liste des types d’Activity disponibles. Les détails de l’Adventure sont demandés par le biais de la requête persistante `wknd-shared/adventures-by-slug`.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-slug`
