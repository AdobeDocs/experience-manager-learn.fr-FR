---
title: Étendre des propriétés de page dans AEM Sites
description: Découvrez comment étendre les champs de métadonnées des propriétés de page dans Adobe Experience Manager Sites. Cette vidéo présente le moyen le plus efficace d’y parvenir en utilisant les fonctionnalités de Sling Resource Merger.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 497
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '198'
ht-degree: 100%

---

# Étendre les propriétés de page {#extending-page-properties-in-aem-sites}

La personnalisation des champs de métadonnées des propriétés de page est une exigence courante dans toute implémentation de Sites. Cette vidéo présente le moyen le plus efficace d’y parvenir en utilisant les fonctionnalités de Sling Resource Merger.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

La vidéo ci-dessus montre la personnalisation des propriétés de page pour le [site de référence WKND](https://github.com/adobe/aem-guides-wknd).

## Exemple de package de propriétés de page WKND

Vous pouvez utiliser l’[exemple de package de propriétés de page WKND](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) fourni contenant les personnalisations des onglets **WKND** et **De base** affichées dans la vidéo ci-dessus. La personnalisation de l’onglet **Médias sociaux** n’est pas fournie car le [composant de page WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) utilise désormais la version V3 des composants principaux de la gestion de contenu web et, dans la version V3, le [partage social est obsolète](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Toutefois, à des fins d’apprentissage, vous pouvez pointer le composant page WKND vers la version V2 des composants principaux de la gestion de contenu web à l’aide de la valeur de propriété `sling:resourceSuperType` et recouvrir l’onglet [Médias sociaux](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95). Pour plus d’informations, voir [Configuration des propriétés de page](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=fr#configuring-your-page-properties).

Cet exemple de package doit être installé sur l’instance locale du SDK AEM ou d’AEM 6.X.X à des fins d’apprentissage.
