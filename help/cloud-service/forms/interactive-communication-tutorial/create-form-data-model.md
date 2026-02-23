---
title: Créer un modèle de données de formulaire pour un document IC
description: Découvrez comment créer un modèle de données de formulaire dans AEM Forms pour récupérer dynamiquement des données pour le document de communication interactive.
version: Experience Manager as a Cloud Service
feature: Interactive Communication
role: Developer
level: Intermediate
doc-type: Feature Video
duration: 170
last-substantial-update: 2026-02-20T00:00:00Z
jira: KT-20353
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---


# Créer un modèle de données de formulaire pour un document IC

Créez un modèle de données Forms pour intégrer des sources de données externes à la communication interactive dans Adobe AEM. Ce processus implique la configuration d’un service RESTful, le chargement d’un fichier Swagger et la configuration de points d’entrée de service pour récupérer et lier les données de manière dynamique. Découvrez comment vous connecter en toute sécurité à des services externes et tester le modèle pour garantir la récupération réussie des données.

Un serveur d’API simulé a été implémenté pour simuler le service Orders à des fins de développement et de test. Il expose un point d’entrée pour récupérer les commandes d’un utilisateur donné (par exemple, par ID d’utilisateur), renvoyant les données de commande prédéfinies ou générées dynamiquement dans le même schéma que l’API de production.

Le fichier Swagger utilisé pour la création du modèle de données de formulaire peut être [téléchargé ici](assets/UsersAndOrders.json)

>[!VIDEO](https://video.tv.adobe.com/v/3480005/?learn=on&enablevpops)

## Étapes suivantes

[Créer un modèle](./create-template.md)
