---
title: Envoyer un e-mail lors de l’envoi d’un formulaire adaptatif
description: Envoyer un e-mail de confirmation lors de l’envoi d’un formulaire adaptatif à l’aide du composant Envoyer un e-mail
feature: Adaptive Forms
doc-type: article
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
duration: 60
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 100%

---

# Envoyer un e-mail lors de l’envoi d’un formulaire adaptatif {#sending-email-on-adaptive-form-submission}

Il est courant d’envoyer un e-mail de confirmation à l’auteur ou à l’autrice lors de l’envoi abouti d’un formulaire adaptatif. Pour ce faire, sélectionnons « Envoyer un e-mail » comme action d’envoi.

Vous pouvez utiliser un modèle d’e-mail ou simplement saisir le corps de l’e-mail, comme illustré dans cette capture d’écran ci-dessous.

Notez la syntaxe permettant d’insérer des valeurs de champ de formulaire dans l’e-mail. Vous avez également la possibilité d’inclure des pièces jointes de formulaire dans l’e-mail, en cochant la case « Inclure les pièces jointes » dans les propriétés de configuration.

Lorsque le formulaire adaptatif est envoyé, le ou la destinataire reçoit un e-mail.

![SendEmail](assets/sendemailaction.gif)

## Configurations requises {#configurations-needed}

Vous devrez configurer le service de messagerie Day CQ. Pour cela, pointez votre navigateur vers le [gestionnaire de configuration Felix](http://localhost:4502/system/console/configMgr).

La capture d’écran présente les propriétés de configuration du serveur de messagerie Adobe.

![mailservice](assets/mailservice.png)

Pour essayer cela sur votre serveur, procédez comme suit :

* [Importez les ressources](assets/timeoffrequest.zip) associées à cet article dans AEM à l’aide du gestionnaire de packages.

* Ouvrez le [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Remplissez les détails. Veillez à indiquer une adresse e-mail valide dans le champ E-mail.

* Envoyez le formulaire.
