---
title: Extraire un document de la liste des documents dans un workflow AEM
description: Composant de workflow personnalisé pour extraire un document spécifique d’une liste de documents
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
source-git-commit: bac637440d1cc5af0e0abb119ca2f4e93f69cf34
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# Extraire un document de la liste des documents

Un cas d’utilisation courant consiste à envoyer les données de formulaire et la pièce jointe à un système externe à l’aide de l’étape d’appel du modèle de données de formulaire dans un processus AEM. Par exemple, lors de la création d’un cas dans ServiceNow, vous souhaitez envoyer les détails du cas avec un document à l’appui. Les pièces jointes ajoutées au formulaire adaptatif sont stockées dans une variable de type liste de tableaux de documents et pour extraire un document spécifique de cette liste de tableaux, vous devez écrire du code personnalisé.

Cet article décrit les étapes à suivre pour utiliser le composant de workflow personnalisé afin d’extraire et de stocker le document dans une variable de document.

## Créer un workflow

Un processus doit être créé pour gérer l’envoi du formulaire. Les variables suivantes doivent être définies pour le workflow.

* Une variable de type ArrayList de Document (cette variable contient les pièces jointes de formulaire ajoutées par l’utilisateur).
* Variable de type Document.(Cette variable contiendra le document extrait de ArrayList)

* Ajoutez le composant personnalisé à votre workflow et configurez ses propriétés
  ![extract-item-workflow](assets/extract-document-array-list.png)

## Configurer un formulaire adaptatif

* Configuration de l’action d’envoi du formulaire adaptatif pour déclencher le processus d’AEM
  ![submit-action](assets/store-attachments.png)

## Tester la solution

[Déployer le lot personnalisé à l’aide de la console web OSGi](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[Importez le composant de workflow à l’aide du gestionnaire de modules](assets/Extract-item-from-documents-list.zip)

[Importer l’exemple de workflow](assets/extract-item-sample-workflow.zip)

[Importation du formulaire adaptatif](assets/test-attachment-extractions-adaptive-form.zip)

[Aperçu du formulaire](http://localhost:4502/content/dam/formsanddocuments/testattachmentsextractions/jcr:content?wcmmode=disabled)

Ajoutez une pièce jointe au formulaire et envoyez-la.

>[!NOTE]
>
>Le document extrait peut ensuite être utilisé dans toute autre étape de workflow telle que Envoyer un courrier électronique ou Invoquer l’étape FDM.




