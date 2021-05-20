---
title: Personnalisation des icônes de composant dans Adobe Experience Manager Sites
description: Les icônes de composant permettent aux auteurs d’identifier rapidement un composant avec des icônes ou des abréviations significatives. Les auteurs peuvent désormais trouver plus rapidement que jamais les composants requis pour créer leurs expériences web.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: Composants principaux
topic: Développement
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 8%

---


# Personnalisation des icônes de composant {#developing-component-icons-in-aem-sites}

Les icônes de composant permettent aux auteurs d’identifier rapidement un composant avec des icônes ou des abréviations significatives. Les auteurs peuvent désormais trouver plus rapidement que jamais les composants requis pour créer leurs expériences web.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

L’explorateur de composants s’affiche désormais dans un thème gris cohérent, affichant les éléments suivants :

* **[!UICONTROL Groupe de composants]**
* **[!UICONTROL Libellé du composant]** 
* **[!UICONTROL Description du composant]**
* **[!UICONTROL Icône de composant]**
   * Les deux premières lettres du titre du composant *(par défaut)*
   * Image PNG personnalisée *(configurée par un développeur)*
   * Image SVG personnalisée *(configurée par un développeur)*
   * Icône CoralUI *(configurée par un développeur)*

## Options de configuration des icônes de composant {#component-icon-configuration-options}

### Abréviations {#abbreviations}

Par défaut, les 2 premiers caractères du titre du composant (**[cq:Component]@jcr:title**) sont utilisés comme abréviation. Par exemple, si **[cq:Component]@jcr:title=Article List**, l’abréviation s’afficherait sous la forme &quot;**Ar**&quot;.

L’abréviation peut être personnalisée via la propriété **[cq:Component]@abbreviation** . Bien que cette valeur puisse accepter plus de 2 caractères, il est recommandé de limiter l’abréviation à 2 caractères afin d’éviter toute perturbation visuelle.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Icônes CoralUI {#coralui-icons}

Les icônes CoralUI, fournies par AEM, peuvent être utilisées pour les icônes de composant. Pour configurer une icône CoralUI, définissez une propriété **[cq:Component]@cq:icon** sur la valeur d’attribut de l’icône HTML de l’icône CoralUI souhaitée (énumérée dans la [documentation CoralUI](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Images PNG {#png-images}

Les images PNG peuvent être utilisées pour les icônes de composant. Pour configurer une image PNG en tant qu’icône de composant, ajoutez l’image de votre choix en tant que **nt:file** nommée **cq:icon.png** sous **[cq:Component]**.

Le fichier PNG doit avoir un arrière-plan transparent ou une couleur d’arrière-plan définie sur **#707070**.

Les images PNG seront mises à l’échelle **20 px par 20 px**. Cependant, pour s’adapter aux affichages Retina **40px** par **40px** peut être préférable.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Images SVG {#svg-images}

Les images SVG (vectorielles) peuvent être utilisées pour les icônes de composant. Pour configurer une image SVG en tant qu’icône de composant, ajoutez le SVG souhaité en tant que **nt:file** nommé **cq:icon.svg** sous **[cq:Component]**.

Les images SVG doivent avoir une couleur d’arrière-plan définie sur **#707070** et une taille de **20 px par 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Ressources supplémentaires {#additional-resources}

* [Icônes CoralUI disponibles](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
