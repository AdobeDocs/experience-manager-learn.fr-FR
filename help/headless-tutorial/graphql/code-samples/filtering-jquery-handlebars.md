---
title: Filtrer avec jQuery et Handlebars
description: Implémentation JavaScript utilisant jQuery et Handlebars pour filtrer les WKND Adventures à afficher.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
duration: 33
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 100%

---

# Filtrer avec jQuery et Handlebars

Découvrez comment les API GraphQL d’AEM Headless peuvent filtrer les données à l’aide d’une application JavaScript qui utilise [jQuery](https://jquery.com/) et [Handlebars](https://handlebarsjs.com/). Cette application crée une liste des WKND Adventures filtrables par type d’Activity.

Ce code permet d’utiliser le [Client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) d’Adobe pour appeler des requêtes GraphQL persistantes. Cette application utilise la requête persistante `wknd-shared/adventures-all` pour collecter toutes les Adventures et obtenir une liste des types d’Activity disponibles. Lorsqu’un type d’Activity est sélectionné, il est transmis à la requête persistante `wknd-shared/adventures-by-activity` et les détails de l’Adventure sont récupérés uniquement pour les Adventures du type d’Activity spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`
