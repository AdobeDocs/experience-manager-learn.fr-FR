---
title: Tester la solution
description: Testez la solution en ajoutant des pièces jointes au formulaire et en déclenchant le workflow d’envoi de l’email.
sub-product: formulaires
feature: Workflow
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: Développement
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 540e11c0861eacc795122328b2359c7db6378aec
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 8%

---


# Étapes nécessaires pour tester les deux approches

* Configuration de [Service de messagerie Day CQ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) pour envoyer des courriers électroniques à partir du serveur AEM Forms
* Déployez le lot [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) à l’aide de la [console web felix](http://localhost:4502/system/console/bundles)

## Envoi d’un fichier zip en tant que pièce jointe à un email



* Déployez le workflow [SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Ce workflow utilise le composant d’envoi de courrier électronique pour envoyer le fichier zipped_attachments.zip qui est enregistré sous le dossier de charge utile par l’étape de processus personnalisée. Configurez les adresses email des expéditeurs et des destinataires en fonction de vos besoins.
* Importez l’[exemple de formulaire](assets/zip-form-attachments-form.zip) à partir de [Forms et de l’interface utilisateur du document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Prévisualisez le ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) formulaire, ajoutez deux pièces jointes et envoyez le formulaire.
* Le workflow doit être déclenché et une notification électronique avec le fichier zip doit être envoyée.

## Envoi de pièces jointes de formulaire en tant que fichiers individuels

* Déployez le workflow [SendForm.](assets/send-form-attachments-model.zip) Ce processus utilise le composant d’envoi de courrier électronique pour envoyer les pièces jointes du formulaire sous forme de fichiers individuels. Configurez l&#39;adresse email des expéditeurs et des destinataires en fonction de vos besoins.
* Importez l’[exemple de formulaire](assets/send-list-attachments-form.zip) à partir de [Forms et de l’interface utilisateur du document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Prévisualisez le ](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) formulaire, ajoutez deux pièces jointes et envoyez le formulaire.
* Le workflow doit être déclenché et une notification électronique avec les pièces jointes du formulaire sera envoyée.
