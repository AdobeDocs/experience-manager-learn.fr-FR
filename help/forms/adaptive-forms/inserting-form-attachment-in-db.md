---
title: 'Insérer une pièce jointe de formulaire dans la base de données '
description: Insérez des pièces jointes de formulaire dans la base de données à l’aide du processus AEM.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 10488
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# Insérer un attachement de formulaire dans la base de données

Cet article décrit le cas pratique de stockage de pièces jointes de formulaire dans la base de données MySQL.

Une tâche courante des clients consiste à stocker les données de formulaire capturées et la pièce jointe de formulaire dans un tableau de base de données.
Pour réaliser ce cas pratique, les étapes suivantes ont été suivies :

## Créer un tableau de base de données destiné à contenir les données de formulaire et la pièce jointe

Un tableau appelé newhire a été créé pour contenir les données de formulaire. Remarquez l’image du nom de colonne de type **LONGBLOB** pour stocker la pièce jointe du formulaire
![table-schema](assets/insert-picture-table.png)

## Création d’un modèle de données de formulaire

Un modèle de données de formulaire a été créé pour communiquer avec la base de données MySQL. Vous devez créer les éléments suivants :

* [Source de données JDBC dans AEM](./data-integration-technical-video-setup.md)
* [Modèle de données de formulaire basé sur la source de données JDBC](./jdbc-data-model-technical-video-use.md)

## Créer un workflow

Lorsque vous configurez votre formulaire adaptatif pour l’envoyer à un processus AEM, vous avez la possibilité d’enregistrer les pièces jointes du formulaire dans une variable de processus ou d’enregistrer les pièces jointes dans un dossier spécifié sous la charge utile. Pour ce cas d’utilisation, nous devons enregistrer les pièces jointes dans une variable de workflow de type ArrayList de Document. À partir de cette liste ArrayList, nous devons extraire le premier élément et initialiser une variable document . Les variables de workflow appelées **listOfDocuments** et **employeePhoto** ont été créées.
Lorsque le formulaire adaptatif est envoyé pour déclencher le processus, une étape du processus initialise la variable employeePhoto à l’aide du script ECMA. Voici le code de script ECMA :

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

L’étape suivante du processus consiste à insérer les données et la pièce jointe du formulaire dans le tableau à l’aide du composant de service Invoquer le modèle de données de formulaire .
![insert-pic](assets/fdm-insert-pic.png)
[Le workflow complet avec l’exemple de script ecma peut être téléchargé ici](assets/add-new-employee.zip).

>[!NOTE]
> Vous devrez créer un modèle de données de formulaire basé sur JDBC et utiliser ce modèle de données de formulaire dans le workflow.

## Créer un formulaire adaptatif

Créez votre formulaire adaptatif en fonction du modèle de données de formulaire créé à l’étape précédente. Faites glisser et déposez les éléments de modèle de données de formulaire sur le formulaire. Configurez l’envoi du formulaire pour déclencher le workflow et spécifiez les propriétés suivantes, comme illustré dans la capture d’écran ci-dessous.
![form-attachments](assets/form-attachments.png)