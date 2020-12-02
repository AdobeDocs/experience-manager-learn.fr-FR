---
title: EXTRACTION des données OCR
description: Extrayez les données des documents émis par l’administration pour remplir les formulaires.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
translation-type: tm+mt
source-git-commit: c0db84f25334106c798d555c754d550113e91eac
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 3%

---



# EXTRACTION des données OCR

Extraire automatiquement les données d’un large éventail de documents émis par l’administration pour renseigner vos formulaires adaptatifs.

Un certain nombre d&#39;organisations offrent ce service et, tant qu&#39;elles disposent d&#39;API REST bien documentées, vous pouvez facilement vous intégrer à AEM Forms en utilisant la fonctionnalité d&#39;intégration des données. Pour les besoins de ce didacticiel, j’ai utilisé [ID Analyzer](https://www.idanalyzer.com/) pour démontrer l’extraction des données OCR des documents téléchargés.

Les étapes suivantes ont été suivies pour mettre en oeuvre l’extraction de données OCR avec AEM Forms à l’aide du service ID Analyzer.

## Créer un compte développeur

Créez un compte de développeur avec [ID Analyzer](https://portal.idanalyzer.com/signin.html). Notez la clé d&#39;API. Cette clé sera nécessaire pour appeler les API REST du service de l’analyseur d’ID.

## Créer un fichier Swagger/OpenAPI

OpenAPI Specification (anciennement Swagger Specification) est un format de description d’API pour les API REST. Un fichier OpenAPI vous permet de décrire l’intégralité de votre API, notamment :

* Points de terminaison (/utilisateurs) et opérations disponibles sur chaque point de terminaison (GET /utilisateurs, POST /utilisateurs)
* Paramètres d’opération Entrée et sortie pour chaque opération
Méthodes d’authentification
* Coordonnées, licences, conditions d&#39;utilisation et autres informations.
* Les spécifications d’API peuvent être écrites dans YAML ou JSON. Le format est facile à apprendre et à lire pour les humains et les machines.

Pour créer votre premier fichier swagger/OpenAPI, veuillez suivre la [documentation d’OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/).

>[!NOTE]
> AEM Forms prend en charge OpenAPI Specification version 2.0 (fka Swagger).

Utilisez l&#39;[éditeur de swagger](https://editor.swagger.io/) pour créer votre fichier de swagger afin de décrire les opérations qui envoient et vérifient le code OTP envoyé à l&#39;aide de SMS. Le fichier swagger peut être créé au format JSON ou YAML. Le fichier swagger terminé peut être téléchargé à partir de [ici](assets/drivers-license-swagger.zip)

## Créer une source de données

Pour intégrer AEM/AEM Forms à des applications tierces, nous devons [créer une source de données](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) dans la configuration des services cloud. Veuillez utiliser le [fichier swagger](assets/drivers-license-swagger.zip) pour créer votre source de données.

## Créer un modèle de données de formulaire

L’intégration des données AEM Forms fournit une interface utilisateur intuitive pour créer et utiliser [des modèles de données de formulaire](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Basez le modèle de données de formulaire sur la source de données créée à l’étape précédente.

![fdm](assets/test-dl-fdm.PNG)

## Créer une bibliothèque cliente

Nous devons obtenir la chaîne codée base64 du document téléchargé. Cette chaîne codée base64 est ensuite transmise comme l&#39;un des paramètres de notre appel REST.
La bibliothèque cliente peut être téléchargée [à partir d&#39;ici.](assets/drivers-license-client-lib.zip)

## Créer un formulaire adaptatif

Intégrez les appels POST du modèle de données de formulaire à votre formulaire adaptatif pour extraire les données du document téléchargé par l’utilisateur dans le formulaire. Vous êtes libre de créer votre propre formulaire adaptatif et d’utiliser l’appel POST du modèle de données de formulaire pour envoyer la chaîne codée en base 64 du document téléchargé.

## Déployer sur votre serveur

Si vous souhaitez utiliser les exemples de ressources avec votre clé d’API, procédez comme suit :

* [Téléchargez la ](assets/drivers-license-source.zip) source de données et importez-la dans AEM à l’aide du gestionnaire de  [packages.](http://localhost:4502/crx/packmgr/index.jsp)
* [Téléchargez le ](assets/drivers-license-fdm.zip) modèle de données de formulaire et importez-le dans AEM à l’aide du gestionnaire de  [packages.](http://localhost:4502/crx/packmgr/index.jsp)
* [Télécharger la bibliothèque cliente](assets/drivers-license-client-lib.zip)
* Vous pouvez télécharger l’exemple de formulaire adaptatif à [partir d’ici](assets/adaptive-form-dl.zip). Cet exemple de formulaire utilise les appels de service du modèle de données de formulaire fourni dans le cadre de cet article.
* Importez le formulaire dans AEM à partir de l’[interface utilisateur Forms et Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Ouvrez le formulaire en [mode d&#39;édition.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Indiquez votre clé d&#39;API comme valeur par défaut dans le champ apikey et enregistrez vos modifications.
* Ouvrez l’éditeur de règles pour le champ Chaîne de base 64. Notez l’appel de service lorsque la valeur de ce champ est modifiée.
* Enregistrez le formulaire
* [Prévisualisation du formulaire](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), télécharger la photo d&#39;accueil de votre permis de conduire


