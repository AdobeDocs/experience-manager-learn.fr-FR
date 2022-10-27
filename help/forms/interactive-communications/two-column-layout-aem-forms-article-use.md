---
title: Création de deux dispositions de colonnes pour les documents de canal d’impression
seo-title: Creating two column layouts for print channel documents
description: Création de deux mises en page de colonnes pour le document de canal d’impression
seo-description: Create 2 column layouts for print channel document
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Mises en page à deux colonnes dans le document Canal d’impression

Cet article court décrit les étapes nécessaires à la création d’une mise en page à 2 colonnes dans le canal d’impression. Le cas d’utilisation consiste à générer 2 documents de page avec une page 1 avec une mise en page à 2 colonnes et une page 2 avec une mise en page à 1 colonne standard.

Vous trouverez ci-dessous les étapes générales de création de mises en page à 2 colonnes à l’aide d’AEM Forms Designer.

* Créer 2 zones de contenu dans le gabarit de page 1
* Nommez les 2 zones de contenu &quot;leftcolumn&quot; et &quot;right column&quot;.
* Créer un deuxième gabarit avec une zone de contenu (valeur par défaut)
* Sélectionnez l’onglet pagination (Sous-formulaire sans titre) (page 1) et (Sous-formulaire sans titre) (page 2), puis définissez les propriétés comme illustré dans les captures d’écran ci-dessous.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Une fois les propriétés de pagination définies, nous pouvons ajouter des sous-formulaires ou des zones cible sous (sous-formulaire sans titre) (page 1).

Nous pouvons ensuite ajouter des fragments de document à ces sous-formulaires ou zones cible. Lorsque la colonne de gauche est pleine, le contenu est dirigé vers la colonne de droite.

Pour le tester sur votre serveur local, téléchargez les ressources liées à cet article. Faites défiler la page vers le bas.

* [Télécharger et installer Exemple de document de canal d’impression à l’aide du gestionnaire de modules](assets/print-channel-with-two-column-layout.zip)
* [Aperçu du document de canal d’impression](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
