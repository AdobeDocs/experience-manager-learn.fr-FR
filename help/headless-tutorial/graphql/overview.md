---
title: Prise en main d’AEM sans affichage - GraphQL
description: Présentation des API et fonctionnalités GraphQL d’AEM.
feature: Fragments de contenu, API
topic: Sans affichage, gestion de contenu
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---


# Prise en main d’AEM sans affichage - GraphQL

AEM des API GraphQL pour les fragments de contenu
prend en charge les scénarios CMS sans interface dans lesquels les applications clientes externes effectuent le rendu d’expériences à l’aide de contenu géré dans AEM.

Une API de diffusion de contenu moderne est essentielle à l’efficacité et aux performances des applications frontales JavaScript. L’utilisation d’une API REST présente des défis :

* Un grand nombre de requêtes pour récupérer un objet à la fois
* Souvent, le contenu est &quot;sur-diffusé&quot;, ce qui signifie que l’application reçoit plus que nécessaire

Pour surmonter ces défis, GraphQL fournit une API basée sur les requêtes qui permet aux clients de demander AEM uniquement pour le contenu dont ils ont besoin et de recevoir à l’aide d’un seul appel API.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

Cette vidéo présente un aperçu de l’API GraphQL implémentée dans AEM. L’API GraphQL d’AEM est principalement conçue pour fournir AEM de fragments de contenu aux applications en aval dans le cadre d’un déploiement sans interface utilisateur.

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