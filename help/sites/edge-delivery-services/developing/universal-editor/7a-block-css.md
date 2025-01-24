---
title: Développement d’un bloc avec CSS
description: Développez un bloc avec CSS pour les Edge Delivery Services, modifiable à l’aide de l’éditeur universel.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Développement d’un bloc avec CSS

Les blocs en Edge Delivery Services sont stylisés à l’aide de CSS. Le fichier CSS d’un bloc est stocké dans le répertoire du bloc et porte le même nom que le bloc. Par exemple, le fichier CSS d’un bloc nommé `teaser` se trouve à l’emplacement `blocks/teaser/teaser.css`.

Idéalement, un bloc ne doit avoir besoin que de CSS pour le style, sans compter sur JavaScript pour modifier le DOM ou ajouter des classes CSS. Le besoin de JavaScript dépend de la [modélisation de contenu](./5-new-block.md#block-model) du bloc et de sa complexité. Si nécessaire, vous pouvez ajouter [block JavaScript](./7b-block-js-css.md).

En utilisant une approche uniquement CSS, les éléments d’HTML sémantique nu (principalement) du bloc sont ciblés et stylisés.

## HTML en bloc

Pour comprendre comment mettre en forme un bloc, passez d’abord en revue le DOM exposé par les Edge Delivery Services, car c’est ce qui est disponible pour la mise en forme. Vous pouvez trouver le DOM en examinant le bloc desservi par l’environnement de développement local de l’interface de ligne de commande AEM. Évitez d’utiliser le DOM de l’éditeur universel, car il diffère légèrement.

>[!BEGINTABS]

>[!TAB DOM pour le style]

Voici le DOM du bloc de teaser qui est la cible du style.

Notez la `<p class="button-container">...` qui est [augmentée automatiquement](./4-website-branding.md#inferred-elements) comme un élément déduit par JavaScript Edge Delivery Services.

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                        <picture>
                            <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                            <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                            <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                            <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                        </picture>
                    </div>
                </div>
                <div>
                    <div>
                        <h2 id="wknd-adventures">WKND Adventures</h2>
                        <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                        <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB Comment trouver le DOM ]

Pour trouver le DOM à mettre en forme, ouvrez la page avec le bloc sans style dans votre environnement de développement local, sélectionnez le bloc et inspectez le DOM.

![DOM du bloc Inspect](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]

## Bloquer CSS

Créez un fichier CSS dans le dossier du bloc, en utilisant le nom du bloc comme nom de fichier. Par exemple, pour le bloc **teaser**, le fichier se trouve à l’emplacement `/blocks/teaser/teaser.css`.

Ce fichier CSS est automatiquement chargé lorsque Edge Delivery Services JavaScript détecte un élément DOM sur la page représentant un teaser.

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="Nom de fichier de l’exemple de code ci-dessous."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in .6s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
}

/* The image is rendered to the first div in the block */
.block.teaser picture {
    position: absolute;
    z-index: -1;
    inset: 0;
    box-sizing: border-box;
}

.block.teaser picture img {
    object-fit: cover;
    object-position: center;
    width: 100%;
    height: 100%;
}

/** 
The teaser's text is rendered to the second (also the last) div in the block.

These styles are scoped to the second (also the last) div in the block (.block.teaser > div:last-child).

This div order can be used to target different styles to the same semantic elements in the block. 
For example, if the block has two images, we could target the first image with `.block.teaser > div:first-child img`, 
and the second image with `.block.teaser > div:nth-child(2) img`.
**/
.block.teaser > div:last-child {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    background: var(--background-color);
    padding: 1.5rem 1.5rem 1rem;
    width: 80vw;
    max-width: 1200px;
}

/** 
The following elements reside within `.block.teaser > div:last-child` and could be scoped as such, for example:

 .block.teaser > div:last-child p { .. }

However since these element can only appear in the second/last div per our block's model, it's unnecessary to add this additional scope.
**/

/* Regardless of the authored heading level, we only want one style the heading */
.block.teaser h1,
.block.teaser h2,
.block.teaser h3,
.block.teaser h4,
.block.teaser h5,
.block.teaser h6 {
    font-size: var(--heading-font-size-xl);
    margin: 0;
}

.block.teaser h1::after,
.block.teaser h2::after,
.block.teaser h3::after,
.block.teaser h4::after,
.block.teaser h5::after,
.block.teaser h6::after {
    border-bottom: 0;
}

.block.teaser p {
    font-size: var(--body-font-size-s);
    margin-bottom: 1rem;
}

/* Add underlines to links in the text */
.block.teaser a:hover {
    text-decoration: underline;
}

/* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
.block.teaser .button-container {
    margin: 0;
    padding: 0;
}

.block.teaser .button {
    background-color: var(--primary-color);
    border-radius: 0;
    color: var(--dark-color);
    font-size: var(--body-font-size-xs);
    font-weight: bold;
    padding: 1em 2.5em;
    margin: 0;
    text-transform: uppercase;
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/

@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## Aperçu du développement

Comme le CSS est écrit dans le projet de code, les modifications sont rechargées à chaud par l’interface de ligne de commande d’AEM, ce qui permet de comprendre rapidement et facilement comment le CSS affecte le bloc.

![Aperçu CSS uniquement](./assets/7a-block-css/local-development-preview.png)

## Étiqueter votre code

Assurez-vous que vous [modifiez fréquemment](./3-local-development-environment.md#linting) votre code pour vous assurer qu’il est propre et cohérent. La liaison permet de détecter les problèmes tôt et de réduire le temps de développement global. N’oubliez pas que vous ne pouvez pas fusionner votre travail de développement pour `main` tant que tous vos problèmes de lint ne sont pas résolus.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:css
```

## Aperçu dans l’éditeur universel

Pour afficher les modifications dans l’éditeur universel d’AEM, ajoutez-les, validez-les et envoyez-les à la branche de référentiel Git utilisée par l’éditeur universel. Cette étape permet de s’assurer que l’implémentation du bloc n’a pas perturbé l’expérience de création.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add CSS-only implementation for teaser block"
$ git push origin teaser
```

Vous pouvez désormais prévisualiser les modifications dans l’éditeur universel lorsque vous ajoutez le paramètre de requête `?ref=teaser`.

![ Teaser dans l’éditeur universel ](./assets/7a-block-css/universal-editor-preview.png)

