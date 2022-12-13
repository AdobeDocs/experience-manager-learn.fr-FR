---
title: Enregistrement de l’extension de la console Fragment de contenu AEM
description: Découvrez comment enregistrer des extensions de console de fragments de contenu.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---


# Enregistrement d’une extension

AEM les extensions de la console de fragments de contenu sont des applications spécialisées du créateur d’applications, reposant sur React et utilisant [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) Structure de l’interface utilisateur.

Pour définir où et comment l’extension apparaît dans la console de fragments de contenu d’AEM, deux configurations spécifiques sont requises dans l’application App Builder de l’extension : routage de l’application et enregistrement de l’extension.

## Itinéraires des applications{#app-routes}

L’extension `App.js` déclare la variable [Routeur React](https://reactrouter.com/en/main) qui inclut un itinéraire d’index qui enregistre l’extension dans la console de fragments de contenu AEM.

L’itinéraire de l’index est appelé lors du chargement initial de AEM la console de fragments de contenu, et la cible de cet itinéraire définit la manière dont l’extension est exposée dans la console.

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Enregistrement d’une extension

`ExtensionRegistration.js` doit être immédiatement chargé via l’itinéraire d’index de l’extension et agit sur le point d’enregistrement de l’extension, en définissant :

1. le type d’extension ; a [menu d’en-tête](./header-menu.md) ou [barre d’actions](./action-bar.md) bouton .
   + [Menu En-tête](./header-menu.md) les extensions sont signalées par la variable `headerMenu` propriété sous `methods`.
   + [Barre d’actions](./action-bar.md) les extensions sont signalées par la variable `actionBar` propriété sous `methods`.
1. La définition du bouton d’extension, dans `getButton()` fonction . Cette fonction renvoie un objet avec des champs :
   + `id` est un identifiant unique du bouton.
   + `label` est le libellé du bouton d’extension dans la console Fragment de contenu AEM
   + `icon` est l’icône du bouton d’extension dans la console Fragment de contenu AEM. L’icône est une [React Spectrum](https://spectrum.adobe.com/page/icons/) nom de l’icône, sans espaces.
1. Le gestionnaire de clics du bouton, dans défini dans une `onClick()` fonction .
   + [Menu d’en-tête](./header-menu.md) les extensions ne transmettent pas de paramètres au gestionnaire de clics.
   + [Barre d’actions](./action-bar.md) Les extensions fournissent une liste des chemins d’accès aux fragments de contenu sélectionnés dans le `selections` .

### Extension du menu d’en-tête

![Extension du menu d’en-tête](./assets/extension-registration/header-menu.png)

Les boutons d’extension du menu d’en-tête s’affichent lorsqu’aucun fragment de contenu n’est sélectionné. Étant donné que les extensions du menu d’en-tête n’agissent pas sur une sélection de fragment de contenu, aucun fragment de contenu n’est fourni à `onClick()` gestionnaire.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Passer à la création d’une extension de menu d’en-tête</p>
      <p class="has-text-blackest">Découvrez comment enregistrer et définir une extension de menu d’en-tête dans la console Fragments de contenu AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Découvrez comment créer une extension de menu d’en-tête">Découvrez comment créer une extension de menu d’en-tête</span>
        </a>
      </div>
    </div>
  </div>
</div>

### Extension de la barre d’actions

![Extension de la barre d’actions](./assets/extension-registration/action-bar.png)

Les boutons d’extension de la barre d’actions s’affichent lorsqu’un ou plusieurs fragments de contenu sont sélectionnés. Les chemins d’accès au fragment de contenu sélectionné sont mis à la disposition de l’extension via le `selections` , dans le `onClick(..)` gestionnaire.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Passer à la création d’une extension de barre d’actions</p>
      <p class="has-text-blackest">Découvrez comment enregistrer et définir une extension de barre d’actions dans la console Fragments de contenu AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Découvrez comment créer une extension de barre d’actions">Découvrez comment créer une extension de barre d’actions</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Inclusion conditionnelle d’extensions

AEM les extensions de la console de fragments de contenu peuvent exécuter une logique personnalisée afin de limiter le moment où l’extension apparaît dans AEM console de fragments de contenu. Cette vérification est effectuée avant la `register` dans le `ExtensionRegistration` et renvoie immédiatement si l’extension ne doit pas être affichée.

Le contexte disponible pour ce contrôle est limité :

+ L’hôte AEM sur lequel l’extension est en cours de chargement.
+ Jeton d’accès AEM de l’utilisateur actuel.

Les contrôles les plus courants pour le chargement d’une extension sont les suivants :

+ Utilisation de l’hôte AEM (`new URLSearchParams(window.location.search).get('repo')`) pour déterminer si l’extension doit se charger.
   + Affichez uniquement l’extension sur les environnements AEM qui font partie d’un programme spécifique (comme illustré dans l’exemple ci-dessous).
   + Affiche uniquement l’extension sur un environnement AEM spécifique (c’est-à-dire l’hôte AEM).
+ Utilisation d’une [Action Adobe I/O Runtime](./runtime-action.md) pour effectuer un appel HTTP vers AEM afin de déterminer si l’utilisateur actuel doit voir l’extension.

L’exemple ci-dessous illustre la limitation de l’extension à tous les environnements du programme. `p12345`.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
