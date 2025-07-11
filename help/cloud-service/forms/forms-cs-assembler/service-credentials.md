---
title: Informations d’identification de service AEM
description: Téléchargez les informations d’identification du service à partir de la Developer Console d’AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '106'
ht-degree: 100%

---

# Informations d’identification de service

Les intégrations à AEM as a Cloud Service doivent pouvoir s’authentifier en toute sécurité auprès du service AEM. La Developer Console d’AEM génère des informations d’identification de service, qui sont utilisées par les applications, systèmes et services externes pour interagir par programmation avec les services de création et de publication via HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/342221?quality=12&learn=on&captions=fre_fr)

Le fichier d’informations d’identification du service téléchargé est stocké sous la forme d’un fichier de ressources appelé service_token.json dans l’Eclipse fourni. Les valeurs du fichier service_token sont utilisées pour générer le JWT et échanger le JWT contre un jeton d’accès. La classe d’utilitaire GetServiceCredentials est utilisée pour récupérer les valeurs de propriété du fichier de ressource service_token.json.
