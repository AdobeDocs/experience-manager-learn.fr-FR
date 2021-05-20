---
title: Configuration d’un formulaire adaptatif pour déclencher AEM processus
description: Configuration des options de payload lors du déclenchement AEM processus lors de l’envoi du formulaire
sub-product: formulaires
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 9%

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
* Accédez à [Formulaire et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur Créer -> Téléchargement du fichier
* L’exemple de formulaire adaptatif sera placé dans un dossier appelé [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

La vidéo suivante explique comment configurer un formulaire adaptatif pour déclencher un processus AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

La vidéo suivante présente la charge utile de workflow et d’autres détails dans le référentiel crx.

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)


