---
title: Création de fragments de contenu dans AEM
description: 'Les fragments de contenu sont une abstraction de contenu dans AEM qui permet la création et la gestion de contenu texte indépendamment des canaux pris en charge. '
sub-product: content-services
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 1faf22f2e664b775c11e16cb1dfa18b363a7316b
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 8%

---


# Création de fragments de contenu {#authoring-content-fragments}

Les fragments de contenu sont une abstraction de contenu dans AEM qui permet la création et la gestion de contenu texte indépendamment des canaux pris en charge.

>[!NOTE]
>
>La fonctionnalité AEM Fragment de contenu couverte par ces vidéos a été introduite pour la première fois dans [AEM 6.3 + FP 19008 et FP 19614](https://helpx.adobe.com/experience-manager/6-3/release-notes/content-services-fragments-featurepack.html).


Les fragments de contenu AEM sont un contenu éditorial texte qui peut inclure certains éléments de données structurées associés mais considérés comme du contenu pur sans informations de conception ou de mise en page. Les fragments de contenu sont généralement créés en tant que contenu indépendant du canal, destiné à être utilisé et réutilisé sur plusieurs canaux, ce qui à son tour encapsule le contenu dans un contexte spécifique.

Cette série de vidéos couvre le cycle de vie de création de fragments de contenu en AEM. Vous trouverez des détails sur la [diffusion de fragments de contenu ici](content-fragments-delivery-feature-video-use.md).

1. Activation et définition de modèles de fragments de contenu
2. Création de fragments de contenu
3. Téléchargement de fragments de contenu
4. Fonctionnalités éditoriales

## Defining Content Fragment Models {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

aem les modèles de fragments de contenu, les schémas de données des fragments de contenu, doivent être activés via AEM navigateur [[!UICONTROL de]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)configuration, ce qui permet de définir les modèles de fragments de contenu par configuration.

## Création de fragments de contenu  {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

Les configurations AEM sont appliquées aux hiérarchies de dossiers AEM Assets pour permettre la création de leurs modèles de fragments de contenu en tant que fragments de contenu. Les fragments de contenu prennent en charge une expérience de création de formulaires enrichie permettant de modéliser le contenu en tant que collection d’éléments.

Les fragments de contenu peuvent comporter plusieurs variantes, chacune d’elles répondant à un cas d’utilisation différent (pensée, pas nécessairement canal) du contenu.

*Exemple de biographie d&#39;athlète à importer :*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## Téléchargement de fragments de contenu {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

aem Fragments de contenu peuvent être téléchargés depuis l’auteur AEM sous la forme d’un fichier Zip contenant des variantes, des éléments et des métadonnées.

*Exemple de fichier zip de téléchargement de fragment de contenu :*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## Fonctionnalités éditoriales du fragment de contenu {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> L’annotation et la comparaison de versions pour les fragments de contenu ont été introduites dans [AEM Service Pack 2](https://helpx.adobe.com/fr/experience-manager/aem-releases-updates.html) 6.4 et [AEM Service Pack 3](https://helpx.adobe.com/fr/experience-manager/6-3/release-notes/sp3-release-notes.html)6.3.

## Étapes suivantes

Découvrez comment [diffuser des fragments](content-fragments-delivery-feature-video-use.md)de contenu.

## Ressources supplémentaires {#additional-resources}

* [Diffusion de fragments de contenu](content-fragments-delivery-feature-video-use.md)
* [Composants principaux WCM AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)
* [Composant de fragment de contenu principal de gestion de contenu Web AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/content-fragment-component.html)

Pour télécharger et installer le package ci-dessous sur une instance AEM 6.4+ pour l’état final à partir de la série vidéo :

**[aem_demo_fluide-experience-content-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
