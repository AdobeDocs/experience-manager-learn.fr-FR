---
title: Extensions du menu d’en-tête de la console Fragments de contenu AEM
description: Découvrez comment créer des extensions de menu d’en-tête de la console Fragment de contenu AEM.
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
source-wordcount: '366'
ht-degree: 0%

---


# Extension du menu d’en-tête

![Extension du menu d’en-tête](./assets/header-menu/header-menu.png){align="center"}

Extensions qui incluent un menu d’en-tête, qui permet d’insérer un bouton dans l’en-tête de la console de fragments de contenu AEM qui s’affiche lors de la __non__ Les fragments de contenu sont sélectionnés. Comme les boutons d’extension de menu d’en-tête ne s’affichent que lorsqu’aucun fragment de contenu n’est sélectionné, ils n’ont généralement aucune incidence sur les fragments de contenu existants. En revanche, les extensions des menus d’en-tête sont généralement les suivantes :

+ Créez de nouveaux fragments de contenu à l’aide d’une logique personnalisée, telle que la création d’un ensemble de fragments de contenu, liés via des références de contenu.
+ Utilisation d’un ensemble de fragments de contenu sélectionné par programmation, comme l’exportation de tous les fragments de contenu créés la semaine dernière.

## Enregistrement d’une extension

`ExtensionRegistration.js` est le point d’entrée de l’extension AEM et définit :

1. le type d’extension ; dans le cas d’un bouton de menu d’en-tête.
1. La définition du bouton d’extension, dans `getButton()` fonction .
1. Le gestionnaire de clics du bouton, dans la variable `onClick()` fonction .

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## Modal

![Modal](./assets/modal/modal.png)

AEM les extensions de menu d’en-tête de la console de fragments de contenu peuvent nécessiter :

+ Entrée supplémentaire de l’utilisateur pour effectuer l’action souhaitée.
+ La possibilité de fournir à l’utilisateur des informations détaillées sur le résultat de l’action.

Pour prendre en charge ces exigences, l’extension AEM Content Fragment Console permet un modal personnalisé qui s’affiche sous la forme d’une application React.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Passer à la création d’un modal</p>
      <p class="has-text-blackest">Découvrez comment créer un modal qui s’affiche lorsque vous cliquez sur le bouton d’extension du menu d’en-tête.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Découvrez comment créer un modal">Découvrez comment créer un modal</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Aucun modal

Il arrive que les extensions AEM menu d’en-tête de la console Fragment de contenu ne nécessitent aucune interaction supplémentaire avec l’utilisateur, par exemple :

+ Appel d’un processus principal qui ne nécessite pas l’entrée de l’utilisateur, tel que l’importation ou l’exportation.
+ Ouverture d’une nouvelle page web, par exemple vers la documentation interne sur les directives de contenu.

Dans ces instances, l’extension AEM Content Fragment Console ne nécessite pas de [modal](#modal)et peut effectuer le travail directement dans le bouton du menu d’en-tête `onClick` gestionnaire.

L’extension AEM Content Fragment Console permet à un indicateur de progression de recouvrir AEM Content Fragment Console pendant le travail, empêchant l’utilisateur d’effectuer d’autres actions. L’utilisation de l’indicateur de progression est facultative, mais utile pour communiquer la progression du travail synchrone à l’utilisateur.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```