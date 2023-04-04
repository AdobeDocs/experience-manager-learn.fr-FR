---
title: Configuration de modèles de ressources avec AEM Assets et InDesign Server
description: Les modèles de ressources permettent aux marketeurs de créer, gérer et diffuser des ressources numériques pour le numérique et l’impression. La création de brochures marketing, cartes de visite, prospectus, publicités et cartes postales est beaucoup plus facile avec les modèles de ressources intégrés au serveur InDesign. La configuration du serveur d’InDesign avec AEM est traitée dans cette section.
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Configuration de modèles de ressources avec AEM Assets et InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Les modèles de ressources permettent aux marketeurs de créer, gérer et diffuser des ressources numériques pour le numérique et l’impression. La création de brochures marketing, cartes de visite, prospectus, publicités et cartes postales est beaucoup plus facile avec les modèles de ressources intégrés au serveur InDesign. La configuration du serveur d’InDesign avec AEM est traitée dans cette section.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **must** être connecté à un serveur d’InDesign en cours d’exécution lorsque le modèle INDD est chargé. Une partie du traitement initial sur le fichier INDD nécessite un serveur InDesign.

## Télécharger l’évaluation des InDesigns Server {#download-indesign-server-trial}

Télécharger [Site web de téléchargement d’évaluation InDesign Server](https://www.adobeprerelease.com/)

## Démarrage de l’InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
