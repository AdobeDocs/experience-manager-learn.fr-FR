---
title: Installation de l’édition de la communauté IntelliJ
description: Installation et importation du projet AEM dans IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# Installation d’IntelliJ

Installer [Edition de la communauté IntelliJ](https://www.jetbrains.com/idea/download/#section=windows). Vous pouvez accepter les paramètres par défaut lors de l’installation.

## Importation du projet AEM

* Launch IntelliJ
* Importez le projet AEM que vous avez créé à l’étape précédente. Une fois le projet importé, votre écran doit ressembler à ceci : ![aem-banking-app](assets/aem-banking-app.png). Vous travaillez généralement avec les sous-projets core, ui.apps, ui.config et ui.content .
* Si vous ne voyez pas la fenêtre Maven et Terminal, accédez à view->Tools Window et sélectionnez Maven and Terminal.

## Ajout du module polices

Si vous souhaitez utiliser des polices personnalisées dans votre fichier de PDF, vous devrez transmettre les polices personnalisées à l’instance AEM Forms CS. Suivez les étapes suivantes :

* Créez un dossier appelé **polices** dans C:\CloudManager\aem-banking-application
* Extraire le contenu de [font.zip](assets/fonts.zip) dans le dossier des polices nouvellement créé.
* Certaines polices personnalisées sont incluses dans le module Polices. Vous pouvez ajouter les polices personnalisées de votre entreprise au dossier C:\CloudManager\aem-banking-application\fonts\src\main\resources folder of the fonts module
* Ouvrez le fichier C:\CloudManager\aem-banking-application\pom.xml .
* Ajoutez la ligne suivante :  ```<module>fonts</module>``` dans la section modules du fichier pom.xml
* Enregistrez votre fichier pom.xml
* Actualisez le projet aem-banking-application dans IntelliJ

Structure du projet avec module de polices
![fonts-module](assets/fonts-module.png)

Module Polices inclus dans le POM des projets
![fonts-pom](assets/fonts-module-pom.png)
