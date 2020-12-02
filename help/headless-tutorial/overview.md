---
title: aem didacticiels sans en-tête
description: Ensemble de didacticiels sur l’utilisation de Adobe Experience Manager en tant que CMS sans en-tête.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 5%

---


# aem didacticiels sans en-tête

Adobe Experience Manager propose plusieurs options pour définir des points de terminaison sans en-tête et diffuser son contenu au format JSON. Utilisez des didacticiels pratiques pour découvrir comment utiliser les différentes options et choisir ce qui vous convient.

## Didacticiel sur les API AEM GraphQL

>[!CAUTION]
>
> L’AEM API GraphQL pour la Diffusion de fragments de contenu sera publiée au début de 2021.
> La documentation correspondante est disponible à des fins de prévisualisation.

aem API GraphQL pour les fragments de contenu
prend en charge les scénarios CMS sans tête dans lesquels les applications clientes externes effectuent le rendu d’expériences à l’aide de contenu géré dans AEM.

Une API de diffusion de contenu moderne est la clé de l&#39;efficacité et des performances des applications frontales basées sur JavaScript. L’utilisation d’une API REST présente des défis :

* Nombre important de requêtes pour récupérer un objet à la fois
* Souvent, le contenu est &quot;trop diffusé&quot;, ce qui signifie que l’application reçoit plus que ce dont elle a besoin.

Pour surmonter ces défis, GraphQL fournit une API basée sur les requêtes qui permet aux clients de requête uniquement le contenu dont ils ont besoin et de recevoir un appel d&#39;API unique.

* Découvrez comment utiliser AEM API GraphQL en suivant le didacticiel [Prise en main des API GraphQL AEM](./graphql/overview.md)

## Didacticiel sur AEM Content Services

aem Content Services utilise les pages AEM traditionnelles pour composer des points de terminaison d’API REST sans en-tête et les composants AEM définissent, ou référencent, le contenu à exposer sur ces points de terminaison.

aem Content Services permet les mêmes abstractions de contenu utilisées pour la création de pages Web dans AEM Sites, pour définir le contenu et les schémas de ces API HTTP. L’utilisation de Pages AEM et de Composants AEM permet aux spécialistes du marketing de composer et de mettre à jour rapidement des API JSON flexibles capables d’alimenter n’importe quelle application.

* Découvrez comment utiliser AEM Content Services en suivant le didacticiel [Prise en main d’AEM Content Services](./content-services/overview.md)

## aem GraphQL et AEM Content Services

|  | aem API GraphQL | aem Content Services |
|--------------------------------|:-----------------|:---------------------|
| Définition du schéma | Modèles de fragments de contenu structuré | Composants AEM |
| Contenu | Fragments de contenu | Composants AEM |
| Détection de contenu | Par requête GraphQL | Par page AEM |
| Format de diffusion | JSON GraphQL | aem ComponentExporter JSON |

## Autres didacticiels utiles

Voici d&#39;autres didacticiels AEM relatifs aux concepts sans tête :

* [Prise en main de l’Éditeur AEM SPA et d’Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Prise en main de l’Éditeur AEM SPA et de React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)