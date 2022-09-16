---
title: Filtrage des requêtes
description: Une implémentation JavaScript qui permet de sélectionner des fragments de contenu spécifiques, puis d’afficher leurs détails .
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
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Filtrage des requêtes

Cet exemple JavaScript et Handlebars illustre comment filtrer les résultats GraphQL et afficher les résultats sélectionnés.

Ce code :

+ Se connecte à [wknd.site](https://wknd.site)Service de publication AEM de et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-slug`
