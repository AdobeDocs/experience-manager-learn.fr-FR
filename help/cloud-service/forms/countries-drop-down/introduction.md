---
title: Créer un composant de liste déroulante de pays
description: Créez un composant de liste déroulante de pays basé sur un composant déroulant de base d’AEM Forms.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: aef151bc-daf1-4abd-914a-6299f3fb58e4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 100%

---

# Créer un composant déroulant de pays à partir d’un composant déroulant

La création d’un composant principal dans Adobe Experience Manager (AEM) est un processus passionnant qui implique plusieurs étapes, notamment la définition de la structure du composant, la personnalisation de la boîte de dialogue et la mise en œuvre d’un modèle Sling pour les fonctionnalités dynamiques.

À la fin de ce tutoriel, vous aurez appris à :

* créer et utiliser un modèle Sling pour récupérer dynamiquement les données ;
* personnaliser cq-dialog en ajoutant de nouveaux champs et en en masquant d’autres ;
* définir une structure de composants robuste adaptée à la réutilisation.

Le composant, nommé « Pays », permet aux utilisateurs et utilisatrices de sélectionner un continent et de remplir de manière dynamique une liste déroulante avec les pays correspondant au continent choisi. Il sera créé sur la base du composant déroulant prêt à l’emploi, amélioré pour ce cas d’utilisation spécifique.

Explorons et créons ce composant dynamique et puissant !

## Conditions préalables

La création d’un composant principal dans Adobe Experience Manager (AEM) requiert plusieurs conditions préalables pour garantir un processus de développement fluide. Voici ce dont vous aurez besoin avant de commencer :

* Environnement de développement AEM : une installation fonctionnelle compatible avec le cloud s’exécutant localement.
* Accéder aux outils de développement AEM tels que Visual Studio Code ou IntelliJ
* Configuration MAven et projet AEM avec le dernier archétype
* Connaissances de base des concepts AEM
* Outils et configuration de base tels que le référentiel Git, version appropriée du JDK


## Étapes suivantes

[Créer une structure de composants](./component.md)
