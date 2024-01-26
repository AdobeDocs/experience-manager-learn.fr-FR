---
title: Utiliser le service de modèle de données de formulaire comme étape dans le workflow d’AEM 6.5
description: AEM Forms 6.5 a introduit la possibilité de créer des variables dans le workflow AEM. Grâce à cette nouvelle fonctionnalité, utiliser « Appeler le service de modèle de données de formulaire » dans le workflow AEM est devenue très facile. La vidéo suivante vous guide tout au long des étapes à suivre pour utiliser Appeler le service de modèle de données de formulaire dans le workflow AEM.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
duration: 253
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 100%

---

# Utiliser le service de modèle de données de formulaire comme étape dans le workflow d’AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

À partir d’AEM Forms 6.4, nous avons la possibilité d’utiliser le service de modèle de données de formulaire dans le cadre du workflow AEM. La vidéo suivante décrit les étapes nécessaires à la configuration de l’étape Modèle de données de formulaire dans le workflow AEM.

>La fonctionnalité présentée dans cette vidéo nécessite AEM Forms 6.5.1.


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

Pour tester cette fonctionnalité sur votre serveur, suivez les instructions ci-dessous.

* Configurez Tomcat avec le fichier SampleRest.war comme décrit [ici](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/set-up-tomcat.html?lang=fr). Le fichier war déployé dans Tomcat contient le code permettant de renvoyer le score de crédit du demandeur ou de la demandeuse. Le score de crédit est un nombre aléatoire compris entre 200 et 800.

* [Importez les ressources dans AEM à l’aide du gestionnaire de packages.](assets/aem65-loanapplication.zip)
* Le package contient les éléments suivants :

   * Le modèle de workflow qui utilise l’étape Modèle de données de formulaire.
   * Le modèle de données de formulaire utilisé à l’étape Modèle de données de formulaire.
   * Le formulaire adaptatif pour déclencher le workflow lors de l’envoi.
* Ouvrez le formulaire [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Remplissez les informations et envoyez-le. Lors de l’envoi du formulaire, le [workflow loanapplication](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) est déclenché.

![Workflow](assets/invokefdm651.PNG).
Le workflow utilise le composant Division OU pour acheminer la demande vers l’administrateur ou l’administratrice si le score de crédit est supérieur à 500. Si le score de crédit est inférieur à 500, la demande est acheminée vers le cavery.
