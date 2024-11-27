---
title: Composant Liste déroulante Créer un pays
description: Créez un composant de liste déroulante de pays basé sur un composant de liste déroulante de base d’aem forms.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 3%

---

# Création d’un composant déroulant de pays à partir d’un composant déroulant

La création d’un composant principal dans Adobe Experience Manager (AEM) est un processus passionnant qui implique plusieurs étapes, notamment la définition de la structure du composant, la personnalisation de la boîte de dialogue et la mise en oeuvre d’un modèle Sling pour les fonctionnalités dynamiques.

À la fin de ce tutoriel, vous aurez appris à :

* Créez et utilisez un modèle Sling pour récupérer dynamiquement les données.
* Personnalisez la boîte de dialogue cq en ajoutant de nouveaux champs et en en masquant d’autres.
* Définissez une structure de composants robuste adaptée à la réutilisation.

Le composant, nommé &quot;Pays&quot;, permet aux utilisateurs de sélectionner un continent et de remplir de manière dynamique une liste déroulante avec les pays correspondant au continent choisi. Il sera créé sur la base du composant de liste déroulante d’usine, amélioré pour ce cas d’utilisation spécifique.

Explorons et créons ce composant dynamique et puissant !

## Conditions préalables

La création d’un nouveau composant principal dans Adobe Experience Manager (AEM) requiert plusieurs conditions préalables pour garantir un processus de développement fluide. Voici ce dont vous aurez besoin avant de commencer :

* Environnement de développement AEM : installation fonctionnelle prête pour le cloud s’exécutant localement
* Accès aux outils de développement AEM tels que Visual Studio Code ou IntelliJ
* Configuration et AEM d’un projet MAven avec le dernier archétype
* Connaissance de base des concepts AEM
* Outils de base et configuration tels que le référentiel Git, la version appropriée du JDK


## Étapes suivantes

[Création d’une structure de composant](./component.md)
