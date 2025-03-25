---
title: Configurer le panneau Plan d’investissement
description: Il s’agit de la partie 11 du tutoriel en plusieurs étapes qui explique comment créer votre premier document de communication interactive. Dans cette partie, nous allons ajouter des graphiques en secteurs pour afficher le modèle de plan d’investissement et le plan d’investissement actuel.
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 69
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 100%

---

# Configurer le panneau Plan d’investissement

Dans cette partie, nous allons ajouter des graphiques en secteurs afin d’afficher le modèle de plan d’investissement et le plan d’investissement actuel.

* Connectez-vous à AEM Forms et accédez à Adobe Experience Manager > Forms > Formulaires et documents.

* Ouvrez le dossier 401KStatement.

* Ouvrez le dossier 401KStatement en mode édition.

* Nous ajouterons 2 graphiques en secteurs pour représenter le modèle de plan d’investissement et le plan d’investissement actuel de la personne propriétaire du compte.

## Plan d’investissement actuel {#current-asset-mix}

* Appuyez sur le panneau « CurrentAssetMix » (plan d’investissement actuel) à droite, sélectionnez l’icône « + » et insérez le composant de texte. Remplacez le texte par défaut par « Current Asset Mix ».

* Appuyez sur le panneau « CurrentAssetMix », sélectionnez l’icône « + » et insérez le composant de graphique. Appuyez sur le composant de graphique que vous venez d’insérer et cliquez sur l’icône de clé à molette pour ouvrir la feuille des propriétés de configuration du graphique.

* Définissez les propriétés, comme illustré dans l’image ci-dessous. Assurez-vous que le type de graphique est Graphique en secteurs.

* Remarquez l’objet de modèle de données lié aux axes X et Y. Vous devez sélectionner l’élément racine du modèle de données de formulaire, puis analyser en profondeur pour sélectionner l’élément approprié.

* ![currentassetmix](assets/currentassetmixchart.png)

## Modèle de plan d’investissement {#model-asset-mix}

* Appuyez sur le panneau « RecommandedAssetMix » (plan d’investissement recommandé) à droite, sélectionnez l’icône « + » et insérez le composant de texte. Remplacez le texte par défaut par « Model Asset Mix » (modèle de plan d’investissement).

* Appuyez sur le panneau « RecommandedAssetMix », sélectionnez l’icône « + » et insérez le composant de graphique. Appuyez sur le composant de graphique que vous venez d’insérer et cliquez sur l’icône de clé à molette pour ouvrir la feuille des propriétés de configuration du graphique.

* Définissez les propriétés, comme illustré dans l’image ci-dessous. Assurez-vous que le type de graphique est Graphique en secteurs.

* Remarquez l’objet de modèle de données lié aux axes X et Y. Vous devez sélectionner l’élément racine du modèle de données de formulaire, puis analyser en profondeur pour sélectionner l’élément approprié.

* ![assettype](assets/modelassettypechart.png)

## Étapes suivantes

[Préparer la diffusion du document de canal web](./parttwelve.md)
