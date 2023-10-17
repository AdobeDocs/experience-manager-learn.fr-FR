---
title: Authentification à deux facteurs par SMS
description: Ajoutez une couche de sécurité supplémentaire pour vous aider à confirmer l’identité d’un utilisateur ou d’une utilisatrice lorsque la personne souhaite effectuer certaines activités.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '598'
ht-degree: 100%

---

# Vérifier les utilisateurs et utilisatrices à l’aide de leurs numéros de téléphone mobile

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

Pour intégrer AEM/AEM Forms à des applications tierces, nous devons [créer une source de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=fr) dans la configuration des services cloud.

## Créer un modèle de données de formulaire

L’intégration de données d’AEM Forms fournit une interface utilisateur intuitive permettant de créer et d’utiliser des [modèles de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=fr). Un modèle de données de formulaire repose sur les sources de données pour l’échange de données.
Le modèle de données de formulaire complété peut être [téléchargé ici](assets/sms-2fa-fdm.zip).

![Modèle de données de formulaire.](assets/2FA-fdm.PNG)

## Créer un formulaire adaptatif

Intégrez les appels POST du modèle de données de formulaire à votre formulaire adaptatif pour vérifier le numéro de téléphone mobile saisi par l’utilisateur ou l’utilisatrice dans le formulaire. Libre à vous de créer votre propre formulaire adaptatif et d’utiliser l’appel POST du modèle de données de formulaire pour envoyer et vérifier le code OTP selon vos besoins.

Si vous souhaitez utiliser les exemples de ressources avec vos clés d’API, procédez comme suit :

* [Téléchargez le modèle de données de formulaire](assets/sms-2fa-fdm.zip) et importez-le dans AEM à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Téléchargez l’exemple de formulaire adaptatif [ici](assets/sms-2fa-verification-af.zip). Cet exemple de formulaire utilise les appels de service du modèle de données de formulaire fourni dans le cadre de cet article.
* Importez le formulaire dans AEM à partir de l’[interface utilisateur des formulaires et des documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Ouvrez le formulaire adaptatif en mode de modification. Ouvrez l’éditeur de règles pour le champ suivant :

![sms-send](assets/check-sms.PNG)

* Modifiez la règle associée au champ. Fournissez les clés d’API appropriées.
* Enregistrez le formulaire.
* [Prévisualisez le formulaire](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) et testez la fonctionnalité.
