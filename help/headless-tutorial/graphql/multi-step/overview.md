---
title: Prise en main à l’aide du tutoriel pratique d’AEM Headless - GraphQL
description: Tutoriel complet illustrant comment développer et exposer du contenu à l’aide des API AEM GraphQ
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 94%

---

# Prise en main d’AEM Headless - GraphQL

Tutoriel complet illustrant comment développer et exposer du contenu à l’aide des API AEM GraphQ et utilisé par une application externe, dans un scénario CMS découplé.

Ce tutoriel explique comment les API AEM GraphQL et les fonctionnalités découplées peuvent être utilisées pour alimenter les expériences affichées dans une application externe.

Ce tutoriel couvre les sujets suivants :

* Créer une configuration de projet
* Créer des modèles de fragment de contenu pour modéliser les données
* Créer des fragments de contenu en fonction des modèles précédemment créés
* Découvrir comment interroger les fragments de contenu dans AEM à l&#39;aide de l’outil de développement GraphiQL intégré
* Stocker ou conserver les requêtes GraphQL dans AEM
* Utiliser des requêtes GraphQL persistantes à partir d’un exemple d’application React

## Prérequis {#prerequisites}

Pour suivre ce tutoriel, vous devez suivre les étapes ci-après :

* Compétences de base en HTML et JavaScript
* Les outils suivants doivent être installés localement :
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Un IDE (par exemple, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### Environnement AEM

Pour suivre ce tutoriel, il est recommandé de disposer d’un accès administrateur AEM à un environnement AEM as a Cloud Service. Si vous n’avez pas accès à l’environnement AEM as a Cloud Service, vous pouvez utiliser le [SDK de démarrage rapide local d’AEM as a Cloud Service](/help/cloud-service/local-development-environment/aem-runtime.md). Toutefois, il est important de noter que certains écrans de l’interface utilisateur du produit, tels que la navigation dans les fragments de contenu, sont différents.

## Commençons !

Commencez le tutoriel par la section [Définition de modèles de fragment de contenu](content-fragment-models.md).

## Projet GitHub

Le code source et les packages de contenu sont disponibles sur la page [AEM Guides - Projet GitHub WKND GraphQL](https://github.com/adobe/aem-guides-wknd-graphql).

Si vous rencontrez un problème avec le tutoriel ou le code, laissez une [Problème GitHub](https://github.com/adobe/aem-guides-wknd-graphql/issues).
