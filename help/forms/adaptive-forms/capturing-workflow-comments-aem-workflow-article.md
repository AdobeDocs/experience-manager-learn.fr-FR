---
title: Capturer des commentaires de workflow dans le workflow des formulaires adaptatifs
description: Capturer des commentaires de workflow dans le workflow AEM
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 83
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 100%

---

# Capturer des commentaires de workflow dans le workflow des formulaires adaptatifs{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Cela s’applique uniquement à AEM Forms 6.4. Dans AEM Forms 6.5, utilisez la fonction des variables pour réaliser ce cas d’utilisation.]

Une requête courante consiste à inclure dans un e-mail les commentaires renseignés par le reviseur ou la réviseuse de tâche. Dans AEM Forms 6.4, il n’existe aucun mécanisme prêt à l’emploi pour capturer les commentaires saisis par l’utilisateur ou l’utilisatrice et les inclure dans un e-mail.

Pour répondre à cette exigence, un exemple de lot OSGi est fourni. Il peut être utilisé pour capturer des commentaires et les stocker en tant que propriété de métadonnées de workflow.

La copie d’écran suivante montre comment utiliser l’étape de processus dans le [workflow AEM](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) pour capturer des commentaires et les stocker en tant que propriété de métadonnées. « Capturer des commentaires de workflow » est le nom de la classe Java qui doit être utilisée dans l’étape du processus. Vous devez transmettre le nom de la propriété de métadonnées qui contiendra les commentaires. Dans la copie d’écran ci-dessous, managerComments est la propriété de métadonnées qui stockera les commentaires.

![workflowcomments1](assets/workflowcomments1.gif)

Pour tester cette fonctionnalité sur votre système, veuillez procéder comme suit :
* [Assurez-vous que l’étape du processus dans le workflow est configurée pour utiliser la capture des commentaires de workflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Déployez le lot Developingwithserviceuser.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Déployez le lot SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Ce lot contient un exemple de code pour capturer les commentaires et les stocker en tant que propriété de métadonnées.

* [Téléchargez et décompressez les ressources liées à cet article sur votre système de fichiers.](assets/capturecomments.zip) Les ressources contiennent un modèle de workflow et un exemple de formulaire adaptatif.

* Importez les 2 fichiers zip dans AEM à l’aide du gestionnaire de packages.

* [Prévisualisez le formulaire en accédant à cette URL.](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Renseignez les champs du formulaire et envoyez-le.

* [Consultez votre boîte de réception AEM.](http://localhost:4502/aem/inbox)

* Ouvrez la tâche à partir de la boîte de réception et envoyez le formulaire. Veuillez saisir des commentaires lorsqu’on vous y invite.

Les commentaires sont stockés dans la propriété de métadonnées appelée `managerComments` dans le référentiel AEM. Pour rechercher les commentaires, connectez-vous à crx en tant qu’administrateur ou administratrice. Les instances de workflow sont stockées dans le chemin suivant :

`/var/workflow/instances/server0`

Sélectionnez l’instance de workflow appropriée et recherchez la propriété managerComments dans le nœud des métadonnées.
