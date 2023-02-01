---
title: Filtrage des applications ExpressJS et Pug
description: Une application ExpressJS/Pug simple qui filtre les aventures WKND modélisées à l’aide de fragments de contenu.
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
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---


# Filtrage des applications ExpressJS et Pug

Découvrez AEM la possibilité de filtrer les données à l’aide d’une API GraphQL sans affichage [ExpressJS](https://expressjs.com/)/[Pug](https://pugjs.org/) application. Cette application ExpressJS/Pug crée une liste des aventures WKND filtrables par type d’activité.

Ce code illustre l’utilisation d’Adobe. [AEM client headless pour NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) pour appeler des requêtes GraphQL persistantes à l’aide d’un script JavaScript basé sur Node.js. Cette application utilise la variable `wknd-shared/adventures-all` requête persistante pour collecter toutes les aventures et obtenir une liste des types d’activité disponibles. Lorsqu’un utilisateur sélectionne un type d’activité, le type sélectionné est transmis à la variable `wknd-shared/adventures-by-activity` requête persistante et récupère les détails de l’aventure uniquement pour les aventures du type d’activité spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`
