---
title: Créer un service cloud de configuration cloud Acrobat Sign
description: Créez l’intégration d’AEM Forms et d’Acrobat Sign à l’aide de la configuration des services cloud.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="dʼAEM Forms as a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
duration: 225
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 100%

---

# Créer une configuration cloud Acrobat Sign

La configuration des services cloud dans AEM vous permet de créer une intégration entre AEM et d’autres applications cloud.

La vidéo suivante décrit les étapes nécessaires à la création d’une configuration de services cloud pour intégrer AEM à Acrobat Sign.

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Résolution des problèmes

Si vous obtenez une erreur lors de la configuration de la configuration cloud d’Abobe Sign, les étapes suivantes peuvent vous permettre de résoudre le problème.
* Assurez-vous que l’URL de redirection spécifiée dans l’application d’API Acrobat Sign est au format suivant
&lt;your instance name>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Par exemple : https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS est le nom du conteneur qui va contenir la configuration cloud.
* Vérifiez que l’URL oAuth est correcte.
* Vérifiez votre ID client et votre secret client.
* Essayer le mode fenêtre incognito

