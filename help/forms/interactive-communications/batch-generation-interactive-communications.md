---
title: Utilisation de l’API de lot pour générer des documents de communication interactive
description: Exemples de ressources pour générer des documents de canal d’impression à l’aide de l’API de lot
feature: Communication interactive
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 7%

---


# API de lot

Vous pouvez utiliser l’API Batch pour produire plusieurs communications interactives à partir d’un modèle. Le modèle est une communication interactive sans données. L’API Batch combine des données à un modèle afin de produire une communication interactive. L’API est utile pour la production en masse de communications interactives. Par exemple, les factures de téléphone, les relevés de carte de crédit pour plusieurs clients.

[En savoir plus sur l’API de génération de lots](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Cet article fournit des exemples de ressources pour générer des documents de communications interactives à l’aide de l’API Batch.

## Génération de lots à l’aide du dossier de contrôle

* Importez le [modèle de communication interactive](assets/Beneficiaries-confirmation.zip) dans votre serveur AEM Forms.
* Importez la [configuration du dossier de contrôle](assets/batch-generation-api.zip). Cela crée un dossier appelé `batchAPI` dans votre lecteur C.

**Si vous exécutez AEM Forms sur un système d’exploitation autre que Windows, procédez comme suit :**

1. [Ouvrir le dossier de contrôle](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Sélectionnez BatchAPIWatchedFolder et cliquez sur Modifier.
3. Modifiez le chemin pour qu’il corresponde à votre système d’exploitation.

![path](assets/watched-folder-batch-api-basic.PNG)

* Téléchargez et extrayez le contenu du [fichier zip](assets/jsonfile.zip). Le fichier zip contient le dossier `jsonfile` qui contient le fichier `beneficiaries.json`. Ce fichier contient les données pour générer 3 documents.

* Déposez le dossier `jsonfile` dans le dossier input de votre dossier de contrôle.
* Une fois le dossier sélectionné pour traitement, vérifiez le dossier result de votre dossier de contrôle. 3 fichiers PDF devraient être générés.

## Génération de lots à l’aide de requêtes REST

Vous pouvez appeler l’[API de lot](https://helpx.adobe.com/fr/experience-manager/6-5/forms/javadocs/index.html) par le biais de requêtes REST. Vous pouvez exposer les points de fin REST pour que d’autres applications appellent l’API pour générer des documents.
Les exemples de ressources fournis exposent le point de terminaison REST pour générer des documents de communication interactive. Le servlet accepte les paramètres suivants :

* fileName : emplacement du fichier de données sur le système de fichiers.
* templatePath - Chemin d’accès au modèle IC
* saveLocation : emplacement d’enregistrement des documents générés dans le système de fichiers
* channelType - Print,Web ou les deux
* recordId : chemin JSON vers l’élément pour définir le nom d’une communication interactive

La capture d’écran suivante montre les paramètres et leurs valeurs.
![exemple de requête](assets/generate-ic-batch-servlet.PNG)

## Déploiement d’exemples de ressources sur votre serveur

* Importez [ICTemplate](assets/ICTemplate.zip) à l’aide de [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Importez [Gestionnaire d’envoi personnalisé](assets/BatchAPICustomSubmit.zip) à l’aide de [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Importez [Formulaire adaptatif](assets/BatchGenerationAPIAF.zip) à l’aide de l’[interface Forms et Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Déployez et démarrez [Groupe OSGI personnalisé](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) à l’aide de [la console web Felix](http://localhost:4502/system/console/bundles)
* [Déclencher la génération de lot en envoyant le formulaire](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
