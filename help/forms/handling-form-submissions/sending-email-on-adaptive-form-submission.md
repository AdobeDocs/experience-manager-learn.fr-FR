---
title: Envoi d’un courrier électronique à l’envoi d’un formulaire adaptatif
seo-title: Envoi d’un courrier électronique à l’envoi d’un formulaire adaptatif
description: Envoyer un courrier électronique de confirmation lors de l’envoi du formulaire adaptatif à l’aide du composant Envoyer un courrier électronique
seo-description: Envoyer un courrier électronique de confirmation lors de l’envoi du formulaire adaptatif à l’aide du composant Envoyer un courrier électronique
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: adaptive-forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
translation-type: tm+mt
source-git-commit: 272e43ee4aeb756a3a1fd0ce55eaca94a9553fa4
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---


# Envoi d&#39;un courrier électronique à l&#39;envoi d&#39;un formulaire adaptatif {#sending-email-on-adaptive-form-submission}

L’une des actions courantes consiste à envoyer un courriel de confirmation à l’expéditeur lors de l’envoi réussi d’un formulaire adaptatif. Pour ce faire, nous sélectionnerons &quot;Envoyer un courriel&quot; comme action d’envoi.

Vous pouvez utiliser le modèle de courrier électronique ou simplement taper le corps du courrier électronique, comme le montre cette capture d&#39;écran ci-dessous.

Notez la syntaxe permettant d’insérer des valeurs de champ de formulaire dans le courrier électronique. Nous avons également la possibilité d’inclure des pièces jointes au formulaire dans le courrier électronique, en cochant la case &quot;inclure des pièces jointes&quot; dans les propriétés de configuration.

Lorsque le formulaire adaptatif est envoyé, le destinataire reçoit un courrier électronique.

![SendEmail](assets/sendemailaction.gif)

## Configurations nécessaires {#configurations-needed}

Vous devrez configurer le service Day CQ Mail. Ceci peut être configuré en pointant votre navigateur vers [Felix Configuration Manager](http://localhost:4502/system/console/configMgr).

La capture d’écran présente les propriétés de configuration d’adobe mail server.

![mailservice](assets/mailservice.png)

Pour essayer ceci sur votre serveur, suivez les instructions suivantes :

* [Importez les ](assets/timeoffrequest.zip) actifs associés à cet article dans AEM à l’aide du gestionnaire de packages.

* Ouvrez le [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Renseignez les détails.Veillez à indiquer une adresse électronique valide dans le champ Courriel.

* Envoyez le formulaire.
