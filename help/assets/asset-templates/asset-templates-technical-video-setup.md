---
title: Configurer les modèles de ressources avec AEM Assets et InDesign Server
description: Les modèles de ressources permettent aux personnes chargées du marketing de créer, gérer et diffuser des ressources numériques pour les supports numériques et imprimés. La création de brochures marketing, cartes de visite, prospectus, publicités et cartes postales est beaucoup plus facile avec les modèles de ressources lorsqu’ils sont intégrés à InDesign Server. Cette section traite de la configuration d’InDesign Server avec AEM.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '153'
ht-degree: 100%

---

# Configurer les modèles de ressources avec AEM Assets et InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Les modèles de ressources permettent aux personnes chargées du marketing de créer, gérer et diffuser des ressources numériques pour les supports numériques et imprimés. La création de brochures marketing, cartes de visite, prospectus, publicités et cartes postales est beaucoup plus facile avec les modèles de ressources lorsqu’ils sont intégrés à InDesign Server. Cette section traite de la configuration d’InDesign Server avec AEM.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **doit** être connecté à un serveur InDesign Server en cours d’exécution lors du chargement du modèle INDD. Une partie du traitement initial sur le fichier INDD nécessite InDesign Server.

## Télécharger la version d’essai d’InDesign Server {#download-indesign-server-trial}

Télécharger la [version d’essai d’InDesign Server sur le site web](https://www.adobeprerelease.com/).

## Démarrer InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
