---
title: Comprendre les bonnes pratiques relatives au système de style avec AEM Sites
description: Article détaillé décrivant les bonnes pratiques relatives à l’implémentation du système de style avec Adobe Experience Manager Sites.
feature: Style System
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: ht
source-wordcount: '1536'
ht-degree: 100%

---

# Comprendre les bonnes pratiques relatives au système de style{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Consultez le contenu sur [Comprendre comment coder pour le système de style](style-system-technical-video-understand.md), afin de garantir une compréhension des conventions de type BEM utilisées par le système de style AEM.

Deux versions ou styles principaux sont mis en œuvre pour le système de style AEM :

* **Styles de disposition**
* **Styles d’affichage**

Les **styles de disposition** affectent de nombreux éléments d’un composant pour créer un rendu bien défini et identifiable (conception et disposition) du composant, souvent en s’alignant sur un concept de marque réutilisable spécifique. Par exemple, un composant Teaser peut être présenté dans la disposition traditionnelle basée sur des cartes, un style promotionnel horizontal ou sous la forme d’une disposition Héros superposant du texte sur une image.

Les **styles d’affichage** sont utilisés pour affecter des variations mineures des styles de disposition. Toutefois, ils ne modifient pas la nature ou l’intention fondamentales du style de disposition. Par exemple, un style de disposition Héros peut avoir des styles d’affichage qui modifient le modèle de couleurs du modèle de couleurs principal de la marque, le remplaçant par le modèle de couleurs secondaire de la marque.

## Bonnes pratiques relatives à l’organisation des styles {#style-organization-best-practices}

Lors de la définition des noms de style disponibles pour les créateurs et créatrices AEM, il est préférable de suivre les recommandations suivantes :

* Nommer les styles en utilisant un vocabulaire compris par les créateurs et créatrices.
* Minimiser le nombre d’options de style.
* N’exposer que les options et combinaisons de style autorisées par les normes de la marque.
* N’exposer que les combinaisons de style ayant un effet.
   * Si des combinaisons inefficaces sont exposées, assurez-vous qu’elles ne provoquent au moins pas d’effets négatifs.

Plus le nombre de combinaisons de style possibles augmente pour les créateurs et créatrices AEM, plus il existe de permutations qui doivent passer par l’assurance qualité et être validées par rapport aux normes de la marque. Trop d’options peuvent également dérouter les créateurs et les créatrices, car il se peut qu’il soit difficile de déterminer quelle option ou combinaison est requise pour produire l’effet souhaité.

### Noms de style et classes CSS {#style-names-vs-css-classes}

Les noms de style ou les options présentées aux créateurs et créatrices AEM et les noms de classe CSS de mise en œuvre sont découplés dans AEM.

Cela permet aux créateurs et créatrices AEM d’étiqueter les options de style de manière claire et compréhensible, mais laisse la possibilité aux développeurs et développeuses CSS de nommer les classes CSS d’une manière sémantique et à l’épreuve du temps. Par exemple :

Un composant doit avoir les options pour porter les couleurs **principales** et **secondaires** de la marque ; cependant, les créateurs et créatrices AEM reconnaissent les couleurs comme le **vert** et le **jaune**, plutôt que les aspects principaux et secondaires de la langue de conception.

Le système de style AEM peut afficher ces styles d’affichage de couleur à l’aide de libellés conviviaux pour le créateur ou la créatrice, comme **Vert** et **Jaune**, tout en permettant aux développeurs et développeuses CSS d’utiliser le nommage sémantique de `.cmp-component--primary-color` et `.cmp-component--secondary-color` pour définir la mise en œuvre du style réel dans CSS.

Le nom de style **Vert** est mappé sur `.cmp-component--primary-color`, et le **Jaune** sur `.cmp-component--secondary-color`.

Si la couleur de la marque de l’entreprise change ensuite, il suffit de modifier les mises en œuvre uniques de `.cmp-component--primary-color` et `.cmp-component--secondary-color` ainsi que les noms de style.

## Exemple de cas d’utilisation du composant Teaser {#the-teaser-component-as-an-example-use-case}

Vous trouverez ci-dessous un exemple de cas d’utilisation de style d’un composant Teaser pour plusieurs styles de disposition et d’affichage différents.

Cela permettra d’explorer les manières dont les noms de style (exposés aux créateurs et aux créatrices) et dont les classes CSS de sauvegarde sont organisés.

### Configuration des styles du composant Teaser {#component-styles-configuration}

L’image suivante montre la configuration des [!UICONTROL styles] du composant Teaser pour les variations mentionnées dans le cas d’utilisation.

Les noms du [!UICONTROL groupe de styles], disposition et affichage correspondent aux concepts généraux des styles d’affichage et des styles de disposition utilisés pour catégoriser conceptuellement les types de styles dans cet article.

Les noms du [!UICONTROL groupe de styles] et le nombre de [!UICONTROL groupes de styles] doivent être adaptés au cas d’utilisation du composant et aux conventions de style du composant spécifiques au projet.

Par exemple, le nom du groupe de styles **Affichage** aurait pu être **Couleurs**.

![Groupe de styles Affichage.](assets/style-config.png)

### Menu Sélection de style {#style-selection-menu}

L’image ci-dessous affiche le menu [!UICONTROL Style] avec lequel les créateurs et créatrices interagissent pour sélectionner les styles appropriés pour le composant. Veuillez noter que les noms de [!UICONTROL groupe de styles] ainsi que les noms de style sont tous exposés au créateur ou à la créatrice.

![Menu déroulant Style.](assets/style-menu.png)

### Style par défaut {#default-style}

Le style par défaut est souvent le style le plus couramment utilisé du composant, ainsi que la vue par défaut et sans style du teaser lorsqu’il est ajouté à une page.

En fonction de si le style par défaut est commun ou non, le CSS peut être appliqué directement sur le `.cmp-teaser` (sans modificateur) ou sur un `.cmp-teaser--default`.

Si les règles de style par défaut s’appliquent le plus souvent à toutes les variantes, il est préférable d’utiliser `.cmp-teaser` comme classes CSS du style par défaut, puisque toutes les variations doivent implicitement hériter d’elles, en supposant que les conventions de type BEM soient respectées. Si ce n’est pas le cas, elles doivent être appliquées via le modificateur par défaut, tel que `.cmp-teaser--default`, qui à son tour doit être ajouté au champ [Classes CSS par défaut de la configuration du style du composant](#component-styles-configuration), sinon ces règles de style devront être remplacées dans chaque variation.

Il est même possible d’attribuer un style « nommé » comme style par défaut, par exemple, le style Hero `(.cmp-teaser--hero)` (Héros) défini ci-dessous, toutefois, il est plus clair de mettre en œuvre le style par défaut par rapport aux implémentations des classes CSS `.cmp-teaser` ou `.cmp-teaser--default`.

>[!NOTE]
>
>Notez que le style de disposition par défaut n’a PAS de nom de style d’affichage, mais l’auteur ou l’autrice peut sélectionner une option d’affichage dans l’outil de sélection du système de style AEM.
>
>Cela va à l’encontre des bonnes pratiques :
>
>**Nexposez que les combinaisons de styles qui ont un effet**.
>
>Si un auteur ou une autrice sélectionne le style d’affichage **Vert**, rien ne se passera.
>
>Dans ce cas d’utilisation, nous acceptons cette violation, car tous les autres styles de disposition doivent pouvoir être colorés à l’aide des couleurs de la marque.
>
>Dans la section **Promotion (aligné à droite)** nous allons voir ci-dessous comment empêcher les combinaisons de style à éviter.

![Style par défaut.](assets/default.png)

* **Style de disposition**
   * Par défaut
* **Style d’affichage**
   * Aucun
* **Classes CSS effectives** : `.cmp-teaser--promo` ou `.cmp-teaser--default`

### Style promotionnel {#promo-style}

Le **style de disposition promotionnelle** est utilisé pour promouvoir du contenu important sur le site. Il est disposé horizontalement pour occuper une plage d’espace sur la page web et doit pouvoir être stylisé aux couleurs de la marque, avec le style de disposition Promotion par défaut en texte noir.

Pour ce faire, un **style de disposition** de **Promotion** et les **styles d’affichage** **Vert** et **Jaune** sont configurés dans le système de style AEM pour le composant Teaser.

#### Promotion par défaut

![Promotion par défaut.](assets/promo-default.png)

* **Style de disposition**
   * Nom du style : **Promo**
   * Classe CSS : `cmp-teaser--promo`
* **Style d’affichage**
   * Aucun
* **Classes CSS effectives** : `.cmp-teaser--promo`

#### Style Promo principal

![Style Promo principal.](assets/promo-primary.png)

* **Style de disposition**
   * Nom du style : **Promo**
   * Classe CSS : `cmp-teaser--promo`
* **Style d’affichage**
   * Nom du style : **Vert**
   * Classe CSS : `cmp-teaser--primary-color`
* **Classes CSS effectives** : `cmp-teaser--promo.cmp-teaser--primary-color`

#### Style Promo secondaire

![Style Promo secondaire.](assets/promo-secondary.png)

* **Style de disposition**
   * Nom du style : **Promo**
   * Classe CSS : `cmp-teaser--promo`
* **Style d’affichage**
   * Nom du style : **Jaune**
   * Classe CSS : `cmp-teaser--secondary-color`
* **Classes CSS effectives** : `cmp-teaser--promo.cmp-teaser--secondary-color`

### Style Promo aligné à droite {#promo-r-align}

Le style d’affichage **Promo aligné à droite** est une variante du style Promo qui modifie l’emplacement de l’image et du texte (image à droite, texte à gauche).

L’alignement à droite est par essence un style d’affichage. Il peut être introduit dans le système de style AEM en tant que style d’affichage sélectionné en association avec le style de disposition Promo. Ceci est contraire aux bonnes pratiques :

**N’exposez que les combinaisons de styles dont l’effet**

.a déjà été violé dans le [style par défaut](#default-style).

Puisque l’alignement à droite affecte uniquement le style de disposition Promo, et non les deux autres styles de disposition (par défaut et Héros), il est possible de créer un nouveau style de disposition Promo (aligné à droite) incluant la classe CSS qui aligne à droite le contenu des styles de disposition Promotion : `cmp -teaser--alternate`.

Cette combinaison de plusieurs styles en une seule entrée Style peut également contribuer à réduire le nombre de styles disponibles et de permutations de style, ce qu’il est préférable de limiter.

Notez que le nom de la classe CSS, `cmp-teaser--alternate`, ne doit pas nécessairement correspondre à la nomenclature conviviale « aligné à droite ».

#### Style Promo aligné à droite par défaut

![Style Promo aligné à droite.](assets/promo-alternate-default.png)

* **Style de disposition**
   * Nom du style : **Promo (aligné à droite)**
   * Classes CSS : `cmp-teaser--promo cmp-teaser--alternate`
* **Style d’affichage**
   * Aucun
* **Classes CSS effectives** : `.cmp-teaser--promo.cmp-teaser--alternate`

#### Style Promo aligné à droite principal

![Style Promo aligné à droite principal.](assets/promo-alternate-primary.png)

* **Style de disposition**
   * Nom du style : **Promo (aligné à droite)**
   * Classes CSS : `cmp-teaser--promo cmp-teaser--alternate`
* **Style d’affichage**
   * Nom du style : **Vert**
   * Classe CSS : `cmp-teaser--primary-color`
* **Classes CSS effectives** : `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Style Promo aligné à droite secondaire

![Style Promo aligné à droite secondaire.](assets/promo-alternate-secondary.png)

* **Style de disposition**
   * Nom du style : **Promo (aligné à droite)**
   * Classes CSS : `cmp-teaser--promo cmp-teaser--alternate`
* **Style d’affichage**
   * Nom du style : **Jaune**
   * Classe CSS : `cmp-teaser--secondary-color`
* **Classes CSS effectives** : `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Style Héros {#hero-style}

Le style de disposition Héros affiche l’image des composants en arrière-plan avec le titre et le lien superposés. Le style de disposition Héros, comme celui de Promo, doit pouvoir porter les couleurs de la marque.

Pour colorer le style de disposition Héros avec des couleurs de marque, vous pouvez vous servir des mêmes styles d’affichage que ceux utilisés pour le style de disposition Promo.

Par composant, le nom du style est mappé à un seul jeu de classes CSS, ce qui signifie que les noms de classe CSS qui colorent l’arrière-plan du style de disposition Promo doivent colorer le texte et le lien du style de disposition Héros.

Pour ce faire, il est possible d’établir une portée en définissant les règles CSS. Toutefois, cela nécessite que les développeurs et développeuses CSS comprennent comment ces permutations sont appliquées sur AEM.

CSS pour colorer l’arrière-plan du style de disposition **Promo** avec la couleur principale (verte) :

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS pour colorer le texte du style de disposition **Héros** avec la couleur principale (verte) :

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Paramètres par défaut de Héros

![Style Héros.](assets/hero.png)

* **Style de disposition**
   * Nom du style : **Héros**
   * Classe CSS : `cmp-teaser--hero`
* **Style d’affichage**
   * Aucun
* **Classes CSS effectives** : `.cmp-teaser--hero`

#### Style Héros principal

![Style Héros principal.](assets/hero-primary.png)

* **Style de disposition**
   * Nom du style : **Promo**
   * Classe CSS : `cmp-teaser--hero`
* **Style d’affichage**
   * Nom du style : **Vert**
   * Classe CSS : `cmp-teaser--primary-color`
* **Classes CSS effectives** : `cmp-teaser--hero.cmp-teaser--primary-color`

#### Style Héros secondaire

![Style Héros secondaire.](assets/hero-secondary.png)

* **Style de disposition**
   * Nom du style : **Promo**
   * Classe CSS : `cmp-teaser--hero`
* **Style d’affichage**
   * Nom du style : **Jaune**
   * Classe CSS : `cmp-teaser--secondary-color`
* **Classes CSS effectives** : `cmp-teaser--hero.cmp-teaser--secondary-color`

## Ressources supplémentaires {#additional-resources}

* [Documentation du système de style](https://helpx.adobe.com/fr/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Créer des bibliothèques clientes AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site web de documentation de BEM (Block Element Modifier)](https://getbem.com/)
* [Site web de la documentation LESS](https://lesscss.org/)
* [Site web jQuery](https://jquery.com/)
