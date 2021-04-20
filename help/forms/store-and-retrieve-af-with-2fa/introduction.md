---
title: Stockage et récupération des données de formulaire avec des pièces jointes de la base de données MySQL
description: Didacticiel en plusieurs parties pour vous guider dans les étapes de stockage et de récupération des données de formulaire avec des pièces jointes
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 4%

---


# Stockage et récupération des données de formulaire adaptatif avec 2FA

Ce didacticiel décrit les étapes nécessaires à l’enregistrement et à la récupération des données de formulaire adaptatif avec des pièces jointes à l’aide de 2FA. Ce didacticiel utilise la base de données MySQL pour stocker les données de formulaire adaptatif. La base de données de votre choix peut être utilisée pour stocker les données tant que vous avez déployé les pilotes spécifiques à la base de données dans AEM. À un niveau élevé, les étapes suivantes sont nécessaires pour atteindre le cas d’utilisation :

* Utilisation de l’API GuideBridge pour accéder aux données de formulaire adaptatif

* Appelez un POST à une servlet. Cette servlet stocke les données dans la base de données et les pièces jointes du formulaire dans le référentiel CRX. Les données stockées dans la base de données sont associées à un GUID.

* Lorsque vous souhaitez renseigner le formulaire adaptatif avec les données stockées, vous récupérez les données associées au GUID et renseignez le formulaire adaptatif à l’aide de la méthode **request.setAttribute**.

## Démonstration du cas d&#39;utilisation

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Conditions préalables

L’audience de ce contenu devrait avoir une certaine expérience dans les domaines suivants :

* Formulaire adaptatif
* Modèle de données de formulaire
* Services/composants OSGi
* Bibliothèques clientes AEM
