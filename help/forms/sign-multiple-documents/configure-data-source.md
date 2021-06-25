---
title: Configuration de la source de données AEM
description: Configuration d’une source de données compatible avec MySQL pour stocker et récupérer des données de formulaire
feature: Formulaires adaptatifs
topic: Développement
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 7%

---

# Configuration de la source de données

AEM permet l&#39;intégration à une base de données externe de différentes manières. L’une des méthodes les plus courantes d’intégration d’une base de données consiste à utiliser les propriétés de configuration Apache Sling Connection Pooled DataSource via [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [pilotes MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriés dans AEM.
Créez Apache Sling Connection Pooled DataSource et fournissez les propriétés comme indiqué dans la capture d’écran ci-dessous. Le schéma de base de données vous est fourni dans le cadre de ces ressources de tutoriel.

![data-source](assets/data-source.PNG)

La base de données comporte un tableau appelé formdata avec les 3 colonnes, comme illustré dans la capture d’écran ci-dessous.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Veillez à nommer votre source de données **aemformstutorial**. L’exemple de code utilise le nom pour se connecter à la base de données.

| Nom de la propriété | Valeur |
| ------------------------|--------------------------------------- |
| Datasource Name | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| uri de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Le fichier SQL de création du schéma peut être [téléchargé à partir d’ici](assets/sign-multiple-forms.sql). Vous devez importer ce fichier à l’aide de MySql Workbench pour créer le schéma et la table.
