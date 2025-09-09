---
title: Tutoriel sur l’OpenAPI d’AEM Headless | Diffusion de fragment de contenu
description: Tutoriel complet illustrant comment créer et exposer du contenu à l’aide des API de diffusion de fragments de contenu basées sur OpenAPI d’AEM.
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
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 22%

---

# Prise en main de la diffusion de fragments de contenu AEM avec les API OpenAPI

Explorez ce tutoriel illustrant comment créer et exposer du contenu AEM à l’aide de la diffusion de fragments de contenu AEM avec des API OpenAPI et utilisé par une application externe, dans un scénario CMS découplé. Elle explore ces concepts en pensant à la création d’une application React qui affiche les équipes WKND et les détails des membres associés. Les équipes et les membres sont modélisés à l’aide des modèles de fragment de contenu AEM et consommés par l’application React à l’aide de la diffusion de fragments de contenu AEM avec les API OpenAPI.

![Application WKND Teams](./assets/overview/main.png)

Ce tutoriel couvre les sujets suivants :

* Créer une configuration de projet
* Créer des modèles de fragment de contenu pour modéliser les données
* Créer des fragments de contenu en fonction des modèles précédemment créés
* Découvrez comment interroger les fragments de contenu dans AEM à l’aide de la diffusion de fragments de contenu AEM avec la fonctionnalité « Essayer » de la documentation des API OpenAPI
* Utiliser des données de fragment de contenu via la diffusion de fragments de contenu AEM avec des appels API OpenAPI à partir d’un exemple d’application React
* Améliorez l’application React pour qu’elle soit modifiable dans l’éditeur universel

## Prérequis {#prerequisites}

Pour suivre ce tutoriel, vous devez suivre les étapes ci-après :

* AEM Sites as a Cloud Service
* Compétences de base en HTML et JavaScript
* Les outils suivants doivent être installés localement :
   * [Node.js v22 et ultérieure](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Un IDE (par exemple, [Microsoft® Visual Studio Code](https://code.visualstudio.com/)).

### Environnement AEM as a Cloud Service

Pour suivre ce tutoriel, il est recommandé de disposer d’un accès de **administrateur AEM** à un environnement AEM as a Cloud Service. Vous pouvez également utiliser un environnement **Développement**, **Environnement de développement rapide** ou un environnement dans un **Programme Sandbox**.

## Commençons !

Commencez le tutoriel par la section [Définition de modèles de fragment de contenu](1-content-fragment-models.md).

## Projet GitHub

Le code source et les packages de contenu sont disponibles dans la section [Tutoriels AEM Headless](https://github.com/adobe/aem-tutorials) Référentiel GitHub.

La branche [`main` contient le code source final](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) pour ce tutoriel.
Les instantanés du code à la fin de chaque étape sont disponibles sous forme de balises Git.

* Début du chapitre 4 - Application React : [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* Fin du chapitre 4 - Application React : [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* Fin du chapitre 5 - Éditeur universel : [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez consigner un [Problème GitHub](https://github.com/adobe/aem-tutorials/issues).
