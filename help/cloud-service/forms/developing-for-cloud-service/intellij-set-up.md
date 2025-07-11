---
title: Installer IntelliJ Community Edition
description: Installer et importer le projet AEM dans IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
duration: 43
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '219'
ht-degree: 100%

---

# Installer IntelliJ

Installez [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/#section=windows). Vous pouvez accepter les paramètres par défaut suggérés lors de l’installation.

## Importer le projet AEM

* Lancer IntelliJ
* Importez le projet AEM que vous avez créé à l’étape précédente. Une fois le projet importé, votre écran doit ressembler à ceci : ![aem-banking-app](assets/aem-banking-app.png). Vous travaillez généralement avec les sous-projets principaux, ui.apps, ui.config et ui.content.
* Si vous ne voyez pas la fenêtre Maven et Terminal, accédez à la fenêtre Afficher > Outils et sélectionnez Maven et Terminal.

## Ajouter le module des polices

Si vous souhaitez utiliser des polices personnalisées dans votre fichier PDF, vous devrez transmettre les polices personnalisées à l’instance AEM Forms CS. Veuillez suivre les étapes suivantes :

* Créez un dossier appelé **fonts** dans C:\CloudManager\aem-banking-application
* Extrayez le contenu de [font.zip](assets/fonts.zip) dans le dossier fonts (polices) nouvellement créé.
* Certaines polices personnalisées sont incluses dans le module des polices. Vous pouvez ajouter les polices personnalisées de votre organisation au dossier C:\CloudManager\aem-banking-application\fonts\src\main\resources du module des polices.
* Ouvrez le fichierC:\CloudManager\aem-banking-application\pom.xml.
* Ajoutez la ligne suivante ```<module>fonts</module>``` dans la section de modules du fichier pom.xml.
* Enregistrez votre fichier pom.xml.
* Actualisez le projet aem-banking-application dans IntelliJ.

Structure du projet avec module des polices
![fonts-module](assets/fonts-module.png)

Module des polices inclus dans le fichier POM des projets
![fonts-pom](assets/fonts-module-pom.png)

## Étapes suivantes

[Configurer Git](./setup-git.md)
