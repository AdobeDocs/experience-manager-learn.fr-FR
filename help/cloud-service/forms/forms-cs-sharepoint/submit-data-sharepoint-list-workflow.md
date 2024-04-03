---
title: Envoyer des données vers une liste SharePoint à l’aide d’une étape de workflow
description: Insérer des données dans une liste SharePoint à l’aide de l’étape de workflow d’appel FDM
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
source-git-commit: c2b969829dc44e8235abafe0b53040b9c50fb91b
workflow-type: ht
source-wordcount: '245'
ht-degree: 100%

---

# Insérer des données dans une liste SharePoint à l’aide de l’étape de workflow d’appel FDM


Cet article explique les étapes requises pour insérer des données dans une liste SharePoint à l’aide de l’étape d’appel FDM dans un workflow AEM.

Cet article suppose que vous avez [configuré le formulaire adaptatif pour envoyer des données à une liste SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=fr#connect-af-sharepoint-list)


## Créer un modèle de données de formulaire basé sur la source de données « liste SharePoint »

* Créez un nouveau modèle de données de formulaire basé sur la source de données « liste SharePoint ».
* Ajoutez le modèle approprié et le service Get du modèle de données de formulaire.
* Configurez le service Insert pour insérer l’objet de modèle du niveau supérieur.
* Testez le service Insert.


## Créer un workflow

* Créez un workflow simple à l’aide de l’étape d’appel FDM.
* Configurez l’étape d’appel FDM pour utiliser le modèle de données de formulaire créé lors de l’étape précédente.
* ![associate-fdm](assets/fdm-insert-1.png)

* ![map-input-parameters](assets/fdm-insert-2.png)
* Notez l’utilisation de la notation par points JSON. Les données envoyées sont au format ci-dessous et nous procédons à l’extraction de l’objet ContactUS à partir des données envoyées.

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



## Configurer un formulaire adaptatif pour déclencher un workflow AEM

* Créez un formulaire adaptatif basé sur le modèle de données de formulaire créé lors de l’étape précédente.
* Faites glisser certains champs depuis la source de données vers votre formulaire.
* Configurer l’action d’envoi du formulaire comme illustré ci-dessous
* ![submit-action](assets/configure-af.png)



## Tester le formulaire

Prévisualisez le formulaire créé lors de l’étape précédente. Remplissez le formulaire et envoyez-le. Les données du formulaire doivent être insérées dans la liste SharePoint.
