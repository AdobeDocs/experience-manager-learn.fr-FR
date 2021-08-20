---
title: Utilisation du service de modèle de données de formulaire comme étape dans AEM flux de travaux 6.5
description: AEM Forms 6.5 a introduit la possibilité de créer des variables dans le workflow AEM. Grâce à cette nouvelle fonctionnalité, l’utilisation du "service de modèle de données de formulaire d’appel" dans AEM Workflow est devenue très simple. La vidéo suivante vous guide tout au long des étapes à suivre pour utiliser le service de modèle de données de formulaire Invoke dans AEM Workflow.
feature: Workflow
type: Tutorial
version: 6.5.
topic: Développement
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 1%

---


# Utilisation du service de modèle de données de formulaire comme étape dans AEM flux de travaux 6.5 {#using-form-data-model-service-as-step-in-workflow}

À compter d’AEM Forms 6.4, nous pouvons désormais utiliser le service de modèle de données de formulaire dans le cadre d’AEM Workflow. La vidéo suivante décrit les étapes nécessaires à la configuration de l’étape Modèle de données de formulaire dans AEM Workflow

>!![NOTE]La fonctionnalité présentée dans cette vidéo nécessite AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Pour tester cette fonctionnalité sur votre serveur, suivez les instructions ci-dessous.

* Configurez tomcat avec le fichier SampleRest.war comme décrit [ici](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html). Le fichier war déployé dans Tomcat comporte le code pour renvoyer le score de crédit du demandeur. Le score de crédit est un nombre aléatoire compris entre 200 et 800.

* [ Importez les ressources dans AEM à l’aide du gestionnaire de modules.](assets/aem65-loanapplication.zip)
* Le module contient les éléments suivants :

   * Modèle de workflow qui utilise l’étape FDM.
   * Modèle de données de formulaire utilisé à l’étape FDM.
   * Formulaire adaptatif pour déclencher le processus lors de l’envoi.
* Ouvrez le [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Renseignez les détails et envoyez-les. Lors de l’envoi du formulaire, le [processus loanapplication](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) est déclenché.

![ flux de travail ](assets/invokefdm651.PNG).
Le workflow utilise le composant Division ou pour acheminer la demande vers l’administrateur si le score de crédit est supérieur à 500. Si le score de crédit est inférieur à 500, la demande est acheminée vers la cavery.
