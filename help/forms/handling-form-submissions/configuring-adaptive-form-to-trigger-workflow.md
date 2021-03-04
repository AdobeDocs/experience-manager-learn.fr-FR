---
title: Configuration d’un formulaire adaptatif pour déclencher AEM processus
description: Configurez les options de charge utile lors du déclenchement du processus AEM lors de l’envoi du formulaire.
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
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 10%

---


# Configuration d’un formulaire adaptatif pour déclencher AEM processus

## Conditions préalables

L’exemple de formulaire utilisé dans ce flux de travail est basé sur un modèle de formulaire adaptatif personnalisé qui doit être importé dans votre serveur AEM. L’exemple de formulaire fourni doit être importé après l’importation du modèle.

### Obtention des modèles de formulaire adaptatif

* Télécharger [Modèle de formulaire adaptatif](assets/af-form-template.zip)
* [Importation du modèle à l’aide du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp)
* Téléchargement et installation du modèle de formulaire adaptatif

### Obtention de l’exemple de formulaire adaptatif

* Télécharger [Formulaire adaptatif](assets/peak-application-form.zip)
* Accédez à [Formulaire et Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur Créer -> Télécharger le fichier
* L’exemple de formulaire adaptatif sera placé dans un dossier appelé [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

La vidéo suivante explique comment configurer un formulaire adaptatif pour déclencher un flux de travail AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

La vidéo suivante présente la charge utile de flux de travaux et d’autres détails dans le référentiel crx.

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)


