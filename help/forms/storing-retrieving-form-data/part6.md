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
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 6%

---


# Déployez ceci sur votre serveur

>[!NOTE]
>
>Les éléments suivants sont nécessaires pour que cette opération s&#39;exécute sur votre système.
>
>* AEM Forms(version 6.3 ou ultérieure)
>* Base de données MYSQL


Pour tester cette fonctionnalité sur votre instance AEM Forms, procédez comme suit.

* Téléchargez et décompressez les ressources [du](assets/store-retrieve-form-data.zip) didacticiel sur votre système local.
* Déployez et début les lots techmarketingdemos.jar et mysqldriver.jar à l’aide de la console Web [Felix.](http://localhost:4502/system/console/configMgr)
* Importez le fichier aemformstutorial.sql à l’aide de MYSQL Workbench. Vous allez créer le schéma et les tables nécessaires dans votre base de données pour que ce didacticiel fonctionne.
* Importez StoreAndRetrieve.zip à l’aide de [AEM gestionnaire de packages.](http://localhost:4502/crx/packmgr/index.jsp) Ce package contient le modèle de formulaire adaptatif, la lib du client de composant de page et un exemple de configuration de formulaire adaptatif et de source de données.
* Connectez-vous à [configMgr.](http://localhost:4502/system/console/configMgr) Recherchez &quot;Apache Sling Connection Pooled DataSource&quot;. Ouvrez l’entrée de source de données associée à aemformstutorial et saisissez le nom d’utilisateur et le mot de passe propres à votre instance de base de données.
* Ouvrir le formulaire [adaptatif](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Renseignez certains détails et cliquez sur le bouton &quot;Enregistrer et continuer plus tard&quot;.
* Vous devez récupérer une URL contenant un GUID.
* Copiez l’URL et collez-la dans un nouvel onglet du navigateur. **Assurez-vous qu’il n’y a pas d’espace vide à la fin de l’URL.**
* Le formulaire adaptatif doit être renseigné avec les données de l’étape précédente.
