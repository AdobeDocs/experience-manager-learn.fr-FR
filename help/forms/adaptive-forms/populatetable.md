---
title: 'Renseigner le tableau de formulaire adaptatif '
seo-title: Renseigner le tableau de formulaire adaptatif
description: Renseignez le tableau Formulaire adaptatif avec les résultats des appels de service de modèles de données de formulaire.
seo-description: Renseignez le tableau Formulaire adaptatif avec les résultats des appels de service de modèles de données de formulaire.
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Renseignez le tableau Formulaire adaptatif avec les résultats de l’appel du service de modèle de données de formulaire.

[Le formulaire en direct est hébergé ici](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)Dans cet article, nous examinons comment renseigner le tableau des formulaires adaptatifs en récupérant les données issues de l’appel de service de modèle de données de formulaire. Nous allons créer un calendrier d&#39;amortissement dans un tableau qui liste chaque paiement régulier d&#39;une hypothèque au fil du temps. Les résultats de l&#39;amortissement sont retournés par notre modèle de données de formulaire. Le service Modèle de données de formulaire est appelé sur le événement de clic du bouton Calculer, comme le montre la capture d’écran. Les paramètres d’entrée et de sortie de l’appel de service sont mappés de manière appropriée, comme le montre la capture d’écran. La sortie est mappée aux colonnes de Row1![clickevent](assets/amortization.PNG)

La ligne 1 est configurée pour augmenter en fonction des données renvoyées par l’appel de service. Notez les paramètres de répétition spécifiés ici. La valeur -1 indique un nombre illimité de lignes dans le tableau![Rangée1.](assets/rowconfiguration.PNG)

## Déployez ceci sur votre serveur

[Installer Tomcat comme indiqué ici](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)[Déployer le fichier SampleRest.war](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)[Installer les ressources ](assets/amortizationschedule.zip) à l’aide du gestionnaire de packagesAEM Ouvrir le formulaire[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)de planification d’amortissement Saisissez la valeur appropriée et cliquez sur calculateAmortisation Schedule pour renseigner le formulaire

