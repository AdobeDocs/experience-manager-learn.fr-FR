---
title: Déployer sur l’environnement de développement
description: Déployez le code de votre branche de référentiel de cloud manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Code Deployment
kt: 8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 8%

---

# Déployer sur l’environnement de développement

Au cours de l’étape précédente, nous avons transféré notre branche principale de notre référentiel git local vers la branche MyFirstAF du référentiel cloud manager.

L’étape suivante consiste à déployer le code dans l’environnement de développement.
Connectez-vous à Cloud Manager et sélectionnez votre programme.

Sélectionnez Déployer pour le développement comme illustré ci-dessous.


![première étape](assets/deploy-first-step1.png)


Sélectionnez Pipeline de déploiement comme indiqué
![première étape](assets/deploy1.png)

Sélectionnez le code source et la branche Git appropriée.
![première étape](assets/deploy2.png)
Veillez à mettre à jour vos modifications

Exécution du pipeline
![run-pipeline](assets/run-pipeline.png)

Une fois le code déployé, les modifications doivent s’afficher dans votre instance de service cloud d’AEM Forms.

## Étapes suivantes

[Mise à jour du projet d’archétype Maven](./updating-project-archetype.md)
