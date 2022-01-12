---
title: Tester la solution
description: Exécutez le fichier Main.java pour tester la solution.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: f712e86600ed18aee43187a5fb105324b14b7b89
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---


# Importer un projet Eclipse

Téléchargez et décompressez le fichier [fichier zip](./assets/aem-forms-cs-doc-gen.zip)

Lancez Eclipse et importez le projet dans Eclipse Le projet comprend les fichiers suivants dans le dossier des ressources :

* DataFile1, DataFile2 et DataFile3 - Exemples de fichiers de données XML à fusionner avec le modèle pour générer le fichier de PDF final.
* custom_fonts.xdp - modèle XDP.
* service_token.json - Vous devrez remplacer le contenu de ce fichier par les informations d’identification spécifiques à votre compte.
* options.json - Les options spécifiées dans ce fichier sont utilisées pour définir les propriétés du fichier de PDF généré par l’API.

![resources-file](./assets/resource-files.png)

## Tester la solution

* Copiez et collez vos informations d’identification du service dans le fichier de ressources service_token.json du projet.
* Ouvrez le fichier DocumentGeneration.java et spécifiez le dossier dans lequel vous souhaitez enregistrer les fichiers de PDF générés.
* Ouvrez Main.java. Définissez la valeur de la variable postURL pour qu’elle pointe vers votre instance.
* Exécutez le fichier Main.java en tant qu’application Java.

>[!NOTE]
> La toute première fois que vous exécutez le programme java, vous obtenez une erreur HTTP 403. Pour passer à l’étape suivante, assurez-vous que vous avez envoyé la variable [autorisations appropriées pour l’utilisateur du compte technique dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Utilisateurs d’AEM Forms** est le rôle que j&#39;ai utilisé pour ce cours.

