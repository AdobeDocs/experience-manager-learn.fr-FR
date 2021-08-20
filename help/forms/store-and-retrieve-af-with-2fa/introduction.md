---
title: Stockage et récupération des données de formulaire avec des pièces jointes de la base de données MySQL
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes impliquées dans le stockage et la récupération des données de formulaire avec des pièces jointes
feature: Formulaires adaptatifs
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Développement
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 4%

---


# Stockage et récupération des données de formulaire adaptatif avec 2FA

Ce tutoriel vous guide tout au long des étapes nécessaires à l’enregistrement et à la récupération des données de formulaire adaptatif avec des pièces jointes à l’aide de la fonction 2FA. Ce tutoriel utilisait la base de données MySQL pour stocker les données de formulaire adaptatif. La base de données de votre choix peut être utilisée pour stocker les données tant que vous avez déployé les pilotes spécifiques à la base de données dans AEM. À un niveau élevé, les étapes suivantes sont nécessaires pour réaliser le cas d’utilisation :

* Utilisation de l’API GuideBridge pour accéder aux données de formulaire adaptatif

* Effectuez un appel POST vers une servlet. Ce servlet stocke les données dans la base de données et les pièces jointes de formulaire dans le référentiel CRX. Les données stockées dans la base de données sont associées à un GUID.

* Lorsque vous souhaitez renseigner le formulaire adaptatif avec les données stockées, vous récupérez les données associées au GUID et renseignez le formulaire adaptatif à l’aide de la méthode **request.setAttribute** .

## Démonstration du cas pratique

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Prérequis

L’audience de ce contenu devrait avoir une certaine expérience dans les domaines suivants :

* Formulaire adaptatif
* Modèle de données de formulaire
* Services/composants OSGi
* AEM des bibliothèques clientes
