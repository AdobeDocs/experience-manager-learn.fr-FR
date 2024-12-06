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
workflow-type: ht
source-wordcount: '209'
ht-degree: 100%

---

# Créer, déployer et tester le composant Pays

Pour créer tous les modules et déployer le package `all` sur une instance locale d’AEM, exécutez l’instruction suivante dans le répertoire racine du projet :

```mvn clean install -PautoInstallSinglePackage```

## Tester le composant

Pour intégrer le composant Pays à votre instance cloud AEM Forms et la configurer, procédez comme suit :

* Extrayez le contenu du fichier compressé [pays](assets/countries.zip). Chaque fichier doit contenir les données d’un continent spécifique.
* Chargez les fichiers JSON sous content/dam/corecomponent. Il s’agit de l’emplacement où le code recherche les fichiers JSON. Si vous souhaitez les stocker à un autre emplacement, vous devez mettre à jour le code Java dans la classe CountriesDropDownImpl. Plus précisément, mettez à jour le chemin d’accès dans la méthode init() où sont chargés les fichiers JSON. Par exemple, si vous souhaitez stocker les fichiers JSON dans content/dam/mydata/, mettez à jour le chemin comme suit :

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* Se connecter à une instance cloud AEM Forms
* Créer un formulaire adaptatif et déposer le composant Pays dans le formulaire
* Configurer le composant Pays à l’aide de l’éditeur de boîte de dialogue et définir les différentes propriétés, y compris le continent
  ![continent](assets/select-continent.png)
* Prévisualiser le formulaire et vérifier que la liste déroulante Pays fonctionne comme prévu

