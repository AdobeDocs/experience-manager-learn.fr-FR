---
title: Extraction de données OCR
description: Extrayez les données des documents émis par l’administration pour remplir les formulaires.
feature: Barcoded Forms
version: 6.4,6.5
kt: 6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
source-git-commit: b918afdddf1f047b478e0521883a633f7b0610c6
workflow-type: tm+mt
source-wordcount: '709'
ht-degree: 3%

---

# Extraction de données OCR

Extrayez automatiquement les données d’un large éventail de documents émis par le gouvernement pour remplir vos formulaires adaptatifs.

Un certain nombre d’organisations fournissent ce service et tant qu’elles disposent d’API REST bien documentées, vous pouvez facilement l’intégrer à AEM Forms à l’aide de la fonctionnalité d’intégration des données. Pour les besoins de ce tutoriel, j’ai utilisé [Analyseur d’ID](https://www.idanalyzer.com/) pour démontrer l’extraction des données OCR des documents chargés.

Les étapes suivantes ont été suivies pour mettre en oeuvre l’extraction des données OCR avec AEM Forms à l’aide du service ID Analyzer.

## Créer un compte de développeur

Création d’un compte développeur avec [Analyseur d’ID](https://portal.idanalyzer.com/signin.html). Notez la clé API. Cette clé sera nécessaire pour appeler les API REST du service d’ID Analyzer.

## Création d’un fichier Swagger/OpenAPI

OpenAPI Specification (anciennement Swagger Specification) est un format de description d’API pour les API REST. Un fichier OpenAPI vous permet de décrire l’ensemble de votre API, notamment :

* Points de terminaison disponibles (/users) et opérations sur chaque point de terminaison (GET /users, POST /users)
* Paramètres d’opération Entrée et sortie pour chaque opération Méthodes d’authentification
* Coordonnées, licences, conditions d’utilisation et autres informations.
* Les spécifications d’API peuvent être écrites dans YAML ou JSON. Ce format est facile à apprendre et à lire pour les humains comme pour les machines.

Pour créer votre premier fichier swagger/OpenAPI, suivez la procédure décrite à la rubrique [Documentation OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms prend en charge la spécification OpenAPI version 2.0 (fka Swagger).

Utilisez la variable [éditeur de swagger](https://editor.swagger.io/) pour créer votre fichier swagger afin de décrire les opérations qui envoient et vérifient le code OTP envoyé à l’aide de SMS. Le fichier swagger peut être créé au format JSON ou YAML. Le fichier swagger terminé peut être téléchargé à partir de [here](assets/drivers-license-swagger.zip)

## Considérations relatives à la définition du fichier swagger

* Les définitions sont obligatoires.
* $ref doit être utilisé pour les définitions de méthode
* Préférer la définition de sections pour consommer et produire
* Ne définissez pas de paramètres de corps de requête ou de réponse intégrés. Essayez de modulariser autant que possible. Par exemple, la définition suivante n’est pas prise en charge

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

Les éléments suivants sont pris en charge avec une référence à la définition requestBody

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Exemple de fichier Swagger pour votre référence](assets/sample-swagger.json)

## Création d’une source de données

Pour intégrer AEM/AEM Forms à des applications tierces, nous devons [création d’une source de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) dans la configuration des services cloud. Veuillez utiliser la variable [fichier swagger](assets/drivers-license-swagger.zip) pour créer votre source de données.

## Création d’un modèle de données de formulaire

L’intégration de données AEM Forms offre une interface utilisateur intuitive pour créer et utiliser des [modèles de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Basez le modèle de données de formulaire sur la source de données créée à l’étape précédente.

![fdm](assets/test-dl-fdm.PNG)

## Créer une bibliothèque cliente

Nous devons obtenir la chaîne codée base64 du document téléchargé. Cette chaîne codée en base64 est ensuite transmise comme l’un des paramètres de notre appel REST.
La bibliothèque cliente peut être téléchargée. [d’ici.](assets/drivers-license-client-lib.zip)

## Créer un formulaire adaptatif

Intégrez les appels de POST du modèle de données de formulaire à votre formulaire adaptatif pour extraire les données du document téléchargé par l’utilisateur dans le formulaire. Vous êtes libre de créer votre propre formulaire adaptatif et d’utiliser l’appel du POST du modèle de données de formulaire pour envoyer la chaîne codée en base64 du document téléchargé.

## Déployer sur votre serveur

Si vous souhaitez utiliser les exemples de ressources avec votre clé API, procédez comme suit :

* [Téléchargement de la source de données](assets/drivers-license-source.zip) et importez dans AEM à l’aide de [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* [Téléchargement du modèle de données de formulaire](assets/drivers-license-fdm.zip) et importez dans AEM à l’aide de [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* [Télécharger la bibliothèque cliente](assets/drivers-license-client-lib.zip)
* Le téléchargement de l’exemple de formulaire adaptatif peut être [téléchargé ici](assets/adaptive-form-dl.zip). Cet exemple de formulaire utilise les appels de service du modèle de données de formulaire fourni dans le cadre de cet article.
* Importez le formulaire dans AEM à partir du [Interface utilisateur de Forms et de Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Ouvrez le formulaire dans [mode d’édition.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Spécifiez votre clé API comme valeur par défaut dans le champ apikey et enregistrez vos modifications.
* Ouvrez l’éditeur de règles pour le champ Chaîne de base 64 . Notez l’appel du service lorsque la valeur de ce champ est modifiée.
* Enregistrez le formulaire
* [Aperçu du formulaire](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), téléchargez la photo d&#39;accueil de votre permis de conduire.
