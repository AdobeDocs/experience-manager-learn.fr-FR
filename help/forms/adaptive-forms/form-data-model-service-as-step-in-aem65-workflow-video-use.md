---
title: Utilisation du service de modèle de données de formulaire comme étape dans AEM flux de travail 6.5
seo-title: Utilisation du service de modèle de données de formulaire comme étape dans AEM flux de travail 6.5
description: AEM Forms 6.5 a introduit la possibilité de créer des variables dans AEM Workflow. Grâce à cette nouvelle fonctionnalité qui utilise le service de modèle de données de formulaire "Invoke Form Data Model Service" dans le flux de travail AEM, est devenu très facile. La vidéo suivante décrit les étapes de l’utilisation du service de modèle de données de formulaire appelé dans AEM Workflow.
seo-description: AEM Forms 6.5 a introduit la possibilité de créer des variables dans AEM Workflow. Grâce à cette nouvelle fonctionnalité qui utilise le service de modèle de données de formulaire "Invoke Form Data Model Service" dans le flux de travail AEM, est devenu très facile. La vidéo suivante décrit les étapes de l’utilisation du service de modèle de données de formulaire appelé dans AEM Workflow.
feature: Workflow
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: Développement
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 1%

---


# Utilisation du service de modèle de données de formulaire comme étape dans AEM flux de travail 6.5 {#using-form-data-model-service-as-step-in-workflow}

A compter de AEM Forms 6.4, nous avons désormais la possibilité d’utiliser le service de modèle de données de formulaire dans le cadre de AEM Workflow. La vidéo suivante décrit les étapes nécessaires à la configuration de l’étape Modèle de données de formulaire dans le flux de travail AEM.

>!![NOTE]La fonctionnalité présentée dans cette vidéo nécessite AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Pour tester cette fonctionnalité sur votre serveur, suivez les instructions ci-dessous.

* Configurez tomcat avec le fichier SampleRest.war comme décrit [ici](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Le fichier de guerre déployé dans Tomcat a le code pour renvoyer le score de crédit du demandeur.Le score de crédit est un nombre aléatoire compris entre 200 et 800.

* [ Importation des actifs dans AEM à l’aide du gestionnaire de packages](assets/aem65-loanapplication.zip)
* Le package contient les éléments suivants :

   * Modèle de flux de travail qui utilise l’étape FDM.
   * Modèle de données de formulaire utilisé à l’étape FDM.
   * Formulaire adaptatif pour déclencher le processus lors de l’envoi.
* Ouvrez [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Renseignez les détails et envoyez. Lors de l’envoi du formulaire, le [processus de demande de prêt](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) est déclenché.

![ flux de travail ](assets/invokefdm651.PNG).
Le processus utilise le composant Ou fractionner pour acheminer la demande vers l’administrateur si le score de crédit est supérieur à 500. Si la note de crédit est inférieure à 500, la demande est acheminée vers la citation.
