---
title: Authentification à AEM en tant que Cloud Service à partir d’une application externe
description: Découvrez comment une application externe peut s’authentifier et interagir par programmation avec AEM en tant que Cloud Service sur HTTP à l’aide de jetons d’accès au développement local et d’informations d’identification du service.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: les API ;
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Sans affichage, intégrations
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 3%

---


# Authentification basée sur les jetons à AEM en tant que Cloud Service

Dans ce tutoriel, découvrez comment une application externe peut s’authentifier et interagir par programmation avec AEM en tant que Cloud Service sur HTTP à l’aide de jetons d’accès.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Prérequis

Assurez-vous que les éléments suivants sont en place avant de suivre ce tutoriel :

1. Accès à d’AEM en tant qu’environnement de Cloud Service (de préférence un environnement de développement ou un programme Sandbox)
1. Abonnement à l’AEM en tant que service de création de l’environnement de Cloud Service AEM profil de produit administrateur
1. Adhésion ou accès à votre administrateur de l’organisation IMS d’Adobe (il devra initialiser une seule fois les [Informations d’identification du service](./service-credentials.md))
1. Dernier [site WKND](https://github.com/adobe/aem-guides-wknd) déployé dans votre environnement de Cloud Service

## Présentation des applications externes

Ce tutoriel utilise une [application Node.js simple](./assets/aem-guides_token-authentication-external-application.zip) exécutée à partir de la ligne de commande pour mettre à jour les métadonnées des ressources sur AEM as a Cloud Service à l’aide de [l’API HTTP Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=fr).

Le flux d’exécution de l’application Node.js est le suivant :

![Application externe](./assets/overview/external-application.png)

1. L’application Node.js est appelée à partir de la ligne de commande.
1. Les paramètres de ligne de commande définissent :
   + L’AEM hôte du service Auteur en tant que Cloud Service auquel se connecter (`aem`)
   + Le dossier de ressources AEM dont les ressources seront mises à jour (`folder`)
   + La propriété et la valeur de métadonnées à mettre à jour (`propertyName` et `propertyValue`)
   + Chemin d’accès local au fichier contenant les informations d’identification requises pour accéder à AEM en tant que Cloud Service (`file`)
1. Le jeton d’accès utilisé pour s’authentifier sur AEM est dérivé du fichier JSON fourni via le paramètre de ligne de commande `file`

   a. Si les informations d’identification du service utilisées pour le développement non local sont fournies dans le fichier JSON (`file`), le jeton d’accès est récupéré à partir des API IMS d’Adobe.
1. L’application utilise le jeton d’accès pour accéder à AEM et répertorier toutes les ressources du dossier spécifié dans le paramètre de ligne de commande `folder`
1. Pour chaque ressource du dossier, l’application met à jour ses métadonnées en fonction du nom et de la valeur de la propriété spécifiés dans les paramètres de ligne de commande `propertyName` et `propertyValue`

Bien que cet exemple d’application soit Node.js, ces interactions peuvent être développées à l’aide de différents langages de programmation et exécutées à partir d’autres systèmes externes.

## Jeton d’accès au développement local

Les jetons d’accès au développement local sont générés pour un AEM spécifique en tant qu’environnement de Cloud Service et permettent d’accéder aux services de création et de publication.  Ces jetons d’accès sont temporaires et ne doivent être utilisés que lors du développement d’applications ou de systèmes externes qui interagissent avec AEM via HTTP. Au lieu d’avoir à obtenir et gérer des informations d’identification de service fiables, un développeur peut rapidement et facilement générer automatiquement un jeton d’accès temporaire qui lui permet de développer son intégration.

+ [Utilisation du jeton d’accès au développement local](./local-development-access-token.md)

## Informations d’identification du service

Les informations d’identification du service sont les informations d’identification fiables utilisées dans tous les scénarios de non-développement - la plus évidente étant la production - qui facilitent la capacité d’une application ou d’un système externe à s’authentifier et à interagir avec l’AEM en tant que Cloud Service sur HTTP. Les informations d’identification du service elles-mêmes ne sont pas envoyées à AEM pour authentification, mais l’application externe les utilise pour générer un jeton d’accès JWT, qui est échangé avec les API _d’Adobe IMS pour_ un jeton d’accès, qui peut ensuite être utilisé pour authentifier les requêtes HTTP vers AEM en tant que Cloud Service.

+ [Utilisation des informations d’identification du service](./service-credentials.md)

## Ressources supplémentaires

+ [Téléchargement de l’exemple d’application](./assets/aem-guides_token-authentication-external-application.zip)
+ Autres exemples de code pour la création et l’échange de JWT
   + [Exemples de code Node.js, Java, Python, C#.NET et PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Exemple de code basé sur JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
