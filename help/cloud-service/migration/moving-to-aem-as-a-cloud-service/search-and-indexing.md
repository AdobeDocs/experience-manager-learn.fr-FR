---
title: Recherche et indexation dans AEM as a Cloud Service
description: Découvrez les index de recherche d’AEM as a Cloud Service, comment convertir AEM définitions d’index 6 et comment déployer des index.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 3%

---

# Recherche et indexation

Découvrez les index de recherche d’AEM as a Cloud Service, comment convertir AEM 6 définitions d’index pour qu’elles soient compatibles avec l’as a Cloud Service et comment déployer les index vers l’as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Outil convertisseur d’index

![Outil convertisseur d’index](./assets/index-converter.png)

Dans le cadre de la refactorisation de votre base de code, utilisez le [Outil convertisseur d’index](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) pour convertir des définitions d’index Oak personnalisées en AEM de définitions d’index compatibles as a Cloud Service.

### Principales activités

* Utilisez la variable [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) pour migrer les workflows de traitement des ressources afin d’utiliser les microservices Asset compute.
* Configurez une [environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr) et déployez les index personnalisés. Assurez-vous que les index mis à jour sont à jour.
* Déployez la base de code mise à jour dans un environnement de développement as a Cloud Service AEM et continuez à valider.
* Si vous modifiez un index prêt à l’emploi **TOUJOURS** copiez la dernière définition d’index à partir d’un environnement as a Cloud Service AEM s’exécutant sur la dernière version. Modifiez la définition d’index copiée selon vos besoins.