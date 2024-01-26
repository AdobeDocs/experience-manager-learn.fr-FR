---
title: Importer des données d’un fichier PDF dans un formulaire adaptatif
description: Tutoriel sur l’import d’un fichier PDF pour remplir un formulaire adaptatif
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 27
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 100%

---

# Déployer les exemples de ressources

Vous pouvez déployer les exemples de ressources pour que cette solution fonctionne sur votre instance AEM Forms locale.

* [Importez la bibliothèque cliente et le composant personnalisé pour charger le formulaire PDF via le gestionnaire de packages.](./assets/client-libs-custom-component.zip)
* Téléchargez et déployez le lot à l’aide de la console web OSGi[Lot Document Services personnalisé](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).
* Téléchargez et déployez le lot à l’aide de la console web OSGi[Lot Développer avec l’utilisateur ou l’utilisatrice du service](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* Téléchargez et déployez le lot à l’aide de la console web OSGi [Importer des données d’un fichier PDF](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar).
* Ajoutez l’entrée _**DevelopingWithServiceUser.core:getresourceresolver=data**_ dans la console de configuration OSGi _**Service de mappage des utilisateurs et utilisatrices du service Apache Sling**_.
