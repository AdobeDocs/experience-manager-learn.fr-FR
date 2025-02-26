---
title: Créer une configuration de services cloud
description: Créer une source de données pour se connecter à Salesforce à l’aide des informations d’identification OAuth
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: ce22dd482417a54d222165deaf485ff69c2856b7
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 41%

---

# Créer une source de données

Créez une source de données soutenue par REST à l’aide du fichier Swagger créé à l’étape précédente.

>[!VIDEO](https://video.tv.adobe.com/v/331755?quality=12&learn=on)

| Configuration | Valeur |
|---------------------|-----------------------------------------------------------------|
| URL OAuth | https://login.salesforce.com/services/oauth2/authorize |
| Champ d’application de l’autorisation | api chatter_api full id openid refresh_token visualforce web |
| URL du jeton d’actualisation | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| URL de jeton d’accès | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**Les noms de domaine de l’URL du jeton d’actualisation et d’accès devront changer pour correspondre aux paramètres de votre compte Salesforce**