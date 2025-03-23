---
title: Exécuter la configuration par lot
description: Lancez le processus de génération de document en exécutant le lot.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 100%

---

# Exécuter la configuration par lot

Pour exécuter le lot, envoyez une requête POST à l’API suivante.

```xml
<baseURL>/confi/<configName>/execution
```

Cette API exige un objet JSON vide comme paramètre dans le corps de la requête.
L’API renvoie une URL unique dans l’en-tête de réponse, identifiée par la clé **location**.
Une requête GET à cette URL unique vous indique le statut d’exécution du lot.

La vidéo suivante montre le déclenchement de la configuration par lot.

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
