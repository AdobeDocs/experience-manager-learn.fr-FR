---
title: Mettre en œuvre la fonctionnalité d’enregistrement et de reprise pour un formulaire adaptatif
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
ht-degree: 100%

---

# Présentation

Dans ce tutoriel, nous allons mettre en œuvre un cas d’utilisation simple qui permet à l’utilisateur ou à l’utilisatrice d’enregistrer et de reprendre le processus de remplissage du formulaire. Le flux du cas d’utilisation est le suivant :

* l’utilisateur ou l’utilisatrice commence à remplir un formulaire adaptatif ;
* l’utilisateur ou l’utilisatrice enregistre le formulaire et souhaite continuer à le compléter ultérieurement ;
* l’utilisateur ou l’utilisatrice reçoit une notification par e-mail contenant un lien vers le formulaire enregistré.

## Prérequis

* Expérience avec AEM Forms CS, notamment la création de modèles de données de formulaire.
* Expérience du déploiement du code à l’aide de Cloud Manager.
* Accès à l’instance compatible avec le cloud d’AEM Forms CS.

Pour mettre en œuvre le cas d’utilisation ci-dessus dans AEM Forms CS, vous aurez besoin des éléments suivants :

* [Instance compatible avec le cloud AEM Forms CS](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=fr#set-up-aem-author-instance)
* [Compte Azure Portal](https://portal.azure.com/)
* [Compte SendGrid](https://sendgrid.com/)

### Étapes suivantes

[Créer un composant de page](./page-component.md)
