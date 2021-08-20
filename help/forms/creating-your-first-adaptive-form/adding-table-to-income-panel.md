---
title: Ajout de composants au panneau Revenus
description: Nous ajouterons un tableau au panneau Revenus. Configurez les lignes du tableau et utilisez l’éditeur de règles pour calculer le total général.
feature: Formulaires adaptatifs
version: 6.4,6.5
thumbnail: 22198.jpg
kt: 4211
topic: Développement
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 1%

---


# Ajout de composants au panneau Revenus {#adding-components-to-income-panel}

Nous ajouterons un tableau au panneau Revenus. Configurez les lignes du tableau et utilisez l’éditeur de règles pour calculer le total général.

**Ajout et configuration d’un composant Tableau**

>[!VIDEO](https://video.tv.adobe.com/v/22198?quality=9&learn=on)



## Rendre la table des revenus dynamique {#make-the-income-table-dynamic}

**Assurez-vous que vous êtes en mode d’édition. Le bouton Modifier se trouve en haut à droite du navigateur.**

* Par défaut, lorsque vous insérez un tableau dans un formulaire adaptatif, il n’est pas dynamique, ce qui signifie que vous ne pouvez pas ajouter de nouvelles lignes au tableau au moment de l’exécution.

* Actualisez votre navigateur.

* Sélectionnez Rangée1 dans la hiérarchie de contenu.

* Cliquez sur l’icône de clé à molette pour ouvrir la feuille de propriétés.

* Définissez les nombres minimal et maximal sur 1 et 5 sous Paramètres de répétition et enregistrez vos modifications en cliquant sur l’icône représentant une coche bleue. Cela signifie que le tableau peut contenir, au maximum, 5 lignes. Pour qu’un nombre de lignes indéfini soit défini sur -1.

## Créer une règle pour calculer le total général {#create-rule-to-calculate-grand-total}


>[!VIDEO](https://video.tv.adobe.com/v/22197?quality=9&learn=on)


