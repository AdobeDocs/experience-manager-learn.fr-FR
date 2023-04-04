---
title: Configuration d’un formulaire adaptatif pour déclencher AEM Workflow - Aperçu
description: Configuration des options de payload lors du déclenchement AEM processus lors de l’envoi du formulaire
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 7%

---

# Configuration d’un formulaire adaptatif pour déclencher AEM processus

## Prérequis

L’exemple de formulaire utilisé dans ce processus est basé sur un modèle de formulaire adaptatif personnalisé qui doit être importé dans votre serveur AEM. L’exemple de formulaire fourni doit être importé après l’importation du modèle.

### Obtention des modèles de formulaire adaptatif

* Télécharger [Modèle de formulaire adaptatif](assets/af-form-template.zip)
* [Importez le modèle à l’aide du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp)
* Télécharger et installer le modèle de formulaire adaptatif

### Obtention de l’exemple de formulaire adaptatif

* Télécharger [Formulaire adaptatif](assets/peak-application-form.zip)
* Accédez à [Formulaire Et Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur Créer -> Téléchargement du fichier
* L’exemple de formulaire adaptatif est placé dans un dossier appelé [Forms d’application](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

La vidéo suivante explique comment configurer un formulaire adaptatif pour déclencher un processus AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

La vidéo suivante présente la charge utile de workflow et d’autres détails dans le référentiel crx.

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
