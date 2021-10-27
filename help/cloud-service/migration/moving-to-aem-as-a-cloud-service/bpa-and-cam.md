---
title: Configuration de votre projet BPA et CAM
description: Découvrez comment Best Practice Analyzer et Cloud Acceleration Manager fournissent un guide personnalisé pour la migration vers AEM as a Cloud Service.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 3%

---

# Analyseur des bonnes pratiques et Cloud Accelerated Manager

Découvrez comment BPA (Best Practice Analyzer) et Cloud Acceleration Manager (CAM) fournissent un guide personnalisé pour la migration vers AEM as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Évaluation de la préparation

![Diagramme de haut niveau BPA et CAM](assets/bpa-cam-diagram.png)

Le package BPA doit être installé sur un clone de l’environnement de production AEM 6.x. Le BPA va générer un rapport qui pourra ensuite être téléchargé dans le CAM, qui fournira des conseils sur les activités clés qui doivent avoir lieu afin de passer à AEM as a Cloud Service.

### Principales activités

* Effectuez un clone de votre environnement de production 6.x. Lorsque vous migrez du contenu et refactorisez le code, disposer d’un clone d’environnement de production sera utile pour tester divers outils et modifications.
* Téléchargez la dernière version de l’outil BPA à partir du [Portail de distribution de logiciels](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) et installez sur votre environnement cloné AEM 6.x.
* Utilisez l’outil BPA pour générer un rapport qui peut être téléchargé vers Cloud Acceleration Manager (CAM). Le mode d’accès CAM est accessible via [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Accelerated Manager**.
* Utilisez le format CAM pour fournir des conseils sur les mises à jour à apporter à la base de code et à l’environnement actuels afin de passer à AEM as a Cloud Service.
