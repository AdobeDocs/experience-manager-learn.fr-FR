---
title: Vérifier les utilisateurs avec OTP
description: Vérifiez le numéro de téléphone mobile associé au numéro d'application à l'aide du protocole OTP.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---



# Vérifier les utilisateurs avec OTP

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
> AEM Forms prend en charge OpenAPI Specification version 2.0 (fka Swagger).

Utilisez l’éditeur [de](https://editor.swagger.io/) swagger pour créer votre fichier de swagger afin de décrire les opérations qui envoient et vérifient le code OTP envoyé à l’aide de SMS. Le fichier swagger peut être créé au format JSON ou YAML. Le fichier swagger terminé peut être téléchargé [ici](assets/two-factore-authentication-swagger.zip)

## Créer une source de données

Pour intégrer AEM/AEM Forms à des applications tierces, nous devons utiliser une source de données [REST à l’aide du fichier](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) swagger dans la configuration des services cloud. La source de données terminée vous est fournie dans le cadre des ressources de ce cours.

## Créer un modèle de données de formulaire

AEM Forms data integration provides an intuitive user interface to create and work with [form data models](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modèle de données de formulaire repose sur les sources de données pour l’échange de données.
Le modèle de données de formulaire renseigné peut être [téléchargé ici](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
