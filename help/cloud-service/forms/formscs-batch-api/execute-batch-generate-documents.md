---
title: Exécution de la configuration du lot
description: Lancez le processus de génération de document en exécutant le lot
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# Exécution de la configuration par lots

Pour exécuter le lot, envoyez une requête de POST à l’API suivante

```xml
<baseURL>/confi/<configName>/execution
```

Cette API exige un objet json vide comme paramètre dans le corps de la requête.
Cette API renvoie une URL unique dans l’en-tête de réponse identifié par **location** clé.
Une demande de GET à cette URL unique vous indique l’état de l’exécution du lot.

La vidéo suivante montre le déclenchement de la configuration du lot

>[!VIDEO](https://video.tv.adobe.com/v/340242/?quality=12&learn=on)
