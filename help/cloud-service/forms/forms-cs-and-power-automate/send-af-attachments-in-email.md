---
title: Envoyer des pièces jointes de formulaire dans un e-mail
description: Extraire et envoyer des pièces jointes de formulaire envoyées dans un e-mail à l’aide du workflow Power Automate
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="AEM Forms Cloud Service" before-title="false"
source-git-commit: ed64dd303a384d48f76c9b8e8e925f5d3b8f3247
workflow-type: tm+mt
source-wordcount: '76'
ht-degree: 94%

---

# Extraire des pièces jointes de formulaire des données envoyées

Extrayez des pièces jointes des formulaires et envoyez les pièces jointes dans un e-mail dans le workflow Power Automate.
La vidéo suivante explique les étapes nécessaires pour former des pièces jointes à partir des données envoyées.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

Voici le schéma d’objet de pièce jointe que vous devez utiliser lors de l’étape de schéma Parse JSON.

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
