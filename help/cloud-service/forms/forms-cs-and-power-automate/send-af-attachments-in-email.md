---
title: Envoyer des pièces jointes de formulaire dans un e-mail
description: Extraire et envoyer des pièces jointes de formulaire envoyées dans un e-mail à l’aide du workflow Power Automate
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '78'
ht-degree: 100%

---

# Extraire des pièces jointes de formulaire des données envoyées

Extrayez des pièces jointes des formulaires et envoyez les pièces jointes dans un e-mail dans le workflow Power Automate.
La vidéo suivante explique les étapes nécessaires pour former des pièces jointes à partir des données envoyées.
>[!VIDEO](https://video.tv.adobe.com/v/3413028?quality=12&learn=on&captions=fre_fr)

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
