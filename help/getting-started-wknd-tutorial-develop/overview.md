---
title: Commencer avec AEM Sites - Tutoriel sur WKND
description: Découvrez comment mettre en œuvre un site AEM pour une marque de style de vie fictive appelée WKND. Découvrez les sujets Experience Manager fondamentaux tels que la configuration de projet, les archétypes Maven, les composants principaux, les modèles modifiables, les bibliothèques clientes et le développement de composants.
topics: development
version: Cloud Service
activity: develop
audience: developer
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 100%

---

# Commencer avec AEM Sites - Tutoriel sur WKND {#introduction}

{{edge-delivery-services}}

Bienvenue dans ce tutoriel en plusieurs parties conçu pour les développeurs et développeuses qui découvrent Adobe Experience Manager (AEM). Ce tutoriel décrit la mise en œuvre d’un site AEM pour une marque de style de vie fictive, WKND. Le tutoriel aborde des sujets fondamentaux tels que la configuration de projet, les composants principaux, les modèles modifiables, les bibliothèques côté client et le développement de composants avec Adobe Experience Manager Sites.

## Présentation {#wknd-tutorial-overview}

Ce tutoriel en plusieurs parties a pour objectif d’apprendre aux développeurs et développeuses comment mettre en œuvre un site web à l’aide des dernières normes et technologies d’Adobe Experience Manager (AEM). Après avoir terminé ce tutoriel, le développeur ou la développeuse doit comprendre les bases de la plateforme et les schémas de conception courants dans AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Options de démarrage d’un projet Sites

Il existe deux approches de base pour démarrer un projet AEM Sites.

**Archétype de projet AEM** - Approche traditionnelle du développement AEM générant un projet AEM minimal à l’aide d’un modèle Maven. Il s’agit de l’approche recommandée pour les projets AEM 6.5/6.4 et les projets AEM as a Cloud Service qui prévoient une personnalisation importante. Le tutoriel présente un aperçu plus approfondi du développement AEM.

[Démarrer le tutoriel avec l’archétype de projet AEM](./project-archetype/overview.md)

**Modèles de site AEM** - Approche low-code également appelée Création rapide de site, permettant de générer un site AEM à l’aide d’un modèle de site prédéfini. Utilisez les composants et les modèles prêts à l’emploi pour mettre rapidement un site en service. Utilisez un workflow de thème pour appliquer des styles et des personnalisations spécifiques à la marque avec uniquement des feuilles CSS et JavaScript. Recommandé pour les nouveaux projets et les développeurs et développeuses qui débutent. Disponible uniquement pour AEM as a Cloud Service.

[Démarrer le tutoriel à l’aide d’un modèle de site](./site-template/create-site.md)

## Kit d’interface utilisateur Adobe XD

Pour rapprocher ce tutoriel d’un scénario réel, la talentueuse équipe de conception d’UX d’Adobe a créé les maquettes du site à l’aide d’[Adobe XD](https://www.adobe.com/fr/products/xd.html). Au cours du tutoriel, divers éléments des conceptions sont implémentés dans un site AEM entièrement modifiable. Remercions tout particulièrement **Lorenzo Buosi** et **Kilian Amendola** pour leur magnifique conception du site WKND.

Téléchargez les kits d’interface utilisateur XD :

* [Kit d’interface utilisateur des composants principaux d’AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit d’interface utilisateur WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Site de référence {#reference-site}

Une version complète de WKND Site est également disponible comme référence ici : [https://wknd.site/](https://wknd.site/).

Le tutoriel couvre les principales compétences de développement nécessaires à un développeur ou une développeuse AEM, bien qu’il ne permette *pas* de créer l’ensemble du site de bout en bout. Le site de référence terminé est une autre ressource intéressante à explorer, qui vous permettra de découvrir d’autres fonctionnalités d’AEM prêtes à l’emploi.

Pour tester le code le plus récent avant de passer au tutoriel, téléchargez et installez la **[dernière version de GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Optimisé par Adobe Stock

La plupart des images du site de référence WKND proviennent d’[Adobe Stock](https://stock.adobe.com/fr) et constituent des ressources tierces, telles que définies dans les conditions supplémentaires des ressources de démonstration à l’adresse [https://www.adobe.com/fr/legal/terms.html](https://www.adobe.com/fr/legal/terms.html). Si vous souhaitez utiliser une image Adobe Stock à d’autres fins que l’affichage de ce site web de démonstration, par exemple en la montrant sur un site web ou dans du matériel marketing, vous pouvez acheter une licence sur Adobe Stock.

Avec Adobe Stock, vous avez accès à plus de 140 millions d’images libres de droits de haute qualité, dont des photos, des graphiques, des vidéos et des modèles, pour lancer vos projets de création.

## Étapes suivantes {#next-steps}

Alors qu’attendez-vous ? Découvrez comment [générer un nouveau projet Adobe Experience Manager à l’aide de l’archétype de projet AEM](./project-archetype/overview.md) ou [créer un site à l’aide d’un modèle de site](./site-template/create-site.md).
