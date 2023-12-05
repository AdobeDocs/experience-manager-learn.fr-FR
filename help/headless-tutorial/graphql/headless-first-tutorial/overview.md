---
title: Premier tutoriel AEM Headless
description: Découvrez comment créer une première application AEM Headless.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 122
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 100%

---

# Premier tutoriel AEM Headless

{{aem-headless-trials-promo}}

Bienvenue dans le tutoriel sur la création d’une expérience web à l’aide de React, entièrement pilotée par les API d’AEM Headless et de GraphQL. Dans ce tutoriel, nous vous guidons tout au long du processus de création d’une application web dynamique et interactive en combinant la puissance de React, des API d’Adobe Experience Manager (AEM) Headless et de GraphQL.

React est une bibliothèque JavaScript populaire pour la création d’interfaces utilisateur, connue pour sa simplicité, sa convivialité et son architecture basée sur des composants. AEM fournit de puissantes fonctionnalités de gestion de contenu et expose des API découplées qui permettent aux développeurs et aux développeuses d’accéder au contenu et aux données stockés dans AEM par le biais de divers canaux et applications.

En exploitant les API d’AEM Headless, vous pouvez récupérer du contenu, des ressources et des données de votre instance AEM et les utiliser pour alimenter votre application React. GraphQL, en tant que langage de requête flexible pour les API, offre un moyen efficace et précis de demander des données spécifiques à votre instance AEM, ce qui permet une intégration transparente entre React et AEM.

![Premier tutoriel AEM Headless.](./assets/overview/overview.png)

Dans ce tutoriel, nous vous guidons tout au long du processus étape par étape de création d’une expérience web à l’aide de React et des API d’AEM Headless avec GraphQL. Vous apprendrez à configurer votre environnement de développement, à établir une connexion entre React et AEM, à récupérer du contenu à l’aide des requêtes GraphQL et à en effectuer le rendu dynamique dans votre application web.

Nous aborderons des sujets tels que la configuration de votre projet React, la création d’une authentification avec AEM, l’interrogation de contenu d’AEM à l’aide de GraphQL, la gestion des données dans vos composants React et l’optimisation des performances en utilisant la mise en cache et la pagination.

À la fin de ce tutoriel, vous aurez une bonne compréhension de la manière d’exploiter React, les API d’AEM Headless et GraphQL pour créer une expérience web puissante et attrayante. C’est parti, commençons à construire votre prochaine application web.

## Conditions préalables

### Compétences

+ Maîtrise de React
+ Maîtrise de GraphQL
+ Connaissances fondamentales d’AEM as a Cloud Service

### AEM as a Cloud Service

Ce tutoriel nécessite un accès administratif à un environnement AEM as a Cloud Service.

### Logiciels

+ [Node.js v16 et ultérieure.](https://nodejs.org/en/)
   + Vérifiez la version de votre nœud en exécutant `node -v` à partir de la ligne de commande.
+ [npm 6+](https://www.npmjs.com/)
   + Vérifiez votre version npm en exécutant `npm -v` à partir de la ligne de commande.
+ [Git](https://git-scm.com/)
   + Vérifiez votre version Git en exécutant `git -v` à partir de la ligne de commande.

Utilisez [node version manager (nvm)](https://github.com/nvm-sh/nvm) pour résoudre le problème lié à la présence de plusieurs versions de node.js sur le même ordinateur.

Assurez-vous que vous disposez des privilèges nécessaires pour installer les logiciels globalement sur votre ordinateur.

## Étape suivante

Maintenant que votre environnement est configuré, passons à l’étape suivante : [configuration et création de contenu dans AEM as a Cloud Service](./1-content-modeling.md).
