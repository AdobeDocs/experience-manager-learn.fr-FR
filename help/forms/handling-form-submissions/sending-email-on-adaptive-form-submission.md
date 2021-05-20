---
title: Envoi de courrier électronique lors de l’envoi d’un formulaire adaptatif
seo-title: Envoi de courrier électronique lors de l’envoi d’un formulaire adaptatif
description: Envoyer un courrier électronique de confirmation lors de l’envoi du formulaire adaptatif à l’aide du composant Envoyer un courrier électronique
seo-description: Envoyer un courrier électronique de confirmation lors de l’envoi du formulaire adaptatif à l’aide du composant Envoyer un courrier électronique
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Formulaires adaptatifs
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 3%

---


# Envoi de courrier électronique lors de l’envoi d’un formulaire adaptatif {#sending-email-on-adaptive-form-submission}

L’une des actions courantes consiste à envoyer un email de confirmation à l’auteur de l’envoi lors d’un envoi réussi de formulaire adaptatif. Pour ce faire, nous sélectionnerons &quot;Envoyer un courrier électronique&quot; comme action d’envoi.

Vous pouvez utiliser un modèle d&#39;email ou simplement saisir le corps de l&#39;email comme illustré dans cette capture d&#39;écran ci-dessous.

Notez la syntaxe pour insérer des valeurs de champ de formulaire dans le courrier électronique. Vous avez également la possibilité d’inclure des pièces jointes de formulaire dans le courrier électronique, en cochant la case &quot;inclure des pièces jointes&quot; dans les propriétés de configuration.

Lorsque le formulaire adaptatif est envoyé, le destinataire reçoit un courrier électronique.

![SendEmail](assets/sendemailaction.gif)

## Configurations Nécessaires {#configurations-needed}

Vous devrez configurer le service de messagerie Day CQ. Cela peut être configuré en pointant votre navigateur vers [Felix Configuration Manager](http://localhost:4502/system/console/configMgr).

La capture d’écran présente les propriétés de configuration du serveur de messagerie Adobe.

![mailservice](assets/mailservice.png)

Pour essayer cela sur votre serveur, procédez comme suit :

* [Importez les ](assets/timeoffrequest.zip) ressources associées à cet article dans AEM à l’aide du gestionnaire de packages.

* Ouvrez le [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Renseignez les détails. Veillez à indiquer une adresse électronique valide dans le champ Courriel.

* Envoyez le formulaire.
