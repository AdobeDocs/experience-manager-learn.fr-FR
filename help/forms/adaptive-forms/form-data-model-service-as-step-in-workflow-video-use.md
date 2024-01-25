---
title: Utiliser le service de modèle de données de formulaire comme étape dans le workflow
description: À partir d’AEM Forms 6.4, nous avons la possibilité d’utiliser le modèle de données de formulaire dans le cadre du workflow AEM. La vidéo suivante décrit les étapes nécessaires à la configuration de l’étape Modèle de données de formulaire dans le workflow AEM.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 213
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 100%

---

# Utiliser le service de modèle de données de formulaire comme étape dans le workflow {#using-form-data-model-service-as-step-in-workflow}

À partir d’AEM Forms 6.4, nous avons la possibilité d’utiliser le modèle de données de formulaire dans le cadre du workflow AEM. La vidéo suivante décrit les étapes nécessaires à la configuration de l’étape Modèle de données de formulaire dans le workflow AEM.


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

Pour tester cette fonctionnalité sur votre serveur, suivez les instructions ci-dessous.
* [Téléchargez et déployez le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé qui définit les propriétés de métadonnées.
>Dans AEM Forms 6.5 et versions ultérieures, cette fonctionnalité est disponible prête à l’emploi tel que [décrit ici](form-data-model-service-as-step-in-aem65-workflow-video-use.md).

* Configurez Tomcat avec le fichier SampleRest.war tel que décrit [ici](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=fr). Le fichier war déployé dans Tomcat contient le code permettant de renvoyer le score de crédit du demandeur. Le score de crédit est un nombre aléatoire compris entre 200 et 800.

* [Importez les ressources dans AEM à l’aide du gestionnaire de packages.](assets/invoke-fdm-as-service-step.zip). Le package contient les éléments suivants :

   * Le modèle de workflow qui utilise l’étape Modèle de données de formulaire.
   * Le modèle de données de formulaire utilisé à l’étape Modèle de données de formulaire.
   * Le formulaire adaptatif pour déclencher le workflow lors de l’envoi.
* Ouvrez le formulaire [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Remplissez les informations et envoyez-le. Lors de l’envoi du formulaire, le [workflow loanapplication](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) est déclenché.

![Workflow](assets/fdm-as-service-step-workflow.PNG).
Le workflow utilise le composant Division OU pour acheminer la demande vers l’administrateur ou l’administratrice si le score de crédit est supérieur à 500. Si le score de crédit est inférieur à 500, la demande est acheminée vers le cavery.
