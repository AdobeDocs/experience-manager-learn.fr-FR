---
title: Créer des tables de base de données
description: Créer une base de données à utiliser par le modèle de données de formulaire
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
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