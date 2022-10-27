---
title: Utilisation de l’API de lot pour générer des documents de communication interactive
description: Exemples de ressources pour générer des documents de canal d’impression à l’aide de l’API de lot
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 20%

---

# API de lot

Vous pouvez utiliser l’API Batch pour générer plusieurs communications interactives à partir d’un modèle. Le modèle consiste en une communication interactive sans données. L’API Batch combine des données avec un modèle pour créer une communication interactive. L’API est utile pour la production en masse de communications interactives. Par exemple, les factures de téléphone ou les relevés de cartes de crédit pour plusieurs clients.

[En savoir plus sur l’API de génération de lots](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Cet article fournit des exemples de ressources pour générer des documents de communications interactives à l’aide de l’API Batch.

## Génération de lots à l’aide du dossier de contrôle

* Importez la variable [Modèle de communication interactive](assets/Beneficiaries-confirmation.zip) dans votre serveur AEM Forms.
* Importez la variable [configuration du dossier de contrôle](assets/batch-generation-api.zip). Cela crée un dossier appelé `batchAPI` dans votre lecteur C.

**Si vous exécutez AEM Forms sur un système d’exploitation autre que Windows, procédez comme suit :**

1. [Ouvrir le dossier de contrôle](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Sélectionnez BatchAPIWatchedFolder et cliquez sur Modifier.
3. Modifiez le chemin pour qu’il corresponde à votre système d’exploitation.

![le chemin](assets/watched-folder-batch-api-basic.PNG)

* Télécharger et extraire le contenu de [fichier zip](assets/jsonfile.zip). Le fichier zip contient le dossier nommé `jsonfile` qui contient `beneficiaries.json` fichier . Ce fichier contient les données pour générer 3 documents.

* Déposez le `jsonfile` dans le dossier input de votre dossier de contrôle.
* Une fois le dossier sélectionné pour traitement, vérifiez le dossier result de votre dossier de contrôle. 3 fichiers PDF devraient être générés.

## Génération de lots à l’aide de requêtes REST

Vous pouvez appeler le [API de lot](https://helpx.adobe.com/fr/experience-manager/6-5/forms/javadocs/index.html) par le biais de requêtes REST. Vous pouvez exposer les points de fin REST pour que d’autres applications appellent l’API pour générer des documents.
Les exemples de ressources fournis exposent le point de terminaison REST pour générer des documents de communication interactive. Le servlet accepte les paramètres suivants :

* fileName : emplacement du fichier de données sur le système de fichiers.
* templatePath - Chemin d’accès au modèle IC
* saveLocation : emplacement d’enregistrement des documents générés dans le système de fichiers
* channelType - Print,Web ou les deux
* recordId : chemin JSON vers l’élément pour définir le nom d’une communication interactive

La capture d’écran suivante montre les paramètres et leurs valeurs.
![exemple de requête](assets/generate-ic-batch-servlet.PNG)

## Déploiement d’exemples de ressources sur votre serveur

* Importer [ICTemplate](assets/ICTemplate.zip) using [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Importer [Gestionnaire d’envoi personnalisé](assets/BatchAPICustomSubmit.zip) using [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Importer [Formulaire adaptatif](assets/BatchGenerationAPIAF.zip) en utilisant la variable [Interface de Forms et de document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Déployer et démarrer [Groupe OSGI personnalisé](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) using [Console web Felix](http://localhost:4502/system/console/bundles)
* [Déclencher la génération de lot en envoyant le formulaire](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
