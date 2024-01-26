---
title: Stocker et récupérer des données de formulaire à partir de la base de données MySQL - Déploiement
description: Tutoriel en plusieurs parties détaillant les étapes impliquées dans le stockage et la récupération des données de formulaire
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 67
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 100%

---

# Déployer ceci sur votre serveur

>[!NOTE]
>
>Les éléments suivants sont nécessaires pour que cette opération s’exécute sur votre système.
>
>* AEM Forms(version 6.3 ou ultérieure)
>* Base de données MySql

Pour tester cette fonctionnalité sur votre instance AEM Forms, procédez comme suit :

* Téléchargez et déployez les fichiers [MySql Driver Jar](assets/mysqldriver.jar) à l’aide de la [console web Felix](http://localhost:4502/system/console/bundles).
* Téléchargez et déployez le [lot OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) en utilisant la [console web Felix](http://localhost:4502/system/console/bundles).
* Téléchargez et installez le [package contenant la bibliothèque cliente, le modèle de formulaire adaptatif et le composant de page personnalisé](assets/store-and-fetch-af-with-data.zip) en utilisant le [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Importez l’[exemple de formulaire adaptatif](assets/sample-adaptive-form.zip) en utilisant l’[interface Formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).

* Importez [form-data-db.sql](assets/form-data-db.sql) à l’aide de MySql Workbench. Vous créez ainsi le schéma et les tableaux nécessaires dans votre base de données pour que ce turoriel fonctionne.
* Connectez-vous à [configMgr.](http://localhost:4502/system/console/configMgr) Recherchez « Apache Sling Connection Pooled DataSource » (source de données mise en pool de la connexion Apache Sling). Créez une nouvelle entrée de source de données mise en pool de la connexion Apache Sling appelée **SaveAndContinue** à l’aide des propriétés suivantes :

| Nom de la propriété | Valeur |
| ------------------------|---------------------------------------|
| Nom de la source de données | SaveAndContinue |
| Classe de pilote JDBC | com.mysql.cj.jdbc.Driver |
| URI de connexion JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

* Ouvrez le [formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled).
* Renseignez certains détails et cliquez sur le bouton « Enregistrer et continuer plus tard ».
* Vous devriez recevoir une URL contenant un GUID.
* Copiez l’URL et collez-la dans un nouvel onglet du navigateur. **Assurez-vous qu’il n’y a pas d’espace vide à la fin de l’URL.**
* Le formulaire adaptatif doit être renseigné avec les données de l’étape précédente.
