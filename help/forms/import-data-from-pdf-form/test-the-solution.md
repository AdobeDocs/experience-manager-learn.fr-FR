---
title: Importation de données d’un fichier de PDF dans un formulaire adaptatif
description: Tutoriel pour remplir un formulaire adaptatif en important un fichier de PDF
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 3%

---

# Déployer des exemples de ressources

Vous pouvez déployer les exemples de ressources pour que cette solution fonctionne sur votre instance AEM Forms locale.

* [Importez la bibliothèque cliente et le composant personnalisé pour télécharger le formulaire pdf via Package Manager.](./assets/client-libs-custom-component.zip)
* Téléchargez et déployez le lot à l’aide de la console web OSGi.[Offre groupée de services de document personnalisés](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Téléchargez et déployez le lot à l’aide de la console web OSGi. [Développement avec le bundle d’utilisateurs du service](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Téléchargez et déployez le lot à l’aide de la console web OSGi.[importation de données à partir du fichier pdf](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* Ajouter l’entrée _**DevelopingWithServiceUser.core:getresourceresolver=data**_ dans le _**Service de mappage des utilisateurs du service Apache Sling**_ Console de configuration OSGi