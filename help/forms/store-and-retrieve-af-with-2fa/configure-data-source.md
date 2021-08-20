---
title: Configuration de la source de données
description: Créer une source de données pointant vers la base de données MySQL
feature: Formulaires adaptatifs
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 4%

---


# Configuration de la source de données

AEM permet l’intégration à une base de données externe de différentes manières. L’une des pratiques les plus courantes et standard d’intégration de la base de données consiste à utiliser les propriétés de configuration Apache Sling Connection Pooled DataSource via [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [pilotes MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriés vers AEM.
Définissez ensuite les propriétés Sling Connection Pooled DataSource spécifiques à votre base de données. La capture d’écran suivante montre les paramètres utilisés pour ce tutoriel. Le schéma de base de données vous est fourni dans le cadre de ces ressources de tutoriel.

![data-source](assets/data-source.JPG)


* Classe de pilote JDBC : `com.mysql.cj.jdbc.Driver`
* URI de connexion JDBC : `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Veillez à nommer votre source de données `StoreAndRetrieveAfData`, car il s’agit du nom utilisé dans le service OSGi.


## Créer une base de données


La base de données suivante a été utilisée à des fins de ce cas pratique. La base de données comporte une table appelée `formdatawithattachments` avec les 4 colonnes, comme illustré dans la capture d&#39;écran ci-dessous.
![data-base](assets/table-schema.JPG)

* La colonne **afdata** contiendra les données du formulaire adaptatif.
* La colonne **attachmentsInfo** contiendra les informations sur les pièces jointes du formulaire.
* Les colonnes **telephoneNumber** contiendront le numéro de mobile de la personne qui remplit le formulaire.

Créez la base de données en important le [schéma de base de données](assets/data-base-schema.sql)
à l’aide de MySQL Workbench.

## Création d’un modèle de données de formulaire

Créez un modèle de données de formulaire et basez-le sur la source de données créée à l’étape précédente.
Configurez le service **get** de ce modèle de données de formulaire comme illustré dans la capture d’écran ci-dessous.
Veillez à ne pas renvoyer de tableau dans le service **get** .

Ce service **get** est utilisé pour récupérer le numéro de téléphone associé à l’ID de l’application.

![get-service](assets/get-service.JPG)

Ce modèle de données de formulaire sera ensuite utilisé dans la balise **MyAccountForm** pour récupérer le numéro de téléphone associé à l’ID de l’application.
