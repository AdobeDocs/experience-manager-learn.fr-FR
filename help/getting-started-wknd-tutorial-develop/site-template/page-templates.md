---
title: Modèles de page
description: Découvrez comment créer et modifier des modèles de page. Comprendre la relation entre un modèle de page et une page. Découvrez comment configurer les stratégies d’un modèle de page afin de garantir une gouvernance granulaire et une cohérence de la marque pour le contenu.  Un modèle d’article de magazine bien structuré sera créé à partir d’une maquette provenant d’Adobe XD.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
source-git-commit: 0225b7f2e495d5c020ea5192302691e3466808ed
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 5%

---

# Modèles de page {#page-templates}

Dans ce chapitre, nous allons explorer la relation entre un modèle de page et une page. Nous allons créer un modèle d’article de magazine sans style basé sur des maquettes de [AdobeXD](https://www.adobe.com/products/xd.html). Lors du processus de création du modèle, les composants principaux et les configurations de stratégie avancées sont traités.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que les étapes décrites dans la section [Créer du contenu et publier les modifications](./author-content-publish.md) Le chapitre a été terminé.

## Objectif

1. Découvrez les détails des modèles de page et comment les stratégies peuvent être utilisées pour appliquer un contrôle granulaire du contenu de la page.
1. Découvrez comment les modèles et les pages sont liés.
1. Créez un modèle et créez une page.

## Ce que vous allez créer {#what-you-will-build}

Dans cette partie du tutoriel, vous allez créer un modèle de page d’article de magazine qui pourra être utilisé pour créer de nouveaux articles de magazine et qui s’aligne sur une structure commune. Le modèle sera basé sur des conceptions et un kit d’interface utilisateur produit dans Adobe XD. Ce chapitre se concentre uniquement sur la construction de la structure ou du squelette du modèle. Aucun style ne sera implémenté, mais le modèle et les pages seront fonctionnels.

## Créer un modèle de page d’article de magazine

Lors de la création d’une page, vous devez sélectionner un modèle. C’est la base pour la création de la page. Le modèle définit la structure de la page créée, le contenu initial et les composants autorisés.

Il existe 3 zones principales de [Modèles de page](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=fr):

1. **Structure** - définit les composants qui font partie du modèle. Ils ne seront pas modifiables par les auteurs de contenu.
1. **Contenu initial** : définit les composants dont le modèle commencera, qui peuvent être modifiés et/ou supprimés par les auteurs de contenu.
1. **Stratégies** - définit les configurations sur le comportement des composants et les options disponibles pour les auteurs.

Créez ensuite un modèle dans AEM qui correspond à la structure des maquettes. Cela se produit dans une instance locale d’AEM. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

Vous pouvez utiliser la miniature suivante pour identifier votre modèle (ou télécharger le vôtre).

![Miniature du modèle de page d’article](./assets/page-templates/article-page-template-thumbnail.png)


### Package de solution

Terminé [solution du modèle Magazine](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) peut être téléchargé et installé via le gestionnaire de modules.

## Mise à jour de l’en-tête et du pied de page avec les fragments d’expérience {#experience-fragments}

Une pratique courante lors de la création d’un contenu global, tel qu’un en-tête ou un pied de page, consiste à utiliser une [Fragment d’expérience](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Les fragments d’expérience permettent aux utilisateurs de combiner plusieurs composants afin de créer un seul composant pouvant faire référence. Les fragments d’expérience ont l’avantage de prendre en charge la gestion multisite et [localisation](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

Le modèle Site a généré un en-tête et un pied de page. Ensuite, mettez à jour les fragments d’expérience pour qu’ils correspondent aux maquettes. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Étapes de haut niveau pour la vidéo ci-dessous :

1. Téléchargez l’exemple de module de contenu **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Téléchargez et installez le module de contenu à l’aide de Package Manager.
1. Mise à jour des fragments d’expérience d’en-tête et de pied de page pour utiliser le logo WKND

## Créer une page d’article Magazine

Créez ensuite une page à l’aide du modèle Page Article du magazine . Créez le contenu de la page pour qu’il corresponde aux maquettes du site. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

Utilisez la variable [texte fourni](./assets/page-templates/la-skateparks-copy.txt) pour remplir le corps de l’article.

## Félicitations ! {#congratulations}

Félicitations, vous venez de créer un modèle et une page avec Adobe Experience Manager Sites.

### Étapes suivantes {#next-steps}

À ce stade, la page d’article de magazine et le site ne correspondent pas aux styles de marque pour WKND. Suivez la [Thème](theming.md) tutoriel pour découvrir les bonnes pratiques relatives à la mise à jour du code frontal CSS et JavaScript utilisé pour appliquer des styles globaux au site.

### Package de solution

Un package de solution pour ce chapitre peut être téléchargé : [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
