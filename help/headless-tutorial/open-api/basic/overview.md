---
title: Tutoriel sur AEM Headless OpenAPI | Diffusion de fragments de contenu
description: Tutoriel complet illustrant comment développer et exposer du contenu à l’aide des API de diffusion de fragments de contenu basées sur OpenAPI d’AEM.
short-description: Tutoriel illustrant comment créer et exposer du contenu AEM à l’aide de la diffusion de fragments de contenu avec des API OpenAPI et l’utiliser dans une application externe pour des scénarios CMS découplés.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
exl-id: 1bb7c415-58f8-4f6c-a0bc-38bdbdb521cf
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 92%

---

# Commencer la diffusion de fragments de contenu AEM avec les API OpenAPI

Explorez ce tutoriel complet illustrant comment développer et exposer du contenu AEM à l’aide de la diffusion de fragments de contenu d’AEM avec les API OpenAPI et consommé par une application externe, dans un scénario CMS découplé. Il explore ces concepts en passant par la création d’une application React qui affiche les détails des équipes WKND et des membres associés. Les équipes et les membres sont modélisés à l’aide de modèles de fragments de contenu AEM et consommés par l’application React à l’aide de la diffusion de fragments de contenu AEM avec les API OpenAPI.

![Application WKND Teams](./assets/overview/main.png)

Ce tutoriel couvre les sujets suivants :

* Créer une configuration de projet
* Créer des modèles de fragment de contenu pour modéliser les données
* Créer des fragments de contenu basés sur les modèles précédemment créés
* Découvrir comment interroger les fragments de contenu dans AEM à l’aide de la diffusion de fragments de contenu AEM avec la fonctionnalité « Essayer » de la documentation des API OpenAPI
* Consommer des données de fragment de contenu via la diffusion de fragments de contenu AEM avec les appels API OpenAPI à partir d’un exemple d’application React
* Améliorer l’application React pour qu’elle soit modifiable dans l’éditeur universel

## Conditions préalables {#prerequisites}

Pour suivre ce tutoriel, vous devez suivre les étapes ci-après :

* AEM Sites as a Cloud Service
* Compétences de base en HTML et JavaScript
* Les outils suivants doivent être installés localement :
   * [Node.js v22 et ultérieure](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Un IDE (par exemple, [Microsoft® Visual Studio Code](https://code.visualstudio.com/)).

### Environnement AEM as a Cloud Service

Pour suivre ce tutoriel, il est recommandé de disposer d’un accès **d’administrateur ou d’administratrice AEM** à un environnement AEM as a Cloud Service. Vous pouvez également utiliser un environnement de **Développement**, un **Environnement de développement rapide** ou un environnement dans un **Programme de sandbox**.

## Commençons !

Commencez le tutoriel par la section [Définition de modèles de fragment de contenu](1-content-fragment-models.md).

## Projet GitHub

Le code source et les modules de contenu sont disponibles dans le référentiel GitHub [Tutoriels sur AEM Headless](https://github.com/adobe/aem-tutorials).

La branche [`main` contient le code source final ](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) de ce tutoriel.
Les instantanés du code à la fin de chaque étape sont disponibles sous forme de balises Git.

* Début du chapitre 4 - Application React : [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* Fin du chapitre 4 - Application React : [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* Fin du chapitre 5 - Éditeur universel : [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez consigner un [Problème GitHub](https://github.com/adobe/aem-tutorials/issues).
