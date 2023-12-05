---
title: Utiliser l’étape Envoyer un e-mail de Forms Workflow
description: L’étape Envoyer un e-mail a été introduite dans AEM Forms 6.4. Elle permet de créer des processus métier ou un workflow qui vous permettront d’envoyer des e-mails avec ou sans pièces jointes. La vidéo suivante décrit les étapes de configuration du composant Envoyer un e-mail.
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 345
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 100%

---

# Utiliser l’étape Envoyer un e-mail de Forms Workflow {#using-send-email-step-of-forms-workflow}

L’étape Envoyer un e-mail a été introduite dans AEM Forms 6.4. Elle permet de créer des processus métier ou un workflow qui vous permettront d’envoyer des e-mails avec ou sans pièces jointes. La vidéo suivante décrit les étapes de configuration du composant Envoyer un e-mail.

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

Dans le cadre de cet article, nous allons vous présenter les cas d’utilisation suivants :

1. Une personne remplit le formulaire de demande de congés.
1. L’envoi du formulaire déclenche un workflow AEM.
1. Le workflow AEM utilise le composant Envoyer un e-mail pour envoyer un e-mail avec le document d’enregistrement en tant que pièce jointe.

Avant d’utiliser l’étape Envoyer un e-mail, assurez-vous de configurer le service de messagerie Day CQ à partir de [configMgr](http://localhost:4502/system/console/configMgr). Indiquez les valeurs propres à votre environnement.

![Configuration du service de messagerie Day CQ.](assets/mailservice.png)

Dans le cadre des ressources associées à cet article, vous obtiendrez les éléments suivants :

1. Formulaire adaptatif qui déclenche le workflow lors de l’envoi.
1. Exemple de workflow qui envoie un e-mail avec le document d’enregistrement en tant que pièce jointe.
1. Lot OSGi qui crée les propriétés de métadonnées.

Pour que l’exemple s’exécute sur votre système, procédez comme suit :

1. [Déployez le lot Developingwithserviceuser.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Téléchargez et installez le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Ce lot contient le code permettant de créer les propriétés de métadonnées dans le cadre de l’étape de processus du workflow.
1. [Configuration du service de messagerie Day CQ.](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importez et installez les ressources associées à cet article à l’aide du gestionnaire de packages dans CRX.](assets/emaildoraemformskt.zip)
1. Lancez le [formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Remplissez les champs obligatoires et envoyez-les.
1. Vous devriez recevoir un e-mail avec DocumentOfRecord en tant que pièce jointe.

Explorer les [modèle de workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Jetez un œil à l’étape de processus du workflow. Le code personnalisé associé à l’étape de processus crée des noms de propriété de métadonnées et définit ses valeurs à partir des données envoyées. Ces valeurs sont ensuite utilisées par le composant Envoyer un e-mail.

>[!NOTE]
>
>Dans AEM Forms 6.5 et versions ultérieures, vous n’avez pas besoin de ce code personnalisé pour créer des propriétés de métadonnées. Utilisez la fonctionnalité de variables dans AEM Workflow.

Assurez-vous que l’onglet Pièces jointes du composant Envoyer un e-mail est configuré conformément à la capture d’écran ci-dessous.
![Onglet Pièces jointes du composant Envoyer un e-mail](assets/sendemailcomponentconfigure.jpg). La valeur « DOR.pdf » doit correspondre à la valeur spécifiée dans le chemin d’accès au document d’enregistrement spécifié dans les options d’envoi de votre formulaire adaptatif.
