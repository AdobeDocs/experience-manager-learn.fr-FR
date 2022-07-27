---
title: AEM les déploiements mobiles sans affichage
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# AEM les déploiements mobiles sans affichage

AEM les déploiements mobiles sans affichage sont des applications mobiles natives pour iOS, Android, etc. qui consomment et interagissent avec le contenu d’AEM sans interface utilisateur graphique.

Les déploiements mobiles nécessitent une configuration minimale, car les connexions HTTP à AEM API sans affichage ne sont pas initiées dans le contexte d’un navigateur.

## Configurations de déploiement

| L’application mobile se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher](./dispatcher-fitlers.md) | ✘ | ✔ | ✔ |
| [Configuration CORS](./cors.md) | ✘ | ✘ | ✘ |
| Nom d’hôte de l’URL d’image | ✔ | ✔ | ✔ |

## Exemples d’applications mobiles

Adobe fournit des exemples d’applications mobiles iOS et Android.


