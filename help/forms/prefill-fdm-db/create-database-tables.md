---
title: Créer des tables de base de données
description: Créer une base de données à utiliser par le modèle de données de formulaire
feature: Adaptive Forms
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 4%

---

# Créer des tables de base de données

Le modèle de données de formulaire peut être basé sur des sources RDBMS, RESTfull, SOAP ou OData. Ce cours se concentre sur le préremplissage d’un formulaire adaptatif à l’aide d’un modèle de données de formulaire soutenu par la source de données SGBDR. Pour les besoins de ce tutoriel, la base de données MYSQL a été utilisée. Nous avons créé les deux tableaux suivants pour démontrer le cas d’utilisation

* **newhire** table : ce tableau stocke les informations contextuelles.

   ![newhire](assets/newhire-table.png)


* **bénéficiaires** table : stocke les bénéficiaires potentiels

   ![bénéficiaires](assets/beneficiaries-table.png)

Vous pouvez importer la variable [fichier sql](assets/db-schema.sql) utilisation de MySQL Workbench pour créer des tableaux avec des exemples de données.

## Étapes suivantes

[Configuration d’un modèle de données de formulaire](./configuring-form-data-model.md)
