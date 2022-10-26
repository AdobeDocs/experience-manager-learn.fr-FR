---
title: Prise en main d’AEM sans affichage - GraphQL
description: Découvrez les API GraphQL du Experience Manager et leurs fonctionnalités.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
source-git-commit: 129dedd4cd6973d5d576bed5f714ce62152923de
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 6%

---

# Prise en main d’AEM Headless - GraphQL  {#getting-started-with-aem-headless}

Les API GraphQL AEM pour les fragments de contenu prennent en charge les scénarios CMS sans interface lorsque des applications clientes externes rendent des expériences à l’aide de contenu géré dans AEM.

Une API de diffusion de contenu moderne est essentielle à l’efficacité et aux performances des applications frontales JavaScript. L’utilisation d’une API REST présente des défis :

* Un grand nombre de requêtes pour récupérer un objet à la fois
* Souvent, le contenu est &quot;sur-diffusé&quot;, ce qui signifie que l’application reçoit plus que nécessaire

Pour surmonter ces défis, GraphQL fournit une API basée sur les requêtes qui permet aux clients de demander AEM uniquement pour le contenu dont ils ont besoin et de recevoir à l’aide d’un seul appel API.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

Cette vidéo présente un aperçu de l’API GraphQL implémentée dans AEM. L’API GraphQL d’AEM est principalement conçue pour fournir AEM de fragments de contenu aux applications en aval dans le cadre d’un déploiement sans interface utilisateur.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Prise en main d’AEM Headless - GraphQL "
>abstract="Découvrez comment diffuser des fragments de contenu à l’aide de GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618" text="Présentation de GraphQL dans AEM"

## AEM série vidéo GraphQL sans affichage

Découvrez les fonctionnalités d’AEM GraphQL grâce à la présentation détaillée des fragments de contenu et AEM les API GraphQL et les outils de développement.

* [AEM série vidéo GraphQL sans affichage](./video-series/modeling-basics.md)

## Tutoriel AEM sans interface graphique

Explorez AEM fonctionnalités GraphQL en créant une application React qui utilise des fragments de contenu via AEM API GraphQL.

* [Tutoriel AEM sans interface graphique](./multi-step/overview.md)

## AEM GraphQL par rapport à AEM Content Services

|  | AEM API GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Définition d’un schéma | Modèles de fragment de contenu structuré | Composants AEM |
| Contenu | Fragments de contenu | Composants AEM |
| Détection de contenu | Par requête GraphQL | Par AEM page |
| Format de diffusion | JSON GraphQL | AEM ComponentExporter JSON |
