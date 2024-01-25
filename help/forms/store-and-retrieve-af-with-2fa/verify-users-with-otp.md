---
title: Vérifier des utilisateurs et utilisatrices à l’aide d’OTP
description: Vérifiez le numéro de téléphone associé au numéro de l’application à l’aide d’OTP.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
duration: 92
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 100%

---

# Vérifier des utilisateurs et utilisatrices à l’aide d’OTP

L’authentification à deux facteurs (2FA) par SMS est une procédure de vérification de sécurité déclenchée par la connexion d’un utilisateur ou d’une utilisatrice à un site web, un logiciel ou une application. Dans le processus de connexion, l’utilisateur ou l’utilisatrice reçoit automatiquement un SMS sur son numéro de mobile contenant un code numérique unique.

Un certain nombre d’organisations fournissent ce service. Tant qu’elles disposent d’API REST bien documentées, vous pouvez facilement intégrer AEM Forms à l’aide des fonctionnalités d’intégration des données d’AEM Forms. Dans ce tutoriel, j’ai utilisé [Nexmo](https://developer.nexmo.com/verify/overview) pour démontrer le cas d’utilisation de la 2FA par SMS.

Nous avons suivi les étapes suivantes pour mettre en œuvre le service de 2FA par SMS avec AEM Forms à l’aide du service de vérification Nexmo.

## Créer un compte de développement

Créez un compte de développement avec [Nexmo](https://dashboard.nexmo.com/sign-in). Notez la clé de l’API et la clé secrète de l’API. Ces clés sont nécessaires pour appeler les API REST du service Nexmo.

## Créer un fichier Swagger/OpenAPI

La spécification OpenAPI (anciennement spécification Swagger) est un format de description d’API pour les API REST. Un fichier OpenAPI vous permet de décrire l’ensemble de votre API, notamment :

* les points d’entrée (/users) et les opérations disponibles sur chaque point d’entrée (GET/users, POST /users) ;
* les paramètres d’opération d’entrée et de sortie pour chaque opération.
Méthodes d’authentification
* Coordonnées, licences, conditions d’utilisation et autres informations.
* Les spécifications d’API peuvent être écrites en YAML ou JSON. Ce format est facile à apprendre et à lire pour les humains comme pour les machines.

Pour créer votre premier fichier Swagger/OpenAPI, suivez la procédure décrite dans la [documentation OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/).

>[!NOTE]
> AEM Forms prend en charge la spécification OpenAPI version 2.0 (fka Swagger).

Utilisez l’[éditeur de Swagger](https://editor.swagger.io/) pour créer votre fichier Swagger pour décrire les opérations qui envoient et vérifient le code OTP envoyé à l’aide d’un SMS. Le fichier Swagger peut être créé au format JSON ou YAML. Le fichier Swagger terminé peut être téléchargé [ici](assets/two-factore-authentication-swagger.zip).

## Créer une source de données

Pour intégrer AEM/AEM Forms à des applications tierces, nous avons besoin de la [source de données basée sur REST à l’aide du fichier Swagger](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=fr) dans la configuration des services cloud. La source de données terminée vous est fournie dans le cadre des ressources de ce cours.

## Créer un modèle de données de formulaire

L’intégration de données d’AEM Forms fournit une interface utilisateur intuitive permettant de créer et d’utiliser des [modèles de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=fr). Un modèle de données de formulaire repose sur les sources de données pour l’échange de données.
Le modèle de données de formulaire complété peut être [téléchargé ici](assets/sms-2fa-fdm.zip).

![Modèle de données de formulaire.](assets/2FA-fdm.PNG)

[Créer le formulaire principal](./create-the-main-adaptive-form.md)
