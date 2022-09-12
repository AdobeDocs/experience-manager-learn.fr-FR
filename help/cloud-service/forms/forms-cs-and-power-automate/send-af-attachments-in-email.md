---
title: Envoi de pièces jointes de formulaire dans un courrier électronique
description: Extraire et envoyer les pièces jointes de formulaire envoyées dans un email à l’aide de l’automatisation du processus
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: aea43a705b3959f8be26238d32b816b3953e153e
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Extraire les pièces jointes de formulaire des données de formulaire envoyées

Extrayez les pièces jointes des formulaires et envoyez les pièces jointes dans un email dans le workflow d’automatisation de l’alimentation.
La vidéo suivante explique les étapes nécessaires pour former des pièces jointes à partir des données envoyées.
>[!VIDEO](https://video.tv.adobe.com/v/3409017/?quality=12&learn=on)

Voici le schéma d’objet de pièce jointe que vous devez utiliser à l’étape Parse JSON schema .

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
