---
title: Microservices AEM Assets et passage à AEM as a Cloud Service
description: Découvrez comment les microservices d’asset compute d’AEM Assets as a Cloud Service vous permettent de générer automatiquement et efficacement n’importe quel rendu pour vos ressources, en remplaçant ce rôle de workflow d’AEM traditionnel.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 8%

---

# Microservices AEM Assets - Passage à AEM as a Cloud Service

Découvrez comment les microservices d’asset compute d’AEM Assets as a Cloud Service vous permettent de générer automatiquement et efficacement n’importe quel rendu pour vos ressources, en remplaçant ce rôle de workflow d’AEM traditionnel.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Outil de migration des workflows

![Outil de migration des workflows de ressources](./assets/asset-workflow-migration.png)

Dans le cadre de la refactorisation de votre base de code, utilisez le [Outil de migration des workflows de ressources](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=fr) pour migrer les workflows existants afin d&#39;utiliser les microservices Asset compute dans AEM as a Cloud Service.

### Principales activités

* Utilisez la variable [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) pour migrer les workflows de traitement des ressources afin d’utiliser les microservices Asset compute.
* Configurez une [environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr) et déployer les workflows mis à jour. Un ajustement manuel peut être nécessaire pour les workflows complexes.
* Continuez à itérer dans un environnement de développement local à l’aide du SDK AEM jusqu’à ce que le workflow mis à jour corresponde à la parité des fonctionnalités.
* Déployez la base de code mise à jour dans un environnement de développement as a Cloud Service AEM et continuez à valider.

