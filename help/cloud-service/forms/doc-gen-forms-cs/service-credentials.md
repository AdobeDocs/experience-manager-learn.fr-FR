---
title: Informations d’identification du service AEM
description: Téléchargez les informations d’identification du service depuis AEM Developer Console.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Formulaires adaptatifs
topic: Développement
kt: 8192
thumbnail: 330519.jpg
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 2%

---


# Informations d’identification du service

Les intégrations à AEM en tant que Cloud Service doivent pouvoir s’authentifier en toute sécurité dans AEM. AEM’Developer Console génère des informations d’identification de service, qui sont utilisées par des applications, systèmes et services externes pour interagir par programmation avec les services AEM Author ou Publish sur HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Le fichier d’informations d’identification du service téléchargé est stocké sous la forme d’un fichier de ressources appelé service_token.json dans l’éclipse fournie. Les valeurs du fichier service_token sont utilisées pour générer le JWT et échanger le JWT contre un jeton d’accès. La classe d’utilitaire GetServiceCredentials est utilisée pour récupérer les valeurs de propriété du fichier de ressource service_token.json.
