---
title: Filtrage de l’application React
description: Application React simple qui filtre les aventures WKND modélisées à l’aide de fragments de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 1%

---


# Filtrage de l’application React

Découvrez AEM la capacité des API GraphQL sans affichage à filtrer les données à l’aide d’une [React](https://reactjs.org/) application. Cette application React crée une liste des aventures WKND filtrables par type d’activité.

Ce code illustre l’utilisation d’Adobe. [AEM client sans affichage pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) pour appeler les requêtes GraphQL persistantes de React. Cette application utilise la variable `wknd-shared/adventures-all` requête persistante pour collecter toutes les aventures et obtenir une liste des types d’activité disponibles. Lorsqu’un utilisateur sélectionne un type d’activité, le type sélectionné est transmis à la variable `wknd-shared/adventures-by-activity` requête persistante et récupère les détails de l’aventure uniquement pour les aventures du type d’activité spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`
