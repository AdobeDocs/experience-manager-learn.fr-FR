---
title: Déployer sur l’environnement de développement
description: Déployez le code de votre branche de référentiel Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Code Deployment
jira: KT-8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
duration: 31
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '118'
ht-degree: 100%

---

# Déployer sur l’environnement de développement

Au cours de l’étape précédente, nous avons transféré notre branche principale de notre référentiel git local vers la branche MyFirstAF du référentiel Cloud Manager.

L’étape suivante consiste à déployer le code dans l’environnement de développement.
Connectez-vous à Cloud Manager et sélectionnez votre programme.

Sélectionnez Déployer pour le développement comme illustré ci-dessous.


![first-step](assets/deploy-first-step1.png)


Sélectionnez Pipeline de déploiement comme indiqué
![first-step](assets/deploy1.png)

Sélectionnez le code source et la branche Git appropriée.
![first-step](assets/deploy2.png)
Veillez à mettre à jour vos modifications.

Exécutez le pipeline
![run-pipeline](assets/run-pipeline.png)

Une fois le code déployé, les modifications doivent s’afficher dans votre instance de service cloud d’AEM Forms.

## Étapes suivantes

[Mettre à jour le projet d’archétype Maven](./updating-project-archetype.md)
