---
title: Filtrer l’application Express
description: Une simple application Express qui filtre les WKND Adventures modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 32
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 100%

---

# Filtrer l’application Express

Explorez la capacité des API GraphQL d’AEM Headless à filtrer des données à l’aide d’une application [Express](https://expressjs.com/) et [Pug](https://pugjs.org/). Cette application Express crée une liste des WKND Adventures filtrables par type d’Activity.

Ce code illustre l’utilisation d’un [client AEM Headless d’Adobe pour NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) pour appeler des requêtes GraphQL persistantes à l’aide d’un script JavaScript basé sur Node.js. Cette application utilise la requête persistante `wknd-shared/adventures-all` pour collecter toutes les Adventures et obtenir une liste des types d’Activity disponibles. Lorsqu’un type d’Activity est sélectionné, il est transmis à la requête persistante `wknd-shared/adventures-by-activity` et les détails de l’Adventure sont récupérés uniquement pour les Adventures du type d’Activity spécifié. Les détails de l’Adventure sont récupérés à partir d’AEM via la requête persistante `wknd-shared/adventures-by-slug`.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, et `wknd-shared/adventures-by-slug`
