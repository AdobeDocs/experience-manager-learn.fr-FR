---
title: Graphiques multiséries en AEM Forms
seo-title: Graphiques multiséries en AEM Forms
description: Créez un modèle de données de formulaire approprié pour créer des graphiques multiséries dans les documents papier et de canal Web.
seo-description: Créez un modèle de données de formulaire approprié pour créer des graphiques multiséries dans les documents papier et de canal Web.
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Graphiques multiséries

AEM Forms 6.5 a introduit la possibilité de créer et de configurer plusieurs graphiques en série. Les graphiques à séries multiples sont généralement utilisés en association avec le type de graphique Ligne, Barre, Colonne. Le graphique suivant est un bon exemple de graphique à séries multiples. Le graphique montre la croissance de 10 000 dollars américains dans 3 fonds mutuels différents sur une période donnée. Pour pouvoir créer et utiliser des graphiques de ce type en AEM Forms, vous devez créer le modèle de données de formulaire approprié.

![multisérie](assets/seriescharts.jfif)

Pour créer des graphiques multiséries en AEM Forms, vous devez créer un modèle de données de formulaire approprié avec les entités et associations nécessaires entre les entités. La capture d&#39;écran suivante met en évidence les entités et les associations entre les 3 entités. Au niveau supérieur, nous avons une entité appelée &quot;Organisation&quot;, qui a une association de type &quot;un à plusieurs&quot; avec l&#39;entité du Fonds. L&#39;entité du Fonds, à son tour, a une association de type &quot;un à plusieurs&quot; avec l&#39;entité Performance.

![formdatamodel](assets/formdatamodel.jfif)


## Créer un modèle de données de formulaire pour les graphiques à séries multiples

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Configurer des graphiques de série de lignes

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Pour tester cette fonctionnalité sur votre système, procédez comme suit :

* [Téléchargez et importez le fichier MutualFundFactSheet.zip à l’aide d’AEM Package Manager.](assets/mutualfundfactsheet.zip)
* [Téléchargez le fichier SeriesChartSampleData.json sur votre disque dur.](assets/serieschartsampledata.json) Il s’agit des données d’exemple qui seront utilisées pour remplir le graphique.
* [Accédez à Forms et aux Documents.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* Sélectionnez délicatement le modèle de communication interactive &quot;MutualFundGrowthFactSheet&quot;.
* Cliquez sur la Prévisualisation | Télécharger des exemples de données.
* Accédez au fichier de données d’exemple fourni dans le cadre de cet article.
* Prévisualisation du canal d&#39;impression de la communication interactive &quot;MutualFundGrowthFactSheet&quot; avec les données d&#39;exemple téléchargées à l&#39;étape précédente.
