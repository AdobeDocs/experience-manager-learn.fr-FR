---
title: Considérations relatives au développement
description: Tenez compte de l’impact sur le processus de développement front-end et back-end une fois que vous avez activé le pipeline front-end.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# Considérations relatives au développement

Après avoir activé le pipeline front-end pour déployer uniquement les ressources front-end dans AEM environnement as a Cloud Service, il y a un impact sur le développement AEM local et vous devez ajuster le modèle d’embranchement git.

## Objectif

* Comment disposer d’un flux de développement front-end et back-end sans friction
* Examinez les dépendances entre la pile complète et le pipeline front-end.


## Considérations sur le développement local

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## Approche ajustée du développement

* Pour le développement local à l’aide du SDK AEM, l’équipe de développement principal a toujours besoin d’une génération clientlib via `ui.frontend` mais pendant le déploiement de Cloud Manager dans AEM environnement as a Cloud Service, vous devez l’ignorer. Cela pose un problème sur la manière d’isoler les modifications de configuration du projet décrites dans la section [Mettre à jour le projet](update-project.md) chapitre.

A __solution__ peut être pour ajuster votre modèle d’embranchement git et s’assurer que les modifications de configuration du projet AEM ne reviennent jamais à __développement local__ branche que les développeurs d’AEM back-end utilisent.


* Dans le cadre d’une amélioration continue de votre projet AEM, si vous introduisez de nouveaux composants ou mettez à jour un composant existant qui comporte des modifications dans les deux `ui.app` et `ui.frontend` , vous devez exécuter les pipelines full-stack et front-end.



