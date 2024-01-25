---
title: Remplissage d’un tableau de formulaire adaptatif
description: Remplir un tableau de formulaire adaptatif avec les résultats des appels du service de modèle de données de formulaire
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 50
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 100%

---

# Remplir un tableau de formulaire adaptatif avec les résultats de l’appel du service de modèle de données de formulaire

[Le formulaire en direct est hébergé ici](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Dans cet article, nous examinons le remplissage d’un tableau de formulaire adaptatif avec les données récupérées à partir de l’appel du service de modèle de données de formulaire. Nous allons créer un calendrier d’amortissement dans un tableau qui répertorie chaque paiement régulier d’un prêt immobilier au fil du temps. Les résultats de l’amortissement sont renvoyés par notre modèle de données de formulaire. Le service du modèle de données de formulaire est appelé sur l’événement Clic du bouton Calculer, comme illustré sur la capture d’écran. Les paramètres d’entrée et de sortie de l’appel de service sont mappés de manière appropriée, comme illustré sur la capture d’écran. La sortie est mappée aux colonnes de Row1
![clickevent](assets/amortization.PNG)

La ligne 1 est configurée pour s’agrandir en fonction des données renvoyées par l’appel de service. Notez les paramètres de répétition qui sont spécifiés ici. La valeur -1 indique un nombre illimité de lignes dans le tableau.
![Row1](assets/rowconfiguration.PNG)

## Déployer ceci sur votre serveur

[Installez Tomcat comme spécifié ici.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Déployez le fichier SampleRest.war contenu dans ce fichier zip dans votre Tomcat.](assets/sample-rest.zip)
[Installez les ressources](assets/amortizationschedule.zip) à l’aide du gestonnaire de modules AEM.
[Ouvrez le formulaire de planification de l’amortissement.](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Saisissez la valeur appropriée et cliquez sur calculer.
La planification de l’amortissement devrait être renseignée dans votre formulaire.
