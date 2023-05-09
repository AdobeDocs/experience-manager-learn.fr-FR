---
title: Stockage et récupération des données de formulaire avec des pièces jointes de la base de données MySQL
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes impliquées dans le stockage et la récupération des données de formulaire avec des pièces jointes
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 3%

---

# Stockage et récupération de données de formulaire adaptatif avec 2FA

Ce tutoriel vous guide tout au long des étapes nécessaires à l’enregistrement et à la récupération des données de formulaire adaptatif avec des pièces jointes à l’aide de la fonction 2FA. Ce tutoriel utilisait la base de données MySQL pour stocker les données de formulaire adaptatif. La base de données de votre choix peut être utilisée pour stocker les données tant que vous avez déployé les pilotes spécifiques à la base de données dans AEM. À un niveau élevé, les étapes suivantes sont nécessaires pour réaliser le cas d’utilisation :

* Utilisation de l’API GuideBridge pour accéder aux données de formulaire adaptatif

* Effectuez un appel POST vers une servlet. Ce servlet stocke les données dans la base de données et les pièces jointes de formulaire dans le référentiel CRX. Les données stockées dans la base de données sont associées à un GUID.

* Lorsque vous souhaitez renseigner le formulaire adaptatif avec les données stockées, vous récupérez les données associées au GUID et renseignez le formulaire adaptatif à l’aide de la variable **request.setAttribute** .

## Démonstration du cas pratique

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Prérequis

L’audience de ce contenu devrait avoir une certaine expérience dans les domaines suivants :

* Formulaire adaptatif
* Modèle de données de formulaire
* Services/composants OSGi
* AEM des bibliothèques clientes


## Étapes suivantes

[Configuration de la source de données](./configure-data-source.md)