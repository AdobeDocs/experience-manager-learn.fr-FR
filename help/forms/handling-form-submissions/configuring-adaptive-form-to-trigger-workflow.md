---
title: Vue d’ensemble de la configuration d’un formulaire adaptatif pour déclencher un workflow AEM
description: Configurer des options de payload lors du déclenchement du workflow AEM à l’envoi d’un formulaire
feature: Workflow
doc-type: article
version: 6.4,6.5
jira: KT-5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
duration: 583
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 100%

---

# Configurer un formulaire adaptatif pour déclencher un workflow AEM

## Conditions préalables

L’exemple de formulaire utilisé dans ce workflow est basé sur un modèle de formulaire adaptatif personnalisé qui doit être importé dans votre serveur AEM. L’exemple de formulaire fourni doit être importé après l’import du modèle.

### Obtenir les modèles de formulaire adaptatif

* Téléchargez un [modèle de formulaire adaptatif](assets/af-form-template.zip).
* [Importez le modèle à l’aide du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Chargez et installez le modèle de formulaire adaptatif.

### Obtenir l’exemple de formulaire adaptatif

* Téléchargez le [formulaire adaptatif](assets/peak-application-form.zip).
* Accédez à [Formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Cliquez sur Créer > Charger un fichier.
* L’exemple de formulaire adaptatif est placé dans un dossier appelé [Formulaires de l’application](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms).

La vidéo suivante explique comment configurer un formulaire adaptatif pour déclencher un workflow AEM.
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

La vidéo suivante présente la payload de workflow et d’autres détails dans le référentiel crx.

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
