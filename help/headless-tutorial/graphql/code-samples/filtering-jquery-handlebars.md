---
title: Filtrer avec jQuery et Handlebars
description: Implémentation JavaScript utilisant jQuery et Handlebars pour filtrer les WKND Adventures à afficher.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: ht
source-wordcount: '146'
ht-degree: 100%

---


# Filtrer avec jQuery et Handlebars

Découvrez comment les API GraphQL d’AEM Headless peuvent filtrer les données à l’aide d’une application JavaScript qui utilise [jQuery](https://jquery.com/) et [Handlebars](https://handlebarsjs.com/). Cette application crée une liste des WKND Adventures filtrables par type d’Activity.

Ce code permet d’utiliser le [Client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) d’Adobe pour appeler des requêtes GraphQL persistantes. Cette application utilise la requête persistante `wknd-shared/adventures-all` pour collecter toutes les Adventures et obtenir une liste des types d’Activity disponibles. Lorsqu’un type d’Activity est sélectionné, il est transmis à la requête persistante `wknd-shared/adventures-by-activity` et les détails de l’Adventure sont récupérés uniquement pour les Adventures du type d’Activity spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`.
