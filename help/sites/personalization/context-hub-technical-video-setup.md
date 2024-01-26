---
title: Configurer ContextHub pour la personnalisation avec AEM Sites
description: ContextHub est un framework permettant de stocker, manipuler et présenter des données contextuelles. L’API JavaScript ContextHub vous permet d’accéder aux magasins pour créer, mettre à jour et supprimer des données si nécessaire. Par conséquent, ContextHub représente une couche de données sur vos pages. Cette page décrit comment ajouter ContextHub à vos pages de site AEM.
feature: Context Hub
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 375
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 93%

---

# Configurer ContextHub pour la personnalisation {#set-up-contexthub}

ContextHub est un framework permettant de stocker, manipuler et présenter des données contextuelles. L’API JavaScript ContextHub vous permet d’accéder aux magasins pour créer, mettre à jour et supprimer des données si nécessaire. En tant que tel, ContextHub représente une couche de données sur vos pages.Cette page décrit comment ajouter ContextHub à vos pages de site AEM.

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>Nous utilisons le site de référence WKND pour cette vidéo et cela ne fait pas partie de la version d’AEM. Vous pouvez télécharger la [dernière version ici](https://github.com/adobe/aem-guides-wknd/releases).

Ajoutez ContextHub à vos pages pour activer les fonctionnalités ContextHub et créer un lien vers les bibliothèques JavaScript ContextHub. L’API JavaScript ContextHub permet d’accéder aux données contextuelles gérées par ContextHub.

## Ajouter ContextHub à un composant de page {#adding-contexthub-to-a-page-component}

Pour activer les fonctionnalités ContextHub et créer un lien vers les bibliothèques JavaScript ContextHub, insérez le composant `contexthub` dans la section `<head>` de votre page web. Le code HTL de votre composant de page devrait ressembler à ceci :

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## Configuration du site et des segments ContextHub {#site-configuration-and-contexthub-segments}

ContextHub propose un moteur de segmentation qui gère les segments et détermine les segments qui sont résolus pour le contexte actuel. Plusieurs segments sont définis. Vous pouvez utiliser l’API JavaScript pour [déterminer les segments résolus](https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/ch-adding.html?lang=fr#determining-resolved-contexthub-segments). Activez les segments ContextHub pour votre site sous l’[[!UICONTROL Explorateur de configurations]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=fr).

## Créer des segments {#create-segments}

Créez des segments AEM qui agissent comme des règles pour les teasers. En d’autres termes, ils définissent lorsque le contenu d’un teaser apparaît sur une page web. Le contenu peut alors cibler les besoins et centres d’intérêts spécifiques des visiteurs et visiteuses en fonction du ou des segments qui leur correspondent.

## Affecter la configuration cloud, le chemin d’accès au segment et le chemin ContextHub à votre site {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Affectez le chemin de configuration cloud, le chemin de segmentation et le chemin ContextHub au nœud racine de votre site afin que vous puissiez créer une expérience personnalisée pour votre audience. ContextHub vous permet de manipuler les données contextuelles et de tester les segments résolus.

![CRXDE Lite.](assets/crx-de-properties.png)

Vous pouvez lire plus d’informations sur ContextHub et la segmentation ci-dessous :

* [ContextHub](https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/contexthub.html?lang=fr)
* [Ajouter ContextHub à la page et accéder aux magasins](https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/ch-adding.html?lang=fr)
* [Fonctionnement de la segmentation](https://experienceleague.adobe.com/docs/experience-manager-65/classic-ui/personalization/classic-personalization-campaigns-segmentation.html?lang=fr)
* [Configuration de la segmentation avec ContextHub](https://experienceleague.adobe.com/docs/experience-manager-65/administering/personalization/segmentation.html?lang=fr)
