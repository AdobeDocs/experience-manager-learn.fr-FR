---
title: Authentification à AEM as a Cloud Service à partir d’une application externe
description: Découvrez comment une application externe peut s’authentifier et interagir par programmation avec AEM as a Cloud Service via HTTP à l’aide de jetons d’accès au développement local et d’informations d’identification de service.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '641'
ht-degree: 100%

---

# Authentification basée sur les jetons à AEM as a Cloud Service

AEM offre de nombreux points d’entrée HTTP accessibles de manière découplée, tels que GraphQL, AEM Content Services et l’API HTTP d’Assets. Souvent, ces clients découplés doivent s’authentifier auprès d’AEM pour accéder à un contenu ou à des actions protégés. Pour faciliter cette opération, AEM prend en charge l’authentification basée sur les jetons des requêtes HTTP provenant d’applications, de services ou de systèmes externes.

Suivez ce tutoriel et découvrez comment une application externe peut s’authentifier par programmation et interagir avec AEM as a Cloud Service via HTTP à l’aide de jetons d’accès.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Prérequis

Assurez-vous que les éléments suivants sont en place avant de suivre ce tutoriel :

1. L’accès à un environnement AEM as a Cloud Service (de préférence un environnement de développement ou un programme sandbox)
1. L’appartenance au profil de produit d’administration du service de création AEM de l’environnement AEM as a Cloud Service
1. L’appartenance ou l’accès à l’administration de l’organisation Adobe IMS (l’administration devra effectuer une initialisation unique des [Informations d’identification de service](./service-credentials.md))
1. Le dernier [WKND Site](https://github.com/adobe/aem-guides-wknd) déployé dans votre environnement Cloud Service

## Présentation de l’application externe

Ce tutoriel utilise une [application Node.js simple](./assets/aem-guides_token-authentication-external-application.zip), exécutée à partir de la ligne de commande, pour mettre à jour les métadonnées de ressource sur AEM as a Cloud Service à l’aide de l’[API HTTP Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=fr).

Le flux d’exécution de l’application Node.js est le suivant :

![Application externe.](./assets/overview/external-application.png)

1. L’application Node.js est appelée à partir de la ligne de commande.
1. Les paramètres de ligne de commande définissent les éléments suivants :
   + L’hôte du service de création AEM as a Cloud Service auquel se connecter (`aem`)
   + Le dossier de ressources d’AEM dont les ressources sont mises à jour (`folder`)
   + La propriété et la valeur de métadonnées à mettre à jour (`propertyName` et `propertyValue`)
   + Le chemin d’accès local au fichier contenant les informations d’identification requises pour accéder à AEM as a Cloud Service (`file`)
1. Le jeton d’accès utilisé pour s’authentifier sur AEM est dérivé du fichier JSON fourni via le paramètre de ligne de commande `file`.

   a. Si les informations d’identification de service utilisées pour le développement non local sont fournies dans le fichier JSON (`file`), le jeton d’accès est récupéré à partir des API d’Adobe IMS.
1. L’application utilise le jeton d’accès pour accéder à AEM et répertorier toutes les ressources du dossier spécifié dans le paramètre de ligne de commande `folder`.
1. Pour chaque ressource du dossier, l’application met à jour ses métadonnées en fonction du nom et de la valeur de la propriété spécifiés dans les paramètres de ligne de commande `propertyName` et `propertyValue`.

Bien que cet exemple d’application soit Node.js, ces interactions peuvent être développées à l’aide de différents langages de programmation et exécutées à partir d’autres systèmes externes.

## Jeton d’accès au développement local

Les jetons d’accès au développement local sont générés pour un environnement AEM as a Cloud Service spécifique et permettent d’accéder aux services de création et de publication.  Ces jetons d’accès sont temporaires et ne doivent être utilisés que lors du développement d’applications ou de systèmes externes qui interagissent avec AEM via HTTP. Au lieu de devoir obtenir et gérer des informations d’identification de service authentiques, l’équipe de développement peut rapidement et en toute facilité générer un jeton d’accès temporaire de manière automatique, ce qui leur permet de développer leur intégration.

+ [Utiliser le jeton d’accès au développement local](./local-development-access-token.md)

## Informations d’identification de service

Les informations d’identification de service sont les informations d’identification authentiques utilisées dans tous les scénarios en dehors du développement (en particulier lors de la production). Elles facilitent l’authentification et l’interaction des applications et systèmes externes avec AEM as a Cloud Service via HTTP. Les informations d’identification de service ne sont pas transmises à AEM pour l’authentification, mais l’application externe les utilise pour générer un jeton JWT qui est échangé avec les API d’Adobe IMS _contre_ un jeton d’accès, qui peut ensuite être utilisé pour authentifier les requêtes HTTP à AEM as a Cloud Service.

+ [Utiliser les informations d’identification de service](./service-credentials.md)

## Ressources supplémentaires

+ [Télécharger l’exemple d’application](./assets/aem-guides_token-authentication-external-application.zip)
+ Autres exemples de code pour la création et l’échange de jeton JWT
   + [Exemples de code Node.js, Java, Python, C#.NET et PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [Exemple de code basé sur JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
