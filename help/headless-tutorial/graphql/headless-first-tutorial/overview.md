---
title: Premier tutoriel AEM sans affichage
description: Découvrez comment être une première application AEM sans affichage.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---


# Premier tutoriel AEM sans affichage

{{aem-headless-trials-promo}}

Bienvenue dans le tutoriel sur la création d’une expérience web à l’aide de React, entièrement piloté par AEM API et GraphQL sans affichage. Dans ce tutoriel, nous vous guiderons tout au long du processus de création d’une application web dynamique et interactive en combinant la puissance de React, des API Adobe Experience Manager (AEM) sans affichage et GraphQL.

React est une bibliothèque JavaScript populaire pour la création d’interfaces utilisateur, connue pour sa simplicité, sa convivialité et son architecture basée sur des composants. AEM fournit de puissantes fonctionnalités de gestion de contenu et expose des API sans affichage qui permettent aux développeurs d’accéder au contenu et aux données stockés dans AEM par le biais de divers canaux et applications.

En exploitant AEM API sans affichage, vous pouvez récupérer du contenu, des ressources et des données de votre instance AEM et les utiliser pour alimenter votre application React. GraphQL, langage de requête flexible pour les API, offre un moyen efficace et précis de demander des données spécifiques à votre instance AEM, ce qui permet une intégration transparente entre React et AEM.

![Premier tutoriel AEM sans tête](./assets/overview/overview.png)

Dans ce tutoriel, nous vous guidons tout au long du processus étape par étape de création d’une expérience web à l’aide des API React et AEM sans affichage avec GraphQL. Vous apprendrez à configurer votre environnement de développement, à établir une connexion entre React et AEM, à récupérer du contenu à l’aide des requêtes GraphQL et à le rendre dynamiquement dans votre application web.

Nous aborderons des sujets tels que la configuration de votre projet React, la création d’une authentification avec AEM, l’interrogation de contenu d’AEM à l’aide de GraphQL, la gestion des données dans vos composants React et l’optimisation des performances en utilisant la mise en cache et la pagination.

D’ici la fin de ce tutoriel, vous aurez une bonne compréhension de la manière d’exploiter React, les API AEM sans affichage et GraphQL pour créer une expérience web puissante et attrayante. Donc, plongeons et commençons à construire votre prochaine application web !

## Prérequis

### Compétences

+ Compétence dans React
+ Compétence dans GraphQL
+ Connaissances de base de AEM as a Cloud Service

### AEM as a Cloud Service

Ce tutoriel nécessite un accès administrateur à un environnement as a Cloud Service AEM.

### Logiciels

+ [Node.js v16 et ultérieure.](https://nodejs.org/en/)
   + Vérifiez votre version de noeud en exécutant `node -v` de la ligne de commande
+ [npm 6+](https://www.npmjs.com/)
   + Vérifiez votre version npm en exécutant `npm -v` de la ligne de commande
+ [Git](https://git-scm.com/)
   + Vérifiez votre version Git en exécutant `git -v` de la ligne de commande

Utilisation [Node version manager (nvm)](https://github.com/nvm-sh/nvm) pour résoudre le problème lié à plusieurs versions de node.js sur le même ordinateur.

Assurez-vous que vous disposez des privilèges nécessaires pour installer les logiciels globalement sur votre ordinateur.

## Étape suivante

Maintenant que votre environnement est configuré, passons à l’étape suivante : [Configuration et création de contenu dans AEM as a Cloud Service](./1-content-modeling.md)
