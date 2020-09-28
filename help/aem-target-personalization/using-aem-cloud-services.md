---
title: Intégration de Adobe Experience Manager avec Adobe Target à l'aide de Cloud Services
seo-title: Intégration de Adobe Experience Manager (AEM) avec Adobe Target à l’aide de Cloud Services hérités
description: Procédure pas à pas pour intégrer Adobe Experience Manager (AEM) à Adobe Target à l’aide d’un Cloud Service AEM
seo-description: Procédure pas à pas pour intégrer Adobe Experience Manager (AEM) à Adobe Target à l’aide d’un Cloud Service AEM
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---


# Utilisation de Cloud Services hérités AEM

Dans cette section, nous aborderons comment configurer Adobe Experience Manager (AEM) avec Adobe Target en utilisant des Cloud Services hérités.

>[!NOTE]
>
> L’Cloud Service hérité AEM avec Adobe Target est **uniquement** utilisé pour établir une connexion directe entre l’auteur AEM et l’arrière-plan Adobe Target permettant la publication de contenu de l’AEM à la Cible. Le lancement d&#39;Adobe est utilisé pour exposer l&#39;Adobe Target au public face à l&#39;expérience du site Web fournie par AEM.

Afin d’utiliser les offres AEM Fragment d’expérience pour alimenter vos activités de personnalisation, vous permet de passer au chapitre suivant et d’intégrer AEM à Adobe Target à l’aide des services cloud hérités. Cette intégration est nécessaire pour transmettre les fragments d’expérience d’AEM à la Cible en tant qu’offres HTML/JSON et pour maintenir les offres de cible synchronisées avec AEM. Cette intégration est requise pour la mise en oeuvre du [scénario 1, dont il est question dans la section](./overview.md#personalization-using-aem-experience-fragment)d’aperçu.

## Conditions préalables

* **AEM**

   * aem instance d’auteur et de publication sont nécessaires pour terminer ce didacticiel. Si vous n’avez pas encore configuré votre instance AEM, vous pouvez suivre les étapes [ici](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Accès à vos organisations Adobe Experience Cloud - <https://>`<yourcompany>`.experience ecloud.adobe.com
   * experience cloud doté des solutions suivantes
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > Le client doit recevoir les E/S Experience Platform Launch et Adobe de la prise en charge [des](https://helpx.adobe.com/fr/contact/enterprise-support.ec.html) Adobes ou contacter votre administrateur système.



### Intégration des AEM avec Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Création d’un Cloud Service Adobe Target à l’aide de l’authentification IMS par Adobe (*utilise l’API* Adobe Target) (00:34)
2. Obtention du code client Adobe Target (01:50)
3. Créer une configuration Adobe IMS pour Adobe Target (02:08)
4. Création d&#39;un compte technique pour l&#39;accès à l&#39;API de Cible dans la console d&#39;E/S d&#39;Adobe (02:08)
5. ajouter l’Cloud Service Adobe Target aux fragments d’expérience AEM (04:12)

À ce stade, vous avez réussi à intégrer [AEM avec Adobe Target à l&#39;aide de Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) hérités, comme indiqué dans l&#39;option 2. Vous devez maintenant être en mesure de créer un fragment d’expérience dans AEM et de publier le fragment d’expérience en tant qu’offre HTML ou Offre JSON sur Adobe Target, puis de l’utiliser pour créer une activité.
