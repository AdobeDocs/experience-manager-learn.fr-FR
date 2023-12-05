---
title: Tester la solution
description: Exécutez le fichier Main.java pour tester la solution
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 63
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 100%

---

# Importer un projet Eclipse

Téléchargez et décompressez le [fichier zip](./assets/aem-forms-cs-doc-gen.zip).

Lancez Eclipse et importez le projet dans Eclipse.
Le projet comprend les fichiers suivants dans le dossier des ressources :

* DataFile1, DataFile2 et DataFile3 - Exemples de fichiers de données XML à fusionner avec le modèle pour générer le fichier PDF final.
* custom_fonts.xdp - Modèle XDP.
* service_token.json - Vous devez remplacer le contenu de ce fichier par les informations d’identification spécifiques à votre compte.
* options.json - Les options spécifiées dans ce fichier sont utilisées pour définir les propriétés du fichier PDF généré par l’API.

![resources-file](./assets/resource-files.png)

## Tester la solution

* Copiez et collez vos informations d’identification du service dans le fichier de ressources service_token.json du projet.
* Ouvrez le fichier DocumentGeneration.java et spécifiez le dossier dans lequel vous souhaitez enregistrer les fichiers PDF générés.
* Ouvrez Main.java. Définissez la valeur de la variable postURL pour qu’elle pointe vers votre instance.
* Exécutez le fichier Main.java en tant qu’application Java.

>[!NOTE]
> La première fois que vous exécutez le programme Java, vous obtenez une erreur HTTP 403. Pour éviter cela, assurez-vous que vous avez accordé les [autorisations appropriées pour l’utilisateur ou l’utilisatrice du compte technique dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=fr#configurer-l’accès-dans-aem).

**Utilisateurs et utilisatrices d’AEM Forms** est le rôle que j’ai utilisé pour ce cours.
