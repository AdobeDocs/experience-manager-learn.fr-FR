---
title: Authentification à deux facteurs SMS
description: ajouter une couche de sécurité supplémentaire pour aider à confirmer l'identité d'un utilisateur lorsqu'il souhaite effectuer certaines activités
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
translation-type: tm+mt
source-git-commit: 4c08b09f59be0eb6644aaec729807b92bc339e82
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---



# Vérifier les utilisateurs à l’aide de leurs numéros de téléphone mobile

L&#39;authentification à deux facteurs SMS (authentification à deux facteurs) est une procédure de vérification de la sécurité qui est déclenchée par la connexion d&#39;un utilisateur à un site Web, un logiciel ou une application. Dans le processus de connexion, l’utilisateur reçoit automatiquement un SMS à son numéro de mobile contenant un code numérique unique.

Un certain nombre d&#39;organisations offrent ce service et tant qu&#39;elles disposent d&#39;API REST bien documentées, vous pouvez facilement intégrer AEM Forms à l&#39;aide des fonctionnalités d&#39;intégration de données d&#39;AEM Forms. Aux fins de ce tutoriel, j&#39;ai utilisé [Nexmo](https://developer.nexmo.com/verify/overview) pour démontrer le cas d&#39;utilisation de SMS 2FA.

Les étapes suivantes ont été suivies pour mettre en oeuvre le service SMS 2FA avec AEM Forms à l&#39;aide du service Nexmo Verify.

## Créer un compte développeur

Créez un compte Développeur avec [Nexmo](https://dashboard.nexmo.com/sign-in). Notez la clé d&#39;API et la clé secrète d&#39;API. Ces clés seront nécessaires pour appeler les API REST du service Nexmo.

## Créer un fichier Swagger/OpenAPI

OpenAPI Specification (anciennement Swagger Specification) est un format de description d’API pour les API REST. Un fichier OpenAPI vous permet de décrire l’intégralité de votre API, notamment :

* Points de terminaison (/utilisateurs) et opérations disponibles sur chaque point de terminaison (GET /utilisateurs, POST /utilisateurs)
* Paramètres d’opération Entrée et sortie pour chaque méthode operationAuthentication
* Coordonnées, licences, conditions d&#39;utilisation et autres informations.
* Les spécifications d’API peuvent être écrites dans YAML ou JSON. Le format est facile à apprendre et à lire pour les humains et les machines.

Pour créer votre premier fichier swagger/OpenAPI, suivez la documentation [OpenAPI.](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> aem forms prend en charge OpenAPI Specification version 2.0 (fka Swagger).

Utilisez l’éditeur [de](https://editor.swagger.io/) swagger pour créer votre fichier de swagger afin de décrire les opérations qui envoient et vérifient le code OTP envoyé à l’aide de SMS. Le fichier swagger peut être créé au format JSON ou YAML. Le fichier swagger terminé peut être téléchargé [ici](assets/two-factore-authentication-swagger.zip)

## Créer une source de données

Pour intégrer AEM/AEM Forms à des applications tierces, nous devons [créer une source](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) de données dans la configuration des services cloud.

## Créer un modèle de données de formulaire

AEM Forms data integration provides an intuitive user interface to create and work with [form data models](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modèle de données de formulaire repose sur les sources de données pour l’échange de données.
Le modèle de données de formulaire renseigné peut être [téléchargé ici](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Créer un formulaire adaptatif

Intégrez les appels POST du modèle de données de formulaire à votre formulaire adaptatif pour vérifier le numéro de téléphone mobile saisi par l’utilisateur dans le formulaire. Vous êtes libre de créer votre propre formulaire adaptatif et d’utiliser l’appel POST du modèle de données de formulaire pour envoyer et vérifier le code OTP selon vos besoins.

Si vous souhaitez utiliser les exemples de ressources avec vos clés d’API, procédez comme suit :

* [Téléchargez le modèle](assets/sms-2fa-fdm.zip) de données de formulaire et importez-le dans AEM à l’aide du gestionnaire de [packages](http://localhost:4502/crx/packmgr/index.jsp)
* Vous pouvez [télécharger l’exemple de formulaire adaptatif à partir d’ici](assets/sms-2fa-verification-af.zip). Cet exemple de formulaire utilise les appels de service du modèle de données de formulaire fourni dans le cadre de cet article.
* Importez le formulaire dans AEM à partir de l’interface utilisateur de [Forms et de Document.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Ouvrez le formulaire en mode de modification. Ouvrez l’éditeur de règles pour le champ suivant.

![sms-send](assets/check-sms.PNG)

* Modifiez la règle associée au champ. Fournir les clés d’API appropriées
* Enregistrez le formulaire
* [Prévisualisation du formulaire](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) et test de la fonctionnalité


