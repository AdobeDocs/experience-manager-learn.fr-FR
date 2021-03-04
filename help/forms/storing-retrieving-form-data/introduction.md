---
title: Stockage et récupération des données de formulaire à partir de la base de données MySQL
description: Didacticiel en plusieurs parties pour vous guider dans les étapes de stockage et de récupération des données de formulaire
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 2%

---


# Stockage et récupération des données de formulaire adaptatif à partir de la base de données MySQL

Ce didacticiel décrit les étapes nécessaires à l’enregistrement et à la récupération des données de formulaire adaptatif à partir de la base de données. Ce didacticiel utilise la base de données MySQL pour stocker les données de formulaire adaptatif. La base de données de votre choix peut être utilisée pour stocker les données tant que vous avez déployé les pilotes spécifiques à la base de données dans AEM. À un niveau élevé, les étapes suivantes sont nécessaires pour atteindre le cas d’utilisation :

* Utilisation de l’API GuideBridge pour accéder aux données de formulaire adaptatif

* Appelez un POST à une servlet. Cette servlet stocke les données dans la base de données. Les données stockées sont associées à un GUID

* Lorsque vous souhaitez renseigner le formulaire adaptatif avec les données stockées, vous récupérez les données associées au GUID et renseignez le formulaire adaptatif à l’aide de la méthode **request.setAttribute**.

## Démonstration du cas d&#39;utilisation

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
