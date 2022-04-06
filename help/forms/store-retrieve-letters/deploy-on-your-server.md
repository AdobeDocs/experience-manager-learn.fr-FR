---
title: Déployer les exemples de ressources sur votre serveur
description: Tester la fonctionnalité Enregistrer en tant que brouillon pour les communications interactives
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Déployer les exemples de ressources sur votre serveur

Suivez les instructions ci-dessous pour que cette fonctionnalité fonctionne sur votre serveur AEM

* Créez un dossier appelé icbrouillons dans votre lecteur c.
* [Création du schéma de la base de données](assets/icdrafts.sql)
* [Importation de la bibliothèque cliente](assets/icdrafts.zip)
* [Importation du formulaire adaptatif](assets/SavedDraftsAdaptiveForm.zip)
* Création d’une source de données appelée _SaveAndContinue_

![Création d’une source de données](assets/data-source.png)

* [Déploiement du lot icbrouts](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Assurez-vous que vous _Activation de l’enregistrement à l’aide de CCRDDocumentInstanceService_ dans la configuration OSGI, comme illustré ci-dessous
   ![Activation des versions préliminaires](assets/enable-drafts.png)
* Ouvrez une communication interactive. Cliquez sur Enregistrer en tant que brouillon pour enregistrer.
* [Afficher les brouillons enregistrés](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Les fichiers XML sont stockés dans le dossier racine de votre installation AEM serveur. Le projet eclipse > vous est fourni pour personnaliser la solution en fonction de vos besoins.

Le projet eclipse avec exemple de mise en oeuvre peut être [téléchargé ici](assets/icdrafts-eclipse-project.zip)
