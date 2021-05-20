---
title: Utilisation de l’étape Envoyer un courrier électronique du Forms Workflow
seo-title: Utilisation de l’étape Envoyer un courrier électronique du Forms Workflow
description: L’étape Envoyer un courrier électronique a été introduite dans AEM Forms 6.4. Cette étape permet de créer des processus d’entreprise ou un workflow qui vous permettront d’envoyer des emails avec ou sans pièces jointes. La vidéo suivante décrit les étapes de configuration du composant Envoyer un courrier électronique
seo-description: L’étape Envoyer un courrier électronique a été introduite dans AEM Forms 6.4. Cette étape permet de créer des processus d’entreprise ou un workflow qui vous permettront d’envoyer des emails avec ou sans pièces jointes. La vidéo suivante décrit les étapes de configuration du composant Envoyer un courrier électronique
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 3%

---


# Utilisation de l’étape Envoyer un courrier électronique du Forms Workflow {#using-send-email-step-of-forms-workflow}

L’étape Envoyer un courrier électronique a été introduite dans AEM Forms 6.4. Cette étape permet de créer des processus d’entreprise ou un workflow qui vous permettront d’envoyer des emails avec ou sans pièces jointes. La vidéo suivante décrit les étapes de configuration du composant Envoyer un courrier électronique.

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

Dans le cadre de cet article, nous allons vous présenter les cas pratiques suivants :

1. Un utilisateur remplit le formulaire de demande de désactivation du délai
1. Lors de l’envoi du formulaire, AEM workflow est déclenché
1. Le workflow AEM utilise le composant Envoyer un courrier électronique pour envoyer un courrier électronique avec le document d’enregistrement en tant que pièce jointe.

Avant d’utiliser l’étape Envoyer un courrier électronique , assurez-vous de configurer le service de messagerie Day CQ à partir de [configMgr](http://localhost:4502/system/console/configMgr). Indiquez les valeurs propres à votre environnement.

![Configuration du service de messagerie Day CQ](assets/mailservice.png)

Dans le cadre des ressources associées à cet article, vous obtiendrez les éléments suivants :

1. Formulaire adaptatif qui déclenche le processus lors de l’envoi
1. Exemple de workflow qui enverra un email avec DOR comme pièce jointe
1. Groupe OSGi qui crée les propriétés de métadonnées

Pour que l’exemple s’exécute sur votre système, procédez comme suit :

1. [Déployer le lot Developer withserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Télécharger et installer le ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)lot setvalueCe lot contient le code permettant de créer les propriétés de métadonnées dans le cadre de l’étape de processus du workflow.
1. [Configuration du service de messagerie Day CQ](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importez et installez les actifs associés à cet article à l’aide du gestionnaire de packages dans CRX.](assets/emaildoraemformskt.zip)
1. Lancez le [formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Renseignez les champs obligatoires et envoyez-les.
1. Vous devriez recevoir un email avec DocumentOfRecord en tant que pièce jointe

Explorez le [modèle de workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Jetez un oeil à l’étape de processus du workflow. Le code personnalisé associé à l’étape de processus crée des noms de propriété de métadonnées et définit ses valeurs à partir des données envoyées. Ces valeurs sont ensuite utilisées par le composant d’envoi de courrier électronique.

>[!NOTE]
>
>Dans AEM Forms 6.5 et versions ultérieures, vous n’avez pas besoin de ce code personnalisé pour créer des propriétés de métadonnées. Utilisez la fonctionnalité de variables dans AEM Workflow.

Assurez-vous que l’onglet Pièces jointes du composant Envoyer un courrier électronique est configuré conformément à la capture d’écran ci-dessous.
![Onglet Envoyer la pièce jointe d’un courrier électronique](assets/sendemailcomponentconfigure.jpg)La valeur &quot;DOR.pdf&quot; doit correspondre à la valeur spécifiée dans le chemin d’accès au document d’enregistrement spécifié dans les options d’envoi de votre formulaire adaptatif.

