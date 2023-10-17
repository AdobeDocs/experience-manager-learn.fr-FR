---
title: Personnaliser les icônes de composant dans Adobe Experience Manager Sites
description: Les icônes de composant permettent aux auteurs et autrices d’identifier rapidement un composant grâce aux icônes ou abréviations explicites. Les auteurs et autrices peuvent désormais trouver plus rapidement que jamais les composants requis pour créer leurs expériences web.
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
workflow-type: ht
source-wordcount: '374'
ht-degree: 100%

---

# Personnaliser les icônes de composant {#developing-component-icons-in-aem-sites}

Les icônes de composant permettent aux auteurs et autrices d’identifier rapidement un composant grâce aux icônes ou abréviations explicites. Les auteurs et autrices peuvent désormais trouver plus rapidement que jamais les composants requis pour créer leurs expériences web.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

L’explorateur de composants arbore un thème gris cohérent et affiche les éléments suivants :

* **[!UICONTROL Groupe de composants]**
* **[!UICONTROL Titre du composant]**.
* **[!UICONTROL Description du composant]**
* **[!UICONTROL Icône de composant]**
   * Les deux premières lettres du titre du composant *(par défaut)*.
   * Image PNG personnalisée *(configurée par l’équipe de développement)*.
   * Image de SVG personnalisée *(configurée par l’équipe de développement)*.
   * Icône CoralUI *(configurée par l’équipe de développement)*.

## Options de configuration des icônes de composant {#component-icon-configuration-options}

### Abréviations {#abbreviations}

Par défaut, l’abréviation est constituée des deux premiers caractères du titre du composant (**[cq:Component]@jcr:title**). Par exemple, pour **[cq:Component]@jcr:title=Article List**, l’abréviation s’affiche comme suit : « **Ar** ».

L’abréviation peut être personnalisée grâce à la propriété **[cq:Component]@abbreviation**. Bien que cette valeur puisse accepter plus de 2 caractères, il est recommandé de limiter l’abréviation à 2 caractères afin qu’elle conserve toute sa signification.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Icônes CoralUI {#coralui-icons}

Les icônes de composant peuvent utiliser les icônes CoralUI, fournies par AEM. Pour configurer une icône CoralUI, définissez la propriété **[cq:Component]@cq:icon** sur la valeur d’attribut HTML d’icône de l’icône CoralUI souhaitée (indiquée dans la [Documentation de CoralUI](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Images PNG {#png-images}

Les images PNG peuvent être utilisées pour les icônes de composant. Pour configurer une image PNG en tant qu’icône de composant, ajoutez l’image souhaitée en tant que **nt:file** et nommez-le **cq:icon.png** sous **[cq:Component]**.

Le fichier PNG doit avoir un arrière-plan transparent ou une couleur d’arrière-plan définie sur **#707070**.

Les images PNG sont réduites à **20 px par 20 px**. Toutefois, pour les écrans Retina, une définition de **40 px** par **40 px** est préférable.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Images SVG {#svg-images}

Les images SVG (vectorielles) peuvent également être utilisées pour les icônes de composant. Pour configurer une image SVG en tant qu’icône de composant, ajoutez le fichier SVG de votre choix en tant que **nt:file** et nommez-le **cq:icon.svg** sous **[cq:Component]**.

Les images SVG doivent avoir une couleur d’arrière-plan définie sur **#707070** et une taille de **20 px par 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Ressources supplémentaires {#additional-resources}

* [Icônes CoralUI disponibles](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
