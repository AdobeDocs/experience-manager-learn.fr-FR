---
title: Filtrage de l’application express
description: Une simple application Express qui filtre les aventures WKND modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
source-git-commit: ac2b3a766caea1013165aedd3478bf859212cc89
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# Filtrage de l’application express

Découvrez AEM la possibilité de filtrer les données à l’aide d’une API GraphQL sans affichage [Express](https://expressjs.com/) et [Pug](https://pugjs.org/) application. Cette application Express crée une liste des aventures WKND filtrables par type d’activité.

Ce code illustre l’utilisation d’Adobe. [AEM client headless pour NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) pour appeler des requêtes GraphQL persistantes à l’aide d’un script JavaScript basé sur Node.js. Cette application utilise la variable `wknd-shared/adventures-all` requête persistante pour collecter toutes les aventures et obtenir une liste des types d’activité disponibles. Lorsqu’un utilisateur sélectionne un type d’activité, le type sélectionné est transmis à la variable `wknd-shared/adventures-by-activity` requête persistante et récupère les détails de l’aventure uniquement pour les aventures du type d’activité spécifié. Les détails de l’aventure sont récupérés à partir d’AEM via le `wknd-shared/adventures-by-slug` requête persistante.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, et `wknd-shared/adventures-by-slug`
