---
title: Configuration de la diffusion du document de canal web
seo-title: Setting up the delivery of web channel document
description: Il s’agit de la dernière partie d’un tutoriel en plusieurs étapes pour créer votre premier document de communication interactive. Dans cette partie, nous examinons la diffusion de documents de canal web par email.
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
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 1%

---

# Configuration de la diffusion du document de canal web {#setting-up-the-delivery-of-web-channel-document}


Dans cette partie, nous examinons la diffusion de documents de canal web par email.

Une fois que vous avez défini et testé votre document de communication interactive de canal web, vous avez besoin d’un mécanisme de diffusion pour diffuser le document de canal web au destinataire.

Pour pouvoir utiliser le courrier électronique comme mécanisme de diffusion pour notre document de canal web, nous devons apporter une modification mineure au modèle de données de formulaire.

[Pour en savoir plus sur la diffusion canal web par email](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Connectez-vous à AEM Forms.

* Accédez à Forms -> Intégrations de données

* Ouvrez le modèle de données RetirementAccountStatement en mode d’édition.

* Sélectionnez l&#39;objet soldes et cliquez sur le bouton Editer.

* Sélectionnez l’icône &quot;crayon&quot; pour ouvrir l’argument id en mode d’édition.

* Définissez la liaison sur &quot;RequestAttribute&quot;.

* Définissez le numéro de compte dans la valeur de liaison comme illustré ci-dessous.

* De cette manière, nous transmettons le numéro de compte via l’attribut request au modèle de données de formulaire.

* Veillez à enregistrer vos modifications.
   ![fdm](assets/requestattribute.gif)

## Tester la diffusion par courrier électronique d’un document de canal web {#test-email-delivery-of-web-channel-document}

* [Installation des exemples de ressources à l’aide du gestionnaire de modules](assets/webchanneldelivery.zip)
* [Connexion à crx](http://localhost:4502/crx/de/index.jsp#)

* Accédez à /home/users

* Recherchez l’utilisateur administrateur sous le noeud de l’utilisateur.

* Sélectionnez le noeud de profil de l’utilisateur administrateur.

* Créez une propriété appelée &quot;accountnumber&quot;. Assurez-vous que le type de propriété est une chaîne.

* Définissez la valeur de cette propriété de numéro de compte sur &quot;3059827&quot;. Vous pouvez définir cette valeur sur n’importe quel nombre aléatoire, comme vous le souhaitez.

* [Ouvrez getad.html](http://localhost:4502/content/getad.html)

* Le code associé à cette URL récupère le numéro de compte de l’utilisateur connecté. Ce numéro de compte est ensuite transmis en tant qu’attribut de demande au FDM. Le FDM récupère ensuite les données associées à ce numéro de compte et renseigne le document de canal web.

>[!NOTE]
>
>Jetez un oeil au **/apps/AEMForms/fetchad/GET.jsp** dans crx. Assurez-vous que la variable String webChannelDocument pointe vers un chemin d’accès de document de communication valide.

## Étapes suivantes

[Configurer une diffusion par courrier électronique](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)