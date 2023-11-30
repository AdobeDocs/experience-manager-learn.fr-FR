---
title: Application Next.js de base
description: Application Next.js de base qui affiche la liste des WKND Adventures et leurs détails.
version: Cloud Service
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
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 100%

---

# Application Next.js de base

Cette application [Next.js](https://nextjs.org/) explique comment interroger du contenu à l’aide d’API GraphQL d’AEM et en utilisant des requêtes persistantes. Cette application rend une liste filtrable des WKND Adventures, et lorsque vous sélectionnez une Adventure, elle affiche les détails complets de celle-ci.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-slug`

Pour une analyse plus approfondie de la conception de cette application Next.js, reportez-vous à la [documentation d’exemple d’application Next.js](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io ne prend pas en charge la modification de l’application Next.js dans l’IDE intégré. Pour modifier cet exemple de code, [ouvrez l’application Next.js directement sur codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
