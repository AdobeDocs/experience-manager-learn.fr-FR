---
title: Installation des conditions préalables
description: Installation du logiciel nécessaire à la configuration de votre environnement de développement
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---

# Installation du logiciel requis

Ce tutoriel vous guide tout au long des étapes nécessaires à la création d’un projet AEM Forms, à la synchronisation du projet AEM Forms avec votre instance d’AEM locale à l’aide de l’outil IntelliJ et repo. Vous apprendrez également comment ajouter votre projet au référentiel git local et envoyer le référentiel git local vers le référentiel cloud manager.





Ce tutoriel fera référence à cette structure de dossiers à l’avenir.

* [Installation de JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). J’ai téléchargé jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Par exemple, si vous avez installé Maven dans c:\maven folder, vous devez créer une variable d’environnement appelée M2_HOME avec la valeur C:\maven\apache-maven-3.6.0. Ajoutez ensuite M2_HOME\bin au chemin et enregistrez votre paramètre.

## Création d’un projet Maven à l’aide de AEM Project Archetype

* Créez un dossier appelé **cloudmanager**(vous pouvez lui donner n’importe quel nom) dans votre lecteur c
* Ouvrez l’invite de commande et accédez à **c:\cloudmanager**
* Copiez et collez le contenu de la [fichier texte](assets/creating-maven-project.txt) dans la fenêtre d’invite de commande. Vous devrez peut-être modifier DarchetypeVersion=30 en fonction de la variable [dernière version](https://github.com/adobe/aem-project-archetype/releases). La dernière version était 30 au moment de la rédaction de cet article.
* Exécutez la commande en appuyant sur la touche Entrée. Si tout se passe correctement, un message de réussite de création s’affiche.

## Étapes suivantes

[Installation d’IntelliJ](./intellij-set-up.md)