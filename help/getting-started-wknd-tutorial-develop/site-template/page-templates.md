---
title: Modèles de page
description: Découvrez comment créer et modifier des modèles de page. Comprenez la relation entre un modèle de page et une page. Découvrez comment configurer les stratégies d’un modèle de page afin de garantir la gouvernance granulaire et la cohérence de marque du contenu.  Un modèle d’article de magazine bien structuré est créé à partir d’une maquette d’Adobe XD.
version: Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1593
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 100%

---

# Modèles de page {#page-templates}

Dans ce chapitre, nous allons explorer la relation entre un modèle de page et une page. Nous allons créer un modèle d’article de magazine sans style basé sur des maquettes d’[Adobe XD](https://www.adobe.com/fr/products/xd.html). Lors du processus de création du modèle, les composants principaux et les configurations de stratégie avancées sont abordés.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que les étapes décrites dans le chapitre [Créer du contenu et publier des modifications](./author-content-publish.md) ont été terminées.

## Objectif

1. Découvrez les détails des modèles de page et comment les stratégies peuvent être utilisées pour appliquer un contrôle précis du contenu de la page.
1. Découvrez comment les modèles et les pages sont liés.
1. Créez un modèle et une page.

## Ce que vous allez créer {#what-you-will-build}

Dans cette partie du tutoriel, vous allez créer un modèle de page d’article de magazine qui pourra être utilisé pour créer de nouveaux articles de magazine et qui s’aligne sur une structure commune. Le modèle est basé sur des conceptions et un kit d’interface utilisateur produits dans Adobe XD. Ce chapitre se concentre uniquement sur la création de la structure, ou squelette, du modèle. Aucun style n’est implémenté, mais le modèle et les pages sont fonctionnels.

## Créer le modèle de page d’article de magazine

Lors de la création d’une page, vous devez sélectionner un modèle, qui est utilisé comme base pour créer la page. Le modèle définit la structure de la page créée, le contenu initial et les composants autorisés.

Il existe 3 domaines principaux dans les [Modèles de page](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=fr) :

1. **Structure** : définit les composants qui font partie du modèle. Ces composants ne sont pas modifiables par les créateurs et créatrices de contenu.
1. **Contenu initial** : définit les composants avec lesquels le modèle commence. Ces composants peuvent être modifiés et/ou supprimés par les créateurs et créatrices de contenu.
1. **Stratégies** : elles définissent des configurations sur le comportement des composants et sur les options disponibles pour les créateurs et créatrices.

Créez ensuite un modèle dans AEM qui correspond à la structure des maquettes. Cela se produit dans une instance locale d’AEM. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

Vous pouvez utiliser la miniature suivante pour identifier votre modèle (ou télécharger la vôtre).

![Miniature du modèle de page d’article.](./assets/page-templates/article-page-template-thumbnail.png)


### Package de solution

Une [solution du modèle de magazine](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) terminée peut être téléchargée et installée via le gestionnaire de packages.

## Mettre à jour l’en-tête et le pied de page avec des fragments d’expérience {#experience-fragments}

Une pratique courante lors de la création d’un contenu global, tel qu’un en-tête ou un pied de page, consiste à utiliser un [Fragment d’expérience](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=fr). Les fragments d’expérience permettent aux utilisateurs et utilisatrices de combiner plusieurs composants afin de créer un seul composant référençable. Les fragments d’expérience ont l’avantage de prendre en charge la gestion multisite et la [localisation](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=fr#localized-site-structure).

Le modèle de site a généré un en-tête et un pied de page. Ensuite, mettez à jour les fragments d’expérience pour qu’ils correspondent aux maquettes. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

Étapes de haut niveau pour la vidéo ci-dessous :

1. Téléchargez le package d’exemple de contenu **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Téléchargez et installez le package de contenu à l’aide du gestionnaire de packages.
1. Mettez à jour les fragments d’expérience d’en-tête et de pied de page pour utiliser le logo WKND.

## Créer une page d’article de magazine

Créez ensuite une page à l’aide du modèle de page d’article de magazine. Créez le contenu de la page pour qu’il corresponde aux maquettes du site. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

Utilisez le [texte fourni](./assets/page-templates/la-skateparks-copy.txt) pour remplir le corps de l’article.

## Félicitations. {#congratulations}

Félicitations, vous venez de créer un modèle et une page avec Adobe Experience Manager Sites.

### Étapes suivantes {#next-steps}

À ce stade, la page d’article de magazine et le site ne correspondent pas aux styles de marque pour WKND. Suivez le tutoriel [Thème](theming.md) pour connaître les bonnes pratiques de mise à jour du code front-end CSS et Javascript utilisé pour appliquer des styles généraux au site.

### Package de solution

Un package de solution pour ce chapitre peut être téléchargé : [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
