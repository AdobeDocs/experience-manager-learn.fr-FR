---
title: Prise en main d’AEM Headless - GraphQL
description: Découvrez les API GraphQL d’Experience Manager et leurs fonctionnalités.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 100%

---

# Prise en main d’AEM Headless - GraphQL {#getting-started-with-aem-headless}

{{aem-headless-trials-promo}}

Les API GraphQL d’AEM pour les fragments de contenu
prennent en charge les scénarios CMS découplés lorsque des applications clientes externes rendent des expériences à l’aide de contenu géré dans AEM.

Une API de diffusion de contenu moderne est essentielle à l’efficacité et aux performances des applications front-end JavaScript. L’utilisation d’une API REST présente plusieurs difficultés :

* De nombreuses requêtes sont nécessaires pour récupérer un objet à la fois.
* Le contenu est souvent « surdiffusé », ce qui signifie que l’application reçoit plus d’informations que nécessaire.

Pour surmonter ces difficultés, GraphQL fournit une API basée sur les requêtes qui permet aux applications clientes externes d’interroger AEM uniquement pour le contenu dont ils ont besoin, et de le recevoir en utilisant un seul appel d’API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Cette vidéo présente un aperçu de l’API GraphQL implémentée dans AEM. L’API GraphQL d’AEM est principalement conçue pour fournir des fragments de contenu d’AEM aux applications en aval dans le cadre d’un déploiement découplé.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Prise en main d’AEM Headless - GraphQL"
>abstract="Découvrez comment diffuser des fragments de contenu à l’aide de GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618/?captions=fre_fr" text="Présentation de GraphQL dans AEM"

## Série de vidéos GraphQL AEM Headless

Découvrez les fonctionnalités GraphQL d’AEM grâce à la présentation détaillée des fragments de contenu et des API et outils de développement GraphQL d’AEM.

* [Série de vidéos GraphQL AEM Headless](./video-series/modeling-basics.md)

## Tutoriel pratique sur GraphQL AEM Headless

Explorez les fonctionnalités GraphQL d’AEM en créant une application React qui utilise des fragments de contenu via les API GraphQL d’AEM.

* [Tutoriel pratique sur GraphQL AEM Headless](./multi-step/overview.md)

## AEM GraphQL et AEM Content Services

|  | API AEM GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Définition d’un schéma | Modèles structurés de fragment de contenu | Composants AEM |
| Contenu | Fragments de contenu | Composants AEM |
| Détection de contenu | Par requête GraphQL | Par Page AEM |
| Format de diffusion | JSON GraphQL | Exporteur JSON de composant AEM |
