---
title: Stockage et récupération des données de formulaire à partir de la base de données MySQL
description: Didacticiel en plusieurs parties pour vous guider dans les étapes de stockage et de récupération des données de formulaire
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 5%

---

# Configurer la source de données

Il existe de nombreuses manières d&#39;AEM permettre l&#39;intégration à une base de données externe. L&#39;une des pratiques les plus courantes et les plus courantes d&#39;intégration de base de données est d&#39;utiliser les propriétés de configuration de DataSource mises en pool de la connexion Apache Sling par l&#39;intermédiaire de [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [pilotes MySql ](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriés en AEM.
Créez Apache Sling Connection Pooled DataSource et fournissez les propriétés comme indiqué dans la capture d’écran ci-dessous. Le schéma de base de données vous est fourni dans le cadre de ce didacticiel.

![source de données](assets/save-continue.PNG)

La base de données comporte une table appelée formdata avec les 3 colonnes, comme le montre la capture d&#39;écran ci-dessous.

![base de données](assets/data-base-tables.PNG)

Le fichier sql permettant de créer le schéma peut être [téléchargé ici](assets/form-data-db.sql). Vous devez importer ce fichier à l&#39;aide de MySql Workbench pour créer le schéma et le tableau.

>[!NOTE]
>Veillez à nommer votre source de données **SaveAndContinue**. L’exemple de code utilise le nom pour se connecter à la base de données.

| Nom de la propriété | Valeur |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| URI de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


