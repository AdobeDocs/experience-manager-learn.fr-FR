---
title: Configuration des données par lots
description: Configuration des données par lots
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 12%

---

# Création d’une configuration par lots

Pour utiliser une API de lot, créez une configuration de lot et exécutez une exécution basée sur cette configuration. La vidéo suivante présente une démonstration de la création d’une configuration par lots à l’aide de l’API

>[!NOTE]
>Assurez-vous que l’utilisateur AEM appartient à ```forms-users``` groupe pour effectuer des appels API.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## Création d’une configuration de lot

Voici le point de terminaison du POST pour la création d’une configuration par lots.

```xml
<baseURL>/config
```

Voici la configuration minimale à spécifier lors de la création de la configuration par lots. Elle doit être transmise en tant qu’objet JSON dans le corps de la requête HTTP

```
{
	"configName": "monthlystatements",
	"dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
	"outputTypes": [
		"PDF"
	],
	"template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## Vérifier la configuration du lot

Pour vérifier la création réussie de la configuration par lots, vous pouvez effectuer un appel de demande de GET vers le point de terminaison suivant.


```xml
<baseURL>/config/monthlystatements
```

Il vous suffit de transmettre un objet JSON vide dans le corps de la requête HTTP.
