---
title: Présentation du thème dans Asset Share Commons
seo-title: Présentation du thème dans Asset Share Commons
description: Matériel pour la compréhension fonctionnelle et technique Ressources Partager des communes
seo-description: Matériel pour la compréhension fonctionnelle et technique Ressources Partager des communes
uuid: 5991a015-392a-4bb5-8332-192681505b07
discoiquuid: 08a5a394-c62b-4748-b303-33117f283612
contentOwner: dgonzale
feature: asset-share, brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 1%

---


# Présentation du thème dans Asset Share Commons {#asset-share-commons-theme}

Une brève introduction à ce thème dans Asset Share Commons. La vidéo passe en revue le processus de création d’un thème avec un modèle de couleurs personnalisé.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

Dans cette vidéo, un nouveau thème sera créé en fonction du thème &quot;Asset Share Commons Dark&quot;. Le modèle de couleurs correspondra à un logo personnalisé pour donner au site une apparence cohérente.

## Variables de thème

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

Télécharger [Thème de bibliothèque client personnalisé](assets/asc-theme-demo.zip)

## Ressources supplémentaires{#additional-resources}

* [Téléchargements de versions d’Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [Téléchargements de versions d’ACS AEM Commons 3.11.0+](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)