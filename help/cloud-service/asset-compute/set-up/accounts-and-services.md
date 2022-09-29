---
title: Configuration de comptes et de services pour l’extensibilité des Assets compute
description: Le développement de travailleurs Asset compute nécessite l’accès à des comptes et à des services, y compris AEM as a Cloud Service, App Builder et le stockage dans le cloud fourni par Microsoft ou Amazon.
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 3%

---

# Configuration des comptes et des services

Ce tutoriel nécessite que les services suivants soient mis en service et accessibles via Adobe ID de l’apprenant.

Tous les services Adobe doivent être accessibles via la même organisation Adobe, à l’aide de votre Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + La mise en service peut prendre entre 2 et 10 jours
+ Stockage dans le cloud
   + [Stockage Azure Blob](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + ou [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Assurez-vous d’avoir accès à tous les services mentionnés ci-dessus avant de poursuivre ce tutoriel.
> 
> Consultez les sections ci-dessous pour savoir comment définir et fournir les services requis.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

L’accès à un environnement as a Cloud Service AEM est requis pour configurer les profils de traitement AEM Assets afin d’appeler le programme de travail d’Asset compute personnalisé.

Idéalement, un programme Sandbox ou un environnement de développement non Sandbox peut être utilisé.

Notez qu’un SDK d’AEM local n’est pas suffisant pour terminer ce tutoriel, car le SDK AEM local ne peut pas communiquer avec les microservices d’Asset compute, mais un véritable environnement as a Cloud Service est requis.

## App Builder{#app-builder}

Le [App Builder](https://developer.adobe.com/app-builder/) est utilisé pour créer et déployer des actions personnalisées sur Adobe I/O Runtime, la plateforme sans serveur d’Adobe. AEM projets d’Asset compute sont des projets App Builder spécialement créés qui s’intègrent à AEM Assets via des profils de traitement et permettent d’accéder aux fichiers binaires de ressources et de les traiter.

Pour accéder à App Builder, inscrivez-vous à l’aperçu.

1. [Inscrivez-vous à la version d’évaluation d’App Builder](https://developer.adobe.com/app-builder/trial/).
1. Patientez environ 2 à 10 jours jusqu’à ce que vous soyez averti par e-mail que vous êtes configuré avant de poursuivre le tutoriel.
   + Si vous n’êtes pas sûr d’avoir reçu les privilèges d’accès, passez aux étapes suivantes et si vous ne pouvez pas créer un __App Builder__ project in [Console Adobe Developer](https://developer.adobe.com/console/) vous n’avez toujours pas reçu les privilèges d’accès.

## Stockage dans le cloud

Le stockage dans le cloud est nécessaire pour le développement local de projets d’Asset compute.

Lorsque les objets Worker Asset compute sont déployés dans Adobe I/O Runtime pour une utilisation directe par AEM as a Cloud Service, cet espace de stockage dans le cloud n’est pas strictement requis, car AEM fournit l’espace de stockage dans le cloud à partir duquel la ressource est lue et le rendu écrit.

### Stockage Microsoft Azure Blob{#azure-blob-storage}

Si vous n’avez pas déjà accès au stockage Blob de Microsoft Azure, inscrivez-vous à un [compte gratuit de 12 mois](https://azure.microsoft.com/en-us/free/).

Ce tutoriel utilisera Azure Blob Storage, cependant. [Amazon S3](#amazon-s3) peut être utilisé, ainsi que des variations mineures du tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Clic publicitaire de la configuration du stockage Azure Blob (sans audio)_

1. Connectez-vous à [Compte Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Accédez au __Comptes de stockage__ Section des services Azure
1. Appuyer __+ Ajouter__ pour créer un compte de stockage Blob
1. Créer __Groupe de ressources__ selon les besoins, par exemple : `aem-as-a-cloud-service`
1. Fournissez une __Nom du compte de stockage__, par exemple : `aemguideswkndassetcomput`
   + Le __Nom du compte de stockage__  utilisé pour [configuration de l’espace de stockage dans le cloud](../develop/environment-variables.md) dans l’outil de développement d’Asset compute local
   + Le __Clés d’accès__ associés au compte de stockage sont également requis lorsque [configuration de l’espace de stockage dans le cloud](../develop/environment-variables.md).
1. Laissez le reste comme valeur par défaut, puis appuyez sur __Réviser + créer__ button
   + Si vous le souhaitez, sélectionnez la variable __location__ proche de toi.
1. Vérifiez la demande d’approvisionnement pour vous assurer qu’elle est correcte, puis appuyez sur __Créer__ si satifié

### Amazon S3{#amazon-s3}

Utilisation [Stockage Azure Blob de Microsoft](#azure-blob-storage) est recommandé pour suivre ce tutoriel, mais [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) peut également être utilisé.

Si vous utilisez le stockage Amazon S3, spécifiez les informations d’identification de l’espace de stockage dans le cloud Amazon S3 lors de la [configuration des variables d’environnement du projet](../develop/environment-variables.md#amazon-s3).

Si vous devez configurer l’espace de stockage dans le cloud spécialement pour ce tutoriel, nous vous recommandons d’utiliser [Stockage Azure Blob](#azure-blob-storage).
