---
title: Ajouter des icônes aux onglets de navigation de gauche
description: Ajouter des icônes pour différencier les onglets actifs et terminés
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
duration: 111
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 100%

---

# Ajouter des icônes pour différencier les onglets actifs et terminés

Lorsque vous disposez d’un formulaire adaptatif avec une navigation par onglets à gauche, vous pouvez afficher des icônes pour indiquer le statut de l’onglet. Par exemple, vous pouvez afficher des icônes pour indiquer un onglet principal et un onglet terminé, comme illustré dans la copie d’écran ci-dessous.

![toolbar-spacing](assets/active-completed.png)

## Créer un formulaire adaptatif

Cet exemple de formulaire adaptatif simple repose sur le modèle de base et le thème Canvas 3.0.
Téléchargez les [icônes présentées dans l’article](assets/icons.zip) ici.


## Personnaliser l’état par défaut

Ouvrez le formulaire en mode d’édition.
Dans le calque de style, sélectionnez un onglet (onglet Général, par exemple).
Lors de l’ouverture de l’éditeur de style de l’onglet, vous vous trouvez dans l’état par défaut, comme illustré dans la copie d’écran ci-dessous.
![navigation-tab](assets/navigation-tab.png)

Définissez les propriétés CSS d’état par défaut, comme illustré ci-dessous.
| Catégorie | Nom de la propriété | Valeur de la propriété |
|:—|:—|:—|
| Dimensions et position | Largeur | 50 px |
| Texte | Épaisseur de la police | Gras |
| Texte | Couleur | #FFF |
| Texte | Hauteur de ligne | 3 |
| Texte | Alignement du texte | Gauche |
| Arrière-plan | Couleur | #056dae |

Enregistrez vos modifications.

## Personnaliser l’état actif

Assurez-vous d’être à l’état actif et appliquez un style aux propriétés CSS suivantes.

| Catégorie | Nom de la propriété | Valeur de la propriété |
|:---|:---|:---|
| Dimensions et position | Largeur | 50 px |
| Texte | Épaisseur de la police | Gras |
| Texte | Couleur | #FFF |
| Texte | Hauteur de ligne | 3 |
| Texte | Alignement du texte | Gauche |
| Arrière-plan | Couleur | #056dae |

Personnalisez l’image d’arrière-plan comme illustré dans la copie d’écran ci-dessous.

Enregistrez vos modifications.



![active-state](assets/active-state.png)

## Personnaliser l’état visité

Assurez-vous d’être à l’état visité et personnalisez les propriétés suivantes.

| Catégorie | Nom de la propriété | Valeur de la propriété |
|:---|:---|:---|
| Dimensions et position | Largeur | 50 px |
| Texte | Épaisseur de la police | Gras |
| Texte | Couleur | #FFF |
| Texte | Hauteur de ligne | 3 |
| Texte | Alignement du texte | Gauche |
| Arrière-plan | Couleur | #056dae |

Personnalisez l’image d’arrière-plan comme illustré dans la copie d’écran ci-dessous.


![visited-state](assets/visited-state.png)

Enregistrez vos modifications.

Prévisualisez le formulaire et vérifiez que les icônes fonctionnent comme prévu.
