---
title: Filigranes en AEM Assets
description: aem en tant que Cloud Service, les fonctions de filigrane permettent aux rendus d’image personnalisés d’être mis en filigrane à l’aide de n’importe quelle image PNG.
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# Filigranes

aem en tant que Cloud Service, les fonctions de filigrane permettent aux rendus d’image personnalisés d’être mis en filigrane à l’aide de n’importe quelle image PNG.

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
