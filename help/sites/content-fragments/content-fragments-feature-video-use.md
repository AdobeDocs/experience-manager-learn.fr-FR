---
title: Création de fragments de contenu dans AEM
description: Les fragments de contenu sont une abstraction de contenu dans AEM qui permet la création et la gestion de contenu textuel, indépendamment des canaux pris en charge.
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: Cloud Service
topic: Content Management
role: User
level: Beginner
exl-id: d33c033a-9577-4d4e-99be-f3c7e2a4ce73
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 12%

---

# Création de fragments de contenu {#authoring-content-fragments}

Les fragments de contenu sont une abstraction de contenu dans AEM qui permet la création et la gestion de contenu textuel, indépendamment des canaux pris en charge.

Les fragments de contenu AEM sont du contenu éditorial texte qui peut inclure certains éléments de données structurés associés, mais considérés comme du contenu pur sans informations de conception ou de mise en page. Les fragments de contenu sont généralement créés en tant que contenu indépendant du canal, destiné à être utilisé et réutilisé sur plusieurs canaux, ce qui à son tour encapsule le contenu dans une expérience spécifique au contexte.

Cette série vidéo couvre le cycle de vie de la création de fragments de contenu dans AEM. Détails sur [La diffusion de fragments de contenu se trouve ici](content-fragments-delivery-feature-video-use.md).

1. Activation et définition de modèles de fragment de contenu
2. Création de fragments de contenu
3. Téléchargement de fragments de contenu
4. Capacités éditoriales

>[!CONTEXTUALHELP]
>id="aemcloud_sites_admin_content_fragments"
>title="Gestion des fragments"
>abstract="Découvrez comment les fragments de contenu vous permettent de concevoir, créer, organiser et utiliser du contenu indépendant des pages."

## Définition de modèles de fragment de contenu {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

AEM les modèles de fragments de contenu, les schémas de données des fragments de contenu, doivent être activés via AEM [[!UICONTROL Explorateur de configuration]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=fr), qui permet de définir des modèles de fragment de contenu par configuration.

## Création de fragments de contenu {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

Les configurations AEM sont appliquées aux hiérarchies de dossiers AEM Assets pour permettre la création de leurs modèles de fragments de contenu en tant que fragments de contenu. Les fragments de contenu prennent en charge une expérience de création riche basée sur des formulaires permettant de modéliser le contenu en tant que collection d’éléments.

Les fragments de contenu peuvent avoir plusieurs variantes, chaque variante répondant à un cas d’utilisation différent (pensée, pas nécessairement canal) pour le contenu.

*Exemple de biographie d&#39;athlète à importer :*\
**[sandra-parent-bio.txt](assets/sandra-sprient-bio.txt)**

## Téléchargement de fragments de contenu {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

Les fragments de contenu AEM peuvent être téléchargés à partir de l’auteur AEM en tant que fichier Zip contenant des variantes, des éléments et des métadonnées.

*Exemple de fichier zip de téléchargement de fragment de contenu :*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## Fonctionnalités éditoriales des fragments de contenu {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> Les annotations et la comparaison de versions pour les fragments de contenu ont été introduites dans [AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=fr) et [AEM 6.3 Service Pack 3](https://helpx.adobe.com/fr/experience-manager/6-3/release-notes/sp3-release-notes.html).

## Étapes suivantes

En savoir plus sur [diffusion de fragments de contenu](content-fragments-delivery-feature-video-use.md).

## Ressources supplémentaires {#additional-resources}

* [Diffusion de fragments de contenu](content-fragments-delivery-feature-video-use.md)
* [AEM composants principaux WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [AEM composant de fragment de contenu WCM principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=fr)

Pour télécharger et installer le package ci-dessous sur une instance AEM 6.4+ pour l’état final de la série vidéo :

**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
