---
title: Application React de base
description: Application React de base qui affiche la liste des aventures WKND et leurs détails
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 2%

---


# Application React de base

Ceci [React](https://reactjs.org/) L’application explique comment interroger du contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes. Cette application rend un fichier de WKND Adventures, et lorsque vous sélectionnez une aventure, affiche les détails complets de l’aventure.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-slug`

Pour une analyse plus approfondie de la création de cette application Next.js, reportez-vous à la section [Exemple de documentation de l’application React](../example-apps/react-app.md).
