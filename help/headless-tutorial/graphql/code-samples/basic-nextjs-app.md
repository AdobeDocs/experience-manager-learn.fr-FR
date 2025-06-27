---
title: Application Next.js de base
description: Application Next.js de base qui affiche la liste des WKND Adventures et leurs détails.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 22
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: ht
source-wordcount: '98'
ht-degree: 100%

---

# Application Next.js de base

Cette application [Next.js](https://nextjs.org/) explique comment interroger du contenu à l’aide d’API GraphQL d’AEM et en utilisant des requêtes persistantes. Cette application rend une liste filtrable des WKND Adventures, et lorsque vous sélectionnez une Adventure, elle affiche les détails complets de celle-ci.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-slug`

>[!IMPORTANT]
>
> Codesandbox.io ne prend pas en charge la modification de l’application Next.js dans l’IDE intégré. Pour modifier cet exemple de code, [ouvrez l’application Next.js directement sur codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
