---
title: Stocker et récupérer des données de formulaire à partir de la base de données MySQL - Configurer la source de données
description: Tutoriel en plusieurs parties détaillant les étapes impliquées dans le stockage et la récupération des données de formulaire
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 56
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 100%

---

# Configurer la source de données

AEM permet l’intégration à une base de données externe de nombreuses manières. L’une des pratiques les plus courantes et standard d’intégration de base de données consiste à utiliser les propriétés de configuration de la source de données mise en pool de la connexion Apache Sling via [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [pilotes MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) dans AEM.
Créez la source de données mise en pool de la connexion Apache Sling et fournissez les propriétés comme indiqué dans la capture d’écran ci-dessous. Le schéma de base de données vous est fourni dans le cadre de ces ressources de tutoriel.

![data-source](assets/save-continue.PNG)

La base de données comporte un tableau appelé formdata avec 3 colonnes, comme illustré dans la capture d’écran ci-dessous.

![data-base](assets/data-base-tables.PNG)

Le fichier sql permettant de créer le schéma peut être [téléchargé ici](assets/form-data-db.sql). Vous devez importer ce fichier à l’aide de MySql Workbench pour créer le schéma et la table.

>[!NOTE]
>Veillez à nommer votre source de données **SaveAndContinue**. L’exemple de code utilise ce nom pour se connecter à la base de données.

| Nom de la propriété | Valeur |
| ------------------------|---------------------------------------|
| Nom de la source de données | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| URI de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial |
