---
title: Configuration du panneau Outlook de retraite
seo-title: Configuration du panneau Outlook de retraite
description: Ce didacticiel en plusieurs étapes constitue la 10 partie d’un premier document de communication interactive. Dans cette partie, nous allons configurer le panneau Outlook de retraite en ajoutant des composants de texte et de graphique.
seo-description: Ce didacticiel en plusieurs étapes constitue la 10 partie d’un premier document de communication interactive. Dans cette partie, nous allons configurer le panneau Outlook de retraite en ajoutant des composants de texte et de graphique.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Communication interactive
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 1%

---


# Configuration du panneau Outlook de retraite{#configuring-retirement-outlook-panel}

* Ce didacticiel en plusieurs étapes constitue la 10 partie d’un premier document de communication interactive. Dans cette partie, nous allons configurer le panneau Outlook de retraite en ajoutant des composants de texte et de graphique.

* Connectez-vous à AEM Forms et accédez à Adobe Experience Manager > Forms > Forms et documents.

* Ouvrez le dossier 401KStatement .

* Ouvrez le document 401KStatement en mode d’édition.

**Configuration de la zone cible Panneau gauche**

* Appuyez sur la zone cible LeftPanel à droite et cliquez sur l’icône &quot;+&quot; pour ouvrir la boîte de dialogue Insérer le composant.

* Insérez le composant Texte.

* Appuyez avec douceur sur le nouveau composant de texte ajouté pour afficher la barre d’outils du composant.

* Sélectionnez l&#39;icône &quot;crayon&quot; pour modifier le texte par défaut.

* Remplacez le texte par défaut par &quot;**Vos perspectives de revenu de retraite&quot;**

**Configuration de la zone cible du panneau droit**

* Appuyez sur la zone cible du panneau de droite à droite et cliquez sur l’icône &quot;+&quot; pour ouvrir la boîte de dialogue Insérer le composant.

* Insérez le composant Texte.

* Appuyez doucement sur le nouveau composant de texte ajouté pour afficher la barre d’outils du composant.

* Sélectionnez l&#39;icône &quot;crayon&quot; pour modifier le texte par défaut.

* Remplacez le texte par défaut par &quot;**Revenus mensuels de retraite estimés&quot;**

## Ajouter un fragment de document Perspectives de revenu de retraite {#add-retirement-income-outlook-document-fragment}

* Cliquez sur l’icône Ressources et appliquez le filtre pour afficher les ressources de type &quot;Fragments de document&quot;. Faites glisser et déposez le fragment de document RetirementRevenueOutlook dans la zone cible Panneau de gauche.

* Vous pouvez vous référer [à cette page](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/9.html) lors de l’ajout de fragments de document aux zones de contenu.

## Ajout d’un graphique des revenus mensuels estimés {#adding-estimated-monthly-income-chart}

* Cliquez sur la zone cible du panneau de droite dans la partie droite. Cliquez sur l’icône &quot;+&quot; pour insérer le composant de graphique. Nous utiliserons un graphique en colonnes pour afficher les revenus mensuels estimés. Appuyez doucement sur le composant de graphique nouvellement inséré. Sélectionnez l&#39;icône &quot;clé à molette&quot; pour ouvrir la feuille des propriétés de configuration. Configurez le graphique avec les propriétés suivantes, comme illustré dans la capture d&#39;écran ci-dessous.

**AEM Forms 6.4 - Configuration du graphique à colonnes de revenus mensuels estimés**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configuration du graphique à colonnes de revenus mensuels estimés**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




