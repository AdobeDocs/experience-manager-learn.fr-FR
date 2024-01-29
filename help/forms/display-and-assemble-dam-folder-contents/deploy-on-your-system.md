---
title: Déployer les ressources localement
description: Déployez les ressources du tutoriel sur votre instance AEM locale.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 36
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '140'
ht-degree: 100%

---

# Déployer sur votre système

Suivez la procédure ci-dessous pour appliquer ce cas d’utilisation sur votre instance AEM locale.

* [Déployez le lot DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=fr) de l’archive zip.

* Ajoutez l’entrée suivante dans le service de mappage utilisateur de service Apache Sling **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** à l’aide du [configMgr](http://localhost:4502/system/console/configMgr).

* [Déployez le lot newsletters](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Il contient le code permettant de répertorier le contenu du dossier et d’assembler la ou les newsletters sélectionnées.

* [Importez le package à l’aide du gestionnaire de packages](assets/newsletter.zip). Ce package contient la bibliothèque cliente et des exemples de fichiers pdf pour tester la solution.

* [Importez l’exemple de formulaire adaptatif](assets/sample-adaptive-form.zip). Ce formulaire répertorie les newsletters qui peuvent être sélectionnées.

* [Prévisualisez le formulaire](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Sélectionnez deux newsletters à télécharger. Les newsletters sélectionnées seront combinées dans un seul PDF et vous seront renvoyées.
