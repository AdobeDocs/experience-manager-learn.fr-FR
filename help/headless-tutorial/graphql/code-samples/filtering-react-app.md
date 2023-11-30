---
title: Filtrer l’application React
description: L’application React filtre en toute simplicité les WKND Adventures modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 1eb9487e-a82a-4d15-a776-cf004f2e3f01
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 100%

---

# Filtrer l’application React

Découvrez les possibilités offertes par les API GraphQL d’AEM Headless pour filtrer les données à l’aide d’une application [React](https://reactjs.org/). Cette application React crée une liste des WKND Adventures filtrables par type d’Activity.

Ce code illustre l’utilisation du [client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) d’Adobe pour appeler les requêtes GraphQL persistantes à partir de React. Cette application utilise la requête persistante `wknd-shared/adventures-all` pour collecter toutes les Adventures et obtenir une liste des types d’Activity disponibles. Lorsqu’un type d’Activity est sélectionné, il est transmis à la requête persistante `wknd-shared/adventures-by-activity` et les détails de l’Adventure sont récupérés uniquement pour les Adventures du type d’Activity spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`
