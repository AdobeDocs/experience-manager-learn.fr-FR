---
title: Prise en main d’AEM sans affichage - GraphQL
description: Découvrez les API GraphQL de Experience Manager et leurs fonctionnalités.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 6%

---

# Prise en main d’AEM Headless - GraphQL  {#getting-started-with-aem-headless}

Les API GraphQL d’AEM pour les fragments de contenu prennent en charge les scénarios CMS sans interface lorsque des applications clientes externes rendent des expériences à l’aide de contenu géré dans AEM.

Une API de diffusion de contenu moderne est essentielle à l’efficacité et aux performances des applications frontales JavaScript. L’utilisation d’une API REST présente des défis :

* Un grand nombre de requêtes pour récupérer un objet à la fois
* Souvent, le contenu est &quot;sur-diffusé&quot;, ce qui signifie que l’application reçoit plus que nécessaire

Pour surmonter ces défis, GraphQL fournit une API basée sur les requêtes qui permet aux clients de demander AEM uniquement pour le contenu dont ils ont besoin et de recevoir à l’aide d’un seul appel API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Cette vidéo présente un aperçu de l’API GraphQL implémentée dans AEM. L’API GraphQL d’AEM est principalement conçue pour fournir AEM de fragments de contenu aux applications en aval dans le cadre d’un déploiement sans interface utilisateur.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Prise en main d’AEM Headless - GraphQL "
>abstract="Découvrez comment diffuser des fragments de contenu à l’aide de GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618/?captions=fre_fr" text="Présentation de GraphQL dans AEM"

## AEM Série de vidéos GraphQL sans affichage

Découvrez les fonctionnalités d’AEM GraphQL grâce à la présentation détaillée des fragments de contenu et des API et outils de développement d’AEM GraphQL.

* [AEM Série de vidéos GraphQL sans affichage](./video-series/modeling-basics.md)

## Tutoriel pratique GraphQL sans affichage

Explorez AEM fonctionnalités GraphQL en créant une application React qui utilise des fragments de contenu via les API GraphQL.

* [Tutoriel pratique GraphQL sans affichage](./multi-step/overview.md)

## AEM GraphQL et AEM Content Services

|  | API GraphQL AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Définition d’un schéma | Modèles de fragment de contenu structuré | Composants AEM |
| Contenu | Fragments de contenu | Composants AEM |
| Détection de contenu | Par requête GraphQL | Par AEM page |
| Format de diffusion | GraphQL JSON | AEM ComponentExporter JSON |
