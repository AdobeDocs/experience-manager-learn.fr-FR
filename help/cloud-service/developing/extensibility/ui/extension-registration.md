---
title: Enregistrement de l’extension d’interface utilisateur AEM
description: Découvrez comment enregistrer une extension d’interface utilisateur AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 1%

---

# Enregistrement d’une extension

Les extensions de l’interface utilisateur d’AEM sont des applications spécialisées du générateur d’applications, reposant sur React et utilisant la variable [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) Structure de l’interface utilisateur.

Pour définir où et comment l’extension d’interface utilisateur d’AEM apparaît, deux configurations sont requises dans l’application App Builder de l’extension : routage de l’application et enregistrement de l’extension.

## Itinéraires des applications{#app-routes}

L’extension `App.js` déclare la variable [Routeur React](https://reactrouter.com/en/main) qui inclut un itinéraire d’index qui enregistre l’extension dans l’interface utilisateur d’AEM.

L’itinéraire de l’index est appelé lors du chargement initial de l’interface utilisateur d’AEM, et la cible de cet itinéraire définit la manière dont l’extension est exposée dans la console.

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

Basé sur le modèle d’extension de l’interface utilisateur AEM sélectionné lors de la [initialisation de l’extension de l’application App Builder](./app-initialization.md), différents points d’extension sont pris en charge.

+ [Points d’extension de l’interface utilisateur des fragments de contenu](./content-fragments/overview.md#extension-points)


## Inclusion conditionnelle d’extensions

Les extensions de l’interface utilisateur d’AEM peuvent exécuter une logique personnalisée pour limiter les environnements AEM dans lesquels l’extension apparaît. Cette vérification est effectuée avant la `register` dans le `ExtensionRegistration` et renvoie immédiatement si l’extension ne doit pas être affichée.

Le contexte disponible pour ce contrôle est limité :

+ L’hôte AEM sur lequel l’extension est en cours de chargement.
+ Jeton d’accès AEM de l’utilisateur actuel.

Les contrôles les plus courants pour le chargement d’une extension sont les suivants :

+ Utilisation de l’hôte AEM (`new URLSearchParams(window.location.search).get('repo')`) pour déterminer si l’extension doit se charger.
   + Affichez uniquement l’extension sur les environnements AEM qui font partie d’un programme spécifique (comme illustré dans l’exemple ci-dessous).
   + Affiche uniquement l’extension sur un environnement AEM spécifique (hôte AEM).
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
