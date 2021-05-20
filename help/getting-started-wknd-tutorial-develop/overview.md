---
title: Prise en main du développement AEM Sites – Tutoriel WKND
description: Prise en main du développement AEM Sites – Tutoriel WKND. Le tutoriel WKND est un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent Adobe Experience Manager. Le tutoriel décrit la mise en oeuvre d’un site AEM pour une marque de style de vie fictive, WKND. Le tutoriel aborde des sujets fondamentaux tels que la configuration de projet, les archétypes maven, les composants principaux, les modèles modifiables, les bibliothèques clientes et le développement de composants.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Composants principaux, éditeur de page, modèles modifiables, AEM archétype de projet
topic: Gestion de contenu, développement
role: Developer
level: Beginner
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 6%

---


# Prise en main du développement AEM Sites – Tutoriel WKND {#introduction}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent Adobe Experience Manager (AEM). Ce tutoriel décrit la mise en oeuvre d’un site AEM pour une marque de style de vie fictive WKND. Le tutoriel aborde des sujets fondamentaux tels que la configuration de projet, les composants principaux, les modèles modifiables, les bibliothèques côté client et le développement de composants avec Adobe Experience Manager Sites.

## Présentation {#wknd-tutorial-overview}

Ce tutoriel en plusieurs parties a pour objectif d’apprendre à un développeur comment mettre en oeuvre un site web à l’aide des dernières normes et technologies d’Adobe Experience Manager (AEM). Après avoir terminé ce tutoriel, un développeur doit comprendre les bases de la plateforme et connaître les schémas de conception courants dans AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Options de démarrage d’un projet Sites

Il existe deux approches de base pour démarrer un projet AEM Sites.

**AEM Archétype de projet**  : approche traditionnelle d’AEM développement en générant un projet d’AEM minimal à l’aide d’un modèle Maven. Il s’agit de l’approche recommandée pour les projets AEM 6.5/6.4 et AEM en tant que projets Cloud Service qui anticipent une personnalisation importante. Le tutoriel présente un aperçu plus approfondi du développement AEM.

[Démarrez le tutoriel avec AEM Project Archetype.](./project-archetype/overview.md)

**Modèles de site AEM**  : approche à code faible pour générer un site AEM à l’aide d’un modèle de site prédéfini. Utilisez les composants et les modèles prêts à l’emploi pour mettre rapidement un site en service. Utilisez un workflow de thème pour appliquer des styles et des personnalisations spécifiques à la marque avec uniquement des feuilles CSS et JavaScript. Recommandé pour les nouveaux projets et développeurs. Actuellement disponible uniquement pour AEM en tant que Cloud Service.

[Démarrez le tutoriel à l’aide d’un modèle de site](./site-template/create-site.md)

## Kit d’interface utilisateur Adobe XD

Pour que ce tutoriel se rapproche d’un scénario réel, les talentueux concepteurs de l’expérience utilisateur de l’Adobe ont créé les maquettes pour le site à l’aide de [Adobe XD](https://www.adobe.com/products/xd.html). Au cours du tutoriel, divers éléments des conceptions sont mis en oeuvre dans un site d’AEM entièrement modifiable. Remerciements particuliers à **Lorenzo Buosi** et **Kilian Amendola** qui ont créé le magnifique design pour le site WKND.

Téléchargez les kits d’interface utilisateur XD :

* [AEM Core Component UI Kit](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit d’interface utilisateur WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Site de référence {#reference-site}

Une version complète du site WKND est également disponible comme référence : [https://wknd.site/](https://wknd.site/)

Le tutoriel couvre les principales compétences de développement nécessaires à un développeur d’AEM, mais *ne* générera pas l’ensemble du site de bout en bout. Le site de référence terminé est une autre ressource intéressante à explorer et à découvrir d’AEM fonctionnalités prêtes à l’emploi.

Pour tester le code le plus récent avant de passer au tutoriel, téléchargez et installez la dernière version de **[GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Optimisé par Adobe Stock

La plupart des images du site Web de référence WKND proviennent d’[Adobe Stock](https://stock.adobe.com/) et sont des matériaux tiers, comme défini dans les termes supplémentaires de la ressource de démonstration à l’adresse [https://www.adobe.com/legal/terms.html](https://www.adobe.com/fr/legal/terms.html). Si vous souhaitez utiliser une image Adobe Stock à d’autres fins que l’affichage de ce site web de démonstration, par exemple en la montrant sur un site web ou dans du matériel marketing, vous pouvez acheter une licence sur Adobe Stock.

Avec Adobe Stock, vous avez accès à plus de 140 millions d’images libres de droits de haute qualité, dont des photos, graphiques, vidéos et modèles, pour lancer vos projets de création.

## Étapes suivantes {#next-steps}

Qu&#39;attendez-vous?! Découvrez comment [générer un nouveau projet Adobe Experience Manager à l’aide de l’AEM archétype de projet](./project-archetype/overview.md) ou [créer un site à l’aide d’un modèle de site](./site-template/create-site.md).
