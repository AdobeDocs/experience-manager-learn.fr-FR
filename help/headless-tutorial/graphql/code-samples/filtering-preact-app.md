---
title: Filtrage de l’application Preact
description: Application simple Preact qui filtre les aventures WKND modélisées à l’aide de fragments de contenu.
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
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Filtrage de l’application Preact

Découvrez AEM la capacité des API GraphQL sans affichage à filtrer les données à l’aide d’une [Preact](https://preactjs.com/) application. Cette application Preact crée une liste des aventures WKND filtrables par type d’activité.

Ce code illustre l’utilisation d’Adobe. [AEM client sans affichage pour JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) pour appeler les requêtes GraphQL persistantes de React. Cette application utilise la variable `wknd-shared/adventures-all` requête persistante pour collecter toutes les aventures et obtenir une liste des types d’activité disponibles. Lorsqu’un utilisateur sélectionne un type d’activité, le type sélectionné est transmis à la variable `wknd-shared/adventures-by-activity` requête persistante et récupère les détails de l’aventure uniquement pour les aventures du type d’activité spécifié.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-activity`
