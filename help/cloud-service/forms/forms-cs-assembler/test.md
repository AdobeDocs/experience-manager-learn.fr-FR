---
title: Tester la solution Forms Assembler
description: Exécutez le fichier ExecuteAssemblerService.java pour tester la solution.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 80
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 100%

---

# Importer un projet Eclipse

* Téléchargez et décompressez le [fichier zip](./assets/pdf-manipulation.zip).
* Lancez Eclipse et importez le projet dans Eclipse.
* Le projet comprend les dossiers suivants dans le dossier des ressources :
   * ddxFiles : ce dossier contient le fichier ddx pour décrire la sortie à générer.
   * pdffiles : ce dossier contient les fichiers PDF que vous souhaitez assembler et les fichiers PDF pour tester PDFA Utilities.
   * credentials : ce dossier contient le fichier pdfa-options.json.

![resources-file](./assets/resources.png)

## Tester l’assemblage de fichiers PDF

* Copiez et collez vos informations d’identification du service dans le fichier de ressources service_token.json du projet.
* Ouvrez le fichier AssemblePDFFiles.java et spécifiez le dossier dans lequel vous souhaitez enregistrer les fichiers PDF générés.
* Ouvrez ExecuteAssemblerService.java. Définissez la valeur de la variable _AEM_FORMS_CS_ pour qu’elle pointe vers votre instance.
* Supprimez les commentaires des lignes appropriées pour tester l’assemblage de deux fichiers PDF ou plus.
* Exécutez ExecuteAssemblerService.java en tant qu’application java.

### Tester PDFA Utilities

* Copiez et collez vos informations d’identification du service dans le fichier de ressources service_token.json du projet.
* Ouvrez le fichier PDFAUtilities.java et spécifiez le dossier dans lequel vous souhaitez enregistrer les fichiers PDF générés.
* Ouvrez ExecuteAssemblerService.java. Définissez la valeur de la variable _AEM_FORMS_CS_ pour qu’elle pointe vers votre instance.
* Supprimez les commentaires des lignes appropriées pour tester les opérations PDFA.
* Exécutez ExecuteAssemblerService.java en tant qu’application Java.



>[!NOTE]
> La première fois que vous exécutez le programme Java, vous obtenez une erreur HTTP 403. Pour éviter cela, assurez-vous que vous avez accordé les [autorisations appropriées pour l’utilisateur ou l’utilisatrice du compte technique dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=fr#configurer-l’accès-dans-aem).

**Utilisateurs et utilisatrices d’AEM Forms** est le rôle que j’ai utilisé pour ce cours.
