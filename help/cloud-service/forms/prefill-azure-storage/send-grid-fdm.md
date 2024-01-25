---
title: Envoyer un e-mail à l’aide de SendGrid
description: Déclencher un e-mail avec un lien vers le formulaire enregistré
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
duration: 40
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 100%

---

# Intégrer à SendGrid

L’intégration de données AEM Forms permet de configurer et de connecter des sources de données disparates à AEM Forms. Elle fournit une interface utilisateur intuitive qui permet de créer un schéma de représentation de données unifiée des entités d’entreprise dans les sources de données connectées.

Nous avons utilisé l’API SendGrid pour envoyer des e-mails à l’aide d’un modèle dynamique. Nous partons du principe que vous savez utiliser l’API SendGrid pour envoyer des e-mails à l’aide de modèles dynamiques. Dans le cadre de ce tutoriel, vous avez reçu un fichier Swagger à décrire à l’API.

## Créer l’intégration

Procédez aux étapes suivantes pour réaliser l’intégration entre AEM Forms et SendGrid.

* Créez une source de données RESTful à l’aide du [fichier Swagger](./assets/SendGridWithDynamicTemplate.yaml). [Regardez cette vidéo pour obtenir des instructions détaillées](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=fr) sur la création de sources de données dans AEM Forms.
* Créez un modèle de données de formulaire sur la base de la source de données créée à l’étape précédente.[Consultez la documentation détaillée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html?lang=fr) relative à la création d’un modèle de données de formulaire.

Le modèle de données de formulaire créé pour ce tutoriel est inclus dans les ressources de l’article.

### Étapes suivantes

[Créer une intégration de stockage Azure](./create-fdm.md)
