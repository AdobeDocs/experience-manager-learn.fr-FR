---
title: Modification de l’application React à l’aide d’Universal Editor
description: Découvrez comment modifier le contenu d’un exemple d’application React à l’aide d’Universal Editor.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 1%

---

# Modification de l’application React à l’aide d’Universal Editor

Découvrez comment modifier le contenu d’un exemple d’application React à l’aide d’Universal Editor. Les contenus sont stockés dans les fragments de contenu d’AEM et sont récupérés à l’aide des API GraphQL.

Ce tutoriel vous guide tout au long du processus de configuration de l’environnement de développement local, d’instrumentalisation de l’application React pour modifier son contenu et de modification du contenu à l’aide de l’éditeur universel.

## Ce que vous apprenez

Ce tutoriel couvre les sujets suivants :

- Aperçu rapide d’Universal Editor
- Configuration de l’environnement de développement local
   - **AEM SDK** : il fournit le contenu stocké dans les fragments de contenu pour l’application React à l’aide des API GraphQL.
   - **Application React** : interface utilisateur simple qui affiche le contenu d’AEM.
   - **Universal Editor Service** : _copie locale du service Universal Editor_ qui lie Universal Editor et le SDK AEM.
- Comment instrumenter l’application React pour modifier le contenu à l’aide d’Universal Editor
- Comment modifier le contenu de l’application React à l’aide d’Universal Editor


## Présentation d’Universal Editor

Universal Editor permet aux auteurs et développeurs de contenu (front-end et backend) de passer en revue certains des principaux avantages d’Universal Editor :

- Conçus pour éditer du contenu headless en mode WYSIWYG (What-you-see-is-you-get).
- Offre une expérience d’édition de contenu cohérente à l’échelle de différentes technologies frontales, telles que React, Angular, Vue, etc. Ainsi, les auteurs de contenu peuvent modifier le contenu sans se soucier de la technologie frontale sous-jacente.
- Une instrumentation très minimale est nécessaire pour activer l’éditeur universel dans l’application frontale. Donc, maximiser la productivité du développeur et leur permettre de se concentrer sur la création de l’expérience.
- Séparation des préoccupations entre trois rôles, les auteurs de contenu, les développeurs front-end et les développeurs principaux, ce qui permet à chaque rôle de se concentrer sur ses responsabilités principales.


## Exemple d’application React

Ce tutoriel utilise [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) comme exemple d’application React. L’application React **WKND Teams** affiche une liste des membres de l’équipe et leurs détails.

Les détails de l’équipe, tels que le titre, la description et les membres de l’équipe, sont stockés en tant que fragments de contenu _Team_ dans AEM. De même, les détails de la personne tels que le nom, la biographie et l’image de profil sont stockés en tant que fragments de contenu _Personne_ dans AEM.

Le contenu de l’application React est fourni par AEM à l’aide des API GraphQL et l’interface utilisateur est créée à l’aide de deux composants React, `Teams` et `Person`.

Un tutoriel correspondant est disponible pour apprendre à créer l’application React **WKND Teams**. Vous trouverez le tutoriel [ici](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Étape suivante

Découvrez comment [configurer l’environnement de développement local](./local-development-setup.md).
