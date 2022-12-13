---
title: Fenêtre modale de l’extension de la console de fragments de contenu AEM
description: Découvrez comment créer un modal d’extension de console de fragments de contenu AEM.
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
source-wordcount: '344'
ht-degree: 0%

---


# Fenêtre d’extension

![Fenêtre modale de l’extension de fragment de contenu AEM](./assets/modal/modal.png){align="center"}

AEM modal d’extension Fragment de contenu permet d’associer une interface utilisateur personnalisée aux extensions AEM Fragment de contenu, qu’il s’agisse de l’option [Barre d’actions](./action-bar.md) ou [Menu d’en-tête](./header-menu.md) des boutons.

Les modèles sont des applications React, basées sur [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/), et peuvent créer toute interface utilisateur personnalisée requise par l’extension, y compris, mais sans s’y limiter :

+ Boîtes de dialogue de confirmation
+ [Formulaires de saisie](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Indicateurs de progression](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Résumé des résultats](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Messages d’erreur
+ ... ou même une application React à vues multiples et complète !

## Itinéraires modaux

L’expérience modale est définie par l’extension Application React du générateur d’applications définie sous la variable `web-src` dossier. Comme pour toute application React, l’expérience complète est orchestrée à l’aide de [Itinéraires React](https://reactrouter.com/en/main/components/routes) rendu [Composants React](https://reactjs.org/docs/components-and-props.html).

Au moins un itinéraire est nécessaire pour générer la vue modale initiale. Cet itinéraire initial est appelé dans [enregistrement d’extension](#extension-registration)&#39;s `onClick(..)` , comme illustré ci-dessous.


+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions, typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="content-fragment/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="content-fragment/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Enregistrement d’une extension

Pour ouvrir un modal, un appel à `guestConnection.host.modal.showUrl(..)` est créé à partir du `onClick(..)` fonction . `showUrl(..)` transmet un objet JavaScript avec la clé/les valeurs :

+ `title` fournit le nom du titre du modal affiché pour l’utilisateur.
+ `url` est l’URL qui appelle la variable [Route React](#modal-routes) responsable de la vue initiale du modal.

Il est impératif que la `url` transmis à `guestConnection.host.modal.showUrl(..)` résout l’itinéraire dans l’extension, sinon rien ne s’affiche dans le modal.

Consultez la section [menu d’en-tête](./header-menu.md#modal) et [barre d’actions](./action-bar.md#modal) documentation sur la création d’URL modales.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## Composant modal

Chaque route de l&#39;extension, [ce n&#39;est pas le `index` route](./extension-registration.md#app-routes), correspond à un composant React qui peut effectuer le rendu dans le modal de l’extension.

Un modal peut être constitué de n’importe quel nombre d’itinéraires React, d’un modal simple à un modal complexe à plusieurs itinéraires.

Le scénario suivant illustre un modal à un seul itinéraire, mais cette vue modale peut contenir des liens React qui invoquent d’autres itinéraires ou comportements.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## Fermer le modal

![Bouton de fermeture modale de l’extension de fragment de contenu AEM](./assets/modal/close.png){align="center"}

Les modèles doivent fournir leur propre contrôle serré. Pour ce faire, appelez `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
