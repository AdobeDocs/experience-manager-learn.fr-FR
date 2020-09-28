---
title: Création de deux dispositions de colonnes pour les documents de canal d’impression
seo-title: Création de deux dispositions de colonnes pour les documents de canal d’impression
description: Création de mises en page à 2 colonnes pour le document de canal d’impression
seo-description: Création de mises en page à 2 colonnes pour le document de canal d’impression
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 314f798f7a80f9c554e5bea052f8a64ae397d0de
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# Mise en page de deux colonnes dans le Document de Canal d&#39;impression

Ce court article décrit les étapes nécessaires pour créer une mise en page à 2 colonnes dans le canal d&#39;impression. L’exemple d’utilisation consiste à générer 2 documents de page avec une page 1 avec une mise en page à 2 colonnes et une page 2 avec la mise en page à 1 colonne standard.

Vous trouverez ci-dessous les étapes de haut niveau nécessaires à la création de mises en page à 2 colonnes à l’aide d’AEM Forms Designer.

* Créer 2 zones de contenu dans le gabarit de page 1
* Nommer les 2 zones de contenu &quot;leftcolumn&quot; et &quot;right column&quot;
* Créer un second gabarit avec une seule zone de contenu (par défaut)
* Sélectionnez l’onglet pagination (sous-formulaire sans titre) (page 1) et (sous-formulaire sans titre) (page 2) et définissez les propriétés comme illustré dans les captures d’écran ci-dessous.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Une fois les propriétés de pagination définies, nous pouvons ajouter des sous-formulaires ou des zones de cible sous (Sous-formulaire sans titre) (Page 1).

Nous pouvons ensuite ajouter des fragments de document à ces sous-formulaires ou zones de cible. Lorsque la colonne de gauche est pleine, le contenu est enchaîné à la colonne de droite.

Pour tester cette fonctionnalité sur votre serveur local, téléchargez les ressources associées à cet article. Faites défiler la page jusqu&#39;au bas de cette page

* [Téléchargement et installation d’exemples de Document d’impression à l’aide du gestionnaire de packages](assets/print-channel-with-two-column-layout.zip)
* [Prévisualisation du Document du Canal d&#39;impression](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
