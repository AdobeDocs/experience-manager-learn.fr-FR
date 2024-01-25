---
title: Configurer des données par lot
description: Configurer des données par lot
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 237
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 100%

---

# Créer une configuration par lot

Pour utiliser une API Batch, créez une configuration par lot et lancez une exécution basée sur cette configuration. La vidéo suivante illustre la création d’une configuration par lot à l’aide de l’API.

>[!NOTE]
>Assurez-vous que l’utilisateur ou l’utilisatrice AEM appartient au groupe ```forms-users``` pour effectuer des appels d’API.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## Créer une configuration par lot

Voici le point d’entrée POST pour la création d’une configuration par lot.

```xml
<baseURL>/config
```

Voici la configuration minimale qui doit être spécifiée lors de la création de la configuration par lot. Elle doit être transmise en tant qu’objet JSON dans le corps de la requête HTTP.

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

## Vérifier la configuration par lot

Pour vérifier la création corecte de la configuration par lot, vous pouvez lancer une requête GET au point d’entrée suivant.


```xml
<baseURL>/config/monthlystatements
```

Il suffit de transmettre un objet JSON vide dans le corps de la requête HTTP.
