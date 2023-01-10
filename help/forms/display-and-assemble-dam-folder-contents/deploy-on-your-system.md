---
title: Sélection et téléchargement du contenu du dossier DAM
description: Déployez les ressources du tutoriel sur votre instance d’AEM locale.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# Déployer sur votre système

Suivez les étapes répertoriées ci-dessous pour que ce cas pratique fonctionne sur votre instance AEM locale.

* [Configurez pour utiliser l’utilisateur fd-service en suivant les étapes mentionnées dans cet article.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). Assurez-vous d’avoir déployé le lot DevelopingWithServiceUser .

* [Déployer le lot de newsletters](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Ce lot contient le code permettant de répertorier le contenu du dossier et d’assembler la ou les newsletter sélectionnées.

* [Importez le package à l’aide de Package Manager](assets/newsletter.zip). Ce package contient la bibliothèque cliente et des exemples de fichiers pdf pour tester la solution.

* [Importation de l’exemple de formulaire adaptatif](assets/sample-adaptive-form.zip). Ce formulaire répertorie les newsletters qui peuvent être sélectionnées.

* [Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Sélectionnez deux newsletters à télécharger. Les newsletters sélectionnées seront combinées dans un pdf et vous seront renvoyées.




