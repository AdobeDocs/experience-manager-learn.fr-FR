---
title: Informations d’identification du service AEM Forms
description: Téléchargez les informations d’identification du service à partir de la Developer Console d’AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 100%

---

# Informations d’identification du service AEM Forms

Les intégrations à AEM as a Cloud Service doivent pouvoir s’authentifier en toute sécurité auprès du service AEM. La Developer Console d’AEM génère les informations d’identification de service, qui sont utilisées par les applications externes, les systèmes et les services externes à interagir par programmation avec les services de création et de publication via HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Le fichier d’informations d’identification du service téléchargé est stocké sous la forme d’un fichier de ressources appelé service_token.json dans l’Eclipse fourni. Les valeurs du fichier service_token sont utilisées pour générer le JWT et échanger le JWT contre un jeton d’accès. La classe d’utilitaire GetServiceCredentials est utilisée pour récupérer les valeurs de propriété du fichier de ressource service_token.json.
