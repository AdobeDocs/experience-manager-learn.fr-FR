---
title: Configurer la source de données AEM
description: Configurer une source de données compatible avec MySQL pour stocker et récupérer des données de formulaire
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '203'
ht-degree: 100%

---

# Configurer la source de données

AEM permet l’intégration à une base de données externe de différentes manières. L’une des méthodes les plus courantes d’intégration d’une base de données consiste à utiliser les propriétés de configuration de source de données mise en pool de la connexion Apache Sling via le [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [pilotes MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) dans AEM.
Créez la source de données mise en pool de la connexion Apache Sling et fournissez les propriétés comme indiqué dans la capture d’écran ci-dessous. Le schéma de base de données vous est fourni dans le cadre de ces ressources de tutoriel.

![data-source](assets/data-source.PNG)

La base de données comporte un tableau appelé formdata avec 3 colonnes, comme illustré dans la capture d’écran ci-dessous.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Veillez à nommer votre source de données **aemformstutorial**. L’exemple de code utilise ce nom pour se connecter à la base de données.

| Nom de la propriété | Valeur |
| ------------------------|--------------------------------------- |
| Nom de la source de données | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| URI de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Ressources

Le fichier SQL de création du schéma peut être [téléchargé ici](assets/sign-multiple-forms.sql). Vous devez importer ce fichier à l’aide de MySql Workbench pour créer le schéma et la table.

## Étapes suivantes

[Créer un service OSGi pour stocker et récupérer des données dans la base de données](./create-osgi-service.md)
