---
title: Extension des propriétés de page dans AEM Sites
description: Découvrez comment étendre les champs de métadonnées des propriétés de page dans Adobe Experience Manager Sites. Cette vidéo présente le moyen le plus efficace d’y parvenir en utilisant les fonctionnalités de Sling Resource Merger.
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: 94a29a78edff17ec8089f7056dc118fd335ae484
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# Extension des propriétés de page {#extending-page-properties-in-aem-sites}

La personnalisation des champs de métadonnées des propriétés de page est une exigence courante dans toute implémentation de Sites. Cette vidéo présente le moyen le plus efficace d’y parvenir en utilisant les fonctionnalités de Sling Resource Merger.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=9&learn=on)

La vidéo ci-dessus montre la personnalisation des propriétés de page pour le [Site de référence WKND](https://github.com/adobe/aem-guides-wknd).

## Exemple de package de propriétés de page WKND

Vous pouvez utiliser la variable [exemple de package de propriétés de page WKND](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) contain **WKND** et **De base** personnalisations des onglets affichées dans la vidéo ci-dessus. Le **SocialMedia** la personnalisation des onglets n’est pas fournie comme [Composant de page WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) utilise désormais la version V3 des composants principaux de la gestion de contenu web et dans la version V3 de la fonction [le partage social est obsolète](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Toutefois, à des fins d’apprentissage, vous pouvez pointer le composant Page WKND vers la version V2 des composants principaux WCM à l’aide de la variable `sling:resourceSuperType` de la propriété et recouvrez la propriété [Réseaux sociaux](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) . Pour plus d’informations, voir [Configuration des propriétés de page](https://experienceleague.adobe.com/docs/experience-manager-64/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Cet exemple de package doit être installé sur l’instance locale AEM SDK ou AEM 6.X.X à des fins d’apprentissage.
