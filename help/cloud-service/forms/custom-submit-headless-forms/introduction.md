---
title: Envoyer un formulaire découplé à un service d’envoi personnalisé
description: Personnaliser votre réponse en fonction des données envoyées
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: 78fe677c-d5ab-40f6-a381-800f24e227ae
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 100%

---

# Personnaliser une réponse en fonction des données envoyées

Une fois le formulaire envoyé, il est important de faire part à l’utilisateur ou l’utilisatrice de ses commentaires sur le résultat de l’envoi. La réponse d’envoi peut inclure un identifiant de transaction ou simplement une réponse personnalisée. Pour répondre à ce cas d’utilisation, un service d’envoi personnalisé est écrit dans AEM Forms et le formulaire découplé est envoyé à ce service d’envoi personnalisé.

## Prérequis

Pour mettre en œuvre cette fonctionnalité, une familiarité avec les éléments suivants est recommandée :

* Expérience de Git
* Experience d’AEM Cloud Manager
* Maven (cet article a été testé avec la version 3.8.6)
* Instance de création AEM Forms locale compatible avec le cloud
* Accès à l’environnement AEM Forms as Cloud Service
* IntelliJ ou tout autre IDE


## Étapes suivantes

[Écrire le service d’envoi personnalisé](./custom-submit-service.md)
