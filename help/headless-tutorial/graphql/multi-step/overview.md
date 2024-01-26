---
title: Prise en main à l’aide du tutoriel pratique d’AEM Headless - GraphQL
description: Tutoriel complet illustrant comment développer et exposer du contenu à l’aide des API AEM GraphQ
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
duration: 67
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 81%

---

# Prise en main d’AEM Headless - GraphQL

{{aem-headless-trials-promo}}

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
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Un IDE (par exemple, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### Environnement AEM

Pour suivre ce tutoriel, il est recommandé de disposer AEM accès administrateur à un environnement AEM as a Cloud Service. Si vous n’avez pas accès à un environnement as a Cloud Service AEM, veuillez [Inscrivez-vous à AEM essai sans affichage](https://commerce.adobe.com/business-trial/sign-up?items%5B0%5D%5Bid%5D=649A1AF5CBC5467A25E84F2561274821&amp;cli=headless_exl_banner_campaign&amp;co=US&amp;lang=fr) pour explorer AEM fonctionnalités sans interface.

## Commençons !

Commencez le tutoriel par la section [Définition de modèles de fragment de contenu](content-fragment-models.md).

## Projet GitHub

Le code source et les packages de contenu sont disponibles sur la page [AEM Guides - Projet GitHub WKND GraphQL](https://github.com/adobe/aem-guides-wknd-graphql).

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez consigner un [Problème GitHub](https://github.com/adobe/aem-guides-wknd-graphql/issues).
