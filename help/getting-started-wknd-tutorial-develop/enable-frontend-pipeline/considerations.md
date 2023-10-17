---
title: Considérations relatives au développement
description: Tenez compte de l’impact sur le processus de développement front-end et back-end une fois que vous activez le pipeline front-end.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '201'
ht-degree: 100%

---

# Considérations relatives au développement

Après avoir activé le pipeline front-end pour déployer uniquement les ressources front-end dans l’environnement AEM as a Cloud Service, il y a un impact sur le développement AEM local et vous devez ajuster le modèle d’embranchement Git.

## Objectif

* Comment disposer d’un flux de développement front-end et back-end optimal
* Examinez les dépendances entre le pipeline full-stack et front-end.


## Considérations relatives au développement local

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Approche ajustée relative au développement

* Pour le développement local à l’aide du SDK d’AEM, l’équipe de développement back-end a toujours besoin d’une génération de bibliothèque cliente via le module `ui.frontend`, mais vous devez l’ignorer pendant le déploiement de Cloud Manager dans l’environnement AEM as a Cloud Service. Cela pose une difficulté sur la manière d’isoler les modifications de configuration du projet décrite dans le chapitre [Mettre à jour le projet](update-project.md).

Une __solution__ peut être d’ajuster votre modèle d’embranchement Git et de vous assurer que les modifications de configuration du projet AEM ne reviennent jamais à la branche de __développement local__ que les développeurs et développeuses back-end AEM utilisent.


* Dans le cadre d’une amélioration continue de votre projet AEM, si vous introduisez de nouveaux composants ou mettez à jour un composant existant qui comporte des modifications dans les deux modules `ui.app` et `ui.frontend`, vous devez exécuter les pipelines full-stack et front-end.
