---
title: Espacer les boutons Suivant et Précédent de la barre d’outils
description: Espacer les boutons de la barre d’outils
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 50
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 100%

---

# Espacer les boutons de la barre d’outils

Lorsque vous ajoutez les boutons Suivant et Précédent à la barre d’outils dans AEM Forms, les boutons sont placés par défaut les uns à côté des autres. Vous pouvez appuyer sur le bouton Suivant à l’extrême droite de la barre d’outils tout en maintenant le bouton Précédent/Arrière à gauche.

![toolbar-spacing](assets/toolbar-spacing.png)


## Style de la barre d’outils

Le cas d’utilisation ci-dessus peut être facilement réalisé à l’aide de l’éditeur de style. Une fois que vous avez ajouté le bouton Précédent/Suivant à la barre d’outils, assurez-vous d’avoir sélectionné le calque Style dans le menu d’édition. Lorsque le mode Style est sélectionné, sélectionnez la barre d’outils pour ouvrir sa feuille de propriétés de style. Développez la section Dimensions et position et vérifiez que toutes les propriétés s’affichent. Définissez les propriétés suivantes :
* Dimensions et position
   * Largeur : 100 %
   * Position : relative

Enregistrez vos modifications.

## Style du bouton Suivant

Sélectionnez le bouton Suivant et veillez à ouvrir la feuille de propriétés de style du bouton Suivant (et non le texte du bouton Suivant). Définissez les propriétés suivantes :
* Dimensions et position
   * Position : absolue haut 1 px droite 1 px
* Bordure
   * Rayon de bordure : 4 px (haut, droite, bas, gauche)

Enregistrez vos modifications.
