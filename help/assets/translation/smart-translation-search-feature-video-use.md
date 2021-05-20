---
title: Utilisation de la recherche de traduction dynamique avec AEM Assets
description: La recherche de traduction intelligente permet la recherche et la découverte multilingue automatiquement dans AEM contenu, Ressources et Pages, prenant en charge plus de 50 langues et réduisant le besoin de traduction manuelle du contenu.
version: 6.3, 6.4, 6.5
feature: Rechercher
topic: Gestion de contenu
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# Utilisation de la recherche de traduction dynamique avec AEM Assets{#using-smart-translation-search-with-aem-assets}

La recherche de traduction intelligente permet la recherche et la découverte multilingue automatiquement dans AEM contenu, Ressources et Pages, prenant en charge plus de 50 langues et réduisant le besoin de traduction manuelle du contenu.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Recherche de traduction intelligente permet aux utilisateurs d’effectuer des recherches de contenu dans AEM en utilisant des termes non anglais, afin de faire correspondre les ressources d’comportant des termes anglais équivalents.

La recherche de traduction intelligente est un parfait compliment à l’AEM des balises intelligentes qui sont appliquées aux ressources en anglais.

Cette vidéo suppose que [AEM Recherche de traduction intelligente](smart-translation-search-technical-video-setup.md) a été configurée.

## Fonctionnement de la recherche de traduction dynamique {#how-smart-translation-search-works}

![Diagramme de flux de recherche de traduction dynamique](assets/smart-translation-search-flow.png)

1. AEM utilisateur effectue une recherche de texte intégral, fournissant ainsi un terme de recherche localisé (par exemple, le terme espagnol pour &quot;homme&quot;, &quot;hombre&quot;).
2. La recherche de traduction intelligente, fournie par le lot OSGi de traduction automatique Apache Oak, est engagée et évalue si les termes de recherche fournis peuvent être traduits à l’aide des modules de langue enregistrés.
3. Tous les termes traduits de l’étape #2 sont collectés et la requête est augmentée en interne afin de les inclure comme termes de recherche. Cet ensemble augmenté de termes de recherche s’ils sont évalués normalement par rapport aux index de recherche AEM qui recherchent des correspondances pertinentes.
4. Les résultats de la recherche qui correspondent au terme d’origine (&#39;hombre&#39;) ou au terme traduit (&#39;man&#39;) sont collectés et renvoyés à l’utilisateur comme résultats de la recherche.

## Ressources supplémentaires{#additional-resources}

* [Configuration de la recherche de traduction dynamique avec AEM Assets](smart-translation-search-technical-video-setup.md)
* [Packs de langues Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)