---
title: Comprendre comment coder pour le système de style AEM
description: Dans cette vidéo, nous allons examiner l’anatomie de la page CSS (ou LESS) et du code JavaScript utilisé pour mettre en forme le composant de titre principal d’Adobe Experience Manager à l’aide du système de style, ainsi que la manière dont ces styles sont appliqués au code HTML et au modèle DOM.
feature: Système de style
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
topic: Développement
role: Développeur
level: Intermédiaire, expérimenté
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 4%

---


# Comprendre comment coder pour le système de style{#understanding-how-to-code-for-the-aem-style-system}

Dans cette vidéo, nous allons examiner l’anatomie de la page CSS (ou [!DNL LESS]) et du code JavaScript utilisé pour mettre en forme le composant de titre principal d’Experience Manager à l’aide du système de style, ainsi que la manière dont ces styles sont appliqués au code HTML et au modèle DOM.

>[!NOTE]
>
>Le système de style AEM a été introduit avec [AEM 6.3 SP1](https://helpx.adobe.com/fr/experience-manager/6-1/release-notes/sp3-release-notes.html) + [Feature Pack 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593).
>
>La vidéo suppose que le composant Titre We.Retail a été mis à jour pour hériter de [Composants principaux v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases).

## Comprendre comment coder pour le système de style {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

Le package d’AEM fourni (**Technical-review.sites.style-system-1.0.0.zip**) installe l’exemple de style de titre, les exemples de stratégies pour les composants de titre et de Conteneur de mise en page We.Retail, ainsi qu’un exemple de page.

[technique-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

Voici la définition de [!DNL LESS] pour l&#39;exemple de style trouvé à :

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Pour ceux qui préfèrent le format CSS, en dessous de ce fragment de code se trouve le fichier CSS dans lequel [!DNL LESS] est compilé.

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

Le fichier [!DNL LESS] ci-dessus est compilé en mode natif par Experience Manager à la page CSS suivante.

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

### JavaScript {#example-javascript}

Le script JavaScript suivant collecte et injecte la dernière date et heure de dernière modification de la page en cours sous le texte du titre lorsque le style Exemple est appliqué au composant Titre.

L’utilisation de jQuery est facultative, ainsi que les conventions d’affectation de nom utilisées.

Voici la définition de [!DNL LESS] pour l&#39;exemple de style trouvé à :

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

## Meilleures pratiques de développement {#development-best-practices}

### Meilleures pratiques HTML {#html-best-practices}

* HTML (généré via HTL) doit être aussi structurellement sémantique que possible ; évite de regrouper/imbriquer des éléments de manière inutile.
* Les éléments HTML doivent pouvoir être adressés via des classes CSS de style BEM.

**Bon**  - Tous les éléments du composant peuvent être adressés via la notation BEM :

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Mauvais**  - Les éléments de liste et de liste ne peuvent être adressés que par nom d’élément :

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Il est préférable d&#39;exposer plus de données et de les masquer plutôt que d&#39;exposer trop peu de données nécessitant un développement ultérieur en arrière-plan pour les exposer.

   * L’implémentation de bascules de contenu pouvant être créés peut aider à maintenir ce code HTML, dans lequel les auteurs peuvent sélectionner les éléments de contenu à écrire dans le code HTML. Il peut s’avérer particulièrement important lors de l’écriture d’images au format HTML qui ne peuvent pas être utilisées pour tous les styles.
   * L’exception à cette règle est lorsque des ressources coûteuses, par exemple les images, sont exposées par défaut, car les images de événement masquées par CSS sont, dans ce cas, extraites inutilement.

      * Les composants d’image modernes utilisent souvent JavaScript pour sélectionner et charger l’image la plus appropriée pour le cas d’utilisation (fenêtre d’affichage).

### Meilleures pratiques CSS {#css-best-practices}

>[!NOTE]
>
>Le système de style fait une légère différence technique par rapport à [BEM](https://en.bem.info/), en ce que les éléments `BLOCK` et `BLOCK--MODIFIER` ne sont pas appliqués au même élément, comme spécifié par [BEM](https://en.bem.info/).
>
>En raison de contraintes de produit, le paramètre `BLOCK--MODIFIER` est appliqué au parent de l’élément `BLOCK`.
>
>Tous les autres locataires de [BEM](https://en.bem.info/) doivent être alignés sur.

* Utilisez des préprocesseurs tels que [LESS](https://lesscss.org/) (pris en charge par AEM nativement) ou [SCSS](https://sass-lang.com/) (nécessite un système de génération personnalisé) pour permettre une définition CSS claire et une réutilisation.

* Maintenir l&#39;uniforme du poids/spécificité du sélecteur ; Cela permet d’éviter et de résoudre les conflits en cascade CSS difficiles à identifier.
* Organisez chaque style dans un fichier distinct.
   * Ces fichiers peuvent être combinés à l’aide de LESS/SCSS `@imports` ou si une page CSS brute est requise, par le biais de l’inclusion de fichiers de bibliothèque client HTML ou de systèmes personnalisés de création de fichiers frontaux.
* Evitez de mélanger de nombreux styles complexes.
   * Plus il y a de styles qui peuvent être appliqués en même temps à un composant, plus la variété de permutations est grande. Il peut s’avérer difficile de maintenir/d’assurer l’alignement de la marque.
* Utilisez toujours les classes CSS (suivant la notation BEM) pour définir les règles CSS.
   * Si la sélection d’éléments sans classes CSS (c’est-à-dire d’éléments nus) est absolument nécessaire, placez-les plus haut dans la définition CSS pour indiquer clairement qu’ils ont une spécificité plus faible que les collisions avec des éléments de ce type qui ont des classes CSS sélectionnables.
* Evitez de mettre en forme `BLOCK--MODIFIER` directement, car il est rattaché à la grille réactive. La modification de l’affichage de cet élément peut avoir une incidence sur le rendu et les fonctionnalités de la grille réactive. Par conséquent, seul le style à ce niveau est possible lorsque l’intention est de modifier le comportement de la grille réactive.
* Appliquez une étendue de style à l’aide de `BLOCK--MODIFIER`. `BLOCK__ELEMENT--MODIFIERS` peut être utilisé dans le composant, mais comme `BLOCK` représente le composant et que le composant est ce qui est stylisé, le style est &quot;défini&quot; et l&#39;étendue est définie par `BLOCK--MODIFIER`.

Exemple La structure du sélecteur CSS doit être la suivante :

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Sélecteur de 1er niveau</p> <p>BLOC : MODIFICATEUR</p> </td> 
   <td valign="bottom"><p>Sélecteur de 2e niveau</p> <p>BLOC</p> </td> 
   <td valign="bottom"><p>Sélecteur de troisième niveau</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Sélecteur CSS effectif</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-liste—dark</span></td> 
   <td valign="middle"><span class="code">.cmp-liste</span></td> 
   <td valign="middle"><span class="code">.cmp-liste_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-liste—dark</span></p> <p><span class="code"> .cmp-liste</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-liste_item {  </span></strong></p> <p><strong> color: bleu ;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> color: rouge ;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

Dans le cas des composants imbriqués, la profondeur du sélecteur CSS pour ces éléments de composant imbriqués dépassera le sélecteur de troisième niveau. Répétez le même modèle pour le composant imbriqué, mais dont l’étendue est définie par le `BLOCK` du composant parent. En d’autres termes, début le `BLOCK` du composant imbriqué au troisième niveau et le `ELEMENT` du composant imbriqué au quatrième niveau du sélecteur.

### Meilleures pratiques JavaScript {#javascript-best-practices}

Les meilleures pratiques définies dans cette section concernent &quot;style-JavaScript&quot;, ou JavaScript destiné spécifiquement à manipuler le composant à des fins stylistiques plutôt qu’fonctionnelles.

* Style-JavaScript doit être utilisé judicieusement et est un cas d&#39;utilisation minoritaire.
* Style-JavaScript doit être principalement utilisé pour manipuler le DOM du composant afin de prendre en charge le style par CSS.
* Réévaluez l’utilisation de Javascript si les composants s’affichent plusieurs fois sur une page et comprenez le coût de calcul/et de retirage.
* Réévaluez l’utilisation de JavaScript si elle extrait les nouvelles données/le nouveau contenu de manière asynchrone (via AJAX) lorsque le composant peut apparaître plusieurs fois sur une page.
* Gérez les expériences de publication et de création.
* Réutilisez style-Javascript lorsque cela est possible.
   * Par exemple, si plusieurs styles d&#39;un composant exigent que son image soit déplacée vers une image d&#39;arrière-plan, le script style-JavaScript peut être implémenté une seule fois et attaché à plusieurs `BLOCK--MODIFIERs`.
* Séparez style-JavaScript du code JavaScript fonctionnel lorsque cela est possible.
* Evaluez le coût de JavaScript par rapport à celui de la manifestation de ces modifications DOM dans le code HTML directement via HTL.
   * Lorsqu’un composant qui utilise style-JavaScript nécessite une modification côté serveur, déterminez si la manipulation JavaScript peut être apportée pour le moment et quels sont les effets/ramifications sur les performances et la prise en charge du composant.

#### Considérations sur les performances {#performance-considerations}

* Style-JavaScript doit rester léger et maigre.
* Pour éviter le scintillement et les retraits inutiles, masquez initialement le composant par `BLOCK--MODIFIER BLOCK` et affichez-le lorsque toutes les manipulations DOM dans le code JavaScript sont terminées.
* Les performances des manipulations style-JavaScript s’apparentent aux plug-ins jQuery de base qui se connectent aux éléments et les modifient sur DOMReady.
* Assurez-vous que les requêtes sont compressées et que CSS et JavaScript sont minifiés.

## Ressources supplémentaires {#additional-resources}

* [Documentation du système de style](https://helpx.adobe.com/fr/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Création de bibliothèques clientes AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site Web de documentation de BEM (Block Element Modificateur)](https://getbem.com/)
* [Site Web de documentation LESS](https://lesscss.org/)
* [site Web jQuery](https://jquery.com/)
