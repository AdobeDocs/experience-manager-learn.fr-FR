---
title: Déploiement de l’exemple
description: Obtenir l’exécution du cas d’utilisation sur votre instance AEM Forms locale
feature: formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 1%

---



# Déploiement de l’exemple

Pour que ce cas d&#39;utilisation fonctionne sur votre système, suivez les instructions suivantes :

>[!NOTE]
>On suppose que vous exécutez AEM Forms sur le port 4502.


## Créer une base de données

Cet exemple utilise la base de données MySQL pour stocker les données de formulaire adaptatif. Vous devez créer le schéma de base de données [en important le fichier de schéma](assets/data-base-schema.sql) dans MySQL Workbench.

## Créer une source de données

Vous devez créer une source de données appelée **StoreAndRetrieveAfData**. Le code du lot OSGi utilise ce nom de source de données.

## Créer un modèle de données de formulaire

Le modèle de données de formulaire doit être créé en fonction de cette source de données appelée **StoreAndRetrieveAfData**. Ce modèle de données de formulaire est utilisé pour récupérer le numéro de téléphone mobile associé à l’ID d’application. Le modèle de données de formulaire peut être [téléchargé ici.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Créer un compte développeur avec nexmo

Créez un compte de développeur avec [Nexmo](https://dashboard.nexmo.com/) pour l&#39;envoi et la vérification des codes OTP. Notez la clé d&#39;API et la clé secrète d&#39;API. La source de données et le modèle de données de formulaire ont déjà été créés pour vous par rapport à ce service et sont inclus avec les ressources mentionnées à l’étape précédente.

## Déploiement des lots OSGi suivants

Déployez le lot contenant le code [pour stocker et récupérer les données de la base de données](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar).
Déployez le [lot DevelopingWithServiceUser](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).

## Déploiement de la bibliothèque cliente

L’exemple utilise deux bibliothèques clientes. Importez ces [bibliothèques clientes](assets/client-libraries.zip) dans AEM.

## Importation du modèle de formulaire adaptatif personnalisé

Les exemples de formulaires utilisés dans cette démonstration sont basés sur un modèle personnalisé. Importez le modèle personnalisé [dans AEM](assets/custom-template-with-page-component.zip)

## Importation des exemples de formulaires adaptatifs

Les deux formulaires qui constituent cet exemple doivent être importés dans AEM. Les exemples de formulaires peuvent être [téléchargés ici](assets/sample-forms.zip)

Ouvrez le [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) en mode d’édition. Spécifiez les valeurs de clé d’API et de clé secrète d’API dans les champs appropriés du formulaire adaptatif.

## Test de la solution

Prévisualisation de [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Saisissez votre numéro de mobile, y compris le code du pays, renseignez les informations relatives à votre utilisateur et ajoutez des pièces jointes. Cliquez sur le bouton &quot;Enregistrer et quitter&quot; pour enregistrer le formulaire adaptatif et ses pièces jointes.


## Démonstration du cas d&#39;utilisation

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
