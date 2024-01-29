---
title: Déployer les exemples de ressources sur votre serveur
description: Tester la fonctionnalité Enregistrer en tant que brouillon pour les communications interactives
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 42
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '153'
ht-degree: 100%

---

# Déployer les exemples de ressources sur votre serveur

Suivez les instructions ci-dessous pour que cette fonctionnalité fonctionne sur votre serveur AEM.

* [Créer le schéma de la base de données](assets/icdrafts.sql)
* [Importer la bibliothèque cliente](assets/icdrafts.zip)
* [Importer le formulaire adaptatif](assets/SavedDraftsAdaptiveForm.zip)
* Créer une source de données appelée _SaveAndContinue_

![Créer une source de données.](assets/data-source.png)

| Nom de la propriété | Valeur de la propriété |
|---|---|
| Nom de la source de données | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| URL de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [Déployer le lot icdrafts](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Assurez-vous d’_Activer l’enregistrement à l’aide de CCRDDocumentInstanceService_ dans la configuration OSGI, comme illustré ci-dessous.
  ![Activation des brouillons.](assets/enable-drafts.png)
* Ouvrez une communication interactive. Cliquez sur Enregistrer en tant que brouillon pour enregistrer.
* [Afficher les brouillons enregistrés](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Les fichiers XML sont stockés dans le dossier racine de l’installation du serveur AEM. Le projet eclipse vous est fourni pour personnaliser la solution en fonction de vos besoins.

Le projet eclipse avec un exemple de mise en œuvre peut être [téléchargé ici](assets/icdrafts-eclipse-project.zip).
