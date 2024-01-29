---
title: Présentation des thèmes dans Asset Share Commons
description: Éléments pour la compréhension à la fois fonctionnelle et technique d’Assets Share Commons.
version: 6.4, 6.5
topic: Content Management
feature: Asset Distribution
role: Developer
level: Intermediate
last-substantial-update: 2022-06-22T00:00:00Z
doc-type: Tutorial
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
duration: 732
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '106'
ht-degree: 100%

---

# Présentation des thèmes dans Asset Share Commons {#asset-share-commons-theme}

Une brève introduction aux thèmes dans Asset Share Commons. La vidéo passe en revue le processus de création d’un thème avec un modèle de couleurs personnalisé.

>[!VIDEO](https://video.tv.adobe.com/v/20572?quality=12&learn=on)

Dans cette vidéo, nous créons un thème à partir du thème sombre d’Asset Share Commons. Le modèle de couleurs correspondra à un logo personnalisé pour donner au site un aspect cohérent.

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

Télécharger [Thème de bibliothèque cliente personnalisée](assets/asc-theme-demo.zip)

## Ressources supplémentaires{#additional-resources}

* [Téléchargements de versions d’Asset Share Commons](https://github.com/adobe/asset-share-commons/releases)
* [Téléchargements des versions d’AEM Commons 3.11.0 et ultérieures d’ACS](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
