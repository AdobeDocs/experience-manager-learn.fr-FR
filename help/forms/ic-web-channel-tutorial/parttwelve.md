---
title: Configuration de la diffusion du document de canal Web
seo-title: Configuration de la diffusion du document de canal Web
description: Il s'agit de la dernière partie d'un didacticiel en plusieurs étapes pour créer votre premier document de communications interactives. Dans cette partie, nous examinons la diffusion du document de canal Web par courriel.
seo-description: Il s'agit de la dernière partie d'un didacticiel en plusieurs étapes pour créer votre premier document de communications interactives. Dans cette partie, nous examinons la diffusion du document de canal Web par courriel.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 1%

---


# Configuration de la diffusion du document de canal Web {#setting-up-the-delivery-of-web-channel-document}


Dans cette partie, nous examinons la diffusion du document de canal Web par courriel.

Une fois que vous avez défini et testé votre document de communication interactif de canal Web, vous avez besoin d’un mécanisme de diffusion pour fournir le document de canal Web au destinataire.

Pour pouvoir utiliser le courriel comme mécanisme de diffusion pour notre document de canal Web, nous devons apporter une modification mineure au modèle de données de formulaire.

[Pour en savoir plus sur la diffusion de canal Web par courriel](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Connectez-vous à AEM Forms.

* Accédez à Forms ->Intégrations des données

* Ouvrez le modèle de données RetirementAccountStatement en mode d&#39;édition.

* Sélectionnez l’objet soldes et cliquez sur le bouton Modifier.

* Sélectionnez l&#39;icône représentant un crayon pour ouvrir l&#39;argument id en mode d&#39;édition.

* Modifiez la liaison en &quot;RequestAttribute&quot;.

* Définissez le numéro de compte dans la valeur de liaison comme illustré ci-dessous.

* Ainsi, nous transmettons le numéro de compte via l’attribut de requête au modèle de données de formulaire.

* Veillez à enregistrer vos modifications.
   ![fdm](assets/requestattribute.gif)

## Test de la Diffusion de courriel du Document de Canal Web {#test-email-delivery-of-web-channel-document}

* [Installation des exemples de ressources à l’aide du gestionnaire de packages](assets/webchanneldelivery.zip)
* [Connexion à crx](http://localhost:4502/crx/de/index.jsp#)

* Accédez à /home/users

* Recherchez l’utilisateur administrateur sous le noeud de l’utilisateur.

* Sélectionnez le noeud de profil de l’utilisateur administrateur.

* Créez une propriété appelée &quot;numéro de compte&quot;. Assurez-vous que le type de propriété est une chaîne.

* Définissez la valeur de cette propriété de numéro de compte sur &quot;3059827&quot;. Vous pouvez définir cette valeur sur n’importe quel nombre aléatoire.

* [Ouvrir getad.html](http://localhost:4502/content/getad.html)

* Le code associé à cette URL obtient le numéro de compte de l’utilisateur connecté. Ce numéro de compte est ensuite transmis en tant que contribution demandée au FDM. Le FDM récupère ensuite les données associées à ce numéro de compte et remplit le document du canal Web.

>[!NOTE]
>
>Veuillez consulter le fichier **/apps/AEMForms/fetchad/GET.jsp** dans crx. Assurez-vous que la variable String webChannelDocument pointe vers un chemin d&#39;document de communication valide.
