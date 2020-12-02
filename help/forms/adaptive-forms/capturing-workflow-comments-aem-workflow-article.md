---
title: Capture des commentaires de processus dans le Forms Workflow adaptatif
seo-title: Capture des commentaires de processus dans le Forms Workflow adaptatif
description: Capture des commentaires de processus dans AEM flux de travail
seo-description: Capture des commentaires de processus dans AEM flux de travail
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---


# Capture des commentaires de processus dans le Forms Workflow adaptatif{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[S&#39;applique uniquement à AEM Forms 6.4. Dans AEM Forms 6.5, utilisez la fonction de variables pour obtenir ce cas d&#39;utilisation.]

Une demande courante consiste à inclure dans un courrier électronique les commentaires saisis par le réviseur de tâche. Dans AEM Forms 6.4, il n’existe aucun mécanisme prêt à l’emploi pour capturer les commentaires saisis par l’utilisateur et les inclure dans un courrier électronique.

Pour répondre à cette exigence, un exemple de lot OSGi est fourni qui peut être utilisé pour capturer des commentaires et les stocker en tant que propriété de métadonnées de flux de travail.

La capture d’écran suivante vous montre comment utiliser l’étape de processus dans [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) pour capturer des commentaires et les stocker en tant que propriété de métadonnées. &quot;Capture des commentaires de flux de travaux&quot; est le nom de la classe java qui doit être utilisée dans l’étape du processus. Vous devez transmettre le nom de propriété de métadonnées qui contiendra les commentaires. Dans la capture d’écran ci-dessous, managerComments est la propriété de métadonnées qui stockera les commentaires.

![workflows commentaires1](assets/workflowcomments1.gif)

Pour tester cette fonctionnalité sur votre système, procédez comme suit :
* [Assurez-vous que l’étape de processus du processus est configurée pour utiliser la fonction Capturer les commentaires du processus.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Déploiement du lot Developing withserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Déployez le lot](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) SetValue. Ce lot contient l’exemple de code permettant de capturer les commentaires et de les stocker en tant que propriété de métadonnées.

* [Téléchargez et décompressez les ressources liées à cet article sur votre ](assets/capturecomments.zip) système de fichiers. Les ressources contiennent le modèle de processus et l’exemple de formulaire adaptatif.

* Importez les fichiers zip 2 dans AEM à l’aide du gestionnaire de packages.

* [Prévisualisation du formulaire en accédant à cette URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Renseignez les champs du formulaire et envoyez le formulaire.

* [Vérifiez votre boîte de réception AEM](http://localhost:4502/aem/inbox)

* Ouvrez la tâche à partir de la boîte de réception et envoyez le formulaire. Veuillez saisir quelques commentaires lorsque vous y êtes invité.

Les commentaires seront stockés dans la propriété de métadonnées managerComments dans crx. Pour rechercher les commentaires, connectez-vous à crx en tant qu’administrateur. Les instances de processus sont stockées dans le chemin d’accès suivant :

/var/workflow/instances/server0

Sélectionnez l’instance de processus appropriée et recherchez la propriété managerComments dans le noeud de métadonnées.

