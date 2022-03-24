---
title: Informations d’identification du service AEM
description: Téléchargez les informations d’identification du service depuis AEM Developer Console.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Informations d’identification du service

Les intégrations à AEM as a Cloud Service doivent pouvoir s’authentifier en toute sécurité dans AEM. AEM’Developer Console génère des informations d’identification de service, qui sont utilisées par des applications, systèmes et services externes pour interagir par programmation avec les services AEM Author ou Publish sur HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Le fichier d’informations d’identification du service téléchargé est stocké sous la forme d’un fichier de ressources appelé service_token.json dans l’éclipse fournie. Les valeurs du fichier service_token sont utilisées pour générer le JWT et échanger le JWT contre un jeton d’accès. La classe d’utilitaire GetServiceCredentials est utilisée pour récupérer les valeurs de propriété du fichier de ressource service_token.json.
