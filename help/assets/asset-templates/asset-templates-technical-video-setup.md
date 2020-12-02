---
title: Configuration de modèles de ressources avec AEM Assets et InDesign Server
description: Les modèles de ressources permettent aux spécialistes du marketing de créer, gérer et diffuser des ressources numériques pour le numérique et l’impression. La création de brochures marketing, de cartes de visite, de prospectus, de publicités et de cartes postales est beaucoup plus facile avec les modèles de ressources lorsqu’ils sont intégrés au serveur d’InDesign. La configuration du serveur d’InDesigns avec AEM est traitée dans cette section.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# Configuration de modèles de ressources avec AEM Assets et InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Les modèles de ressources permettent aux spécialistes du marketing de créer, gérer et diffuser des ressources numériques pour le numérique et l’impression. La création de brochures marketing, de cartes de visite, de prospectus, de publicités et de cartes postales est beaucoup plus facile avec les modèles de ressources lorsqu’ils sont intégrés au serveur d’InDesign. La configuration du serveur d’InDesigns avec AEM est traitée dans cette section.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>aem **doit** être connecté à un serveur d&#39;InDesign en cours d&#39;exécution lorsque le modèle INDD est téléchargé. Une partie du traitement initial du fichier INDD nécessite le serveur d’InDesign.

## Télécharger la version d’évaluation de l’InDesign Server {#download-indesign-server-trial}

Télécharger [Site Web de téléchargement d’évaluation des InDesigns Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## InDesign Server de début {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
