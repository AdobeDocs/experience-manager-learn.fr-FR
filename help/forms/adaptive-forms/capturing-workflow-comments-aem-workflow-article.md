---
title: Capture des commentaires de workflow dans le Forms Workflow adaptatif
seo-title: Capture des commentaires de workflow dans le Forms Workflow adaptatif
description: Capture des commentaires de workflow dans AEM workflow
seo-description: Capture des commentaires de workflow dans AEM workflow
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: Workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---


# Capture des commentaires de workflow dans le Forms Workflow adaptatif{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[S’applique uniquement à AEM Forms 6.4. Dans AEM Forms 6.5, utilisez la fonction de variables pour réaliser ce cas d’utilisation.]

Une requête courante consiste à inclure dans un email les commentaires renseignés par le validant de tâche. Dans AEM Forms 6.4, il n’existe aucun mécanisme prêt à l’emploi pour capturer les commentaires saisis par l’utilisateur et les inclure dans un courrier électronique.

Pour répondre à cette exigence, un exemple de lot OSGi est fourni. Il peut être utilisé pour capturer des commentaires et stocker ces commentaires en tant que propriété de métadonnées de workflow.

La capture d’écran suivante montre comment utiliser l’étape de processus dans [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) pour capturer des commentaires et les stocker en tant que propriété de métadonnées. &quot;Capture des commentaires de processus&quot; est le nom de la classe java qui doit être utilisée dans l’étape du processus. Vous devez transmettre le nom de la propriété de métadonnées qui contiendra les commentaires. Dans la capture d’écran ci-dessous, managerComments est la propriété de métadonnées qui stockera les commentaires.

![workflowcomments1](assets/workflowcomments1.gif)

Pour tester cette fonctionnalité sur votre système, procédez comme suit :
* [Assurez-vous que l’étape du processus dans le workflow est configurée pour utiliser la capture des commentaires de processus.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Déployer le lot Developer withserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Déployez le lot SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Ce lot contient un exemple de code pour capturer les commentaires et les stocker en tant que propriété de métadonnées.

* [Téléchargez et décompressez les ressources liées à cet article dans votre ](assets/capturecomments.zip) système de fichiers. Les ressources contiennent un modèle de processus et un exemple de formulaire adaptatif.

* Importez les 2 fichiers zip dans AEM à l’aide du gestionnaire de packages.

* [Aperçu du formulaire en accédant à cette URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Renseignez les champs du formulaire et envoyez-le.

* [Vérifiez votre boîte de réception AEM](http://localhost:4502/aem/inbox)

* Ouvrez la tâche à partir de la boîte de réception et envoyez le formulaire. Entrez des commentaires lorsque vous y êtes invité.

Les commentaires seront stockés dans la propriété de métadonnées appelée managerComments dans crx. Pour rechercher les commentaires, connectez-vous à crx en tant qu’administrateur. Les instances de workflow sont stockées dans le chemin suivant :

/var/workflow/instances/server0

Sélectionnez l’instance de workflow appropriée et recherchez la propriété managerComments dans le noeud metadata .

