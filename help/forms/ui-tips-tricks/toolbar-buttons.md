---
title: Placez les boutons Suivant et Précédent de la barre d’outils.
description: Placer les boutons de la barre d’outils
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 96b78ff5056bd9c2be39fb2cf21b4f92863af089
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 7%

---

# Bouton de la barre d’outils

Lorsque vous ajoutez les boutons Suivant et Précédent à la barre d’outils dans AEM Forms, les boutons sont placés par défaut les uns à côté des autres. Vous pouvez appuyer sur le bouton Suivant à l’extrême droite de la barre d’outils tout en maintenant le bouton précédent/précédent à gauche.

![toolbar-spacing](assets/toolbar-spacing.png)


## Style de la barre d’outils

Le cas d’utilisation ci-dessus peut être facilement réalisé à l’aide de l’éditeur de style. Une fois que vous avez ajouté le bouton Précédent/Suivant à la barre d’outils, assurez-vous d’avoir sélectionné le calque Style dans le menu d’édition. Lorsque le mode Style est sélectionné, sélectionnez la barre d’outils pour ouvrir sa feuille de propriétés de style. Développez la section Dimensions et position et vérifiez que toutes les propriétés s’affichent. Définissez les propriétés suivantes
* Dimensions et position
   * Largeur: 100%
   * Position : relative

Enregistrez vos modifications

## Style du bouton Suivant

Sélectionnez le bouton Suivant et veillez à ouvrir la feuille de propriétés de style du bouton suivant (et non le texte du bouton suivant). Définissez les propriétés suivantes
* Dimensions et position
   * position : top 1 px droite 1 px
* Bordure
   * Rayon de bordure : 4 px(Haut,Droite,Bas,Gauche)

Enregistrez vos modifications
