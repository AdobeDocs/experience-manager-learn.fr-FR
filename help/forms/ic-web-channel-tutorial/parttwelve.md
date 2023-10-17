---
title: Configurer la diffusion d’un document de canal web
seo-title: Setting up the delivery of web channel document
description: Il s’agit de la dernière partie d’un tutoriel en plusieurs étapes permettant de créer votre premier document de communication interactive. Cette partie est consacrée à la diffusion d’un document de canal web par e-mail.
seo-description: This is the final part of a multistep tutorial for creating your first interactive communications document. In this part, we look at the delivery of web channel document via email.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '357'
ht-degree: 100%

---

# Configurer la diffusion d’un document de canal web {#setting-up-the-delivery-of-web-channel-document}


Cette partie est consacrée à la diffusion d’un document de canal web par e-mail.

Une fois que vous avez défini et testé votre document de communication interactive de canal web, vous avez besoin d’un mécanisme de diffusion pour diffuser le document de canal web à la personne destinataire.

Pour utiliser l’e-mail comme mécanisme de diffusion pour notre document de canal web, nous devons apporter une modification mineure au modèle de données de formulaire.

[Pour en savoir plus sur la diffusion canal web par e-mail](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Connectez-vous à AEM Forms.

* Accédez à Forms -> Intégrations de données

* Ouvrez le modèle de données RetirementAccountStatement en mode d’édition.

* Sélectionnez l’objet « balances » et cliquez sur le bouton d’édition.

* Sélectionnez l’icône en forme de crayon pour ouvrir l’argument « id » en mode d’édition.

* Définissez la liaison sur « RequestAttribute ».

* Définissez le numéro de compte « accountnumber » dans la valeur de liaison comme illustré ci-dessous.

* De cette manière, nous transmettons le numéro de compte au modèle de données de formulaire par l’intermédiaire de l’attribut de requête « request ».

* Veillez à enregistrer les modifications.
  ![fdm](assets/requestattribute.gif)

## Tester la diffusion par e-mail d’un document de canal web {#test-email-delivery-of-web-channel-document}

* [Installer les exemples de ressources à l’aide du gestionnaire de packages](assets/webchanneldelivery.zip)
* [Connectez-vous à crx](http://localhost:4502/crx/de/index.jsp#).

* Accédez à /home/users

* Recherchez la personne administratrice sous le nœud de l’utilisateur ou de l’utilisatrice.

* Sélectionnez le nœud de profil de la personne administratrice.

* Créez une propriété appelée « accountnumber ». Assurez-vous que le type de propriété est une chaîne.

* Définissez la valeur de la propriété « accountnumber » sur « 3059827 ». Vous pouvez définir cette valeur sur un nombre de votre choix.

* [Ouvrez getad.html](http://localhost:4502/content/getad.html).

* Le code associé à cette URL permet de récupérer le numéro de compte « accountnumber » de la personne utilisatrice connectée. Ce dernier est ensuite transmis en tant qu’attribut de requête « requestattribute » au modèle de données de formulaire (FDM). Le FDM récupère ensuite les données associées à ce numéro de compte « accountnumber » et renseigne le document de canal web.

>[!NOTE]
>
>Consultez le fichier **/apps/AEMForms/fetchad/GET.jsp** dans crx. Assurez-vous que la variable de chaîne « webChannelDocument » pointe vers un chemin d’accès de document de communication valide.

## Étapes suivantes

[Configurer une diffusion par e-mail](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)