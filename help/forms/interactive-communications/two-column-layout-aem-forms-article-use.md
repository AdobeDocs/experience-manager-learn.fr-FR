---
title: Créer des dispositions à deux colonnes pour les documents de canal d’impression
description: Créer des dispositions à deux colonnes pour les documents de canal d’impression
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
duration: 64
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 100%

---

# Dispositions à deux colonnes dans le document de canal d’impression

Cet article succinct décrit les étapes nécessaires à la création d’une disposition à 2 colonnes dans le canal d’impression. Le cas d’utilisation consiste à générer 2 documents de page, avec une disposition à 2 colonnes pour la première page et une disposition standard à 1 colonne pour la deuxième page.

Pour créer des dispositions à 2 colonnes à l’aide d’AEM Forms Designer, suivez la procédure ci-dessous.

* Créez 2 zones de contenu dans le gabarit de la page 1.
* Nommez les 2 zones de contenu « leftcolumn » (colonne gauche) et « rightcolumn » (colonne droite).
* Créez un deuxième gabarit avec une zone de contenu (valeur par défaut).
* Sélectionnez le (sous-formulaire sans titre) (page 1) et le (sous-formulaire sans titre) (page 2) de l’onglet de pagination, puis définissez les propriétés comme illustré dans les copies d’écran ci-dessous.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Une fois les propriétés de pagination définies, ajoutez les sous-formulaires ou zones cible sous (sous-formulaire sans titre) (page 1).

Ajoutez ensuite des fragments de document à ces sous-formulaires ou zones cible. Lorsque la colonne de gauche est pleine, le contenu passe dans la colonne de droite.

Pour tester les dispositions à deux colonnes sur votre serveur local, téléchargez les ressources mentionnées dans cet article. Faites défiler jusqu’au bas de la page.

* [Télécharger et installer l’exemple de document de canal d’impression à l’aide du gestionnaire de packages](assets/print-channel-with-two-column-layout.zip)
* [Prévisualiser le document de canal d’impression](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
