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
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 5%

---

# Déployer les exemples de ressources sur votre serveur

Suivez les instructions ci-dessous pour que cette fonctionnalité fonctionne sur votre serveur AEM

* [Création du schéma de la base de données](assets/icdrafts.sql)
* [Importation de la bibliothèque cliente](assets/icdrafts.zip)
* [Importation du formulaire adaptatif](assets/SavedDraftsAdaptiveForm.zip)
* Création d’une source de données appelée _SaveAndContinue_

![Création d’une source de données](assets/data-source.png)

| Nom de la propriété | Valeur de la propriété |
|---|---|
| Datasource Name | SaveAndContinue |
| Classe de pilote JDBC  | com.mysql.cj.jdbc.Driver |
| URL de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [Déploiement du lot icbrouts](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Assurez-vous que vous _Activation de l’enregistrement à l’aide de CCRDDocumentInstanceService_ dans la configuration OSGI, comme illustré ci-dessous
   ![Activation des versions préliminaires](assets/enable-drafts.png)
* Ouvrez une communication interactive. Cliquez sur Enregistrer en tant que brouillon pour enregistrer.
* [Afficher les brouillons enregistrés](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Les fichiers XML sont stockés dans le dossier racine de votre installation AEM serveur. Le projet eclipse > vous est fourni pour personnaliser la solution en fonction de vos besoins.

Le projet eclipse avec exemple de mise en oeuvre peut être [téléchargé ici](assets/icdrafts-eclipse-project.zip)
