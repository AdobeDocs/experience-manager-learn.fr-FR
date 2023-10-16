---
title: Ajout de widgets à l’éditeur de texte enrichi (RTE)
description: Découvrez comment ajouter des widgets à l’éditeur de texte enrichi (RTE) dans l’éditeur de fragment de contenu AEM
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# Ajout de widgets à l’éditeur de texte enrichi (RTE)

Découvrez comment ajouter des widgets à l’éditeur de texte enrichi (RTE) dans l’éditeur de fragment de contenu AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

Pour ajouter du contenu dynamique dans l’éditeur de texte enrichi (RTE), la variable **widgets** peut être utilisée. Les widgets permettent d’intégrer l’interface utilisateur simple ou complexe dans l’éditeur de texte enrichi et l’interface utilisateur peut être créée à l’aide du framework JS de votre choix. Ils peuvent être considérés comme des boîtes de dialogue ouvertes en appuyant sur `{` clé spéciale dans l’éditeur de texte enrichi.

En règle générale, les widgets sont utilisés pour insérer le contenu dynamique avec une dépendance système externe ou qui peut changer en fonction du contexte actuel.

La variable **widgets** sont ajoutés au **RTE** dans l’éditeur de fragment de contenu à l’aide de la méthode `rte` point d’extension. Utilisation `rte` point d’extension `getWidgets()` Un ou plusieurs widgets sont ajoutés. Elles sont déclenchées en appuyant sur la fonction `{` clé spéciale pour ouvrir l’option de menu contextuel, puis sélectionnez le widget souhaité pour charger l’interface utilisateur de la boîte de dialogue personnalisée.

Cet exemple montre comment ajouter un widget appelé _Liste des codes de réduction_ pour rechercher, sélectionner et ajouter le code de remise spécifique à l’aventure WKND dans un contenu d’éditeur de texte enrichi. Ces codes de remise peuvent être gérés dans un système externe tel que le système de gestion des commandes (OMS), la gestion des informations sur les produits (PIM), une application locale ou une action Adobe AppBuilder.

Pour simplifier les choses, cet exemple utilise la méthode [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) framework permettant de développer l’interface utilisateur du widget ou de la boîte de dialogue et le nom d’aventure WKND codé en dur, ainsi que les données de code de remise.

## Point d’extension

Cet exemple étend au point d’extension `rte` pour ajouter un widget à l’éditeur de texte enrichi dans l’éditeur de fragment de contenu.

| Interface utilisateur AEM étendue | Point d’extension |
| ------------------------ | --------------------- | 
| [Éditeur de fragment de contenu](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Widgets de l’éditeur de texte enrichi](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Exemple d’extension

L’exemple suivant crée une _Liste des codes de réduction_ widget. En appuyant sur la touche `{` clé spéciale dans l’éditeur de texte enrichi, le menu contextuel s’ouvre, puis en sélectionnant _Liste des codes de réduction_ dans le menu contextuel, l’interface utilisateur de la boîte de dialogue s’ouvre.

Les auteurs de contenu WKND peuvent rechercher, sélectionner et ajouter le code de remise spécifique à l’aventure actuel, le cas échéant.

### Enregistrement d’une extension

`ExtensionRegistration.js`, mappé à l’itinéraire index.html, est le point d’entrée de l’extension AEM et définit :

+ Définition du widget dans `getWidgets()` fonction avec `id, label and url` attributs.
+ La variable `url` valeur d’attribut, un chemin d’URL relatif (`/index.html#/discountCodes`) pour charger l’interface utilisateur de la boîte de dialogue.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {

  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {

          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget",       // Provide a unique ID for the widget
              label: "Discount Code List",          // Provide a label for the widget
              url: "/index.html#/discountCodes",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Ajouter `discountCodes` route `App.js`{#add-widgets-route}

Dans le composant React principal `App.js`, ajoutez le `discountCodes` itinéraire pour effectuer le rendu de l’interface utilisateur pour le chemin d’URL relatif ci-dessus.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### Créer `DiscountCodes` Composant React{#create-widget-react-component}

L’interface utilisateur du widget ou de la boîte de dialogue est créée à l’aide de la fonction [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) framework. La variable `DiscountCodes` le code de composant est le suivant : voici les principales caractéristiques :

+ L’interface utilisateur est rendue à l’aide des composants React Spectrum, tels que [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Bouton](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ La variable `adventureDiscountCodes` a un mappage codé en dur du nom de l’aventure et du code de réduction. Dans un scénario réel, ces données peuvent être récupérées à partir de l’action AppBuilder Adobe ou de systèmes externes tels que PIM, OMS ou une passerelle API locale ou basée sur un fournisseur de cloud.
+ La variable `guestConnection` est initialisé à l’aide de la fonction `useEffect` [React hook](https://react.dev/reference/react/useEffect) et géré en tant qu’état du composant. Il est utilisé pour communiquer avec l’hôte AEM.
+ La variable `handleDiscountCodeChange` récupère le code de réduction du nom de l’aventure sélectionnée et met à jour la variable d’état.
+ La variable `addDiscountCode` fonction utilisant `guestConnection` fournit des instructions d’exécution de l’éditeur de texte enrichi. Dans ce cas `insertContent` code d’instruction et de HTML du code de remise réel à insérer dans l’éditeur de texte enrichi.

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
