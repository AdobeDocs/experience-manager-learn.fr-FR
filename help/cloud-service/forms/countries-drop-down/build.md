---
title: Créer, déployer et tester le composant Pays
description: Créer, déployer et tester
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 2%

---

# Créer, déployer et tester le composant Pays

Pour créer tous les modules et déployer le package `all` sur une instance locale d’AEM, exécutez la commande suivante dans le répertoire racine du projet :

```mvn clean install -PautoInstallSinglePackage```

## Test du composant

Pour intégrer le composant Pays à votre instance AEM Forms Cloud Ready et la configurer, procédez comme suit :

* Extrayez le contenu du fichier zip [countries](assets/countries.zip). Chaque fichier doit contenir les données d’un continent spécifique.
* Téléchargez les fichiers json sous content/dam/corecomponent.This est l’emplacement où le code recherche les fichiers json. Si vous souhaitez stocker les fichiers JSON à un autre emplacement, vous devez mettre à jour le code Java dans la classe CountriesDropDownImpl . Plus précisément, mettez à jour le chemin d’accès dans la méthode init() où les fichiers JSON sont chargés. Par exemple, si vous souhaitez stocker les fichiers JSON dans content/dam/mydata/, mettez à jour le chemin comme suit :

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* Connexion à votre instance AEM Forms prête pour le cloud
* Créez un formulaire adaptatif et déposez le composant Pays dans le formulaire.
* Configurez le composant Pays à l’aide de l’éditeur de boîte de dialogue et définissez les différentes propriétés, y compris le continent.
  ![content](assets/select-continent.png)
* Prévisualiser le formulaire et vérifier que la liste déroulante des pays fonctionne comme prévu

