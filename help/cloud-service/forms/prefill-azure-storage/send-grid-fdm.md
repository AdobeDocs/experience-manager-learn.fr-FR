---
title: Envoyer un e-mail à l’aide de SendGrid
description: Déclencher un email avec un lien vers le formulaire enregistré
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 12%

---

# Intégration à SendGrid

L’intégration de données AEM Forms permet de configurer et de connecter des sources de données disparates à AEM Forms. Elle fournit une interface utilisateur intuitive qui permet de créer un schéma de représentation de données unifiée des entités d’entreprise dans les sources de données connectées.

Nous avons utilisé l’API SendGrid pour envoyer des emails à l’aide d’un modèle dynamique. Nous partons du principe que vous connaissez l’API SendGrid pour envoyer des emails à l’aide de modèles dynamiques. Dans le cadre de ce tutoriel, vous avez reçu un fichier swagger à décrire à l’API.

## Création de l’intégration

Suivez les étapes suivantes pour créer l’intégration entre AEM Forms et SendGrid.

* Créez une source de données RESTful à l’aide du [fichier swagger](./assets/SendGridWithDynamicTemplate.yaml). [Suivez cette vidéo pour obtenir des instructions détaillées](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) sur la création de sources de données dans AEM Forms
* Créez un modèle de données de formulaire basé sur la source de données créée à l’étape précédente.[Consultez la documentation détaillée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) lors de la création d’un modèle de données de formulaire.

Le modèle de données de formulaire créé pour ce tutoriel est inclus dans les ressources de l’article.

### Étapes suivantes

[Création d’une intégration de stockage Azure](./create-fdm.md)


