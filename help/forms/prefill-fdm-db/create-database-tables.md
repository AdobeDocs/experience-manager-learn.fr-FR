---
title: Créer des tables de base de données
description: Créer une base de données qui sera utilisée par le modèle de données de formulaire
feature: Adaptive Forms
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: ht
source-wordcount: '105'
ht-degree: 100%

---

# Créer des tables de base de données

Le modèle de données de formulaire peut être basé sur des sources RDBMS, RESTful, SOAP ou OData. Ce cours se concentre sur le préremplissage d’un formulaire adaptatif à l’aide d’un modèle de données de formulaire soutenu par la source de données RDBMS. Pour les besoins de ce tutoriel, la base de données MYSQL a été utilisée. Nous avons créé les deux tables suivantes pour démontrer le cas d’utilisation :

* Table **newhire** : cette table stocke les informations sur les nouvelles embauches.

  ![newhire](assets/newhire-table.png)


* Table **beneficiaries** : celle-ci stocke les bénéficiaires potentiels.

  ![beneficiaries](assets/beneficiaries-table.png)

Vous pouvez importer le [fichier sql](assets/db-schema.sql) en utilisant Workbench de MySQL pour créer des tables avec des données d’exemple.

## Étapes suivantes

[Configurer un modèle de données de formulaire](./configuring-form-data-model.md)
