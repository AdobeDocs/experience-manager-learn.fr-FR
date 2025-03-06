---
title: Exemple de projet
description: Exemple de projet pouvant être importé et exécuté dans votre environnement
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 8%

---


# Testez-le dans votre environnement local.

* Importer le projet

   * Téléchargez et extrayez le [exemple de projet](./assets/formsdocumentservices.zip)
   * Ouvrez votre **environnement de développement Java** préféré (IntelliJ IDEA, Eclipse ou VS Code) et importez le projet en tant que projet Maven
* Configuration des informations d’identification

   * Localisez le fichier `resources/credentials/server_credentials.json`
   * Ouvrez-le et **mettez à jour les informations d’identification** spécifiques à votre environnement.
   * Assurez-vous qu’il comprend des valeurs valides pour :
     `clientId`, `clientSecret`, `adobeIMSV3TokenEndpointURL` et
     `scopes`

* Exécuter la classe principale

   * Accédez à `src/main/java/Main.java` et exécutez la méthode principale .

* Vérifier l’exécution
   * Vérifiez la sortie dans la fenêtre du terminal

