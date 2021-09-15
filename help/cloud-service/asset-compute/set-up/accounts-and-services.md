---
title: Configuration de comptes et de services pour l’extensibilité des Assets compute
description: Le développement de travailleurs d’Asset compute nécessite l’accès à des comptes et à des services, notamment AEM en tant que Cloud Service, Adobe Project Firefly et le stockage dans le cloud fourni par Microsoft ou Amazon.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 3%

---

# Configuration des comptes et des services

Ce tutoriel nécessite que les services suivants soient mis en service et accessibles via Adobe ID de l’apprenant.

Tous les services Adobe doivent être accessibles via la même organisation Adobe, à l’aide de votre Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe du projet FireFly](#adobe-project-firefly)
   + La mise en service peut prendre entre 2 et 10 jours
+ Stockage dans le cloud
   + [Stockage Azure Blob](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + ou [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Assurez-vous d’avoir accès à tous les services mentionnés ci-dessus avant de poursuivre ce tutoriel.
> 
> Consultez les sections ci-dessous pour savoir comment définir et fournir les services requis.

## AEM en tant que Cloud Service{#aem-as-a-cloud-service}

L’accès à un environnement d’AEM en tant qu’environnement de Cloud Service est requis pour configurer les profils de traitement AEM Assets afin d’appeler le programme de travail d’Asset compute personnalisé.

Idéalement, un programme d’environnement de test ou un environnement de développement hors environnement de test est disponible.

Notez qu’un SDK d’AEM local n’est pas suffisant pour terminer ce tutoriel, car le SDK AEM local ne peut pas communiquer avec les microservices d’Asset compute. Au lieu de cela, un véritable  en tant qu’environnement de Cloud Service est requis.

## Adobe Project Firefly{#adobe-project-firefly}

La structure [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) est utilisée pour créer et déployer des actions personnalisées sur Adobe I/O Runtime, la plateforme sans serveur d’Adobe. AEM projets Asset compute sont des projets Firefly spécialement créés qui s’intègrent à AEM Assets via des profils de traitement et permettent d’accéder aux fichiers binaires de ressources et de les traiter.

Pour accéder à Project Firefly, inscrivez-vous à l’aperçu.

1. [Inscrivez-vous à l’aperçu de Project Firefly](https://adobeio.typeform.com/to/obqgRm).
1. Patientez environ 2 à 10 jours jusqu’à ce que vous soyez averti par e-mail que vous êtes configuré avant de poursuivre le tutoriel.
   + Si vous n’êtes pas certain d’avoir reçu les privilèges d’accès, passez aux étapes suivantes et si vous ne parvenez pas à créer un projet __Project Firefly__ dans [Adobe Developer Console](https://console.adobe.io) , vous n’avez toujours pas été configuré.

## Stockage dans le cloud

Le stockage dans le cloud est nécessaire pour le développement local de projets d’Asset compute.

Lorsque les objets Worker Asset compute sont déployés dans Adobe I/O Runtime pour une utilisation directe par AEM en tant que Cloud Service, ce stockage dans le cloud n’est pas strictement nécessaire, car AEM fournit l’espace de stockage dans le cloud à partir duquel la ressource est lue et le rendu est écrit.

### Stockage Microsoft Azure Blob{#azure-blob-storage}

Si vous n’avez pas déjà accès au stockage Blob Microsoft Azure, inscrivez-vous pour un [compte gratuit de 12 mois](https://azure.microsoft.com/en-us/free/).

Ce tutoriel utilisera Azure Blob Storage, mais [Amazon S3](#amazon-s3) ne peut être utilisé que pour des variations mineures du tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Clic publicitaire de la configuration du stockage Azure Blob (sans audio)_


1. Connectez-vous à votre [compte Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Accédez à la section __Comptes de stockage__ Services Azure
1. Appuyez sur __+ Ajouter__ pour créer un compte de stockage Blob.
1. Créez un nouveau __groupe de ressources__ selon les besoins, par exemple : `aem-as-a-cloud-service`
1. Indiquez un __nom du compte de stockage__, par exemple : `aemguideswkndassetcomput`
   + Le __nom du compte de stockage__ sera utilisé pour [la configuration de l’espace de stockage dans le cloud](../develop/environment-variables.md) pour l’outil de développement d’Asset compute local.
   + Les __clés d’accès__ associées au compte de stockage sont également requises lors de la [configuration de l’espace de stockage dans le cloud](../develop/environment-variables.md).
1. Laissez le reste comme valeur par défaut, puis appuyez sur le bouton __Réviser + créer__
   + Si vous le souhaitez, sélectionnez l’emplacement __à proximité.__
1. Vérifiez la demande d’approvisionnement pour vous assurer qu’elle est correcte, puis appuyez sur le bouton __Créer__ si vous le souhaitez.

### Amazon S3{#amazon-s3}

L’utilisation de [le stockage Blob Microsoft Azure](#azure-blob-storage) est recommandée pour suivre ce tutoriel, mais [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) peut également être utilisé.

Si vous utilisez le stockage Amazon S3, spécifiez les informations d’identification de stockage dans le cloud Amazon S3 lors de la [configuration des variables d’environnement du projet](../develop/environment-variables.md#amazon-s3).

Si vous devez configurer l’espace de stockage dans le cloud spécialement pour ce tutoriel, nous vous recommandons d’utiliser [Azure Blob Storage](#azure-blob-storage).
