---
title: Écrire le document de charge utile dans le système de fichiers
description: Étape de processus personnalisée pour ajouter au système de fichiers le document d’écriture résidant sous le dossier de charge utile
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Extraire le noeud du xml de données envoyées

Cette étape de processus personnalisée consiste à créer un document XML en extrayant le noeud d’un autre document XML. Vous devez l’utiliser lorsque vous souhaitez fusionner les données envoyées avec le modèle xdp pour générer le fichier pdf. Par exemple, lorsque vous envoyez un formulaire adaptatif, les données que vous devez fusionner avec le modèle xdp se trouvent dans l’élément de données . Dans ce cas, vous devez créer un autre document XML en extrayant l’élément de données approprié.

La capture d’écran suivante montre les arguments que vous devez transmettre à l’étape de processus personnalisée.
![process-step](assets/create-xml-process-step.png)
Voici les paramètres :
* Data.xml : fichier xml à partir duquel vous souhaitez extraire le noeud.
* datatomerge.xml : nouveau fichier xml créé avec le noeud extrait
* /afData/afUnboundData/data - Noeud à extraire


La capture d’écran suivante montre le fichier datamerge.xml en cours de création sous le dossier de charge utile.
![create-xml](assets/create-xml.png)

[Le lot personnalisé peut être téléchargé ici](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)