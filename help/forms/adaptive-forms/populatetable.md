---
title: 'Renseigner le tableau de formulaire adaptatif '
seo-title: Renseigner le tableau de formulaire adaptatif
description: Renseigner le tableau de formulaire adaptatif avec les résultats des appels du service de modèle de données de formulaire
seo-description: Renseigner le tableau de formulaire adaptatif avec les résultats des appels du service de modèle de données de formulaire
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Développement
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# Renseigner le tableau de formulaire adaptatif avec les résultats de l’appel du service de modèle de données de formulaire

[Le formulaire en direct est hébergé ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
ici. Dans cet article, nous examinons le remplissage du tableau de formulaires adaptatifs en récupérant des données à partir de l’appel du service de modèle de données de formulaire. Nous allons créer un calendrier d&#39;amortissement dans un tableau qui répertorie chaque paiement régulier d&#39;un prêt au fil du temps. Les résultats de l’amortissement sont renvoyés par notre modèle de données de formulaire. Le service du modèle de données de formulaire est appelé sur l’événement click du bouton calculate , comme illustré dans la capture d’écran. Les paramètres d’entrée et de sortie de l’appel de service sont mappés de manière appropriée, comme illustré dans la capture d’écran. La sortie est mappée aux colonnes de Row1
![clickevent](assets/amortization.PNG)

La ligne 1 est configurée pour s’agrandir en fonction des données renvoyées par l’appel de service. Notez les paramètres de répétition qui sont spécifiés ici. La valeur -1 indique un nombre illimité de lignes dans le tableau.
![Ligne1](assets/rowconfiguration.PNG)

## Déployez-le sur votre serveur

[Installez Tomcat comme spécifié ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[iciDéployer le ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[fichier SampleRest.warInstallez les ressources  ](assets/amortizationschedule.zip) à l’aide du gestionnaire de modules AEM 
[Ouvrez le ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
formulaire de planification de l’amortissement. Saisissez la valeur appropriée et cliquez sur calculer la planification de l’amortissement pour qu’elle soit renseignée dans votre formulaire.

