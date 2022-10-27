---
title: Donner un style aux onglets de navigation de gauche avec des icônes
description: Ajouter des icônes pour indiquer les onglets principal et terminé
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 21%

---

# Ajouter des icônes pour indiquer les onglets principal et terminé

Lorsque vous disposez d’un formulaire adaptatif avec une navigation par onglets gauche, vous pouvez afficher des icônes pour indiquer le statut de l’onglet. Par exemple, vous souhaitez afficher une icône pour indiquer un onglet principal et une icône pour indiquer l’onglet terminé comme illustré dans la capture d’écran ci-dessous.

![toolbar-spacing](assets/active-completed.png)

## Création d’un formulaire adaptatif

Un formulaire adaptatif simple basé sur le modèle de base et le thème Canevas 3.0 a été utilisé pour créer l’exemple de formulaire.
Le [icônes utilisées dans cet article](assets/icons.zip) peut être téléchargé ici.


## Style de l’état par défaut

Ouvrez le formulaire en mode d’édition Assurez-vous que vous vous trouvez dans le calque de style et sélectionnez n’importe quel onglet (onglet Général, par exemple).
Vous vous trouvez dans l’état par défaut lorsque vous ouvrez l’éditeur de style pour l’onglet, comme illustré dans la capture d’écran ci-dessous.
![navigation-onglet](assets/navigation-tab.png)

Définissez les propriétés CSS de l’état par défaut, comme illustré ci-dessous. | Catégorie | Nom de la propriété | Valeur de la propriété | |:—|:—|:—| | Dimensions et position | Largeur | 50 px | | Texte | Poids de police | Gras | | Texte | Couleur | #FFF | |Texte | Hauteur de ligne | 3 | |Texte | Alignement du texte | Left | |Arrière-plan| Couleur | #056dae |

Enregistrez vos modifications

## Style de l’état Principal

Assurez-vous que vous êtes à l’état Principal et appliquez un style aux propriétés CSS suivantes.

| Catégorie | Nom de la propriété | Valeur de la propriété |
|:---|:---|:---|
| Dimensions et position | Largeur | 50px |
| Texte | Épaisseur de la police | Gras |
| Texte | Couleur | #FFF |
| Texte | Hauteur de ligne | 3 |
| Texte | Texte Aligner | Gauche |
| Contexte | Couleur | #056dae |

Mettez en forme l’image d’arrière-plan comme illustré dans la capture d’écran ci-dessous.

Enregistrez vos modifications.



![principal état](assets/active-state.png)

## Style de l’état visité

Assurez-vous que vous êtes à l’état visité et mettez en forme les propriétés suivantes

| Catégorie | Nom de la propriété | Valeur de la propriété |
|:---|:---|:---|
| Dimensions et position | Largeur | 50 px |
| Texte | Épaisseur de la police | Gras |
| Texte | Couleur | #FFF |
| Texte | Hauteur de ligne | 3 |
| Texte | Texte Aligner | Gauche |
| Contexte | Couleur | #056dae |

Mettez en forme l’image d’arrière-plan comme illustré dans la capture d’écran ci-dessous.


![visité-state](assets/visited-state.png)

Enregistrez vos modifications

Prévisualisez le formulaire et testez que les icônes fonctionnent comme prévu.
