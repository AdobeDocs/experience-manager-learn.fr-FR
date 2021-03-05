---
title: AEM didacticiels sans en-tête
description: Ensemble de didacticiels sur l’utilisation de Adobe Experience Manager en tant que CMS sans en-tête.
feature: Fragments de contenu, API
topic: Sans tête, Gestion de contenu
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 5%

---


# AEM didacticiels sans en-tête

Adobe Experience Manager propose plusieurs options pour définir des points de terminaison sans en-tête et diffuser son contenu au format JSON. Utilisez des didacticiels pratiques pour découvrir comment utiliser les différentes options et choisir ce qui vous convient.

## Didacticiel sur les API AEM GraphQL

AEM API GraphQL pour les fragments de contenu
prend en charge les scénarios CMS sans tête dans lesquels les applications clientes externes effectuent le rendu d’expériences à l’aide de contenu géré dans AEM.

Une API de diffusion de contenu moderne est la clé de l&#39;efficacité et des performances des applications frontales basées sur JavaScript. L’utilisation d’une API REST présente des défis :

* Nombre important de requêtes pour récupérer un objet à la fois
* Souvent, le contenu est &quot;trop diffusé&quot;, ce qui signifie que l’application reçoit plus que ce dont elle a besoin.

Pour surmonter ces défis, GraphQL fournit une API basée sur les requêtes qui permet aux clients de requête uniquement le contenu dont ils ont besoin et de recevoir un appel d&#39;API unique.

* Découvrez comment utiliser les API GraphQL AEM dans le didacticiel [Prise en main des API GraphQL AEM](./graphql/overview.md)

## Didacticiel sur l’authentification basée sur un jeton

AEM expose divers points de terminaison HTTP qui peuvent être interactifs sans en-tête, depuis GraphQL, AEM Content Services jusqu’à l’API HTTP Assets. Souvent, ces consommateurs sans tête peuvent avoir besoin de s&#39;authentifier auprès d&#39;AEM pour accéder à un contenu ou à des actions protégés. Pour ce faire, AEM prend en charge l’authentification par jeton des requêtes HTTP provenant d’applications, de services ou de systèmes externes.

* Découvrez comment vous authentifier sur AEM via HTTP à l’aide de jetons d&#39;accès dans le didacticiel d’application externe ](./authentication/overview.md) intitulé [Authentification en tant que Cloud Service dans le  .

## Didacticiel sur AEM Content Services

AEM Content Services utilise les pages AEM traditionnelles pour composer des points de terminaison d’API REST sans en-tête et les composants AEM définissent, ou référencent, le contenu à exposer sur ces points de terminaison.

AEM Content Services permet les mêmes abstractions de contenu utilisées pour la création de pages Web dans AEM Sites, pour définir le contenu et les schémas de ces API HTTP. L’utilisation de Pages AEM et de Composants AEM permet aux spécialistes du marketing de composer et de mettre à jour rapidement des API JSON flexibles capables d’alimenter n’importe quelle application.

* Découvrez comment utiliser AEM Content Services dans le didacticiel [Prise en main d’AEM Content Services](./content-services/overview.md)

## AEM GraphQL et AEM Content Services

|  | AEM API GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Définition du schéma | Modèles de fragments de contenu structuré | Composants AEM |
| Contenu | Fragments de contenu | Composants AEM |
| Détection de contenu | Par requête GraphQL | Par page AEM |
| Format de diffusion | JSON GraphQL | AEM ComponentExporter JSON |

## Autres didacticiels utiles

Voici d&#39;autres didacticiels AEM relatifs aux concepts sans tête :

* [Prise en main de l’Éditeur AEM SPA et d’Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Prise en main de l’Éditeur AEM SPA et de React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)