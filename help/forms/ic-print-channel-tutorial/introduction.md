---
title: Création de votre première communication interactive pour le canal d’impression
seo-title: Creating your first interactive communication for the print channel
description: Les communications interactives sont une nouveauté dans AEM Forms 6.4. Ce document décrit les étapes nécessaires à la création d’une communication interactive pour le canal d’impression.
seo-description: Interactive Communications is new to AEM Forms 6.4. This document will walk you through the steps needed to create an interactive communication for the print channel.
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 1949aeff-ae56-4abd-8e63-23c2fb4859f2
last-substantial-update: 2019-08-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# Création de votre première communication interactive pour le canal d’impression

Les communications interactives sont une nouveauté dans AEM Forms 6.4. Ce document décrit les étapes nécessaires à la création d’une communication interactive pour le canal d’impression.

## Conditions préalables {#prerequistes}

[Téléchargez et importez la ressource associée à ce tutoriel dans AEM à l’aide du gestionnaire de packages.](assets/gettingstartedassets.zip)Ce fichier zip contient des images, des fragments de document, la configuration du dossier de contrôle et le fichier de mise en page (xdp) dans le cadre du package de ressources.

[Téléchargez et décompressez ce fichier.](assets/warfileandswaggerfile.zip) Ce fichier contient le fichier SampleRest.war qui doit être déployé sur le fichier Tomcat et le fichier swagger qui doit être utilisé pour configurer votre source de données.

Lorsque vous aurez terminé ce tutoriel, vous aurez appris ce qui suit :

* Création d’une source de données
* Création d’un modèle de données de formulaire
* Créer des fragments de document
* Configuration de tableaux et de tableaux
* Utiliser des dossiers de contrôle pour générer des documents en mode batch
