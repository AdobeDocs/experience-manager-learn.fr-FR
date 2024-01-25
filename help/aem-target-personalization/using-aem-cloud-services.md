---
title: Intégrer Adobe Experience Manager à Adobe Target à l’aide des services cloud
description: Procédure d’intégration d’Adobe Experience Manager (AEM) à Adobe Target à l’aide d’AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 346
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 100%

---

# Utiliser les services cloud hérités d’AEM

Cette section est consacrée à la configuration d’Adobe Experience Manager (AEM) avec Adobe Target à l’aide des services cloud hérités.

>[!NOTE]
>
> L’utilisation conjointe du service cloud hérité d’AEM et d’Adobe Target vise **uniquement** à établir une connexion directe entre l’instance de création AEM et le serveur principal Adobe Target, à des fins de publication de contenu plus facile d’AEM vers Target. Adobe Launch permet d’afficher le contenu Adobe Target sur le site web accessible au public, qui repose sur AEM.

Dans le chapitre suivant, vous allez découvrir comment tirer parti des offres de fragments d’expérience AEM pour optimiser vos activités de personnalisation, puis intégrer AEM à Adobe Target à l’aide des services cloud hérités. Cette intégration est requise pour transmettre les fragments d’expérience d’AEM à Target en tant qu’offres HTML/JSON et pour que les offres cibles restent synchronisées avec AEM. Cette intégration est requise pour implémenter le [Scénario 1 abordé lors de la vue d’ensemble](./overview.md#personalization-using-aem-experience-fragment).

## Conditions préalables

* **AEM**

   * Vous devez disposer d’une instance de création et de publication AEM avant de suivre ce tutoriel. Si vous n’avez pas encore configuré votre instance AEM, suivez les étapes [ici](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Accéder à la solution Adobe Experience Cloud de votre organisation - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud est fourni avec les solutions suivantes
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > La personne cliente doit être configurée dans Experience Platform Launch et Adobe I/O par le [support Adobe](https://helpx.adobe.com/fr/contact/enterprise-support.ec.html). Contactez l’administration système le cas échéant.

### Intégrer AEM à Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Création d’un service cloud Adobe Target à l’aide de l’authentification Adobe IMS (*utilise l’API Adobe Target*) (00:34)
2. Obtention du code client Adobe Target (01:50)
3. Création d’une configuration Adobe IMS pour Adobe Target (02:08)
4. Création d’un compte technique pour l’accès à l’API Target dans la console Adobe I/O (02:08)
5. Ajout du service cloud Adobe Target aux fragments d’expérience AEM (04:12)

À ce stade, vous avez correctement intégré [AEM à Adobe Target à l’aide des services cloud hérités](./using-aem-cloud-services.md#integrating-aem-target-options), comme indiqué à l’option 2. Vous pouvez maintenant créer un fragment d’expérience dans AEM et le publier en tant qu’offre HTML ou JSON sur Adobe Target. Utilisez-le ensuite pour créer une activité.
