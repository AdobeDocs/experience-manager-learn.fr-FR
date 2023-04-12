---
title: Application React de base
description: Application React de base qui affiche la liste des WKND Adventures et leurs détails
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
workflow-type: ht
source-wordcount: '92'
ht-degree: 100%

---


# Application React de base

Cette application [React](https://reactjs.org/) montre comment interroger du contenu à l’aide d’API GraphQL d’AEM et en utilisant des requêtes persistantes. Cette application rend une liste filtrable des WKND Adventures, et lorsque vous sélectionnez une Adventure, elle affiche les détails complets de celle-ci.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-slug`

Pour une analyse plus approfondie de la conception de cette application Next.js, reportez-vous à la [documentation d’exemple d’application React](../example-apps/react-app.md).
