---
title: Graphiques à séries multiples dans AEM Forms
description: Créez un modèle de données de formulaire approprié pour créer des graphiques à séries multiples dans les documents de canal papier et web.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '283'
ht-degree: 100%

---

# Graphiques à séries multiples

AEM Forms 6.5 offre la possibilité de créer et de configurer des graphiques à séries multiples. Les graphiques à séries multiples sont généralement utilisés avec les types de graphique à barres, en courbes et histogramme. Le graphique suivant est un bon exemple de graphique à séries multiples. Le graphique montre une augmentation de 10 000 dollars pour 3 fonds communs de placement différents sur une période. Pour pouvoir créer et utiliser des graphiques de ce type dans AEM Forms, vous devez créer le modèle de données de formulaire approprié.

![Graphique à séries multiples.](assets/seriescharts.jfif)

Pour créer des graphiques à séries multiples dans AEM Forms, vous devez créer un modèle de données de formulaire approprié avec les entités et associations nécessaires entre les entités. La capture d’écran suivante présente les entités et les associations entre les 3 entités. Au niveau supérieur, nous avons une entité appelée « Organisation », qui a une association un-à-multiple avec l’entité Fonds. L’entité Fonds, à son tour, a une association de type un-à-multiple avec l’entité Performance.

![Modèle de données de formulaire.](assets/formdatamodel.jfif)

## Créer un modèle de données de formulaire pour les graphiques à séries multiples

>[!VIDEO](https://video.tv.adobe.com/v/26352?quality=12&learn=on)

### Configurer des graphiques à séries linéaires

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=12&learn=on)

Pour tester cela sur votre système, veuillez procéder comme suit :

* [Téléchargez et importez le fichier MutualFundFactSheet.zip à l’aide du gestionnaires de packages AEM.](assets/mutualfundfactsheet.zip)
* [Téléchargez le fichier SeriesChartSampleData.json sur votre disque dur.](assets/serieschartsampledata.json) Il s’agit des données d’exemple utilisées pour remplir le graphique.
* [Accédez à Formulaires et documents.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Sélectionnez le modèle de communication interactive « MutualFundGrowthFactSheet ».
* Cliquez sur Aperçu | Canal d’impression | Télécharger des exemples de données.
* Accédez au fichier de données d’exemple fourni dans le cadre de cet article.
* Prévisualisez le canal d’impression de la communication interactive « MutualFundGrowthFactSheet » avec les données d’exemple téléchargées à l’étape précédente.
