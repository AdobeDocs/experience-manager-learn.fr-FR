---
title: Comprendre comment coder pour le système de style AEM
description: Dans cette vidéo, nous examinerons l’anatomie du code CSS (ou LESS) et du code JavaScript utilisés pour appliquer un style au composant du titre principal d’Adobe Experience Manager à l’aide du système de style, ainsi que la manière dont ces styles sont appliqués au HTML et au DOM.
feature: Style System
version: 6.4, 6.5, Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1061
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 100%

---

# Comprendre comment coder pour le système de style{#understanding-how-to-code-for-the-aem-style-system}

Dans cette vidéo, nous allons examiner l’anatomie du code CSS (ou [!DNL LESS]) et du code JavaScript utilisés pour appliquer un style au composant de titre principal d’Experience Manager à l’aide du système de style, ainsi que la manière dont ces styles sont appliqués au HTML et au DOM.


## Comprendre comment coder pour le système de style {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

Le package AEM fourni (**technical-review.sites.style-system-1.0.0.zip**) installe l’exemple de style du titre, les exemples de stratégies pour les composants de conteneur de disposition We.Retail et de titre, ainsi qu’un exemple de page.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### Code CSS {#the-css}

Voici la définition de [!DNL LESS] pour l’exemple de style qui se trouve à :

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Pour les personnes qui préfèrent le CSS, vous trouverez sous cet extrait de code le CSS dans lequel ce [!DNL LESS] est compilé.

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

Le [!DNL LESS] ci-dessus est compilé en mode natif par Experience Manager au code CSS suivant.

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### Code JavaScript {#example-javascript}

Le code JavaScript suivant collecte et injecte la date et l’heure de dernière modification de la page en cours sous le texte du titre lorsque le style Exemple est appliqué au composant Titre.

L’utilisation de jQuery est facultative, ainsi que les conventions de nommage utilisées.

Voici la définition de [!DNL LESS] pour l’exemple de style qui se trouve à :

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## Bonnes pratiques de développement {#development-best-practices}

### Bonnes pratiques relatives au HTML {#html-best-practices}

* Le HTML (généré via HTL) doit être aussi structurellement sémantique que possible, en évitant les regroupements/imbrications inutiles d’éléments.
* Les éléments HTML doivent être adressables via des classes CSS de style BEM.

**Bon** : tous les éléments du composant sont adressables via la notation BEM :

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Mauvais** : la liste et les éléments de liste ne sont adressables que par nom d’élément :

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Il est préférable d’exposer plus de données et de les masquer que d’exposer trop peu de données nécessitant un développement back-end ultérieur pour les exposer.

   * L’implémentation de boutons (bascule) de contenu pouvant être créés peut contribuer à alléger ce code HTML, dans lequel les auteurs et autrices peuvent sélectionner les éléments de contenu qui sont écrits dans le code HTML. Cela peut s’avérer particulièrement important lors de l’écriture d’images dans le code HTML qui peuvent ne pas être utilisées pour tous les styles.
   * L’exception à cette règle est lorsque des ressources coûteuses, par exemple les images, sont exposées par défaut, car les images d’événement masquées par CSS sont, dans ce cas, récupérées inutilement.

      * Les composants d’image modernes utilisent souvent JavaScript pour sélectionner et charger l’image la plus appropriée au cas d’utilisation (viewport).

### Bonnes pratiques CSS {#css-best-practices}

>[!NOTE]
>
>Le système de style crée une légère différence technique par rapport à [BEM](https://en.bem.info/), dans le sens où `BLOCK` et `BLOCK--MODIFIER` ne sont pas appliqués au même élément, comme spécifié par [BEM](https://en.bem.info/).
>
>En raison des contraintes de produit, le `BLOCK--MODIFIER` s’applique au parent de l’élément `BLOCK`.
>
>Les autres clientes et clients de [BEM](https://en.bem.info/) doivent s’aligner sur ce qui suit :

* Utiliser des préprocesseurs tels que [LESS](https://lesscss.org/) (pris en charge par AEM en mode natif) ou [SCSS](https://sass-lang.com/) (nécessite un système de génération personnalisé) pour permettre une définition CSS claire et une réutilisation.

* Uniformiser le poids/la spécificité du sélecteur. Cela permet d’éviter et de résoudre les conflits de cascade CSS difficiles à identifier.
* Organiser chaque style dans un fichier distinct.
   * Ces fichiers peuvent être combinés à l’aide de `@imports` LESS/SCSS ou, si une page CSS brute est requise, par le biais de l’inclusion de fichiers de bibliothèque cliente HTML ou de systèmes de création de ressources front-end personnalisés.
* Éviter de mélanger de nombreux styles complexes.
   * Plus il y a de styles pouvant être appliqués simultanément à un composant, plus la variété des permutations est grande. Cela peut devenir difficile de maintenir, vérifier et assurer la qualité de l’alignement de la marque.
* Toujours utiliser les classes CSS (à la suite de la notation BEM) pour définir des règles CSS.
   * Si la sélection d’éléments sans classes CSS (c’est-à-dire des éléments nus) est absolument nécessaire, déplacez-les plus haut dans la définition CSS pour indiquer clairement qu’ils ont une spécificité inférieure à celle des collisions avec des éléments de ce type qui possèdent des classes CSS sélectionnables.
* Éviter de mettre en forme le `BLOCK--MODIFIER` directement, car il est lié à la grille réactive. La modification de l’affichage de cet élément peut avoir une incidence sur le rendu et la fonctionnalité de la grille réactive. Par conséquent, le style à ce niveau est à définir seulement lorsque l’intention est de modifier le comportement de la grille réactive.
* Appliquer une portée de style à l’aide de `BLOCK--MODIFIER`. Les `BLOCK__ELEMENT--MODIFIERS` peuvent être utilisés dans le composant, mais puisque le `BLOCK` représente le composant, et le composant est ce qui est mis en forme, le style est « défini » et délimité via `BLOCK--MODIFIER`.

L’exemple de structure du sélecteur CSS doit être le suivant :

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Sélecteur de premier niveau</p> <p>BLOCK--MODIFIER</p> </td> 
   <td valign="bottom"><p>Sélecteur de deuxième niveau</p> <p>BLOCK</p> </td> 
   <td valign="bottom"><p>Sélecteur de troisième niveau</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Sélecteur CSS effectif</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list--dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list--dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"></span><strong><span class="code">.cmp-list__item {</span></strong></p> <p><strong> color: blue;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image--hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image--hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> color: red;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

Dans le cas des composants imbriqués, la profondeur du sélecteur CSS pour ces éléments de composant imbriqués dépassera le sélecteur de troisième niveau. Répétez le même modèle pour le composant imbriqué, mais défini par le `BLOCK` de composant parent. En d’autres termes, démarrez le `BLOCK` de composant imbriqué au troisième niveau et le `ELEMENT` de composant imbriqué se trouvera au quatrième niveau du sélecteur.

### Bonnes pratiques relatives à JavaScript {#javascript-best-practices}

Les bonnes pratiques définies dans cette section concernent « style-JavaScript », ou JavaScript spécialement conçu pour manipuler le composant à des fins de style plutôt que fonctionnelles.

* Style-JavaScript doit être utilisé de manière judicieuse et constitue un cas d’utilisation minoritaire.
* Style-JavaScript doit être principalement utilisé pour manipuler le DOM du composant pour une prise en charge du style par CSS.
* Réévaluez l’utilisation de Javascript si les composants apparaissent de nombreuses fois sur une page et comprenez le coût de calcul/tracé.
* Réévaluez l’utilisation de Javascript s’il extrait de nouvelles données/du nouveau contenu de manière asynchrone (via AJAX) lorsque le composant peut apparaître plusieurs fois sur une page.
* Gérez à la fois les expériences de Publication et de Création.
* Réutilisez style-Javascript lorsque cela est possible.
   * Par exemple, si plusieurs styles d’un composant nécessitent que son image soit déplacée vers une image d’arrière-plan, le script style-JavaScript peut être implémenté une seule fois et joint à plusieurs `BLOCK--MODIFIERs`.
* Séparez style-JavaScript du JavaScript fonctionnel lorsque cela est possible.
* Évaluez le coût du JavaScript par rapport au fait de manifester ces modifications DOM dans le HTML directement via HTL.
   * Lorsqu’un composant qui utilise style-JavaScript nécessite une modification côté serveur, évaluez si la manipulation JavaScript peut être faite à ce moment-là et quels en sont les effets et ramifications sur les performances et la capacité de prise en charge du composant.

#### Considérations sur les performances {#performance-considerations}

* Style-JavaScript doit rester léger.
* Pour éviter le scintillement et les tracés inutiles, masquez dès le début le composant via `BLOCK--MODIFIER BLOCK`, puis affichez-le lorsque toutes les manipulations DOM dans JavaScript sont terminées.
* La performance des manipulations style-JavaScript est semblable à celle des plug-ins jQuery de base qui se joignent aux éléments et les modifient sur DOMReady.
* Assurez-vous que les requêtes sont compressées et que CSS et JavaScript sont minimisés.

## Ressources supplémentaires {#additional-resources}

* [Documentation du système de style](https://helpx.adobe.com/fr/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Créer des bibliothèques clientes AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site web de documentation de BEM (Block Element Modifier)](https://getbem.com/)
* [Site web de la documentation LESS](https://lesscss.org/)
* [Site web jQuery](https://jquery.com/)
