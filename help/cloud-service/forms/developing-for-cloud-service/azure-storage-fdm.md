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
source-git-commit: 8484897297940ab28619c4b1af5362a5937eadfa
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# Inclure la configuration des services cloud dans votre projet

Créez un conteneur de configuration appelé &quot;FormsTutorial&quot; pour contenir votre configuration de services cloud Créez une configuration de services cloud pour le stockage Azure appelée &quot;Stocker les envois de formulaire dans Azure&quot; dans le conteneur &quot;FormsTutorial&quot;. Fournissez les détails du compte de stockage Azure et la clé de compte.

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
>Désormais, lorsque vous créez et déployez votre projet, le modèle de données de formulaire basé sur la configuration des services cloud disponible dans votre instance cloud est appliqué au projet.