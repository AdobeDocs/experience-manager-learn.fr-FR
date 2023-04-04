---
title: Ajout de composants au panneau Revenus
description: Nous ajouterons un tableau au panneau Revenus. Configurez les lignes du tableau et utilisez l’éditeur de règles pour calculer le total général.
feature: Adaptive Forms
version: 6.4,6.5
thumbnail: 22198.jpg
kt: 4211
topic: Development
role: Developer
level: Beginner
exl-id: e7674c46-259f-4dbd-96db-c40369534911
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Ajout de composants au panneau Revenus {#adding-components-to-income-panel}

Nous ajouterons un tableau au panneau Revenus. Configurez les lignes du tableau et utilisez l’éditeur de règles pour calculer le total général.

**Ajout et configuration d’un composant Tableau**

>[!VIDEO](https://video.tv.adobe.com/v/22198?quality=12&learn=on)



## Rendre la table des revenus dynamique {#make-the-income-table-dynamic}

**Assurez-vous que vous êtes en mode d’édition. Le bouton Modifier se trouve en haut à droite du navigateur.**

* Par défaut, lorsque vous insérez un tableau dans un formulaire adaptatif, il n’est pas dynamique, ce qui signifie que vous ne pouvez pas ajouter de nouvelles lignes au tableau au moment de l’exécution.

* Actualisez votre navigateur.

* Sélectionnez Rangée1 dans la hiérarchie de contenu.

* Cliquez sur l’icône de clé à molette pour ouvrir la feuille de propriétés.

* Définissez les nombres minimal et maximal sur 1 et 5 sous Paramètres de répétition et enregistrez vos modifications en cliquant sur l’icône représentant une coche bleue. Cela signifie que le tableau peut contenir, au maximum, 5 lignes. Pour qu’un nombre de lignes indéfini soit défini sur -1.

## Créer une règle pour calculer le total général {#create-rule-to-calculate-grand-total}


>[!VIDEO](https://video.tv.adobe.com/v/22197?quality=12&learn=on)
