---
title: Modèles de page
seo-title: Prise en main de AEM Sites - Modèles de page
description: Découvrez comment créer et modifier des modèles de page. Comprendre la relation entre un modèle de page et une page. Découvrez comment configurer les stratégies d'un modèle de page afin de fournir une gouvernance granulaire et une cohérence de marque pour le contenu.  Un modèle d'article de magazine bien structuré sera créé à partir d'une maquette d'Adobe XD.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Gestion de contenu
feature: Composants principaux, Modèles modifiables, Éditeur de page
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 4%

---


# Modèles de page {#page-templates}

>[!CAUTION]
>
> Les fonctions de création rapide du site présentées ici seront publiées au second semestre 2021. La documentation correspondante est disponible à des fins de prévisualisation.

Ce chapitre porte sur la relation entre un modèle de page et une page. Nous créerons un modèle d’article de magazine sans style basé sur certaines maquettes de [AdobeXD](https://www.adobe.com/products/xd.html). Dans le processus de création du modèle, les composants principaux et les configurations de stratégie avancées sont traités.

## Conditions préalables {#prerequisites}

Il s’agit d’un didacticiel en plusieurs parties et on suppose que les étapes décrites dans le chapitre [Contenu d’auteur et modifications de publication](./author-content-publish.md) ont été terminées.

## Intention

1. Inspect une conception de page créée dans Adobe XD et la mappe aux composants principaux.
1. Comprendre les détails des modèles de page et comment les stratégies peuvent être utilisées pour imposer un contrôle granulaire du contenu de la page.
1. Découvrez comment les modèles et les pages sont liés.

## Ce que vous allez créer {#what-you-will-build}

Dans cette partie du didacticiel, vous allez créer un nouveau modèle de page d&#39;article de magazine qui pourra être utilisé pour créer de nouveaux articles de magazine et s&#39;aligner sur une structure commune. Le modèle sera basé sur des conceptions et un kit d’interface utilisateur produit dans AdobeXD. Ce chapitre porte uniquement sur la création de la structure ou du squelette du modèle. Aucun style ne sera implémenté mais le modèle et les pages seront fonctionnels.

## Planification de l’interface utilisateur avec Adobe XD {#adobexd}

Dans la plupart des cas, la planification d’un nouveau début Web avec des maquettes et des conceptions statiques. [Adobe ](https://www.adobe.com/products/xd.html) XDest un outil de conception qui permet de créer des expériences utilisateur. Ensuite, nous allons examiner un kit d’interface utilisateur et des maquettes pour vous aider à planifier la structure du modèle de page d’article.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Téléchargez le fichier [ de conception d’article ](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** WKND.

>[!NOTE]
>
> Un [kit d&#39;interface utilisateur des composants principaux générique ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) est également disponible comme point de départ pour les projets personnalisés.

## Création d’un modèle de page d’article de magazine

Lors de la création d’une page, vous devez sélectionner un modèle. C’est la base pour la création de la page. Le modèle définit la structure de la page résultante, le contenu initial et les composants autorisés.

Les [modèles de page](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html) se divisent en trois zones principales :

1. **Structure**  : définit les composants qui font partie du modèle. Ils ne seront pas modifiables par les auteurs de contenu.
1. **Contenu**  initial : définit les composants que le modèle utilisera, qui peuvent être modifiés et/ou supprimés par les auteurs de contenu.
1. **Stratégies**  : définit des configurations sur le comportement des composants et sur les options disponibles pour les auteurs.

Créez ensuite un nouveau modèle dans AEM qui correspond à la structure des maquettes. Cela se produit dans une instance locale d&#39;AEM. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### Package de solution

Une solution [finie du modèle Magazine](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip) peut être téléchargée et installée via Package Manager.

## Mettre à jour l’en-tête et le pied de page avec des fragments d’expérience {#experience-fragments}

Une pratique courante lors de la création de contenu global, tel qu’un en-tête ou un pied de page, consiste à utiliser un [fragment d’expérience](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragments d’expérience, permet aux utilisateurs de combiner plusieurs composants afin de créer un composant unique et référencable. Les fragments d’expérience ont l’avantage de prendre en charge la gestion multisite et [la localisation](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

Le modèle de site a généré un en-tête et un pied de page. Ensuite, mettez à jour les fragments d’expérience pour qu’ils correspondent aux maquettes. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Procédure de haut niveau pour la vidéo ci-dessous :

1. Téléchargez l’exemple de package de contenu **[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**.
1. Téléchargez et installez le package de contenu à l’aide de Package Manager.
1. Mettez à jour les fragments d’expérience d’en-tête et de pied de page pour utiliser le logo WKND.

## Création d’une page d’article Magazine

Créez ensuite une page à l’aide du modèle Page d’article du magazine. Créez le contenu de la page en fonction des maquettes de site. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## Félicitations! {#congratulations}

Félicitations, vous venez de créer un nouveau modèle et une nouvelle page avec Adobe Experience Manager Sites.

### Étapes suivantes {#next-steps}

A ce stade, la page d&#39;article du magazine et le site ne correspondent pas aux styles de marque pour WKND. Suivez le didacticiel [Theming](theming.md) pour découvrir les meilleures pratiques de mise à jour du code frontal CSS et JavaScript utilisé pour appliquer des styles globaux au site.

### Package de solution

Vous pouvez télécharger un package de solution pour ce chapitre : [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
