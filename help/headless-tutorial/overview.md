---
title: Tutoriels AEM sans affichage
description: Ensemble de tutoriels sur l’utilisation d’Adobe Experience Manager as a Headless CMS.
feature: Fragments de contenu, API
topic: Sans affichage, gestion de contenu
role: Developer
level: Beginner
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 4%

---


# Tutoriels AEM sans affichage

Adobe Experience Manager dispose de plusieurs options pour définir des points de terminaison sans interface et diffuser son contenu au format JSON. Utilisez des tutoriels pratiques pour découvrir comment utiliser les différentes options et choisir ce qui vous convient.

## Tutoriel sur les API AEM GraphQL

AEM des API GraphQL pour les fragments de contenu
prend en charge les scénarios CMS sans interface dans lesquels les applications clientes externes effectuent le rendu d’expériences à l’aide de contenu géré dans AEM.

Une API de diffusion de contenu moderne est essentielle à l’efficacité et aux performances des applications frontales JavaScript. L’utilisation d’une API REST présente des défis :

* Un grand nombre de requêtes pour récupérer un objet à la fois
* Souvent, le contenu est &quot;sur-diffusé&quot;, ce qui signifie que l’application reçoit plus que nécessaire

Pour surmonter ces défis, GraphQL fournit une API basée sur les requêtes qui permet aux clients de demander AEM uniquement pour le contenu dont ils ont besoin et de recevoir à l’aide d’un seul appel API.

* Découvrez comment utiliser AEM API GraphQL dans le [didacticiel Prise en main des API GraphQL AEM](./graphql/overview.md)

## Tutoriel sur l’authentification basée sur les jetons

AEM expose divers points de terminaison HTTP qui peuvent être interactifs sans interface, depuis GraphQL, AEM Content Services jusqu’à l’API HTTP Assets. Souvent, ces clients sans interface peuvent avoir besoin de s’authentifier auprès d’AEM pour accéder à un contenu ou à des actions protégés. Pour faciliter cette opération, AEM prend en charge l’authentification par jeton des requêtes HTTP provenant d’applications, de services ou de systèmes externes.

* Découvrez comment vous authentifier sur AEM via HTTP à l’aide de jetons d’accès dans le didacticiel [Authentification en AEM en tant que Cloud Service à partir d’une application externe](./authentication/overview.md)

## Tutoriel sur AEM Content Services

AEM Content Services utilise les pages AEM traditionnelles pour composer des points de terminaison d’API REST sans interface utilisateur, et les composants d’AEM définissent, ou référencent, le contenu à exposer sur ces points de terminaison.

AEM Content Services permet les mêmes abstractions de contenu que celles utilisées pour la création de pages web dans AEM Sites pour définir le contenu et les schémas de ces API HTTP. L’utilisation des composants Pages d’AEM et AEM permet aux marketeurs de composer et de mettre à jour rapidement des API JSON flexibles pouvant alimenter n’importe quelle application.

* Découvrez comment utiliser AEM Content Services dans le [didacticiel Prise en main d’AEM Content Services](./content-services/overview.md)

## AEM GraphQL par rapport à AEM Content Services

|  | AEM API GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Définition d’un schéma | Modèles de fragment de contenu structuré | Composants AEM |
| Contenu | Fragments de contenu | Composants AEM |
| Détection de contenu | Par requête GraphQL | Par AEM page |
| Format de diffusion | JSON GraphQL | AEM ComponentExporter JSON |

## Autres tutoriels utiles

Voici d’autres tutoriels AEM sur les concepts sans interface utilisateur graphique :

* [Prise en main de l’Éditeur AEM SPA et d’Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Prise en main de l’Éditeur AEM SPA et de React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)