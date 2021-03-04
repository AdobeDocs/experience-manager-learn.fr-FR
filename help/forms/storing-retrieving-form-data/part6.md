---
title: Stockage et récupération des données de formulaire à partir de la base de données MySQL
description: Didacticiel en plusieurs parties pour vous guider dans les étapes de stockage et de récupération des données de formulaire
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 12%

---


# Déployez ceci sur votre serveur

>[!NOTE]
>
>Les éléments suivants sont nécessaires pour que cette opération s&#39;exécute sur votre système.
>
>* AEM Forms(version 6.3 ou ultérieure)
>* Base de données MySql


Pour tester cette fonctionnalité sur votre instance AEM Forms, procédez comme suit.

* Téléchargez et déployez les fichiers [Jar du pilote MySql](assets/mysqldriver.jar) à l’aide de la console Web [felix](http://localhost:4502/system/console/bundles).
* Téléchargez et déployez le [lot OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) à l’aide de la [console Web felix](http://localhost:4502/system/console/bundles).
* Téléchargez et installez le [package contenant la lib client, le modèle de formulaire adaptatif et le composant de page personnalisé](assets/store-and-fetch-af-with-data.zip) à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Importez l&#39;[exemple de formulaire adaptatif](assets/sample-adaptive-form.zip) à l&#39;aide de l&#39;interface [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importez le fichier [form-data-db.sql](assets/form-data-db.sql) à l’aide de MySql Workbench. Vous allez créer le schéma et les tables nécessaires dans votre base de données pour que ce didacticiel fonctionne.
* Connectez-vous à [configMgr.](http://localhost:4502/system/console/configMgr) Recherchez &quot;Apache Sling Connection Pooled DataSource&quot;. Créez une nouvelle entrée de source de données mise en pool de connexion Apache Sling appelée **SaveAndContinue** à l’aide des propriétés suivantes :

| Nom de la propriété | Valeur |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| URI de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


* Ouvrez le [formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled).
* Renseignez certains détails et cliquez sur le bouton &quot;Enregistrer et continuer plus tard&quot;.
* Vous devez récupérer une URL contenant un GUID.
* Copiez l’URL et collez-la dans un nouvel onglet du navigateur. **Assurez-vous qu’il n’y a pas d’espace vide à la fin de l’URL.**
* Le formulaire adaptatif doit être renseigné avec les données de l’étape précédente.
