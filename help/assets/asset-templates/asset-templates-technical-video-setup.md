---
title: Configuration de modèles de ressources avec AEM Assets et InDesign Server
description: Les modèles de ressources permettent aux marketeurs de créer, gérer et diffuser des ressources numériques pour le numérique et l’impression. La création de brochures marketing, cartes de visite, prospectus, publicités et cartes postales est beaucoup plus facile avec les modèles de ressources intégrés au serveur InDesign. La configuration du serveur d’InDesign avec AEM est traitée dans cette section.
version: 6.3, 6.4, 6.5
topic: Gestion de contenu
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 1%

---


# Configuration de modèles de ressources avec AEM Assets et InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Les modèles de ressources permettent aux marketeurs de créer, gérer et diffuser des ressources numériques pour le numérique et l’impression. La création de brochures marketing, cartes de visite, prospectus, publicités et cartes postales est beaucoup plus facile avec les modèles de ressources intégrés au serveur InDesign. La configuration du serveur d’InDesign avec AEM est traitée dans cette section.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **doit** être connecté à un serveur d’InDesign en cours d’exécution lorsque le modèle INDD est téléchargé. Une partie du traitement initial sur le fichier INDD nécessite un serveur InDesign.

## Télécharger l’évaluation de l’InDesign Server {#download-indesign-server-trial}

Télécharger le [site Web de téléchargement d’essai d’InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## Démarrage de l’InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
