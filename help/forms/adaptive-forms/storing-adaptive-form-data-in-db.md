---
title: Stockage des données de formulaire adaptatif
seo-title: Stockage des données de formulaire adaptatif
description: Stockage de données de formulaire adaptatif dans DataBase dans le cadre de votre processus AEM
seo-description: Stockage de données de formulaire adaptatif dans DataBase dans le cadre de votre processus AEM
feature: Forms adaptatif, Workflow, Modèle de données de formulaire
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---


# Stockage des envois de formulaires adaptatifs dans la base de données

Il existe plusieurs méthodes pour stocker les données de formulaire envoyées dans la base de données de votre choix. Une source de données JDBC peut être utilisée pour stocker directement les données dans la base de données. Un lot OSGI personnalisé peut être écrit pour stocker les données dans la base de données. Cet article utilise une étape de processus personnalisée dans AEM processus pour stocker les données.
Le cas d’utilisation consiste à déclencher un processus AEM sur un envoi de formulaire adaptatif et une étape du processus consiste à stocker les données envoyées dans la base de données.

**Suivez les étapes mentionnées ci-dessous pour que cela fonctionne sur votre système.**

* [Téléchargez le fichier Zip et extrayez son contenu sur votre disque dur.](assets/storeafdataindb.zip)

   * Importez le fichier StoreAFInDBWorkflow.zip dans AEM à l’aide du gestionnaire de packages. Le package comporte un exemple de flux de travaux qui stocke les données AF dans la base de données. Ouvrez le modèle de processus. Le processus ne comporte qu’une seule étape. Cette étape appelle le code écrit dans le lot pour stocker les données AF dans la base de données. Je ne transmets qu&#39;un seul argument au processus. Il s’agit du nom du formulaire adaptatif dont les données sont enregistrées.
   * Déployez le fichier insertdata.core-0.0.1-SNAPSHOT.jar à l’aide de la console Web Felix. Ce lot a le code pour écrire les données de formulaire envoyées dans la base de données.

* Accédez à [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * Recherchez &quot;JDBC Connection Pool&quot;. Créez un pool de connexions JDBC Day Commons. Spécifiez les paramètres spécifiques à votre base de données.

   * ![pool de connexions jdbc](assets/jdbc-connection-pool.png)
   * Rechercher &quot;**Insérer des données de formulaire dans DB**&quot;
   * Spécifiez les propriétés spécifiques à votre base de données.
      * DataSourceName : nom de la source de données que vous avez configurée précédemment.
      * NomTable : nom de la table dans laquelle vous souhaitez stocker les données AF
      * FormName - Nom de colonne contenant le nom du formulaire
      * NomColonne - Nom de colonne pour contenir les données AF

   ![insertdata](assets/insertdata.PNG)

* Créez un formulaire adaptatif.

* Associez le formulaire adaptatif au flux de travail AEM (StoreAFValuesinDB) comme illustré dans la capture d’écran ci-dessous.

* Veillez à spécifier &quot;data.xml&quot; dans le chemin d’accès au fichier de données, comme illustré dans la capture d’écran ci-dessous.

   ![envoi](assets/submissionafforms.png)

* Prévisualisation du formulaire et envoi

* Si tout s’est bien passé, vous devriez voir les données du formulaire stockées dans le tableau et la colonne spécifiés par vous.



