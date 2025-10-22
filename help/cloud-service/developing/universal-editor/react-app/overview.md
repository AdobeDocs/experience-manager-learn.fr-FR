---
title: Modification de l’application React à l’aide de l’éditeur universel
description: Découvrez comment modifier le contenu d’un exemple d’application React à l’aide de l’éditeur universel.
short-description: Découvrez comment modifier le contenu d’un exemple d’application React à l’aide de l’éditeur universel. Les contenus sont stockés dans les fragments de contenu d’AEM et sont récupérés à l’aide des API GraphQL.
version: Experience Manager as a Cloud Service
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
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 100%

---

# Modification de l’application React à l’aide de l’éditeur universel

Découvrez comment modifier le contenu d’un exemple d’application React à l’aide de l’éditeur universel. Les contenus sont stockés dans les fragments de contenu d’AEM et sont récupérés à l’aide des API GraphQL.

Ce tutoriel vous guide tout au long du processus de configuration de l’environnement de développement local, d’instrumentalisation de l’application React pour modifier son contenu et de modification du contenu à l’aide de l’éditeur universel.

## Ce que vous apprenez.

Ce tutoriel couvre les sujets suivants :

- Vue d’ensemble rapide de l’éditeur universel
- Configuration de l’environnement de développement local
   - **SDK AEM** : il fournit les contenus stockés dans les fragments de contenu pour l’application React à l’aide des API GraphQL.
   - **Application React** : interface d’utilisation simple qui affiche les contenus d’AEM.
   - **Service Éditeur universel** : _copie locale du service Éditeur universel_ qui lie l’éditeur universel et le SDK AEM.
- Instrumentalisation de l’application React pour modifier les contenus à l’aide de l’éditeur universel
- Modification des contenus de l’application React à l’aide de l’éditeur universel


## Vue d’ensemble de l’éditeur universel

L’éditeur universel permet aux personnes chargées de la création de contenu et du développement (front-end et back-end) de passer en revue certains des principaux avantages de l’éditeur universel :

- Conçu pour éditer du contenu découplé en mode WYSIWYG.
- Offre une expérience d’édition de contenu cohérente à l’échelle de différentes technologies front-end, telles que React, Angular, Vue, etc. Ainsi, les personnes chargées de la création de contenu peuvent modifier le contenu sans se soucier de la technologie front-end sous-jacente.
- Une instrumentalisation très minimale est nécessaire pour activer l’éditeur universel dans l’application front-end. Cela permet donc de maximiser la productivité des personnes chargées du développement et de les aider à se concentrer sur la création de l’expérience.
- Séparation des préoccupations entre trois rôles : les personnes chargées de la création de contenu, les personnes chargées du développement front-end et celles chargées du développement back-end. Cela permet à chaque rôle de se concentrer sur ses responsabilités principales.


## Exemple d’application React

Ce tutoriel utilise [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) comme exemple d’application React. L’application React **WKND Teams** affiche une liste des personnes membres de l’équipe et leurs détails.

Les détails de l’équipe, tels que le titre, la description et les personnes membres de l’équipe, sont stockés en tant que fragments de contenu _Équipe_ dans AEM. De même, les détails de la personne tels que le nom, la biographie et l’image de profil sont stockés en tant que fragments de contenu _Personne_ dans AEM.

Le contenu de l’application React est fourni par AEM à l’aide des API GraphQL et l’interface d’utilisation est créée à l’aide de deux composants React, `Teams` et `Person`.

Un tutoriel correspondant est disponible pour apprendre à créer l’application React **WKND Teams**. Vous trouverez le tutoriel [ici](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Étape suivante

Découvrez comment [configurer l’environnement de développement local](./local-development-setup.md).
