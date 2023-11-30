---
title: Transmettre la configuration des services cloud et le modèle de données de formulaire vers l’instance cloud
description: Créez et transmettez un formulaire adaptatif basé sur le modèle de données de formulaire de stockage Azure vers l’instance cloud.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 100%

---

# Inclure la configuration des services cloud dans votre projet

Créez un conteneur de configuration appelé « FormTutorial » qui contiendra votre configuration de services cloud.
Créez une configuration de services cloud pour Azure Storage appelée « FormsCSAndAzureBlob » dans le conteneur « FormTutorial » en fournissant les détails du compte de stockage Azure et la clé d’accès Azure.

Ouvrez votre projet AEM dans IntelliJ. Ajoutez le dossier FormTutorial dans le projet ui.content comme illustré ci-dessous.
![cloud-services-configuration](assets/cloud-services-configuration.png)

Ajoutez l’entrée suivante dans le fichier filter.xml du projet ui.content.

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Inclure le modèle de données de formulaire dans votre projet

Créez un modèle de données de formulaire basé sur la configuration des services cloud que vous avez créée à l’étape précédente. Pour inclure le modèle de données de formulaire dans votre projet, créez la structure de dossiers appropriée dans votre projet AEM dans intelliJ. Par exemple, mon modèle de données de formulaire se trouve dans un dossier appelé « registrations ».
![fdm-content](assets/ui-content-fdm.png)

Inclure l’entrée appropriée dans le fichier filter.xml du projet ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Désormais, lorsque vous créez et déployez votre projet à l’aide de Cloud Manager, vous devrez saisir à nouveau votre clé d’accès Azure dans la configuration des services cloud. Pour éviter de saisir à nouveau la clé d’accès, il est recommandé de créer une configuration basée sur le contexte à l’aide des variables d’environnement, comme expliqué dans l’[article suivant](./context-aware-fdm.md).

## Étapes suivantes

[Créer une configuration basée sur le contexte](./context-aware-fdm.md)
