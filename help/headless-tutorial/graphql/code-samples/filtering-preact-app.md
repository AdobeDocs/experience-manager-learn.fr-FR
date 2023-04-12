---
title: Filtrer l’application Preact
description: Une application simple Preact qui filtre les WKND Adventures modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: a21b78456354c18ad137e69a5d18258d652169b1
workflow-type: ht
source-wordcount: '137'
ht-degree: 100%

---


# Filtrer l’application Preact

Découvrez la possibilité offerte par les API GraphQL d’AEM Headless pour filtrer les données à l’aide d’une application [Preact](https://preactjs.com/). Cette application Preact crée une liste des WKND Adventures filtrables par type d’Activity.

Ce code illustre l’utilisation du [client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) d’Adobe pour appeler les requêtes GraphQL persistantes à partir de React. Cette application utilise la requête persistante `wknd-shared/adventures-all` pour collecter toutes les Adventures et obtenir une liste des types d’Activity disponibles. Lorsqu’un type d’Activity est sélectionné, il est transmis à la requête persistante `wknd-shared/adventures-by-activity` et les détails de l’Adventure sont récupérés uniquement pour les Adventures du type d’Activity spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`
