---
title: Configurer la source de données
description: Création de DataSource pointant vers la base de données MySQL
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 3%

---


# Configurer la source de données

Il existe de nombreuses manières d&#39;AEM permettre l&#39;intégration à une base de données externe. L&#39;une des pratiques les plus courantes et les plus courantes de l&#39;intégration de base de données est d&#39;utiliser les propriétés de configuration de DataSource mises en pool Apache Sling Connection via [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les pilotes [](https://mvnrepository.com/artifact/mysql/mysql-connector-java) MySQL appropriés pour AEM.
Définissez ensuite les propriétés DataSource mises en pool de la connexion Sling propres à votre base de données. La capture d&#39;écran suivante montre les paramètres utilisés pour ce didacticiel. Le schéma de base de données vous est fourni dans le cadre de ce didacticiel.

![source de données](assets/data-source.JPG)


* JDBC Driver Class: `com.mysql.cj.jdbc.Driver`
* JDBC Connection URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Veillez à nommer votre source de données `StoreAndRetrieveAfData` car il s’agit du nom utilisé dans le service OSGi.


## Créer une base de données


La base de données suivante a été utilisée aux fins de ce cas d&#39;utilisation. La base de données comporte une table appelée `formdatawithattachments` avec les 4 colonnes, comme le montre la capture d&#39;écran ci-dessous.
![base de données](assets/table-schema.JPG)

* La colonne **afdata** contient les données du formulaire adaptatif.
* La colonne **attachmentsInfo** contient les informations relatives aux pièces jointes du formulaire.
* Les colonnes **phoneNumber** contiennent le numéro de téléphone portable de la personne qui remplit le formulaire.

Veuillez créer la base de données en important le schéma [de base de données](assets/data-base-schema.sql)à l&#39;aide de MySQL Workbench.

## Créer un modèle de données de formulaire

Créez un modèle de données de formulaire et basez-le sur la source de données créée à l’étape précédente.
Configurez le service **get** de ce modèle de données de formulaire comme illustré dans la capture d’écran ci-dessous.
Veillez à ne pas renvoyer de tableau dans le service **get** .

Ce service **get** est utilisé pour récupérer le numéro de téléphone associé à l&#39;ID d&#39;application.

![get-service](assets/get-service.JPG)

Ce modèle de données de formulaire sera ensuite utilisé dans **MyAccountForm** pour récupérer le numéro de téléphone associé à l&#39;identifiant de l&#39;application.
