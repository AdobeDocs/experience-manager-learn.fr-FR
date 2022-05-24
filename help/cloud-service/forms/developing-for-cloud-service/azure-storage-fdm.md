---
title: Pousser la configuration des services cloud et le modèle de données de formulaire vers l’instance cloud
description: Créez et poussez un formulaire adaptatif basé sur le modèle de données de formulaire de stockage Azure vers l’instance cloud.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 2ac0f6b3964590e5443700f730a3fc02cb3f63bc
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Inclure la configuration des services cloud dans votre projet

Créez un conteneur de configuration appelé &quot;FormTutorial&quot; pour contenir votre configuration de services cloud Créez une configuration de services cloud pour Azure Storage appelée &quot;FormsCSAndAzureBlob&quot; dans le conteneur &quot;FormTutorial&quot; en fournissant les détails du compte de stockage Azure et la clé d’accès Azure.

Ouvrez votre projet AEM dans IntelliJ. Veillez à ajouter le dossier FormTutorial comme illustré ci-dessous dans le projet ui.content .
![cloud-services-configuration](assets/cloud-services-configuration.png)

Veillez à ajouter l’entrée suivante dans le fichier filter.xml du projet ui.content

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Inclure le modèle de données de formulaire dans votre projet

Créez un modèle de données de formulaire basé sur la configuration des services cloud que vous avez créée à l’étape précédente. Pour inclure le modèle de données de formulaire dans votre projet, créez la structure de dossiers appropriée dans votre projet AEM dans intelliJ. Par exemple, mon modèle de données de formulaire se trouve dans un dossier appelé inscriptions
![fdm-content](assets/ui-content-fdm.png)

Inclure l’entrée appropriée dans le fichier filter.xml du projet ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Maintenant, lorsque vous créez et déployez votre projet à l’aide de Cloud Manager, vous devrez saisir à nouveau votre clé d’accès Azure dans la configuration des services cloud. Pour éviter de réentrer dans la clé d’accès, il est recommandé de créer une configuration contextuelle à l’aide des variables d’environnement, comme expliqué dans la section [article suivant](./context-aware-fdm.md)
