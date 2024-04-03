---
title: Préremplir un formulaire adaptatif avec les données d’une liste SharePoint
description: Découvrez comment préremplir un formulaire adaptatif à l’aide d’un modèle de données de formulaire soutenu par une liste SharePoint.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14795
duration: 60
exl-id: 9abe9f9d-8fb3-4e01-a830-1dad1c27274d
source-git-commit: 3dc1aea74e2a7cf30da9f6fb96ecc5c7edcf6e34
workflow-type: ht
source-wordcount: '234'
ht-degree: 100%

---

# Préremplir un formulaire adaptatif avec les données d’une liste SharePoint

Dans la version précédente d’AEM Forms (6.5), du code personnalisé devait être écrit à l’aide de l’attribut de requête pour préremplir un formulaire adaptatif soutenu par un modèle de données formulaire. Dans AEM Forms as a Cloud Service, il n’est plus nécessaire d’écrire du code personnalisé.

Cet article explique les étapes nécessaires pour préremplir un formulaire adaptatif avec des données récupérées à partir d’une liste SharePoint en utilisant le service de préremplissage de modèle de données de formulaire.

Cet article suppose que vous avez [configuré le formulaire adaptatif pour envoyer des données à une liste SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=fr#connect-af-sharepoint-list)

Voici les données de la liste SharePoint :
![sharepoint-list](assets/list-data.png)

Voici les étapes à suivre pour préremplir un formulaire adaptatif avec les données associées à un Guid spécifique :

## Configurer le service Get

* Créez un service Get pour l’objet au niveau supérieur du modèle de données de formulaire à l’aide de l’attribut Guid.
  ![get-service](assets/mapping-request-attribute.png)

Sur cette capture d’écran, la colonne Guid est liée avec un attribut de requête appelé `submissionid`.

Le service Get entièrement configuré ressemble à ceci :

![get-service](assets/fdm-request-attribute.png)

## Configurer le formulaire adaptatif pour utiliser le service de préremplissage de modèle de données de formulaire

* Ouvrez un formulaire adaptatif basé sur le modèle de données de formulaire avec liste SharePoint. Associez le service de préremplissage de modèle de données de formulaire.
  ![form-prefill-service](assets/form-prefill-service.png)

## Tester le formulaire

Prévisualisez le formulaire en incluant le `submissionid` dans l’URL, comme illustré ci-dessous.

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```
