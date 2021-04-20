---
title: Création de tables de base de données
description: Créer une base de données à utiliser par le modèle de données de formulaire
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 3%

---


# Créer des tables de base de données

Le modèle de données de formulaire peut être basé sur les sources RDBMS, RESTfull, SOAP ou OData. Ce cours est axé sur le pré-remplissage du formulaire adaptatif à l’aide du modèle de données de formulaire soutenu par la source de données RDBMS. Dans ce didacticiel, la base de données MYSQL a été utilisée. Nous avons créé les deux tableaux suivants pour montrer le cas d&#39;utilisation

* **** newhiretable : ce tableau stocke les informations du néwhire.

   ![newhire](assets/newhire-table.png)


* **** bénéficiaires possibles - Ce système permet de stocker les bénéficiaires potentiels

   ![bénéficiaires](assets/beneficiaries-table.png)

Vous pouvez importer le [fichier sql](assets/db-schema.sql) à l’aide de MySQL Workbench pour créer des tableaux contenant des données d’exemple.