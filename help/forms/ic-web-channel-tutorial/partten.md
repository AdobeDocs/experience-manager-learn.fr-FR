---
title: Configurer le panneau d’estimation de retraite
seo-title: Configuring Retirement Outlook Panel
description: Il s’agit de la partie 10 du tutoriel en plusieurs étapes sur la création de votre premier document de communication interactive. Dans cette partie, nous allons configurer le panneau d’estimation de retraite en ajoutant des composants de texte et de graphique.
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '349'
ht-degree: 100%

---

# Configurer le panneau d’estimation de retraite{#configuring-retirement-outlook-panel}

* Il s’agit de la partie 10 du tutoriel en plusieurs étapes sur la création de votre premier document de communication interactive. Dans cette partie, nous allons configurer le panneau d’estimation de retraite en ajoutant des composants de texte et de graphique.

* Connectez-vous à AEM Forms et accédez à Adobe Experience Manager > Forms > Formulaires et documents.

* Ouvrez le dossier 401KStatement.

* Ouvrez le document 401KStatement en mode édition.

**Configurer la zone cible Panneau gauche**

* Appuyez sur la zone cible Panneau gauche à droite et cliquez sur l’icône « + » pour ouvrir la boîte de dialogue Insérer le composant.

* Insérez le composant de texte.

* Appuyez doucement sur le nouveau composant de texte ajouté pour afficher la barre d’outils du composant.

* Sélectionnez l’icône en forme de crayon pour modifier le texte par défaut.

* Remplacez le texte par défaut par **« Estimation de votre pension de retraite »**.

**Configurer la zone cible Panneau droit**

* Appuyez sur la zone cible Panneau droit à droite et cliquez sur l’icône « + » pour ouvrir la boîte de dialogue Insérer le composant.

* Insérez le composant de texte.

* Appuyez doucement sur le nouveau composant de texte ajouté pour afficher la barre d’outils du composant.

* Sélectionnez l’icône en forme de crayon pour modifier le texte par défaut.

* Remplacez le texte par défaut par **« Pension mensuelle de retraite estimée »**.

## Ajouter un fragment de document Estimation de pension de retraite {#add-retirement-income-outlook-document-fragment}

* Cliquez sur l’icône Ressources et appliquez le filtre pour afficher les ressources de type « Fragments de document ». Faites glisser et déposez le fragment de document RetirementRevenueOutlook dans la zone cible Panneau de gauche.

* Vous pouvez vous référer à [cette page](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html?lang=fr) pour l’ajout d’un fragment de document aux zones de contenu.

## Ajouter un graphique des revenus mensuels estimés {#adding-estimated-monthly-income-chart}

* Cliquez sur la zone cible Panneau droit dans la partie droite. Cliquez sur l’icône « + » pour insérer le composant de graphique. Nous utiliserons un graphique en colonnes pour afficher les revenus mensuels estimés. Appuyez doucement sur le composant de graphique nouvellement inséré. Sélectionnez l’icône en forme de clé à molette pour ouvrir la feuille des propriétés de configuration. Configurez le graphique avec les propriétés suivantes, comme illustré dans la copie d’écran ci-dessous.

**AEM Forms 6.4 - Configuration du graphique à colonnes des revenus mensuels estimés**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configuration du graphique à colonnes des revenus mensuels estimés**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## Étapes suivantes

[Configurer un graphique en secteurs](./parteleven.md)
