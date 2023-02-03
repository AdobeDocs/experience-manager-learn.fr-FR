---
title: Application SvelteKit simple
description: Application SvelteKit simple qui affiche des aventures WKND modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# Filtrage de l’application SvelteKit

Découvrez AEM les API GraphQL sans affichage pour répertorier les données à l’aide d’une [SvelteKit](https://kit.svelte.dev/) application. Cette application SvelteKit crée une liste des aventures WKND, qui peut être sélectionnée pour afficher les détails de l’aventure.

Ce code illustre l’utilisation d’Adobe. [AEM client sans affichage pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) pour appeler les requêtes GraphQL persistantes à partir de SvelteKit. Cette application utilise la variable `wknd-shared/adventures-all` requête persistante pour collecter toutes les aventures et obtenir une liste des types d’activité disponibles. Les détails de l’aventure sont demandés par le biais du `wknd-shared/adventures-by-slug` requête persistante.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-slug`
