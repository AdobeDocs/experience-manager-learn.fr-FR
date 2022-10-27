---
title: Authentification à deux facteurs SMS
description: Ajoutez un niveau de sécurité supplémentaire pour vous aider à confirmer l’identité d’un utilisateur lorsqu’il souhaite exécuter certaines activités.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 5%

---

# Vérifier les utilisateurs à l’aide de leurs numéros de téléphone mobile

L’authentification à deux facteurs SMS (authentification à deux facteurs) est une procédure de vérification de sécurité déclenchée par la connexion d’un utilisateur à un site web, un logiciel ou une application. Dans le processus de connexion, l’utilisateur reçoit automatiquement un SMS à son numéro de mobile contenant un code numérique unique.

Un certain nombre d’organisations fournissent ce service. Tant qu’elles disposent d’API REST bien documentées, vous pouvez facilement intégrer AEM Forms à l’aide des fonctionnalités d’intégration des données d’AEM Forms. Pour les besoins de ce tutoriel, j’ai utilisé [Nexmo](https://developer.nexmo.com/verify/overview) pour démontrer le cas d’utilisation de SMS 2FA.

Les étapes suivantes ont été suivies pour mettre en oeuvre le service SMS 2FA avec AEM Forms à l’aide du service de vérification de code.

## Créer un compte de développeur

Création d’un compte développeur avec [Nexmo](https://dashboard.nexmo.com/sign-in). Notez la clé API et la clé secrète de l’API. Ces clés sont nécessaires pour appeler les API REST du service Nexmo.

## Création d’un fichier Swagger/OpenAPI

OpenAPI Specification (anciennement Swagger Specification) est un format de description d’API pour les API REST. Un fichier OpenAPI vous permet de décrire l’ensemble de votre API, notamment :

* Points de terminaison disponibles (/users) et opérations sur chaque point de terminaison (GET /users, POST /users)
* Paramètres d’opération Entrée et sortie pour chaque opération Méthodes d’authentification
* Coordonnées, licences, conditions d’utilisation et autres informations.
* Les spécifications d’API peuvent être écrites dans YAML ou JSON. Ce format est facile à apprendre et à lire pour les humains comme pour les machines.

Pour créer votre premier fichier swagger/OpenAPI, suivez la procédure décrite à la rubrique [Documentation OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms prend en charge la spécification OpenAPI version 2.0 (fka Swagger).

Utilisez la variable [éditeur de swagger](https://editor.swagger.io/) pour créer votre fichier swagger afin de décrire les opérations qui envoient et vérifient le code OTP envoyé à l’aide de SMS. Le fichier swagger peut être créé au format JSON ou YAML. Le fichier swagger terminé peut être téléchargé à partir de [here](assets/two-factore-authentication-swagger.zip)

## Création d’une source de données

Pour intégrer AEM/AEM Forms à des applications tierces, nous devons [création d’une source de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) dans la configuration des services cloud.

## Création d’un modèle de données de formulaire

L’intégration de données AEM Forms offre une interface utilisateur intuitive pour créer et utiliser des [modèles de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=fr). Un modèle de données de formulaire repose sur les sources de données pour l’échange de données.
Le modèle de données de formulaire complété peut être [téléchargé ici](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Créer un formulaire adaptatif

Intégrez les appels de POST du modèle de données de formulaire à votre formulaire adaptatif pour vérifier le numéro de téléphone portable saisi par l’utilisateur dans le formulaire. Vous êtes libre de créer votre propre formulaire adaptatif et d’utiliser l’appel du POST du modèle de données de formulaire pour envoyer et vérifier le code OTP selon vos besoins.

Si vous souhaitez utiliser les exemples de ressources avec vos clés d’API, procédez comme suit :

* [Téléchargement du modèle de données de formulaire](assets/sms-2fa-fdm.zip) et importez dans AEM à l’aide de [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Le téléchargement de l’exemple de formulaire adaptatif peut être [téléchargé ici](assets/sms-2fa-verification-af.zip). Cet exemple de formulaire utilise les appels de service du modèle de données de formulaire fourni dans le cadre de cet article.
* Importez le formulaire dans AEM à partir du [Interface utilisateur de Forms et de Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Ouvrez le formulaire en mode de modification. Ouvrez l’éditeur de règles pour le champ suivant :

![sms-send](assets/check-sms.PNG)

* Modifiez la règle associée au champ. Fournir les clés d’API appropriées
* Enregistrez le formulaire
* [Aperçu du formulaire](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) et tester la fonctionnalité.
