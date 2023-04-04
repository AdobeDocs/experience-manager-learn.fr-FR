---
title: Personnalisation des icônes de composant dans Adobe Experience Manager Sites
description: Les icônes de composant permettent aux auteurs d’identifier rapidement un composant avec des icônes ou des abréviations significatives. Les auteurs peuvent désormais trouver plus rapidement que jamais les composants requis pour créer leurs expériences web.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 7%

---

# Personnalisation des icônes de composant {#developing-component-icons-in-aem-sites}

Les icônes de composant permettent aux auteurs d’identifier rapidement un composant avec des icônes ou des abréviations significatives. Les auteurs peuvent désormais trouver plus rapidement que jamais les composants requis pour créer leurs expériences web.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

L’explorateur de composants s’affiche désormais dans un thème gris cohérent, affichant les éléments suivants :

* **[!UICONTROL Groupe de composants]**
* **[!UICONTROL Titre du composant]**
* **[!UICONTROL Description du composant]**
* **[!UICONTROL Icône de composant]**
   * Les deux premières lettres du titre du composant *(par défaut)*
   * Image PNG personnalisée *(configuré par un développeur)*
   * Image de SVG personnalisée *(configuré par un développeur)*
   * Icône CoralUI *(configuré par un développeur)*

## Options de configuration des icônes de composant {#component-icon-configuration-options}

### Abréviations {#abbreviations}

Par défaut, les 2 premiers caractères du titre du composant (**[cq:Component]@jcr:title**) sont utilisés comme abréviation. Par exemple, si **[cq:Component]@jcr:title=Liste des articles** L’abréviation s’afficherait sous la forme &quot;**Ar**&quot;.

L’abréviation peut être personnalisée à partir du **[cq:Component]@abbreviation** . Bien que cette valeur puisse accepter plus de 2 caractères, il est recommandé de limiter l’abréviation à 2 caractères afin d’éviter toute perturbation visuelle.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Icônes CoralUI {#coralui-icons}

Les icônes CoralUI, fournies par AEM, peuvent être utilisées pour les icônes de composant. Pour configurer une icône CoralUI, définissez une **[cq:Component]@cq:icon** à la valeur d’attribut de l’icône de HTML de l’icône CoralUI souhaitée (énumérée dans la variable [Documentation CoralUI](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Images PNG {#png-images}

Les images PNG peuvent être utilisées pour les icônes de composant. Pour configurer une image PNG en tant qu’icône de composant, ajoutez l’image souhaitée en tant que **nt:file** named **cq:icon.png** sous le **[cq:Component]**.

Le fichier PNG doit avoir un arrière-plan transparent ou une couleur d’arrière-plan définie sur **#707070**.

Les images PNG sont mises à l’échelle en **20 px par 20 px**. Toutefois, pour les écrans Retina **40 px** par **40 px** peut être préférable.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Images du SVG {#svg-images}

Les images de SVG (vectorielles) peuvent être utilisées pour les icônes de composant. Pour configurer une image de SVG en tant qu’icône de composant, ajoutez le SVG de votre choix en tant que **nt:file** named **cq:icon.svg** sous le **[cq:Component]**.

Les images SVG doivent avoir une couleur d’arrière-plan définie sur **#707070** et d’une taille de **20 px par 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Ressources supplémentaires {#additional-resources}

* [Icônes CoralUI disponibles](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
