---
title: Tester la solution
description: Exécutez ExecuteAssemblerService.java pour tester la solution.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# Importer un projet Eclipse

* Téléchargez et décompressez le fichier [fichier zip](./assets/pdf-manipulation.zip)
* Lancez Eclipse et importez le projet dans Eclipse.
* Le projet comprend les dossiers suivants dans le dossier des ressources :
   * ddxFiles : ce dossier contient le fichier ddx pour décrire la sortie que vous souhaitez générer.
   * pdffiles - Ce dossier contient les fichiers pdf que vous souhaitez assembler.

![resources-file](./assets/resources.png)

## Tester la solution

* Copiez et collez vos informations d’identification du service dans le fichier de ressources service_token.json du projet.
* Ouvrez le fichier AssemblePDFFiles.java et spécifiez le dossier dans lequel vous souhaitez enregistrer les fichiers de PDF générés.
* Ouvrez ExecuteAssemblerService.java. Définissez la valeur de la variable assemblyURL pour qu’elle pointe vers votre instance.
* Exécutez ExecuteAssemblerService.java en tant qu’application java

>[!NOTE]
> La toute première fois que vous exécutez le programme java, vous obtenez une erreur HTTP 403. Pour passer à l’étape suivante, assurez-vous que vous avez envoyé la variable [autorisations appropriées pour l’utilisateur du compte technique dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Utilisateurs d’AEM Forms** est le rôle que j&#39;ai utilisé pour ce cours.
