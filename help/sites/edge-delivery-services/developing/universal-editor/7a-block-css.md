---
title: Développer un bloc avec CSS
description: Développez un bloc avec CSS pour Edge Delivery Services, modifiable à l’aide de l’éditeur universel.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 14cda9d4-752b-4425-a469-8b6f283ce1db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '437'
ht-degree: 100%

---

# Développer un bloc avec CSS

Les blocs dans Edge Delivery Services sont stylisés à l’aide de CSS. Le fichier CSS d’un bloc est stocké dans le répertoire du bloc et porte le même nom que le bloc. Par exemple, le fichier CSS d’un bloc nommé `teaser` se trouve à l’emplacement `blocks/teaser/teaser.css`.

Idéalement, un bloc ne doit avoir besoin que de CSS pour le style, sans compter sur JavaScript pour modifier le DOM ou ajouter des classes CSS. Le besoin de JavaScript dépend de la [modélisation de contenu](./5-new-block.md#block-model) du bloc et de sa complexité. Si nécessaire, vous pouvez ajouter du [JavaScript de bloc](./7b-block-js-css.md).

En utilisant une approche uniquement CSS, les éléments de code HTML sémantique (principalement) nus du bloc sont ciblés et stylisés.

## HTML de bloc

Pour comprendre comment mettre en forme un bloc, passez d’abord en revue le DOM exposé par Edge Delivery Services, car c’est ce qui est disponible pour la mise en forme. Vous pouvez trouver le DOM en examinant le bloc desservi par l’environnement de développement local de l’interface de ligne de commande d’AEM. Évitez d’utiliser le DOM de l’éditeur universel, car il diffère légèrement.

>[!BEGINTABS]

>[!TAB DOM à mettre en forme]

Voici le DOM du bloc de teaser qui est la cible de la mise en forme.

Notez l’élément `<p class="button-container">...` qui est [augmenté automatiquement](./4-website-branding.md#inferred-elements) comme un élément déduit par le JavaScript Edge Delivery Services.

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

>[!TAB Comment trouver le DOM]

Pour trouver le DOM à mettre en forme, ouvrez la page avec le bloc sans style dans votre environnement de développement local, sélectionnez le bloc et inspectez le DOM.

![Inspecter le DOM du bloc](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]

## CSS de bloc

Créez un fichier CSS dans le dossier du bloc, en utilisant le nom du bloc comme nom de fichier. Par exemple, pour le bloc **teaser**, le fichier se trouve à l’emplacement `/blocks/teaser/teaser.css`.

Ce fichier CSS est automatiquement chargé lorsque le JavaScript Edge Delivery Services détecte un élément DOM sur la page représentant un bloc de teaser.

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="Nom de fichier de l’exemple de code ci-dessous."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` using CSS nesting (https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting) to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in .6s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
    overflow: hidden; 

    /* The image is rendered to the first div in the block */
    picture {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;

        img {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
        }
    }

    /** 
    The teaser's text is rendered to the second (also the last) div in the block.

    These styles are scoped to the second (also the last) div in the block (.block.teaser > div:last-child).

    This div order can be used to target different styles to the same semantic elements in the block. 
    For example, if the block has two images, we could target the first image with `.block.teaser > div:first-child img`, 
    and the second image with `.block.teaser > div:nth-child(2) img`.
    **/
    & > div:last-child {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;

        /** 
        The following elements reside within `.block.teaser > div:last-child` and could be scoped as such, for example:

        .block.teaser > div:last-child p { .. }

        However since these element can only appear in the second/last div per our block's model, it's unnecessary to add this additional scope.
        **/

        /* Regardless of the authored heading level, we only want one style the heading */
        h1,
        h2,
        h3,
        h4,
        h5,
        h6 {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        h1::after,
        h2::after,
        h3::after,
        h4::after,
        h5::after,
        h6::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
        }

        /* Add underlines to links in the text */
        a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        .button-container {
            margin: 0;
            padding: 0;

            .button {
                background-color: var(--primary-color);
                border-radius: 0;
                color: var(--dark-color);
                font-size: var(--body-font-size-xs);
                font-weight: bold;
                padding: 1em 2.5em;
                margin: 0;
                text-transform: uppercase;
            }
        }
    }
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

![Prévisualisation CSS uniquement](./assets/7a-block-css/local-development-preview.png)

## Appliquer lint à votre code

Veillez à [appliquer lint fréquemment](./3-local-development-environment.md#linting) à votre code pour vous assurer qu’il est clair et cohérent. Le linting permet de détecter les problèmes tôt et de réduire le temps de développement global. N’oubliez pas que vous ne pouvez pas fusionner votre travail de développement avec `main` tant que tous vos problèmes de linting ne sont pas résolus.

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

![Teaser dans l’éditeur universel](./assets/7a-block-css/universal-editor-preview.png)
