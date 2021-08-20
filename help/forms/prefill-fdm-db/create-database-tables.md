---
title: Créer des tables de base de données
description: Créer une base de données à utiliser par le modèle de données de formulaire
feature: Formulaires adaptatifs
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 2%

---


# Créer des tables de base de données

Le modèle de données de formulaire peut être basé sur des sources RDBMS, RESTfull, SOAP ou OData. Ce cours se concentre sur le préremplissage d’un formulaire adaptatif à l’aide d’un modèle de données de formulaire soutenu par la source de données SGBDR. Pour les besoins de ce tutoriel, la base de données MYSQL a été utilisée. Nous avons créé les deux tableaux suivants pour démontrer le cas d’utilisation

* **** newhiretable : ce tableau stocke les informations contextuelles.

   ![newhire](assets/newhire-table.png)


* **** avantagariestable : stocke les bénéficiaires potentiels

   ![bénéficiaires](assets/beneficiaries-table.png)

Vous pouvez importer le [fichier sql](assets/db-schema.sql) à l’aide de MySQL Workbench pour créer des tableaux avec des exemples de données.