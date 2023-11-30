---
title: Filtrer l’application Angular
description: Une application Angular simple permettant de filtrer les WKND Adventures modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 100%

---

# Filtrer l’application Angular

Explorez la capacité des API GraphQL AEM Headless à filtrer les données à l’aide d’une application [Angular](https://angular.io/). Cette application Angular crée une liste de WKND Adventures filtrables par type d’Activity.

Ce code illustre l’utilisation du [client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) d’Adobe pour invoquer des requêtes GraphQL persistantes depuis Angular. Cette application utilise la requête persistante `wknd-shared/adventures-all` pour collecter toutes les Adventures et obtenir une liste des types d’Activity disponibles. Lorsqu’un type d’Activity est sélectionné, il est transmis à la requête persistante `wknd-shared/adventures-by-activity` et les détails de l’Adventure sont récupérés uniquement pour les Adventures du type d’Activity spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`
