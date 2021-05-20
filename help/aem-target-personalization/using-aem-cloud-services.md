---
title: Intégration d’Adobe Experience Manager à Adobe Target à l’aide de Cloud Services
seo-title: Intégration d’Adobe Experience Manager (AEM) à Adobe Target à l’aide de Cloud Services hérités
description: Présentation détaillée de l’intégration d’Adobe Experience Manager (AEM) à Adobe Target à l’aide d’AEM Cloud Service
seo-description: Présentation détaillée de l’intégration d’Adobe Experience Manager (AEM) à Adobe Target à l’aide d’AEM Cloud Service
feature: Fragments d’expérience
topic: Personnalisation
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---


# Utilisation AEM Cloud Services hérités

Dans cette section, nous allons discuter de la configuration d’Adobe Experience Manager (AEM) avec Adobe Target à l’aide de Cloud Services hérités.

>[!NOTE]
>
> Le Cloud Service hérité d’AEM avec Adobe Target est **utilisé uniquement** pour établir une connexion directe d’auteur AEM à l’arrière-plan Adobe Target, ce qui facilite la publication de contenu d’AEM vers Target. Adobe Launch est utilisé pour exposer Adobe Target sur l’expérience du site Web public diffusée par AEM.

Afin d’utiliser les offres de fragments d’expérience AEM pour alimenter vos activités de personnalisation, vous permet de passer au chapitre suivant et d’intégrer AEM à Adobe Target à l’aide des services cloud hérités. Cette intégration est nécessaire pour transmettre les fragments d’expérience d’AEM à Target sous forme d’offres HTML/JSON et pour que les offres cibles restent synchronisées avec AEM. Cette intégration est requise pour la mise en oeuvre de [Scénario 1 décrit dans la section d’aperçu](./overview.md#personalization-using-aem-experience-fragment).

## Prérequis

* **AEM **

   * AEM instances de création et de publication sont nécessaires pour suivre ce tutoriel. Si vous n’avez pas encore configuré votre instance AEM, vous pouvez suivre les étapes [ici](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Accès à vos organisations Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud fourni avec les solutions suivantes
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > Le client doit être configuré avec l’Experience Platform Launch et l’Adobe I/O à partir de la [prise en charge de l’Adobe](https://helpx.adobe.com/fr/contact/enterprise-support.ec.html) ou contacter votre administrateur système.



### Intégration d’AEM à Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Créer un Cloud Service Adobe Target à l’aide de l’authentification IMS Adobe (*Utilise l’API Adobe Target*) (00:34)
2. Obtention du code client Adobe Target (01:50)
3. Création d’une configuration IMS d’Adobe pour Adobe Target (02:08)
4. Création d’un compte technique pour l’accès à l’API Target dans Adobe I/O Console (02:08)
5. Ajout d’un Cloud Service Adobe Target à AEM des fragments d’expérience (04:12)

À ce stade, vous avez correctement intégré [AEM à Adobe Target à l’aide des Cloud Services hérités](./using-aem-cloud-services.md#integrating-aem-target-options) comme indiqué dans l’option 2. Vous devez maintenant pouvoir créer un fragment d’expérience dans AEM et publier le fragment d’expérience en tant qu’offre HTML ou offre JSON sur Adobe Target. Vous pouvez ensuite l’utiliser pour créer une activité.
