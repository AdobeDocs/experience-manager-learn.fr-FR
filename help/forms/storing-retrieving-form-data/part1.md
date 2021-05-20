---
title: Stockage et récupération des données de formulaire à partir de la base de données MySQL
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes impliquées dans le stockage et la récupération des données de formulaire
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 6%

---

# Configuration de la source de données

AEM permet l’intégration à une base de données externe de différentes manières. L’une des pratiques les plus courantes et standard d’intégration de la base de données consiste à utiliser les propriétés de configuration Apache Sling Connection Pooled DataSource via [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [pilotes MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriés dans AEM.
Créez Apache Sling Connection Pooled DataSource et fournissez les propriétés comme indiqué dans la capture d’écran ci-dessous. Le schéma de base de données vous est fourni dans le cadre de ces ressources de tutoriel.

![data-source](assets/save-continue.PNG)

La base de données comporte un tableau appelé formdata avec les 3 colonnes, comme illustré dans la capture d’écran ci-dessous.

![data-base](assets/data-base-tables.PNG)

Le fichier sql permettant de créer le schéma peut être [téléchargé ici](assets/form-data-db.sql). Vous devez importer ce fichier à l’aide de MySql Workbench pour créer le schéma et la table.

>[!NOTE]
>Veillez à nommer votre source de données **SaveAndContinue**. L’exemple de code utilise le nom pour se connecter à la base de données.

| Nom de la propriété | Valeur |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| uri de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


