---
title: 'Renseigner le tableau de formulaire adaptatif '
seo-title: Renseigner le tableau de formulaire adaptatif
description: Renseignez le tableau Formulaire adaptatif avec les résultats des appels de service de modèles de données de formulaire.
seo-description: Renseignez le tableau Formulaire adaptatif avec les résultats des appels de service de modèles de données de formulaire.
feature: formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---


# Renseignez le tableau Formulaire adaptatif avec les résultats de l’appel du service de modèle de données de formulaire.

[Le formulaire en direct est hébergé ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
iciDans cet article, nous examinons comment renseigner le tableau des formulaires adaptatifs en récupérant les données issues de l’appel de service de modèle de données de formulaire. Nous allons créer un calendrier d&#39;amortissement dans un tableau qui liste chaque paiement régulier d&#39;une hypothèque au fil du temps. Les résultats de l&#39;amortissement sont retournés par notre modèle de données de formulaire. Le service Modèle de données de formulaire est appelé sur le événement de clic du bouton Calculer, comme le montre la capture d’écran. Les paramètres d’entrée et de sortie de l’appel de service sont mappés de manière appropriée, comme le montre la capture d’écran. La sortie est mappée aux colonnes de Row1
![clickevent](assets/amortization.PNG)

La ligne 1 est configurée pour augmenter en fonction des données renvoyées par l’appel de service. Notez les paramètres de répétition spécifiés ici. La valeur -1 indique un nombre illimité de lignes dans le tableau.
![Ligne1](assets/rowconfiguration.PNG)

## Déployez ceci sur votre serveur

[Installer Tomcat comme indiqué ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[iciDéployer le ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[fichier SampleRest.warInstaller les ressources  ](assets/amortizationschedule.zip) à l&#39;aide du gestionnaire de packages AEM 
[Ouvrir le ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
formulaire de planification de l&#39;amortissementEntrez la valeur appropriée et cliquez sur Calculer la planification de l&#39;amortissement pour qu&#39;elle soit renseignée dans votre formulaire

