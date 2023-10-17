---
title: Configurer le panneau Plan d’investissement
seo-title: Configuring Investment Mix Panel
description: Il s’agit de la partie 11 du tutoriel en plusieurs étapes qui explique comment créer votre premier document de communication interactive. Dans cette partie, nous allons ajouter des graphiques en secteurs pour afficher le modèle de plan d’investissement et le plan d’investissement actuel.
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document.In this part, we will add pie charts to display the current and model investment mix.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '334'
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
