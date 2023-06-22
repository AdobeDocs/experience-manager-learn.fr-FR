---
title: Envoyer un formulaire sans en-tête à un service d’envoi personnalisé
description: Personnalisation de votre réponse en fonction des données envoyées
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 2%

---


# Personnalisation de la réponse en fonction des données envoyées

Une fois le formulaire envoyé, il est important de faire part à l’utilisateur de ses commentaires sur le résultat de l’envoi. La réponse d’envoi peut inclure un identifiant de transaction ou simplement une réponse personnalisée. Pour répondre à ce cas d’utilisation, un service d’envoi personnalisé est écrit dans AEM Forms et le formulaire sans en-tête est envoyé à ce service d’envoi personnalisé.

## Prérequis

Pour mettre en oeuvre cette fonctionnalité, il est recommandé de se familiariser avec les éléments suivants :

* Expérience avec Git
* Expérience avec AEM Cloud Manager
* Maven (cet article a été testé avec la version 3.8.6)
* Instance de création locale prête pour AEM Forms Cloud
* Accès à l’environnement AEM Forms as Cloud Service
* IntelliJ ou tout autre IDE


## Étapes suivantes

[Écrire le service d’envoi personnalisé](./custom-submit-service.md)