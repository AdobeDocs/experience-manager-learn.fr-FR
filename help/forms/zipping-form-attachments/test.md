---
title: Test de la solution - Étapes nécessaires pour tester les deux approches
description: Testez la solution en ajoutant des pièces jointes au formulaire et en déclenchant le workflow d’envoi de l’email.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 7%

---

# Étapes nécessaires pour tester les deux approches

* Configurer [Service de messagerie Day CQ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) pour envoyer des courriers électroniques à partir du serveur AEM Forms
* Déployez la variable [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) regroupement utilisant [console web felix](http://localhost:4502/system/console/bundles)

## Envoi d’un fichier zip en tant que pièce jointe à un email



* Déployez la variable [Workflow SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Ce workflow utilise le composant d’envoi de courrier électronique pour envoyer le fichier zipped_attachments.zip qui est enregistré sous le dossier de charge utile par l’étape de processus personnalisée. Configurez les adresses email des expéditeurs et des destinataires en fonction de vos besoins.
* Importez la variable [exemple de formulaire](assets/zip-form-attachments-form.zip) de [Interface Utilisateur De Forms Et Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Aperçu du formulaire](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) et ajoutez quelques pièces jointes et envoyez le formulaire.
* Le workflow doit être déclenché et une notification électronique avec le fichier zip doit être envoyée.

## Envoi de pièces jointes de formulaire en tant que fichiers individuels

* Déployez la variable [Workflow SendForm.](assets/send-form-attachments-model.zip) Ce processus utilise le composant d’envoi de courrier électronique pour envoyer les pièces jointes du formulaire sous forme de fichiers individuels. Configurez l&#39;adresse email des expéditeurs et des destinataires en fonction de vos besoins.
* Importez la variable [exemple de formulaire](assets/send-list-attachments-form.zip) de [Interface Utilisateur De Forms Et Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Aperçu du formulaire](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) et ajoutez quelques pièces jointes et envoyez le formulaire.
* Le workflow doit être déclenché et une notification électronique avec les pièces jointes du formulaire est envoyée.
