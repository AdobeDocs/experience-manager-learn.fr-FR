---
title: Application Next.js de base
description: Application Next.js de base qui affiche la liste des aventures WKND et leurs détails
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: e3fb145e7a9f33dd010f6c40e42573d41e54b302
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---


# Application Next.js de base

Ceci [Next.js](https://nextjs.org/) L’application explique comment interroger du contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes. Cette application rend un fichier de WKND Adventures, et lorsque vous sélectionnez une aventure, affiche les détails complets de l’aventure.

Ce code :

+ Se connecte à un service de publication AEM et ne nécessite pas d’authentification
+ Utilise les requêtes persistantes de WKND : `wknd-shared/adventures-all` et `wknd-shared/adventures-by-slug`

Pour une analyse plus approfondie de la création de cette application Next.js, reportez-vous à la section [Exemple de documentation de l’application Next.js](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io ne prend pas en charge la modification de l’application Next.js dans l’IDE intégré. Pour modifier cet exemple de code, [ouvrez l’application Next.js directement sur codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-3n6zdv).
