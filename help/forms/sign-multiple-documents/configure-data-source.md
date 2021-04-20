---
title: Configuration de la source de données AEM
description: Configuration de la source de données sauvegardée MySQL pour stocker et récupérer les données de formulaire
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 7%

---

# Configuration de la source de données

Il existe de nombreuses manières d&#39;AEM permettre l&#39;intégration à une base de données externe. L&#39;un des moyens les plus courants d&#39;intégrer une base de données consiste à utiliser les propriétés de configuration de DataSource mises en pool Apache Sling Connection par le biais de [configMgr](http://localhost:4502/system/console/configMgr).
La première étape consiste à télécharger et déployer les [pilotes MySql ](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriés en AEM.
Créez Apache Sling Connection Pooled DataSource et fournissez les propriétés comme indiqué dans la capture d’écran ci-dessous. Le schéma de base de données vous est fourni dans le cadre de ce didacticiel.

![source de données](assets/data-source.PNG)

La base de données comporte une table appelée formdata avec les 3 colonnes, comme le montre la capture d&#39;écran ci-dessous.

![base de données](assets/data-base.PNG)


>[!NOTE]
>Veillez à nommer votre source de données **aemformstutorial**. L’exemple de code utilise le nom pour se connecter à la base de données.

| Nom de la propriété | Valeur |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| URI de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Le fichier sql permettant de créer le schéma peut être [téléchargé ici](assets/sign-multiple-forms.sql). Vous devez importer ce fichier à l&#39;aide de MySql Workbench pour créer le schéma et le tableau.


