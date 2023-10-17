---
title: Filtrer l’application Vue
description: L’application Vue filtre en toute simplicité les WKND Adventures modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 8f96093a-4449-4249-9257-028e2ffd979b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '137'
ht-degree: 100%

---

# Filtrer l’application Vue

Découvrez les possibilités offertes par les API GraphQL d’AEM Headless pour filtrer les données à l’aide d’une application [Vue](https://vuejs.org/). Cette application React crée une liste des WKND Adventures filtrables par type d’Activity.

Ce code illustre l’utilisation du [client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) d’Adobe pour appeler les requêtes GraphQL persistantes à partir de Vue. Cette application utilise la requête persistante `wknd-shared/adventures-all` pour collecter toutes les Adventures et obtenir une liste des types d’Activity disponibles. Lorsqu’un type d’Activity est sélectionné, il est transmis à la requête persistante `wknd-shared/adventures-by-activity` et les détails de l’Adventure sont récupérés uniquement pour les Adventures du type d’Activity spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`
