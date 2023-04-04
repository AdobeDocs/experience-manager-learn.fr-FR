---
title: Stockage et récupération des données de formulaire à partir de la base de données MySQL Introduction
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes impliquées dans le stockage et la récupération des données de formulaire
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Stockage et récupération de données de formulaire adaptatif à partir de la base de données MySQL

Ce tutoriel vous guide tout au long des étapes nécessaires à l’enregistrement et à la récupération des données de formulaire adaptatif de la base de données. Ce tutoriel utilisait la base de données MySQL pour stocker les données de formulaire adaptatif. La base de données de votre choix peut être utilisée pour stocker les données tant que vous avez déployé les pilotes spécifiques à la base de données dans AEM. À un niveau élevé, les étapes suivantes sont nécessaires pour réaliser le cas d’utilisation :

* Utilisation de l’API GuideBridge pour accéder aux données de formulaire adaptatif

* Effectuez un appel POST vers une servlet. Ce servlet stocke les données dans la base de données. Les données stockées sont associées à un GUID.

* Lorsque vous souhaitez renseigner le formulaire adaptatif avec les données stockées, vous récupérez les données associées au GUID et renseignez le formulaire adaptatif à l’aide de la variable **request.setAttribute** .

## Démonstration du cas pratique

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)
