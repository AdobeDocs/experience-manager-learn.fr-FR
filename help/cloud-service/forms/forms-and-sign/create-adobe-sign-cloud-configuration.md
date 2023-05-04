---
title: Création d’un Cloud Service de configuration Acrobat Sign Cloud
description: Créez l’intégration d’AEM Forms et d’Acrobat Sign à l’aide de la configuration des services cloud.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 7428
thumbnail: 332437.jpg
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 3%

---

# Créer une configuration cloud Acrobat Sign

La configuration des services cloud dans AEM vous permet de créer une intégration entre AEM et d’autres applications cloud.

La vidéo suivante décrit les étapes nécessaires à la création d’une configuration de services cloud pour intégrer AEM à Acrobat Sign

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Résolution des problèmes

Si vous obtenez une erreur lors de la configuration de la configuration du cloud Abobe Sign, les étapes suivantes peuvent être entreprises pour vous empêcher de tirer le meilleur parti.
* Assurez-vous que l’URL de redirection spécifiée dans l’application API Acrobat Sign est au format suivant
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Par exemple : https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS est le nom du conteneur qui va contenir la configuration de cloud.
* Assurez-vous que l’URL oAuth est correcte
* Vérifiez votre ID de client et votre secret de client.
* Essayer le mode de fenêtre incognito

