---
title: Enregistrement de l’extension de l’interface utilisateur AEM
description: Découvrez comment enregistrer une extension de l’interface utilisateur AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 100%

---

# Enregistrement d’une extension

Les extensions de l’interface utilisateur d’AEM sont des applications spécialisées du Créateur d’applications, basées sur React et qui utilisent le framework de l’interface utilisateur [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/).

Pour définir où et comment l’extension de l’interface utilisateur AEM apparaît, deux configurations sont requises dans l’application du Créateur d’applications de l’extension : le routage de l’application et l’enregistrement de l’extension.

## Itinéraires de l’application{#app-routes}

L’`App.js` de l’extension déclare le [routeur React](https://reactrouter.com/en/main) qui inclut un itinéraire d’index qui enregistre l’extension dans l’interface utilisateur AEM.

L’itinéraire de l’index est appelé lors du chargement initial de l’interface utilisateur AEM, et la cible de cet itinéraire définit la manière dont l’extension est exposée dans la console.

+ `./src/aem-ui-extension/web-src/src/components/App.js`

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

`ExtensionRegistration.js` doit être immédiatement chargé via l’itinéraire d’index de l’extension et agit sur le point d’enregistrement de l’extension.

Selon le modèle d’extension de l’interface utilisateur AEM sélectionné lors de l’[initialisation de l’extension de l’application du Créateur d’applications](./app-initialization.md), différents points d’extension sont pris en charge.

+ [Points d’extension de l’interface utilisateur des fragments de contenu](./content-fragments/overview.md#extension-points)


## Inclusion conditionnelle d’extensions

Les extensions de l’interface utilisateur AEM peuvent exécuter une logique personnalisée pour limiter les environnements AEM dans lesquels l’extension apparaît. Cette vérification est effectuée avant l’appel `register` dans le composant `ExtensionRegistration` et renvoie immédiatement si l’extension ne doit pas être affichée.

Le contexte disponible pour cette vérification est limité :

+ l’hôte AEM sur lequel l’extension est en cours de chargement ;
+ le jeton d’accès AEM de la personne utilisatrice actuelle.

Les vérifications les plus courantes pour charger une extension sont les suivantes :

+ Utiliser l’hôte AEM (`new URLSearchParams(window.location.search).get('repo')`) pour déterminer si l’extension doit charger.
   + Afficher uniquement l’extension sur les environnements AEM qui font partie d’un programme spécifique (comme illustré dans l’exemple ci-dessous).
   + Afficher uniquement l’extension sur un environnement AEM spécifique (hôte AEM).
+ Utiliser une [Action Adobe I/O Runtime](./runtime-action.md) pour effectuer un appel HTTP vers AEM afin de déterminer si la personne utilisatrice actuelle doit voir l’extension.

L’exemple ci-dessous illustre la limitation de l’extension à tous les environnements du programme `p12345`.

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
