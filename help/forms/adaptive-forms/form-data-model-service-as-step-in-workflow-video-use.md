---
title: Utilisation du service de modèle de données de formulaire comme étape dans le processus
seo-title: Utilisation du service de modèle de données de formulaire comme étape dans le processus
description: À compter d’AEM Forms 6.4, nous pouvons désormais utiliser le modèle de données de formulaire dans le cadre d’AEM Workflow. La vidéo suivante décrit les étapes nécessaires à la configuration de l’étape Modèle de données de formulaire dans AEM Workflow.
seo-description: À compter d’AEM Forms 6.4, nous pouvons désormais utiliser le modèle de données de formulaire dans le cadre d’AEM Workflow. La vidéo suivante décrit les étapes nécessaires à la configuration de l’étape Modèle de données de formulaire dans AEM Workflow.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 0%

---


# Utilisation du service de modèle de données de formulaire comme étape dans le workflow {#using-form-data-model-service-as-step-in-workflow}

À compter d’AEM Forms 6.4, nous pouvons désormais utiliser le modèle de données de formulaire dans le cadre d’AEM Workflow. La vidéo suivante décrit les étapes nécessaires à la configuration de l’étape Modèle de données de formulaire dans AEM Workflow


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Pour tester cette fonctionnalité sur votre serveur, suivez les instructions ci-dessous.
* [Téléchargez et déployez le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé qui définit les propriétés de métadonnées.
>!![NOTE]Dans AEM Forms 6.5 et versions ultérieures, cette fonctionnalité est disponible clé en main, comme  [décrit ici](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Configurez tomcat avec le fichier SampleRest.war comme décrit [ici](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html). Le fichier war déployé dans Tomcat comporte le code pour renvoyer le score de crédit du demandeur. Le score de crédit est un nombre aléatoire compris entre 200 et 800

* [Importez les actifs dans AEM à l’aide de Package Manager](assets/invoke-fdm-as-service-step.zip). Le package contient les éléments suivants :

   * Modèle de workflow qui utilise l’étape FDM.
   * Modèle de données de formulaire utilisé à l’étape FDM.
   * Formulaire adaptatif pour déclencher le processus lors de l’envoi.
* Ouvrez le [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Renseignez les détails et envoyez-les. Lors de l’envoi du formulaire, le [processus loanapplication](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) est déclenché.

![ flux de travail ](assets/fdm-as-service-step-workflow.PNG).
Le workflow utilise le composant Division ou pour acheminer la demande vers l’administrateur si le score de crédit est supérieur à 500. Si le score de crédit est inférieur à 500, la demande est acheminée vers la censure
