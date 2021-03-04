---
title: Filigranes en AEM Assets
description: AEM en tant que Cloud Service, les fonctions de filigrane permettent aux rendus d’image personnalisés d’être mis en filigrane à l’aide de n’importe quelle image PNG.
feature: Microservices Asset compute
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Gestion de contenu
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 4%

---


# Filigranes

AEM en tant que Cloud Service, les fonctions de filigrane permettent aux rendus d’image personnalisés d’être mis en filigrane à l’aide de n’importe quelle image PNG.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## Configuration OSGi

Le stub de configuration OSGi suivant peut être mis à jour et ajouté au sous-projet `ui.config` de votre projet AEM.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
