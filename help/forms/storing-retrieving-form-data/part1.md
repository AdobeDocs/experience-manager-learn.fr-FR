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
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 3%

---

# Configurer la source de données

Il existe de nombreuses manières d&#39;AEM permettre l&#39;intégration à une base de données externe. L&#39;une des pratiques les plus courantes et les plus courantes de l&#39;intégration de base de données est d&#39;utiliser les propriétés de configuration de DataSource mises en pool Apache Sling Connection via [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les pilotes [](https://mvnrepository.com/artifact/mysql/mysql-connector-java) My SQL appropriés dans AEM.
Définissez ensuite les propriétés Sling Connection Pooled DataSource. Ces propriétés sont spécifiques à votre base de données. La capture d&#39;écran suivante montre les paramètres utilisés pour ce didacticiel. Le schéma de base de données vous est fourni dans le cadre de ce didacticiel.
![source de données](assets/data-source.png)

La base de données comporte une table appelée formdata avec les 3 colonnes, comme le montre la capture d&#39;écran ci-dessous![data-base](assets/data-base-tables.PNG)
