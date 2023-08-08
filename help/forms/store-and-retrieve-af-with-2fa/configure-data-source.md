---
title: Configurer la source de données
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
source-git-commit: 51e21c11df63d33a6900fbc331a756f2a7655bcb
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 8%

---

# Configurer la source de données

AEM permet l’intégration à une base de données externe de différentes manières. L’une des pratiques les plus courantes et standard d’intégration de base de données consiste à utiliser les propriétés de configuration Apache Sling Connection Pooled DataSource via le [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [Pilotes MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) à AEM.
Définissez ensuite les propriétés Sling Connection Pooled DataSource spécifiques à votre base de données. La capture d’écran suivante montre les paramètres utilisés pour ce tutoriel. Le schéma de base de données vous est fourni dans le cadre de ces ressources de tutoriel.

>[!NOTE]
>Veillez à nommer votre source de données. `StoreAndRetrieveAfData` comme nom utilisé dans le service OSGi.


![data-source](assets/data-source.JPG)

| Nom de la propriété | Valeur de la propriété |   |
|---------------------|------------------------------------------------------------------------------------|---|
| Nom de la source de données | StoreAndRetrieveAfData |   |
| Classe de lecteur JDBC | jdbc:mysql://localhost:3306/aemformstutorial |   |
| URI de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&amp;autoReconnect=true |   |
|                     |                                                                                    |   |


## Créer une base de données


La base de données suivante a été utilisée à des fins de ce cas pratique. La base de données comporte une table appelée `formdatawithattachments` avec les 4 colonnes comme illustré dans la capture d’écran ci-dessous.
![data-base](assets/table-schema.JPG)

* La colonne **afdata** contiendra les données du formulaire adaptatif.
* La colonne **attachmentsInfo** contient les informations sur les pièces jointes du formulaire.
* Les colonnes **telephoneNumber** contiendra le numéro de mobile de la personne qui remplit le formulaire.

Créez la base en important la variable [schéma de base de données](assets/data-base-schema.sql)
à l’aide de MySQL Workbench.

## Créer un modèle de données de formulaire

Créez le modèle de données de formulaire et basez-le sur la source de données créée à l’étape précédente.
Configurez la variable **get** service de ce modèle de données de formulaire comme illustré dans la capture d’écran ci-dessous.
Veillez à ne pas renvoyer de tableau dans la variable **get** service.

L’objectif de **get** est de récupérer le numéro de téléphone associé à l’ID de l’application.

![get-service](assets/get-service.JPG)

Ce modèle de données de formulaire sera ensuite utilisé dans la variable **MyAccountForm** pour récupérer le numéro de téléphone associé à l’ID de l’application.

## Étapes suivantes

[Écrire du code pour enregistrer les pièces jointes du formulaire](./store-form-attachments.md)
