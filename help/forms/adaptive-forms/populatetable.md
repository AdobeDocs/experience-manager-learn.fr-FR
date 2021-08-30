---
title: 'Renseigner le tableau de formulaire adaptatif '
description: Renseigner le tableau de formulaire adaptatif avec les résultats des appels du service de modèle de données de formulaire
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
source-git-commit: 2b7f0f6c34803672cc57425811db89146b38a70a
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---


# Renseigner le tableau de formulaire adaptatif avec les résultats de l’appel du service de modèle de données de formulaire

[Le formulaire en direct est hébergé ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
ici. Dans cet article, nous examinons le remplissage du tableau de formulaires adaptatifs en récupérant des données à partir de l’appel du service de modèle de données de formulaire. Nous allons créer un calendrier d&#39;amortissement dans un tableau qui répertorie chaque paiement régulier d&#39;un prêt au fil du temps. Les résultats de l’amortissement sont renvoyés par notre modèle de données de formulaire. Le service du modèle de données de formulaire est appelé sur l’événement click du bouton calculate , comme illustré dans la capture d’écran. Les paramètres d’entrée et de sortie de l’appel de service sont mappés de manière appropriée, comme illustré dans la capture d’écran. La sortie est mappée aux colonnes de Row1
![clickevent](assets/amortization.PNG)

La ligne 1 est configurée pour s’agrandir en fonction des données renvoyées par l’appel de service. Notez les paramètres de répétition qui sont spécifiés ici. La valeur -1 indique un nombre illimité de lignes dans le tableau.
![Ligne1](assets/rowconfiguration.PNG)

## Déployez-le sur votre serveur

[Installez Tomcat comme spécifié ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[iciDéployez le fichier SampleRest.war contenu dans ce fichier zip dans votre ](assets/sample-rest.zip)
[TomcatInstallez les ressources  ](assets/amortizationschedule.zip) à l’aide du gestionnaire de modules AEM 
[Ouvrez le ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
formulaire de planification d’amortissement. Saisissez la valeur appropriée et cliquez sur calculer la planification d’amortissement pour être renseigné dans votre formulaire.

