---
title: Déploiement de l’exemple
description: Cas d’utilisation en cours d’exécution sur votre instance AEM Forms locale
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---



# Déploiement de l’exemple

Pour que ce cas pratique fonctionne sur votre système, suivez les instructions suivantes :

>[!NOTE]
>On suppose que vous exécutez AEM Forms sur le port 4502.


## Créer une base de données

Cet exemple utilise la base de données MySQL pour stocker les données de formulaire adaptatif. Vous devez créer le schéma de base de données [en important le fichier de schéma](assets/data-base-schema.sql) dans MySQL Workbench.

## Création d’une source de données

Vous devez créer une source de données appelée **StoreAndRetrieveAfData**. Le code du lot OSGi utilise ce nom de source de données.

## Création d’un modèle de données de formulaire

Le modèle de données de formulaire doit être créé en fonction de cette source de données appelée **StoreAndRetrieveAfData**. Ce modèle de données de formulaire est utilisé pour récupérer le numéro de téléphone mobile associé à l’ID de l’application. Le modèle de données de formulaire peut être [téléchargé ici.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Créer un compte développeur avec nexmo

Créez un compte développeur avec [Nexmo](https://dashboard.nexmo.com/) pour envoyer et vérifier les codes OTP. Notez la clé API et la clé secrète de l’API. La source de données et le modèle de données de formulaire ont déjà été créés pour vous par rapport à ce service et sont inclus avec les actifs mentionnés à l’étape précédente.

## Déployer les lots OSGi suivants

Déployez le lot comportant le code [pour stocker et récupérer les données de la base de données](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar).
Téléchargez et décompressez le fichier [developing-with-service-user.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/assets/common-osgi-bundles/developing-with-service-user.zip).
Déployez le fichier DevelopingWithServiceUser.jar à l’aide de la console web Felix.

## Déploiement de la bibliothèque cliente

L’exemple utilise 2 bibliothèques clientes. Importez ces [bibliothèques clientes](assets/client-libraries.zip) dans AEM.

## Importation du modèle de formulaire adaptatif personnalisé

Les exemples de formulaires utilisés dans cette démonstration sont basés sur un modèle personnalisé. Importez le [modèle personnalisé dans AEM](assets/custom-template-with-page-component.zip)

## Importation des exemples de formulaires adaptatifs

Les deux formulaires qui constituent cet exemple doivent être importés dans AEM. Les exemples de formulaires peuvent être [téléchargés ici](assets/sample-forms.zip)

Ouvrez le [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) en mode d’édition. Spécifiez les valeurs de clé API et de secret API dans les champs appropriés du formulaire adaptatif.

## Tester la solution

Aperçu de [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Saisissez votre numéro de mobile, y compris le code de pays, renseignez les détails de votre utilisateur et ajoutez des pièces jointes. Cliquez sur le bouton &quot;Enregistrer et quitter&quot; pour enregistrer le formulaire adaptatif et ses pièces jointes.


## Démonstration du cas pratique

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
