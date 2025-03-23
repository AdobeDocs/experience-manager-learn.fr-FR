---
title: Ajouter une image de marque à un site web
description: Définissez des variables CSS et CSS globales et des polices web pour un site Edge Delivery Services.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# Ajouter une image de marque à un site web

Commencez par configurer l’image de marque globale en mettant à jour les styles globaux, en définissant des variables CSS et en ajoutant des polices web. Ces éléments fondamentaux garantissent la cohérence et la facilité de maintenance du site web. Ils doivent en outre être appliqués de manière cohérente sur l’ensemble du site.

## Créer un problème GitHub

Pour que tout reste organisé, utilisez GitHub pour effectuer le suivi du travail. Tout d’abord, créez un problème GitHub pour ce travail :

1. Accédez au référentiel GitHub (voir le chapitre [Créer un projet de code](./1-new-code-project.md) pour plus de détails).
2. Cliquez sur l’onglet **Problèmes** puis sur **Nouveau problème**.
3. Rédigez un **titre** et une **description** pour le travail à effectuer.
4. Cliquez sur **Soumettre le nouveau problème**.

Le problème GitHub est utilisé ultérieurement lors de la [création d’une requête d’extraction](#merge-code-changes).

![GitHub crée un nouveau problème](./assets/4-website-branding/github-issues.png)

## Créer une branche de travail

Pour maintenir l’organisation et assurer la qualité du code, créez une branche pour chaque travail. Cette pratique empêche le nouveau code d’affecter négativement les performances et garantit que les modifications ne sont pas activées avant leur achèvement.

Pour ce chapitre, qui se concentre sur les styles de base du site web, créez une branche nommée `wknd-styles`.

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## CSS global

Edge Delivery Services utilise un fichier CSS global, situé à l’adresse `styles/styles.css`, pour configurer les styles communs à l’ensemble du site web. Le `styles.css` contrôle des aspects tels que les couleurs, les polices et l’espacement, en veillant à ce que tout semble cohérent sur l’ensemble du site.

Le CSS global doit être indépendant des constructions de niveau inférieur telles que les blocs, en se concentrant sur l’aspect général du site et sur les traitements visuels partagés.

Gardez à l’esprit que les styles CSS globaux peuvent être remplacés si nécessaire.

### Variables CSS

Les [variables CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) sont un excellent moyen de stocker les paramètres de conception tels que les couleurs, les polices et les tailles. En utilisant des variables, vous pouvez modifier ces éléments en un seul endroit et les mettre à jour sur l’ensemble du site.

Pour commencer à personnaliser les variables CSS, procédez comme suit :

1. Ouvrez le fichier `styles/styles.css` dans l’éditeur de code.
2. Recherchez la déclaration `:root`, où sont stockées les variables CSS globales.
3. Modifiez les variables de couleur et de police pour qu’elles correspondent à la marque WKND.

Voici un exemple :


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

Explorez les autres variables de la section `:root` et passez en revue les paramètres par défaut.

Lorsque vous développez un site web et que vous vous trouvez à répéter les mêmes valeurs CSS, pensez à créer de nouvelles variables pour rendre les styles plus faciles à gérer. Voici quelques exemples d’autres propriétés CSS qui peuvent bénéficier des variables CSS : `border-radius`, `padding`, `margin` et `box-shadow`.

### Éléments nus

Les éléments nus sont stylisés directement par le biais de leur nom d’élément au lieu d’utiliser une classe CSS. Par exemple, plutôt que de mettre en forme une classe CSS `.page-heading`, les styles sont appliqués à l’élément `h1` à l’aide de `h1 { ... }`.

Dans le fichier `styles/styles.css`, un ensemble de styles de base est appliqué aux éléments HTML nus. Les sites web Edge Delivery Services donnent la priorité à l’utilisation d’éléments nus, car ils s’alignent sur l’HTML sémantique native d’Edge Delivery Service.

Pour nous aligner sur le branding WKND, nous allons mettre en forme certains éléments nus en `styles.css` :

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

Ces styles garantissent que les éléments `h2`, sauf s’ils sont remplacés, sont mis en forme de manière cohérente avec la marque WKND, ce qui contribue à créer une hiérarchie visuelle claire. Le soulignement jaune partiel sous chaque `h2` ajoute une touche distinctive aux en-têtes.

### Eléments déduits

Dans Edge Delivery Services, le code `scripts.js` et `aem.js` du projet améliore automatiquement des éléments HTML nus spécifiques en fonction de leur contexte dans HTML.

Par exemple, les éléments d’ancrage (`<a>`) créés sur leur propre ligne (plutôt que sur le texte qui les entoure) sont considérés comme des boutons en fonction de ce contexte. Ces ancres sont automatiquement enveloppées avec un conteneur `div` avec des `button-container` de classe CSS et l’élément d’ancrage dispose d’une classe CSS `button` ajoutée.

Par exemple, lorsqu’un lien est créé sur sa propre ligne, Edge Delivery Services JavaScript met à jour son DOM de la manière suivante :

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

Ces boutons peuvent être personnalisés pour correspondre à la marque WKND : les boutons s’affichent sous la forme de rectangles jaunes avec du texte noir.

Voici un exemple de mise en forme des « boutons déduits » dans `styles.css` :

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

Ce CSS définit les styles de bouton de base et inclut des traitements spécifiques à WKND, tels que du texte en majuscules, un arrière-plan jaune et du texte noir. Les propriétés `background-color` et `color` utilisent des variables CSS qui permettent au style de bouton de rester aligné sur les couleurs de la marque. Cette approche garantit que les boutons sont stylisés de manière cohérente sur l’ensemble du site tout en restant flexibles.

## Polices web

Les projets Edge Delivery Services optimisent l’utilisation des polices web pour maintenir des performances élevées et minimiser l’impact sur les scores Lighthouse. Cette méthode assure un rendu rapide sans compromettre l’identité visuelle du site. Voici comment implémenter efficacement des polices web pour des performances optimales.

### Faces de police

Ajoutez des polices web personnalisées à l’aide des déclarations de `@font-face` CSS dans le fichier `styles/fonts.css`. L’ajout du `@font-faces` à `fonts.css` garantit que les polices web sont chargées au moment optimal, ce qui contribue à la conservation des scores Lighthouse.

1. Ouvrez `styles/fonts.css`.
2. Ajoutez les déclarations de `@font-face` suivantes pour inclure les polices de marque WKND : `Asar` et `Source Sans Pro`.

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

Les polices utilisées dans ce tutoriel proviennent de Google Fonts, mais vous pouvez les obtenir auprès de n’importe quel fournisseur de polices, y compris [Adobe Fonts](https://fonts.adobe.com/).

+++Utilisation de fichiers de polices web locaux

Vous pouvez également copier les fichiers de polices web dans un projet, dans le dossier `/fonts`, et les référencer dans les déclarations `@font-face`.

Ce tutoriel utilise les polices web hébergées à distance afin qu’il soit plus facile de suivre le processus.

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

Enfin, mettez à jour les variables CSS `styles/styles.css` pour utiliser les nouvelles polices :

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

Les `roboto-fallback` et `roboto-condensed-fallback` sont des polices de secours qui sont mises à jour dans la section [Polices de secours](#fallback-fonts) pour s’aligner afin de prendre en charge les polices `Asar` et web `Source Sans Pro` personnalisées.

### Polices de remplacement

Les polices web ont souvent un impact sur les performances en raison de leur taille, augmentant potentiellement les scores de décalage de mise en page cumulé (CLS) et réduisant les scores globaux de Lighthouse. Pour garantir un affichage instantané du texte pendant le chargement des polices web, les projets Edge Delivery Services utilisent des polices de remplacement natives dans le navigateur. Cette approche permet de maintenir une expérience utilisateur fluide pendant que la police souhaitée s’applique.

Pour sélectionner la meilleure police de secours, utilisez l’extension Adobe [Helix Font Fallback Chrome](https://www.aem.live/developer/font-fallback), qui détermine une police étroitement correspondante que les navigateurs peuvent utiliser avant le chargement de la police personnalisée. Les déclarations de police de secours qui en résultent doivent être ajoutées au fichier `styles/styles.css` pour améliorer les performances et garantir une expérience fluide pour les utilisateurs.

![Extension Helix Font Fallback Chrome](./assets/4-website-branding/font-fallback-chrome-plugin.png){align=center}

Pour utiliser l’extension [Chrome de secours de polices Helix](https://www.aem.live/developer/font-fallback), assurez-vous que les polices web sont appliquées à la page web dans les mêmes variations que celles utilisées sur le site web Edge Delivery Services. Ce tutoriel présente l’extension sur [wknd.site](http://wknd.site/fr.html). Lors du développement d’un site web, appliquez l’extension au site sur lequel vous travaillez plutôt qu’à [wknd.site](http://wknd.site/fr.html).

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

Ajoutez les noms de famille de polices de remplacement aux variables CSS des polices dans `styles/styles.css` après les « vrais » noms de famille de polices.

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## Aperçu du développement

À mesure que vous ajoutez CSS, l’environnement de développement local de l’interface de ligne de commande d’AEM recharge automatiquement les modifications, ce qui permet de voir rapidement et facilement comment le CSS affecte le bloc.

![Aperçu du développement du CSS de la marque WKND](./assets/4-website-branding/preview.png)


## Télécharger les fichiers CSS finaux

Vous pouvez télécharger les fichiers CSS mis à jour à partir des liens ci-dessous :

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## Afficher les fichiers CSS

Veillez à [fréquemment pelucher](./3-local-development-environment.md#linting) vos modifications de code pour vous assurer qu’elles sont propres et cohérentes. La liaison régulière permet de détecter les problèmes tôt et de réduire le temps de développement global. N’oubliez pas que vous ne pouvez pas fusionner votre travail dans la branche principale tant que tous les problèmes de liaison ne sont pas résolus.

```bash
$ npm run lint:css
```

## Fusionner les modifications de code

Fusionnez les modifications dans la branche `main` sur GitHub pour créer le travail futur sur ces mises à jour.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

Une fois les modifications transmises à la branche `wknd-styles`, créez une requête d’extraction sur GitHub pour les fusionner dans la branche `main`.

1. Accédez au référentiel GitHub à partir du chapitre [Créer un projet](./1-new-code-project.md).
2. Cliquez sur l’onglet **Demandes d’extraction** et sélectionnez **Nouvelle demande d’extraction**.
3. Définissez `wknd-styles` comme branche source et `main` comme branche cible.
4. Vérifiez les modifications et cliquez sur **Créer une demande d’extraction**.
5. Dans les détails de la demande d’extraction, **ajoutez ce qui suit** :

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * Le `Fix #1` fait référence au problème GitHub créé précédemment.
   * Les URL de test indiquent à AEM Code Sync les branches à utiliser pour la validation et la comparaison. L’URL « Après » utilise le `wknd-styles` de branche de travail pour vérifier comment les modifications de code affectent les performances du site web.

6. Cliquez sur **Créer une demande d’extraction**.
7. Attendez que l’application GitHub de synchronisation de code AEM [](./1-new-code-project.md) **termine les vérifications de qualité**. En cas d’échec, résolvez les erreurs et réexécutez les vérifications.
8. Une fois les vérifications effectuées, **fusionnez la requête de tirage** dans `main`.

Une fois les modifications fusionnées dans `main`, elles sont considérées comme déployées en production et le nouveau développement peut se poursuivre en fonction de ces mises à jour.
