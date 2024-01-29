---
title: Configurer des comptes et services pour l’extensibilité d’Asset Compute
description: Le développement de programmes de travail Asset Compute nécessite l’accès à des comptes et à des services, y compris AEM as a Cloud Service, le Créateur d’applications et l’espace de stockage dans le cloud fourni par Microsoft ou Amazon.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 247
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '592'
ht-degree: 100%

---

# Configurer des comptes et des services

Ce tutoriel nécessite que les services suivants soient approvisionnés et accessibles via l’Adobe ID de la personne en apprentissage.

Tous les services Adobe doivent être accessibles via la même organisation Adobe, à l’aide de votre Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Créateur d’applications](#app-builder)
   + L’approvisionnement peut prendre entre 2 et 10 jours.
+ Espace de stockage dans le cloud
   + [Stockage Blob Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + ou [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Assurez-vous d’avoir accès à tous les services mentionnés ci-dessus avant de poursuivre ce tutoriel.
> 
> Consultez les sections ci-dessous pour savoir comment définir et approvisionner les services requis.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

L’accès à un environnement AEM as a Cloud Service est requis pour configurer les profils de traitement AEM Assets afin d’appeler le programme de travail Asset Compute personnalisé.

Idéalement, vous avez accès à un programme de sandbox ou un environnement de développement hors sandbox.

Notez qu’un SDK AEM local n’est pas suffisant pour compléter ce tutoriel, car le SDK AEM local ne peut pas communiquer avec les microservices Asset Compute. Il faut donc disposer d’un véritable environnement AEM as a Cloud Service.

## Créateur d’applications{#app-builder}

Le framework du [Créateur d’applications](https://developer.adobe.com/app-builder/) sert à créer et déployer des actions personnalisées sur Adobe I/O Runtime, la plateforme sans serveur d’Adobe. Les projets Asset Compute d’AEM sont des projets du Créateur d’applications spécialement créés qui s’intègrent à AEM Assets via des profils de traitement et permettent d’accéder aux fichiers binaires de ressources et de les traiter.

Pour accéder au Créateur d’applications, inscrivez-vous obtenir une prévisualisation.

1. [Inscrivez-vous à la version d’essai du Créateur d’applications](https://developer.adobe.com/app-builder/trial/).
1. Patientez environ 2 à 10 jours jusqu’à la réception d’un e-mail vous avertissant de votre approvisionnement avant de poursuivre le tutoriel.
   + Si vous n’avez pas la certitude d’avoir reçu cet approvisionnement, passez aux étapes suivantes. Si vous ne pouvez pas créer un projet avec le __Créateur d’applications__ dans l’[Adobe Developer Console](https://developer.adobe.com/console/), c’est que vous n’avez pas encore reçu l’approvisionnement.

## Espace de stockage dans le cloud

L’espace de stockage dans le cloud est nécessaire pour le développement local de projets Asset Compute.

Lorsque les programmes de travail Asset Compute sont déployés dans Adobe I/O Runtime pour une utilisation directe par AEM as a Cloud Service, cet espace de stockage dans le cloud n’est pas strictement requis, car AEM fournit l’espace de stockage dans le cloud à partir duquel la ressource est lue et vers lequel le rendu est écrit.

### Microsoft Azure Blob Storage{#azure-blob-storage}

Si vous n’avez pas encore accès au stockage Blob Microsoft Azure, inscrivez-vous pour un [compte gratuit de 12 mois](https://azure.microsoft.com/fr-fr/free/).

Ce tutoriel utilise le stockage Blob Azure. Toutefois, vous pouvez utiliser [Amazon S3](#amazon-s3), ce qui n’entraîne que des variations mineures du tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Procédure de configuration du stockage Blob Azure (sans audio)_

1. Connectez-vous à votre [compte Microsoft Azure](https://azure.microsoft.com/fr-fr/account/).
1. Accédez à la section __Comptes de stockage__ des services Azure.
1. Appuyez sur __+ Ajouter__ pour créer un compte de stockage Blob.
1. Créez un __Groupe de ressources__ selon les besoins, par exemple : `aem-as-a-cloud-service`.
1. Fournissez un __Nom de compte de stockage__, par exemple : `aemguideswkndassetcomput`.
   + __Nom du compte de stockage__ utilisé pour [configurer l’espace de stockage dans le cloud](../develop/environment-variables.md) dans l’outil de développement Asset Compute local.
   + Les __Clés d’accès__ associées au compte de stockage sont également requises lors de la [configuration de l’espace de stockage dans le cloud](../develop/environment-variables.md).
1. Laissez les autres valeurs par défaut, puis appuyez sur le bouton __Réviser + créer__.
   + Si vous le souhaitez, sélectionnez l’__emplacement__ proche de vous.
1. Vérifiez que la demande d’approvisionnement est correcte, puis appuyez sur le bouton __Créer__.

### Amazon S3{#amazon-s3}

L’utilisation du [stockage Blob Microsoft Azure](#azure-blob-storage) est recommandée pour suivre ce tutoriel, mais [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) peut également être utilisé.

Si vous utilisez le stockage Amazon S3, spécifiez les informations d’identification de l’espace de stockage dans le cloud Amazon S3 lors de la [configuration des variables d’environnement du projet](../develop/environment-variables.md#amazon-s3).

Si vous devez configurer l’espace de stockage dans le cloud spécialement pour ce tutoriel, nous vous recommandons d’utiliser le [stockage Blob Azure](#azure-blob-storage).
