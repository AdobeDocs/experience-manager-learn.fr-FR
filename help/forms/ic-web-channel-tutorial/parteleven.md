---
title: Configuration du panneau de correctif d’investissement
seo-title: Configuration du panneau de correctif d’investissement
description: Ce didacticiel en plusieurs étapes est la 11 partie du premier document de communication interactive. Dans cette partie, nous allons ajouter des secteurs pour afficher le mélange actuel d’investissement et de modèle.
seo-description: Ce didacticiel en plusieurs étapes est la 11 partie du premier document de communication interactive. Dans cette partie, nous allons ajouter des secteurs pour afficher le mélange actuel d’investissement et de modèle.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Communication interactive
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---


# Configuration du panneau de correctif d’investissement

Dans cette partie, nous allons ajouter des secteurs afin d’afficher la combinaison actuelle d’investissements et de modèles.

* Connectez-vous à AEM Forms et accédez à Adobe Experience Manager > Forms > Forms et documents.

* Ouvrez le dossier 401KStatement .

* Ouvrez l’instruction 401KStatement en mode d’édition.

* Nous ajouterons 2 secteurs pour représenter le mélange actuel et modèle d’investissement du détenteur de compte.

## Combinaison de ressources actuelle {#current-asset-mix}

* Appuyez sur le panneau &quot;CurrentAssetMix&quot; à droite et sélectionnez l’icône &quot;+&quot; et insérez le composant de texte. Remplacez le texte par défaut par &quot;Current Asset Mix&quot;.

* Appuyez sur le panneau &quot;CurrentAssetMix&quot;, sélectionnez l’icône &quot;+&quot; et insérez le composant de graphique. Appuyez sur le nouveau composant de graphique inséré et cliquez sur l’icône &quot;clé à molette&quot; pour ouvrir la feuille de propriétés de configuration du graphique.

* Définissez les propriétés comme illustré ci-dessous. Assurez-vous que le type de graphique est Graphique en secteurs.

* Notez l’objet de modèle de données lié aux axes X et Y. Vous devez sélectionner l’élément racine du modèle de données de formulaire, puis descendre vers le bas pour sélectionner l’élément approprié.

* ![currentassetmix](assets/currentassetmixchart.png)

## Mix de ressource de modèle {#model-asset-mix}

* Appuyez sur le panneau &quot;RecommandedAssetMix&quot; sur le côté droit, sélectionnez l’icône &quot;+&quot; et insérez le composant de texte. Remplacez le texte par défaut par &quot;Model Asset Mix&quot;.

* Appuyez sur le panneau &quot;RecommandedAssetMix&quot;, sélectionnez l’icône &quot;+&quot; et insérez le composant de graphique. Appuyez sur le nouveau composant de graphique inséré et cliquez sur l’icône &quot;clé à molette&quot; pour ouvrir la feuille de propriétés de configuration du graphique.

* Définissez les propriétés comme illustré ci-dessous. Assurez-vous que le type de graphique est Graphique en secteurs.

* Notez l’objet de modèle de données lié aux axes X et Y. Vous devez sélectionner l’élément racine du modèle de données de formulaire, puis descendre vers le bas pour sélectionner l’élément approprié.

* ![assettype](assets/modelassettypechart.png)

