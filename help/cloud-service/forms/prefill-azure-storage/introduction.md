---
title: Mise en oeuvre de la fonctionnalité d’enregistrement et de reprise pour un formulaire adaptatif
description: Découvrez comment stocker et récupérer les données de formulaire adaptatif du compte de stockage Azure.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 3%

---

# Présentation

Dans ce tutoriel, nous allons mettre en oeuvre un cas d’utilisation simple qui permet à l’utilisateur d’enregistrer et de reprendre le processus de remplissage du formulaire. Le flux du cas pratique est le suivant :

* L’utilisateur commence à remplir un formulaire adaptatif.
* L’utilisateur enregistre le formulaire et souhaite le compléter ultérieurement.
* L’utilisateur reçoit une notification par courrier électronique contenant un lien vers le formulaire enregistré.

## Prérequis

* Expérience avec AEM Forms CS, notamment la création de modèles de données de formulaire.
* Expérience du déploiement du code à l’aide de Cloud Manager.
* Accès à l’instance prête pour le cloud d’AEM Forms CS.

Pour mettre en oeuvre le cas d’utilisation ci-dessus dans AEM Forms CS, vous aurez besoin des éléments suivants :

* [Instance prête pour le cloud AEM Forms CS](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Compte de portail Azure](https://portal.azure.com/)
* [Compte SendGrid](https://sendgrid.com/)

### Étapes suivantes

[Créer un composant de page](./page-component.md)
