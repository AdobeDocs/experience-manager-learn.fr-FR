---
title: Utiliser l’API Batch pour générer des documents de communication interactive
description: Exemples de ressources pour générer des documents de canal d’impression à l’aide de l’API Batch
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
workflow-type: ht
source-wordcount: '414'
ht-degree: 100%

---

# API Batch

Vous pouvez utiliser l’API Batch pour générer plusieurs communications interactives à partir d’un modèle. Le modèle consiste en une communication interactive sans données. L’API Batch combine des données avec un modèle pour créer une communication interactive. L’API est utile pour la production en masse de communications interactives. Par exemple, les factures de téléphone ou les relevés de cartes de crédit pour plusieurs clients.

[En savoir plus sur l’API Batch Generation](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html?lang=fr).

Cet article fournit des exemples de ressources pour générer des documents de communication interactive à l’aide de l’API Batch.

## Génération par lots à l’aide du dossier de contrôle

* Importez le [modèle de communication interactive](assets/Beneficiaries-confirmation.zip) sur votre serveur AEM Forms.
* Importez la [configuration du dossier de contrôle](assets/batch-generation-api.zip). Cette opération entraîne la création du dossier `batchAPI` sur le lecteur C.

**Si vous exécutez AEM Forms sur un système d’exploitation autre que Windows, procédez comme suit :**

1. [Ouvrez le dossier de contrôle](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html).
2. Sélectionnez BatchAPIWatchedFolder et cliquez sur Modifier.
3. Modifiez le chemin d’accès en fonction de votre système d’exploitation.

![Chemin.](assets/watched-folder-batch-api-basic.PNG)

* Téléchargez et procédez à l’extraction du contenu du [fichier zip](assets/jsonfile.zip). Le fichier zip renferme un dossier nommé `jsonfile`, qui contient le fichier `beneficiaries.json`. Ce fichier contient les données pour générer3 documents.

* Déposez le dossier `jsonfile` dans le dossier d’entrée du dossier de contrôle.
* Une fois le dossier sélectionné pour traitement, vérifiez le dossier de résultats du dossier de contrôle. Il doit contenir 3 fichiers PDF générés.

## Génération pat lots à l’aide de requêtes REST

Vous pouvez appeler l’[API Batch](https://helpx.adobe.com/fr/experience-manager/6-5/forms/javadocs/index.html) par le biais de requêtes REST. Vous pouvez exposer les points d’entrée REST pour que d’autres applications appellent l’API pour générer des documents.
Les exemples de ressources fournis exposent le point d’entrée REST pour générer des documents de communication interactive. Le servlet accepte les paramètres suivants :

* fileName : emplacement du fichier de données sur le système de fichiers.
* templatePath : chemin d’accès au modèle de communication interactive.
* saveLocation : emplacement d’enregistrement des documents générés dans le système de fichiers.
* channelType : impression, web ou les deux.
* recordId : chemin JSON vers l’élément permettant de définir le nom d’une communication interactive.

La copie d’écran suivante montre les paramètres et les valeurs.
![Exemple de requête.](assets/generate-ic-batch-servlet.PNG)

## Déployer des exemples de ressources sur votre serveur

* Importer [ICTemplate](assets/ICTemplate.zip) à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp)
* Importer le [gestionnaire d’envoi personnalisé](assets/BatchAPICustomSubmit.zip) à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp)
* Importer un [Formulaire adaptatif](assets/BatchGenerationAPIAF.zip) dans l’[interface des formulaires et des documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Déployer et démarrer le [lot OSGI personnalisé](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) à l’aide de la [console web Felix](http://localhost:4502/system/console/bundles)
* [Déclencher la génération par lots lors de la soumission du formulaire](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
