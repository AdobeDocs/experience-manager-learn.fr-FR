---
title: Déployer l’exemple
description: Réalisez le cas d’utilisation sur votre instance AEM Forms locale.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 107
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 100%

---

# Déployer l’exemple

Pour transposer ce cas d’utilisation sur votre système, procédez comme suit :

>[!NOTE]
>Exécutez AEM Forms sur le port 4502.


## Créer une base de données

Cet exemple utilise la base de données MySQL pour stocker les données du formulaire adaptatif. Vous devez créer le [schéma de base de données en important le fichier de schéma](assets/data-base-schema.sql) dans MySQL Workbench.

## Créer une source de données

Vous devez créer une source de données en pool Apache Sling Connection appelée **StoreAndRetrieveAfData** pointant vers le schéma de base de données créé à l’étape précédente. En effet, le code du lot OSGi utilise ce nom de source de données.

## Créer un modèle de données de formulaire

Le modèle de données de formulaire doit être créé en fonction de cette source de données appelée **StoreAndRetrieveAfData**. Ce modèle de données de formulaire permet de récupérer le numéro de téléphone mobile associé à l’ID de l’application. [Téléchargez ici](assets/2-Factor-Authentication-DataSource-and-FDM.zip) le modèle de données de formulaire.

## Créer un compte de développement avec Nexmo

Créez un compte de développement avec [Nexmo](https://dashboard.nexmo.com/) pour envoyer et vérifier les codes OTP. Notez la clé de l’API et la clé secrète de l’API. La source de données et le modèle de données de formulaire ont déjà été créés pour vous dans le cadre de ce service et sont inclus dans les ressources mentionnées à l’étape précédente.

## Déployer les lots OSGi suivants

Déployez le lot qui contient le [code pour stocker et récupérer les données de la base de données](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar).
Téléchargez et décompressez l’archive [developing withserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=fr).
Déployez le fichier DevelopingWithServiceUser.jar à l’aide de la console web Felix.

## Déployer la bibliothèque cliente

L’exemple utilise 2 bibliothèques clientes. Importez ces [bibliothèques clientes](assets/store-af-with-attachments-client-lib.zip) dans AEM.

## Importer le modèle de formulaire adaptatif personnalisé

Les exemples de formulaires utilisés dans cette démonstration reposent sur un modèle personnalisé. Importez le [modèle personnalisé dans AEM](assets/custom-template-with-page-component.zip).

## Importer les exemples de formulaires adaptatifs

Les deux formulaires dans cet exemple doivent être importés dans AEM. [Téléchargez ici](assets/sample-forms.zip) les exemples de formulaires.

Ouvrez le formulaire [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) en mode d’édition. Indiquez les valeurs de la clé API Vonage et de la clé secrète dans les champs appropriés du formulaire adaptatif.

## Tester la solution

Prévisualisez le formulaire [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled).
Saisissez votre numéro de téléphone portable avec l’indicatif du pays, renseignez les détails de l’utilisateur ou de l’utilisatrice et ajoutez des pièces jointes. Cliquez sur le bouton « Enregistrer et quitter » pour enregistrer le formulaire adaptatif et ses pièces jointes.


## Démonstration du cas d’utilisation

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
