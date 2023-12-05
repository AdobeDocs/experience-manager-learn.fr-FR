---
title: Stocker et récupérer des données de formulaire avec des pièces jointes à partir d’une base de données MySQL
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes impliquées dans le stockage et la récupération des données de formulaire avec des pièces jointes
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 62
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 100%

---

# Stocker et récupérer des données de formulaire adaptatif avec la 2FA

Ce tutoriel vous guide tout au long des étapes nécessaires à l’enregistrement et à la récupération des données de formulaire adaptatif avec des pièces jointes à l’aide de la 2FA. Ce tutoriel utilisait la base de données MySQL pour stocker les données de formulaire adaptatif. La base de données de votre choix peut être utilisée pour stocker les données tant que vous avez déployé les pilotes spécifiques à la base de données dans AEM. À un niveau élevé, les étapes suivantes sont nécessaires pour réaliser le cas d’utilisation :

* Utiliser l’API GuideBridge pour accéder aux données de formulaire adaptatif

* Effectuez un appel POST vers un servlet. Ce servlet stocke les données dans la base de données et les pièces jointes de formulaire dans le référentiel CRX. Les données stockées dans la base de données sont associées à un GUID.

* Lorsque vous souhaitez renseigner le formulaire adaptatif avec les données stockées, vous récupérez les données associées au GUID et renseignez le formulaire adaptatif à l’aide de la méthode **request.setAttribute**.

## Démonstration du cas d’utilisation

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Conditions préalables

L’audience de ce contenu devrait avoir une certaine expérience dans les domaines suivants :

* Formulaire adaptatif
* Modèle de données de formulaire
* Services/composants OSGi
* Bibliothèques clientes AEM


## Étapes suivantes

[Configurer la source de données](./configure-data-source.md)