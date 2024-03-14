---
title: Envoi de données vers la liste SharePoint à l’aide de l’étape de workflow
description: Insérez des données dans la liste sharepoint à l’aide de l’étape de workflow d’appel FDM
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
source-git-commit: 3dc1aea74e2a7cf30da9f6fb96ecc5c7edcf6e34
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 3%

---

# Insérez des données dans la liste SharePoint à l’aide de l’étape de workflow d’appel FDM


Cet article explique les étapes requises pour insérer des données dans la liste SharePoint à l’aide de l’étape d’appel FDM dans AEM workflow.

Cet article suppose que vous avez [formulaire adaptatif correctement configuré pour envoyer des données à la liste SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=fr#connect-af-sharepoint-list)


## Création d’un modèle de données de formulaire basé sur la source de données de liste SharePoint

* Créez un modèle de données de formulaire basé sur la source de données de liste SharePoint.
* Ajoutez le modèle approprié et le service get du modèle de données de formulaire.
* Configurez le service insert pour insérer l’objet de modèle de niveau supérieur.
* Testez le service d’insertion.


## Créer un workflow

* Créez un workflow simple avec l’étape d’appel FDM.
* Configurez l’étape d’appel FDM pour utiliser le modèle de données de formulaire créé à l’étape précédente.
* ![associer-fdm](assets/fdm-insert-1.png)

* ![map-input-parameters](assets/fdm-insert-2.png)
* Notez l’utilisation de la notation par points JSON. Les données envoyées sont au format ci-dessous et nous extrayons l’objet ContactUS des données envoyées.

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```



## Configuration d’un formulaire adaptatif pour déclencher AEM processus

* Créez un formulaire adaptatif à l’aide du modèle de données de formulaire créé à l’étape précédente.
* Faites glisser certains champs de la source de données vers votre formulaire.
* Configurer l’action d’envoi du formulaire comme illustré ci-dessous
* ![submit-action](assets/configure-af.png)



## Tester le formulaire

Prévisualisez le formulaire créé à l’étape précédente. Remplissez le formulaire et envoyez-le. Les données du formulaire doivent être insérées dans la liste SharePoint.

