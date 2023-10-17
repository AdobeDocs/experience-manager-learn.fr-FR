---
title: Installer les prérequis
description: Installer le logiciel nécessaire à la configuration de votre environnement de développement
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '235'
ht-degree: 100%

---

# Installer le logiciel requis

Ce tutoriel vous guide tout au long des étapes nécessaires à la création d’un projet AEM Forms, à la synchronisation du projet AEM Forms avec votre instance AEM locale à l’aide d’IntelliJ et de l’outil Repo Tool. Vous apprendrez également comment ajouter votre projet au référentiel Git local et envoyer le référentiel Git local vers le référentiel Cloud Manager.





Ce tutoriel fera référence à cette structure de dossiers à l’avenir.

* [Installez JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). J’ai téléchargé jdk-11.0.6_windows-x64_bin.zip.
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html). Par exemple, si vous avez installé Maven dans le dossier C:\maven, vous devez créer une variable d’environnement appelée M2_HOME avec la valeur C:\maven\apache-maven-3.6.0. Ajoutez ensuite M2_HOME\bin au chemin et enregistrez votre paramètre.

## Créer un projet Maven à l’aide de l’archétype de projet AEM

* Créez un dossier appelé **cloudmanager** (vous pouvez lui donner n’importe quel nom) dans votre lecteur C.
* Ouvrez l’invite de commande et accédez à **C:\cloudmanager**
* Copiez et collez le contenu du [fichier texte](assets/creating-maven-project.txt) dans la fenêtre d’invite de commande. Vous devrez peut-être modifier DarchetypeVersion=30 en fonction de la [dernière version](https://github.com/adobe/aem-project-archetype/releases). La dernière version était 30 au moment de la rédaction de cet article.
* Exécutez la commande en appuyant sur la touche Entrée. Si tout se passe correctement, le message de réussite de la création s’affiche.

## Étapes suivantes

[Installer IntelliJ](./intellij-set-up.md)