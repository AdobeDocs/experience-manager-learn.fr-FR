---
title: Ajouter des badges à l’éditeur de texte enrichi (RTE)
description: Découvrez comment ajouter des badges à l’éditeur de texte enrichi (RTE) dans l’éditeur de fragment de contenu AEM.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: ht
source-wordcount: '781'
ht-degree: 100%

---

# Ajouter des badges à l’éditeur de texte enrichi (RTE)

Découvrez comment ajouter des badges à l’éditeur de texte enrichi (RTE) dans l’éditeur de fragment de contenu AEM..

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

Les [badges de l’éditeur de texte enrichi](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) sont des extensions qui rendent le texte de l’éditeur de texte enrichi (RTE) non modifiable. Cela signifie qu’un badge déclaré comme tel ne peut être que complètement supprimé et ne peut pas être partiellement modifié. Ces badges prennent également en charge les couleurs spéciales dans le RTE, indiquant clairement aux personnes qui créent le contenu que le texte est un badge et qu’il n’est donc pas modifiable. De plus, ils fournissent des indices visuels sur la signification du texte du badge.

Le cas d’utilisation le plus courant pour les badges RTE consiste à les utiliser conjointement avec les [widgets RTE](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). Cela permet au contenu inséré dans l’éditeur de texte enrichi par le widget RTE d’être non modifiable.

En règle générale, les badges associés aux widgets sont utilisés pour ajouter le contenu dynamique avec une dépendance système externe, mais _les personnes qui créent le contenu ne peuvent pas modifier_ le contenu dynamique inséré pour préserver l’intégrité. Ils ne peuvent être supprimés que dans leur ensemble.

Les **badges** sont ajoutés au **RTE** dans l’éditeur de fragment de contenu à l’aide du point d’extension `rte`. La méthode `getBadges()` du point d’extension `rte` permet d’ajouter un ou plusieurs badges.

Cet exemple montre comment ajouter un widget appelé _Service clientèle des réservations pour grands groupes_ pour rechercher, sélectionner et ajouter les informations du service clientèle spécifiques à l’aventure WKND, telles que le **nom du représentant ou de la représentante** et le **numéro de téléphone** dans un contenu d’éditeur de texte enrichi. L’utilisation de la fonctionnalité des badges permet de rendre le **numéro de téléphone** **non modifiable**, mais les personnes qui créent le contenu WKND peuvent modifier le nom du représentant ou de la représentante.

En outre, le **numéro de téléphone** est mis en forme différemment (en bleu), ce qui est un cas d’utilisation supplémentaire de la fonctionnalité des badges.

Pour simplifier les choses, cet exemple utilise le framework [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) pour développer l’interface utilisateur du widget ou de la boîte de dialogue et les numéros de téléphone du service clientèle WKND codés en dur. Pour contrôler l’aspect non modifiable et de style du contenu, le caractère `#` est utilisé dans les attributs `prefix` et `suffix` de la définition des badges.

## Points d’extension

Cet exemple s’étend jusqu’au point d’extension `rte` pour ajouter un badge au RTE dans l’éditeur de fragment de contenu.

| Interface utilisateur AEM étendue | Points d’extension |
| ------------------------ | --------------------- | 
| [Éditeur de fragments de contenu](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Badges de l’éditeur de texte enrichi](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) et [Widgets de l’éditeur de texte enrichi](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Exemple d’extension

L’exemple suivant permet de créer un widget _Service clientèle des réservations pour grands groupes_. En appuyant sur la touche `{` dans l’éditeur de texte enrichi, le menu contextuel des widgets RTE s’ouvre. En sélectionnant l’option _Service clientèle des réservations pour grands groupes_ dans le menu contextuel, la boîte de dialogue modale personnalisée s’ouvre.

Une fois que le numéro de service clientèle souhaité est ajouté à partir de la boîte de dialogue modale, les badges rendent le _numéro de téléphone non modifiable_ et le mettent en forme en bleu.

### Enregistrement d’une extension

`ExtensionRegistration.js`, mappé vers l’itinéraire `index.html`, est le point d’entrée de l’extension AEM et définit les éléments suivants :

+ La définition du badge est définie dans `getBadges()` à l’aide des attributs de configuration `id`, `prefix`, `suffix`, `backgroundColor` et `textColor`.
+ Dans cet exemple, le caractère `#` sert à définir les limites de ce badge, c’est-à-dire toute chaîne de l’éditeur de texte enrichi entourée de `#` est traitée comme une instance de ce badge.

Consultez également les détails clés du widget RTE :

+ La définition du widget dans la fonction `getWidgets()` avec les attributs `id`, `label` et `url`.
+ La valeur d’attribut `url`, un chemin d’URL relatif (`/index.html#/largeBookingsCustomerService`) pour charger la boîte de dialogue modale.


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

          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",                    // Provide a unique ID for the badge
              prefix: "#",                          // Provide a Badge starting character
              suffix: "#",                          // Provide a Badge ending character
              backgroundColor: "",                  // Provide HEX or text CSS color code for the background
              textColor: "#071DF8"                  // Provide HEX or text CSS color code for the text
            }
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",       // Provide a unique ID for the widget
              label: "Large Group Bookings Customer Service",          // Provide a label for the widget
              url: "/index.html#/largeBookingsCustomerService",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/largeBookingsCustomerService`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Ajouter l’itinéraire `largeBookingsCustomerService` dans `App.js`{#add-widgets-route}

Dans le composant React principal `App.js`, ajoutez l’itinéraire `largeBookingsCustomerService` pour effectuer le rendu de l’interface utilisateur pour le chemin d’URL relatif ci-dessus.

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### Créer le composant React `LargeBookingsCustomerService`{#create-widget-react-component}

L’interface utilisateur du widget ou de la boîte de dialogue est créée à l’aide du framework [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html).

Lors de l’ajout des informations du service clientèle, le code du composant React entoure la variable du numéro de téléphone avec les caractères `#` de badges enregistrés pour le convertir en badges, comme `#${phoneNumber}#`, le rendant ainsi non modifiable.

Voici les principaux points forts du code `LargeBookingsCustomerService` :

+ L’interface utilisateur est rendue à l’aide des composants React Spectrum, tels que [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html) et [Button](https://react-spectrum.adobe.com/react-spectrum/Button.html).
+ Le tableau `largeGroupCustomerServiceList` a un mappage codé en dur du nom du représentant ou de la représentante et du numéro de téléphone. Dans un scénario réel, ces données peuvent être récupérées à partir de l’action Adobe AppBuilder ou de systèmes externes, ou de la passerelle API locale ou basée sur le fournisseur cloud.
+ La fonction `guestConnection` est initialisée à l’aide du [hook React](https://react.dev/reference/react/useEffect) `useEffect` et gérée en tant que statut du composant. Elle est utilisé pour communiquer avec l’hôte AEM.
+ La fonction `handleCustomerServiceChange` obtient le nom du représentant ou de la représentante et le numéro de téléphone, puis met à jour les variables de statut du composant.
+ La fonction `addCustomerServiceDetails` utilisant l’objet `guestConnection` fournit des instructions d’exécution de l’éditeur de texte enrichi. Dans ce cas, l’instruction `insertContent` et l’extrait de code HTML.
+ Pour rendre le **numéro de téléphone non modifiable** à l’aide de badges, le caractère spécial `#` est ajouté avant et après la variable `phoneNumber`, comme `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
