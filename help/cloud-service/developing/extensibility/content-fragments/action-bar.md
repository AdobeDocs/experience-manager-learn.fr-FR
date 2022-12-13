---
title: Extensions de la barre d’actions de la console Fragment de contenu AEM
description: Découvrez comment créer des extensions de barre d’actions de la console Fragment de contenu AEM.
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
source-wordcount: '333'
ht-degree: 1%

---


# Extension de la barre d’actions

![Extension de la barre d’actions](./assets/action-bar/action-bar.png){align="center"}

Extensions qui incluent une barre d’actions et qui ajoutent un bouton à l’action de la console AEM de fragments de contenu qui s’affiche lorsque __1 ou plus__ Les fragments de contenu sont sélectionnés. Comme les boutons d’extension de la barre d’actions s’affichent uniquement lorsqu’au moins un fragment de contenu est sélectionné, ils agissent généralement sur les fragments de contenu sélectionnés. Voici quelques exemples :

+ Appel d’un processus ou d’un workflow sur les fragments de contenu sélectionnés.
+ Mise à jour ou modification des données de la sélection de fragments de contenu.

## Enregistrement d’une extension

`ExtensionRegistration.js` est le point d’entrée de l’extension AEM et définit :

1. le type d’extension ; dans le cas présent, un bouton de la barre d’actions.
1. La définition du bouton d’extension, dans `getButton()` fonction .
1. Le gestionnaire de clics du bouton, dans la variable `onClick()` fonction .

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## Modal

![Modal](./assets/modal/modal.png)

AEM les extensions de barre d’actions de la console de fragments de contenu peuvent nécessiter :

+ Entrée supplémentaire de l’utilisateur pour effectuer l’action souhaitée.
+ La possibilité de fournir à l’utilisateur des informations détaillées sur le résultat de l’action.

Pour prendre en charge ces exigences, l’extension AEM Content Fragment Console permet un modal personnalisé qui s’affiche sous la forme d’une application React.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Passer à la création d’un modal</p>
      <p class="has-text-blackest">Découvrez comment créer un modal qui s’affiche lorsque vous cliquez sur le bouton d’extension de la barre d’actions.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Découvrez comment créer un modal">Découvrez comment créer un modal</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Aucun modal

Il arrive que les extensions de la barre d’actions de la console Fragment de contenu AEM ne nécessitent aucune interaction supplémentaire avec l’utilisateur, par exemple :

+ Appel d’un processus principal qui ne nécessite pas l’entrée de l’utilisateur, tel que l’importation ou l’exportation.

Dans ces instances, l’extension AEM de la console Fragment de contenu ne nécessite pas de [modal](#modal)et effectuez le travail directement dans le bouton de la barre d’actions `onClick` gestionnaire.

L’extension de la console AEM de fragments de contenu permet à un indicateur de progression de recouvrir la console AEM fragment de contenu pendant l’exécution du travail, empêchant l’utilisateur d’effectuer d’autres actions. L’utilisation de l’indicateur de progression est facultative, mais utile pour communiquer la progression du travail synchrone à l’utilisateur.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
