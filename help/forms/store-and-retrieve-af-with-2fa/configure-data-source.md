---
title: Configuration de la source de données
description: Créer une source de données pointant vers la base de données MySQL
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 30c882da3a89820b5e11bc2902bb92dd0629efe9
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 3%

---

# Configuration de la source de données

AEM permet l’intégration à une base de données externe de différentes manières. L’une des pratiques les plus courantes et standard d’intégration de base de données consiste à utiliser les propriétés de configuration Apache Sling Connection Pooled DataSource via le [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [Pilotes MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) à AEM.
Définissez ensuite les propriétés Sling Connection Pooled DataSource spécifiques à votre base de données. La capture d’écran suivante montre les paramètres utilisés pour ce tutoriel. Le schéma de base de données vous est fourni dans le cadre de ces ressources de tutoriel.

![data-source](assets/data-source.JPG)


* Classe de pilote JDBC : `com.mysql.cj.jdbc.Driver`
* URI de connexion JDBC : `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Veillez à nommer votre source de données `StoreAndRetrieveAfData` car il s’agit du nom utilisé dans le service OSGi.


## Créer une base de données


La base de données suivante a été utilisée à des fins de ce cas pratique. La base de données comporte une table appelée `formdatawithattachments` avec les 4 colonnes comme illustré dans la capture d’écran ci-dessous.
![data-base](assets/table-schema.JPG)

* La colonne **afdata** contiendra les données du formulaire adaptatif.
* La colonne **attachmentsInfo** contient les informations sur les pièces jointes du formulaire.
* Les colonnes **telephoneNumber** contiendra le numéro de mobile de la personne qui remplit le formulaire.

Créez la base en important la variable [schéma de base de données](assets/data-base-schema.sql)
à l’aide de MySQL Workbench.

## Création d’un modèle de données de formulaire

Créez le modèle de données de formulaire et basez-le sur la source de données créée à l’étape précédente.
Configurez la variable **get** service de ce modèle de données de formulaire comme illustré dans la capture d’écran ci-dessous.
Veillez à ne pas renvoyer de tableau dans la variable **get** service.

L’objectif de **get** est de récupérer le numéro de téléphone associé à l’ID de l’application.

![get-service](assets/get-service.JPG)

Ce modèle de données de formulaire sera ensuite utilisé dans la variable **MyAccountForm** pour récupérer le numéro de téléphone associé à l’ID de l’application.
