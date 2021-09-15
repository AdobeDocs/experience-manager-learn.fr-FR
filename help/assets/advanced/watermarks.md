---
title: Filigranes dans AEM Assets
description: Les fonctionnalités de filigrane d’AEM en tant que Cloud Service permettent aux rendus d’image personnalisés d’être mis en filigrane à l’aide de n’importe quelle image PNG.
feature: Asset Compute Microservices
version: Cloud Service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
exl-id: 252c7c58-3567-440a-a1d5-19c598b6788e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---

# Filigranes

Les fonctionnalités de filigrane d’AEM en tant que Cloud Service permettent aux rendus d’image personnalisés d’être mis en filigrane à l’aide de n’importe quelle image PNG.

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
