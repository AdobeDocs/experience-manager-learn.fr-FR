---
title: Graphiques à séries multiples dans AEM Forms
seo-title: Multi Series Charts in AEM Forms
description: Créez un modèle de données de formulaire approprié pour créer des graphiques à série multiple dans les documents de canal papier et web.
seo-description: Create appropriate Form Data Model to create multi series charts in print and web channel documents.
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
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 2%

---

# Graphiques à séries multiples

AEM Forms 6.5 offre la possibilité de créer et de configurer des graphiques à série multiple. Les graphiques à séries multiples sont généralement utilisés en association avec le type de graphique Ligne,Barre,Colonne . Le graphique suivant est un bon exemple de graphique à série multiple. Le graphique montre une croissance de 10 000 dollars sur une période de temps de 3 fonds communs de placement différents. Pour pouvoir créer et utiliser des graphiques de ce type dans AEM Forms, vous devez créer le modèle de données de formulaire approprié.

![multisérie](assets/seriescharts.jfif)

Pour créer des graphiques à série multiple dans AEM Forms, vous devez créer un modèle de données de formulaire approprié avec les entités et associations nécessaires entre les entités. La capture d&#39;écran suivante présente les entités et les associations entre les 3 entités. Au niveau supérieur, nous avons une entité appelée &quot;Organisation&quot;, qui a une association un-à-multiple avec l&#39;entité du Fonds. L&#39;entité Fond, à son tour, a une association de type &quot;un à plusieurs&quot; avec l&#39;entité Performances.

![formdatamodel](assets/formdatamodel.jfif)


## Créer un modèle de données de formulaire pour les graphiques à séries multiples

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Configuration de graphiques de séries linéaires

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Pour le tester sur votre système, procédez comme suit :

* [Téléchargez et importez le fichier MutualFundFactSheet.zip à l’aide d’AEM Package Manager.](assets/mutualfundfactsheet.zip)
* [Téléchargez le fichier SeriesChartSampleData.json sur votre disque dur.](assets/serieschartsampledata.json) Il s’agit des données d’exemple qui seront utilisées pour remplir le graphique.
* [Accédez à Forms et Documents.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Sélectionnez gentiment le modèle de communication interactive &quot;MutualFundGrowthFactSheet&quot;.
* Clic sur Aperçu | Canal d’impression | Télécharger des exemples de données.
* Accédez au fichier de données d’exemple fourni dans le cadre de cet article.
* Prévisualisez le canal d’impression de la communication interactive &quot;MutualFundGrowthFactSheet&quot; avec les données d’exemple téléchargées à l’étape précédente.
