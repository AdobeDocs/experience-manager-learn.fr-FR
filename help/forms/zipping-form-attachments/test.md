---
title: Tester la solution - Étapes nécessaires pour tester les deux approches
description: Testez la solution en ajoutant des pièces jointes au formulaire et déclenchez le workflow pour envoyer l’e-mail.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 53
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 100%

---

# Étapes nécessaires pour tester les deux approches

* Configurez le [Service de messagerie Day CQ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=fr#configuring-the-mail-service) pour envoyer des e-mails à partir du serveur AEM Forms.
* Déployez le lot [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) à l’aide de la [console web Felix](http://localhost:4502/system/console/bundles).

## Envoyer un fichier zip en tant que pièce jointe à un e-mail



* Déployez le workflow [SendFormAttachmentsViaEmail.](assets/zipped-form-attachments-model.zip) Ce workflow utilise le composant Envoyer un e-mail pour envoyer le fichier zipped_attachments.zip qui est enregistré sous le dossier de charge utile par l’étape de processus personnalisée. Configurez les adresses e-mail des personnes qui envoient et reçoivent les e-mails selon vos besoins.
* Importez l’[exemple de formulaire](assets/zip-form-attachments-form.zip) depuis l’[Interface Utilisateur des formulaires et des documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* [Prévisualisez le formulaire](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled), ajoutez quelques pièces jointes et envoyez le formulaire.
* Le workflow doit être déclenché et une notification électronique avec le fichier zip doit être envoyée.

## Envoyer des pièces jointes de formulaire en tant que fichiers individuels

* Déployez le workflow [SendForm.](assets/send-form-attachments-model.zip) Ce workflow utilise le composant Envoyer un e-mail pour envoyer les pièces jointes du formulaire sous forme de fichiers individuels. Configurez l’adresse e-mail des personnes qui envoient et reçoivent les e-mails selon vos besoins.
* Importez l’[exemple de formulaire](assets/send-list-attachments-form.zip) depuis l’[Interface Utilisateur des formulaires et des documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* [Prévisualisez le formulaire](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled), ajoutez quelques pièces jointes et envoyez le formulaire.
* Le workflow doit être déclenché et une notification électronique avec les pièces jointes du formulaire est envoyée.
