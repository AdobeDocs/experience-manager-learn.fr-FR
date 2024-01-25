---
title: Extensibilité de l’interface utilisateur AEM
description: Découvrez l’extensibilité de l’interface utilisateur AEM à l’aide du créateur d’applications pour créer des extensions.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 58
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 100%

---

# Extensibilité de l’interface utilisateur AEM

Adobe Experience Manager (AEM) offre une interface utilisateur puissante pour créer des expériences numériques. Pour personnaliser et étendre l’interface utilisateur, Adobe a introduit le créateur d’applications. Cet outil permet aux développeurs et développeuses d’améliorer l’expérience utilisateur sans code complexe à l’aide de JavaScript et de React.

Le créateur d’applications fournit une couche d’implémentation pour la création d’extensions qui sont liées à des points d’extension bien définis dans AEM. Le créateur d’applications s’intègre de manière transparente à AEM, ce qui permet d’effectuer des prévisualisations et des tests en temps réel. Le déploiement des modifications dans AEM est rapide et simplifié. Grâce au créateur d’applications, les développeurs et développeuses économisent du temps et du travail, ce qui permet de créer rapidement des prototypes et de collaborer avec les parties prenantes.

## Développer une extension d’interface utilisateur AEM

Les différentes interfaces utilisateur AEM ont des points d’extension différents, mais les concepts de base sont les mêmes.

Les vidéos et les guides fournis ci-dessous présentent l’utilisation d’une extension de la console de fragments de contenu pour illustrer diverses activités. Toutefois, il est important de noter que les concepts abordés peuvent être appliqués à toutes les extensions de l’interface utilisateur AEM.

1. [Créer un projet Adobe Developer Console](./adobe-developer-console-project.md)
1. [Initialiser une extension](./app-initialization.md)
1. [Enregistrer une extension](./extension-registration.md)
1. Implémenter un point d’extension

   Les points d’extension et leurs implémentations varient en fonction de l’interface utilisateur étendue.

   + [Développer une extension de l’interface utilisateur de fragments de contenu](./content-fragments/overview.md)

1. [Développer un modal](./modal.md)
1. [Développer une action Adobe I/O Runtime](./runtime-action.md)
1. [Vérifier une extension](./verify.md)
1. [Déployer une extension](./deploy.md)

## Documentation Adobe Developer

Adobe Developer contient des informations détaillées pour les développeurs et développeuses concernant l’extensibilité de l’interface utilisateur AEM. Veuillez consulter la section [contenu Adobe Developer pour plus de détails techniques](https://developer.adobe.com/uix/docs/).
