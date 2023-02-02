---
title: Déployer les ressources localement
description: Déployez les ressources du tutoriel sur votre instance d’AEM locale.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 5%

---

# Déployer sur votre système

Suivez les étapes répertoriées ci-dessous pour que ce cas pratique fonctionne sur votre instance AEM locale.

* [Déploiement du bundle DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) contenu dans le fichier zip.

* Ajoutez l’entrée suivante dans le service Apache Sling Service User Mapper **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** en utilisant la variable [configMgr](http://localhost:4502/system/console/configMgr).

* [Déployer le lot de newsletters](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Ce lot contient le code permettant de répertorier le contenu du dossier et d’assembler la ou les newsletter sélectionnées.

* [Importez le package à l’aide de Package Manager](assets/newsletter.zip). Ce package contient la bibliothèque cliente et des exemples de fichiers pdf pour tester la solution.

* [Importation de l’exemple de formulaire adaptatif](assets/sample-adaptive-form.zip). Ce formulaire répertorie les newsletters qui peuvent être sélectionnées.

* [Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Sélectionnez deux newsletters à télécharger. Les newsletters sélectionnées seront combinées dans un pdf et vous seront renvoyées.




