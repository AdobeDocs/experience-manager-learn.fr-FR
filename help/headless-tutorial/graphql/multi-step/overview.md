---
title: Prise en main d’AEM tutoriel main sans tête - GraphQL
description: Tutoriel complet illustrant comment créer et exposer du contenu à l’aide des API GraphQL AEM.
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: 328618.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 8%

---

# Prise en main d’AEM Headless - GraphQL 

Tutoriel complet illustrant comment créer et exposer du contenu à l’aide des API GraphQL AEM et utilisé par une application externe, dans un scénario CMS sans interface.

Ce tutoriel explique comment AEM API GraphQL et les fonctionnalités sans tête peuvent être utilisées pour alimenter les expériences affichées dans une application externe.

Ce tutoriel couvre les rubriques suivantes :

* Création d’une configuration de projet
* Création de modèles de fragment de contenu pour modéliser les données
* Créez des fragments de contenu en fonction des modèles précédemment créés.
* Découvrez comment interroger les fragments de contenu dans AEM à l’aide de l’outil de développement GraphiQL intégré.
* Pour stocker ou conserver les requêtes GraphQL à AEM
* Utilisation de requêtes GraphQL persistantes à partir d’un exemple d’application React


## Prérequis {#prerequisites}

Pour suivre ce tutoriel, vous devez suivre les étapes suivantes :

* Compétences de base en HTML et JavaScript
* Les outils suivants doivent être installés localement :
   * [Node.js v14+](https://nodejs.org/en/)
   * [npm 6+](https://www.npmjs.com/)
   * [Git](https://git-scm.com/)
   * Un IDE (par exemple, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### Environnement AEM

Pour suivre ce tutoriel, AEM’accès administrateur à un environnement AEM as a Cloud Service est recommandé. Si vous n’avez pas accès à AEM environnement as a Cloud Service, vous pouvez utiliser la variable [AEM local as a Cloud Service SDK de démarrage rapide](/help/cloud-service/local-development-environment/aem-runtime.md). Toutefois, il est important de noter que certains écrans de l’interface utilisateur du produit, tels que la navigation dans les fragments de contenu, sont différents.

## Commençons !

Commencez le tutoriel par [Définition de modèles de fragment de contenu](content-fragment-models.md).

## Projet GitHub

Le code source et les packages de contenu sont disponibles sur la page [AEM Guides - Projet GitHub WKND GraphQL](https://github.com/adobe/aem-guides-wknd-graphql).

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez laisser une [Problème GitHub](https://github.com/adobe/aem-guides-wknd-graphql/issues).
