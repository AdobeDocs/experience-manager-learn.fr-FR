---
title: Configurer des comptes et des services pour l'extensibilité des Assets compute
description: Les travailleurs en développement d'Assets compute ont besoin d'avoir accès à des comptes et à des services, y compris AEM en tant que Cloud Service, Adobe Project Firefly et enregistrement cloud fourni par Microsoft ou Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 2%

---


# Configurer des comptes et des services

Ce didacticiel nécessite que les services suivants soient fournis et accessibles via l’Adobe ID de l’apprenant.

Tous les services d&#39;Adobe doivent être accessibles via le même Adobe Org, en utilisant votre Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe Projet FireFly](#adobe-project-firefly)
   + La mise en service peut prendre entre 2 et 10 jours
+ Enregistrement cloud
   + [Enregistrement Blob Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + ou [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Assurez-vous d’avoir accès à tous les services mentionnés ci-dessus avant de continuer à utiliser ce didacticiel.
> 
> Consultez les sections ci-dessous sur la façon de définir et de fournir les services requis.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

L’accès à un AEM en tant qu’environnement Cloud Service est requis pour configurer les Profils de traitement AEM Assets afin d’appeler le travailleur d’Asset compute personnalisé.

Idéalement, un programme sandbox ou un environnement de développement non sandbox peut être utilisé.

Notez qu’un SDK AEM local n’est pas suffisant pour compléter ce didacticiel, car le SDK AEM local ne peut pas communiquer avec les microservices d’Asset compute, au lieu de cela, un véritable  en tant qu’environnement Cloud Service est nécessaire.

## Adobe Projet Firefly{#adobe-project-firefly}

La structure [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) est utilisée pour créer et déployer des actions personnalisées sur Adobe I/O Runtime, la plate-forme sans serveur de l&#39;Adobe. Les projets d’Asset compute AEM sont des projets Firefly spécialement conçus qui s’intègrent à AEM Assets via des Profils de traitement et qui permettent d’accéder aux fichiers binaires et de les traiter.

Pour accéder au projet Firefly, inscrivez-vous à la prévisualisation.

1. [Inscrivez-vous à la prévisualisation](https://adobeio.typeform.com/to/obqgRm) Project Firefly.
1. Patientez environ 2 à 10 jours jusqu&#39;à ce que vous soyez averti par e-mail que vous êtes configuré avant de poursuivre le tutoriel.
   + Si vous n’êtes pas certain d’avoir été configuré, passez à la procédure suivante et si vous ne pouvez pas créer un projet __Project Firefly__ dans [Adobe Developer Console](https://console.adobe.io), vous n’avez toujours pas été configuré.

## Enregistrement cloud

L&#39;enregistrement du nuage est nécessaire pour le développement local de projets d&#39;Asset compute.

Lorsque des employés d’Asset compute sont déployés dans le Adobe I/O Runtime pour une utilisation directe par AEM en tant que Cloud Service, cet enregistrement de cloud n’est pas strictement requis car AEM fournit l’enregistrement de cloud à partir duquel la ressource est lue et le rendu écrit.

### Stockage Microsoft Azure Blob{#azure-blob-storage}

Si vous n&#39;avez pas encore accès à l&#39;Enregistrement Microsoft Azure Blob, inscrivez-vous pour un [compte gratuit de 12 mois](https://azure.microsoft.com/en-us/free/).

Ce tutoriel utilisera l&#39;Enregistrement Azure Blob, cependant [Amazon S3](#amazon-s3) peut être utilisé, ainsi que des variantes mineures du tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Clic publicitaire de l&#39;Enregistrement Azure Blob de mise en service (sans audio)_


1. Connectez-vous à votre [compte Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Accédez à la section __Comptes d&#39;Enregistrement__ Services Azure.
1. Appuyez sur __+ Ajoute__ pour créer un compte d’Enregistrement Blob.
1. Créez un groupe de ressources ____ selon les besoins, par exemple : `aem-as-a-cloud-service`
1. Indiquez un __nom de compte d’Enregistrement__, par exemple : `aemguideswkndassetcomput`
   + Le __nom du compte d&#39;Enregistrement__ sera utilisé pour [configurer l&#39;enregistrement cloud](../develop/environment-variables.md) pour l&#39;outil de développement d&#39;Asset compute local
   + Les __clés d&#39;accès__ associées au compte d&#39;enregistrement sont également requises lors de la configuration de l&#39;enregistrement cloud [.](../develop/environment-variables.md)
1. Laissez le reste comme valeur par défaut et appuyez sur le bouton __Réviser + créer__.
   + Si vous le souhaitez, sélectionnez l&#39;emplacement __situé__ à proximité de vous.
1. Vérifiez la demande de mise en service pour vous assurer qu’elle est correcte, puis appuyez sur le bouton __Créer__ s’il est satisfait.

### Amazon S3{#amazon-s3}

Il est recommandé d&#39;utiliser [l&#39;Enregistrement Blob Microsoft Azure](#azure-blob-storage) pour compléter ce didacticiel, mais [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) peut également être utilisé.

Si vous utilisez l’enregistrement Amazon S3, spécifiez les informations d’identification de l’enregistrement cloud Amazon S3 lorsque [vous configurez les variables d’environnement du projet](../develop/environment-variables.md#amazon-s3).

Si vous devez configurer l&#39;enregistrement cloud spécialement pour ce didacticiel, nous vous recommandons d&#39;utiliser [Azure Blob Enregistrement](#azure-blob-storage).
