---
title: Console des fragments de contenu - Champs personnalisés
description: Découvrez comment créer un champ personnalisé dans l’éditeur de fragments de contenu AEM.
feature: Developer Tools, Content Fragments
version: Cloud Service
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-02-27T00:00:00Z
jira: KT-14903
thumbnail: KT-14903.jpeg
source-git-commit: bb902089a709ab00853f4856d9bf42c18a5097e7
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 6%

---


# Champs personnalisés

Découvrez comment créer des champs personnalisés dans l’éditeur de fragments de contenu AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3427585?learn=on)

AEM les extensions d’interface utilisateur doivent être développées à l’aide de la fonction [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) qui conserve une apparence cohérente avec le reste de l&#39;AEM et dispose d&#39;une vaste bibliothèque de fonctionnalités préconfigurées, ce qui réduit le temps de développement.

## Point d’extension

Cet exemple remplace un champ existant de l’éditeur de fragments de contenu par une implémentation personnalisée.

| Interface utilisateur AEM étendue | Point d’extension |
| ------------------------ | --------------------- | 
| [Éditeur de fragments de contenu](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Rendu des éléments de formulaire personnalisés](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/) |

## Exemple d’extension

Cet exemple illustre la limitation des valeurs de champ de l’éditeur de fragments de contenu à un jeu prédéterminé en remplaçant le champ standard par une liste déroulante personnalisée de SKU prédéfinis. Les auteurs peuvent effectuer une sélection dans cette liste spécifique de SKU. Bien que les SKU proviennent généralement d’un système de gestion de l’information sur les produits (PIM), cet exemple simplifie la mise en liste statique des SKU.

Le code source de cet exemple est : [disponible en téléchargement](./assets/editor-custom-field/content-fragment-editor-custom-field-src.zip).

### Définition du modèle de fragment de contenu

Cet exemple est lié à tout champ de fragment de contenu dont le nom est `sku` (via un [correspondance d’expression régulière](#extension-registration) de `^sku$`) et le remplace par un champ personnalisé. L’exemple utilise le modèle de fragment de contenu aventure WKND mis à jour et la définition est la suivante :

![Définition du modèle de fragment de contenu](./assets/editor-custom-field/content-fragment-editor.png)

Bien que le champ SKU personnalisé s’affiche sous forme de liste déroulante, son modèle sous-jacent est configuré en tant que champ de texte. L’implémentation des champs personnalisés doit uniquement s’aligner sur le nom et le type de propriété appropriés, ce qui facilite le remplacement du champ standard par la version de liste déroulante personnalisée.


### Itinéraires de l’application

Dans le composant React principal `App.js`, incluez la variable `/sku-field` itinéraire pour effectuer le rendu de `SkuField` Composant React.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
import React from "react";
import ErrorBoundary from "react-error-boundary";
import { HashRouter as Router, Routes, Route } from "react-router-dom";
import ExtensionRegistration from "./ExtensionRegistration";
import SkuField from "./SkuField"; // Custom component implemented below

function App() {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          <Route index element={<ExtensionRegistration />} />
          <Route
            exact path="index.html"
            element={<ExtensionRegistration />}
          />
          {/* This is the React route that maps to the custom field */}
          <Route
            exact path="/sku-field"
            element={<SkuField />}/>
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
...
```

Cet itinéraire personnalisé de `/sku-field` est mappée à la variable `SkuField` est utilisé ci-dessous dans la variable [Enregistrement d’une extension](#extension-registration).

### Enregistrement d’une extension

`ExtensionRegistration.js`, mappé à l’itinéraire index.html, est le point d’entrée de l’extension AEM et définit les éléments suivants :

+ Définition du widget dans `getDefinitions()` fonction avec `fieldNameExp` et `url` attributs. La liste complète des attributs disponibles est disponible dans le [Référence de l’API de rendu d’élément de formulaire personnalisé](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference).
+ La variable `url` valeur d’attribut, un chemin d’URL relatif (`/index.html#/skuField`) pour charger l’interface utilisateur du champ.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        field: {
          getDefinitions() {
            return [
              // Multiple fields can be registered here.
              {
                fieldNameExp: '^sku$',  // This is a regular expression that matches the field name in the Content Fragment Model to replace with React component specified in the `url` key.
                url: '/#/sku-field',    // The URL, which is mapped vai the Route in App.js, to the React component that will be used to render the field.
              },
              // Other bindings besides fieldNameExp, other bindings can be used as well as defined here:
              // https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference
            ];
          },
        },
      }
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Champ personnalisé

La variable `SkuField` Le composant React met à jour l’éditeur de fragment de contenu avec une interface utilisateur personnalisée, en utilisant Adobe React Spectrum pour son formulaire de sélecteur. Voici quelques points forts :

+ Utilisation `useEffect` pour l’initialisation et la connexion à AEM’éditeur de fragments de contenu, avec un état de chargement affiché jusqu’à la fin de la configuration.
+ Le rendu à l’intérieur d’un iFrame ajuste dynamiquement la hauteur de l’iFrame au moyen de l’option `onOpenChange` pour s’adapter à la liste déroulante du sélecteur React Spectrum Adobe.
+ Communication des sélections de champs à l’hôte à l’aide de `connection.host.field.onChange(value)` dans le `onSelectionChange` , en s’assurant que la valeur sélectionnée est validée et enregistrée automatiquement conformément aux instructions du modèle de fragment de contenu.

Les champs personnalisés sont rendus dans un iFrame injecté dans l’éditeur de fragments de contenu. La communication entre le code de champ personnalisé et l’éditeur de fragment de contenu est réalisée exclusivement par l’intermédiaire de la fonction `connection` , établi par la fonction `attach` de la fonction `@adobe/uix-guest` module.

`src/aem-cf-editor-1/web-src/src/components/SkuField.js`

```javascript
import React, { useEffect, useState } from "react";
import { extensionId } from "./Constants";
import { attach } from "@adobe/uix-guest";
import { Provider, View, lightTheme } from "@adobe/react-spectrum";
import { Picker, Item } from "@adobe/react-spectrum";

const SkuField = (props) => {
  const [connection, setConnection] = useState(null);
  const [validationState, setValidationState] = useState(null);
  const [value, setValue] = useState(null);
  const [model, setModel] = useState(null);
  const [items, setItems] = useState(null);

  /**
   * Mock function that gets a list of Adventure SKUs to display.
   * The data could come anywhere, AEM's HTTP APIs, a PIM, or other system.
   * @returns a list of items for the picker
   */
  async function getItems() {
    // Data to drive input field validation can come from anywhere.
    // Fo example this commented code shows how it could be fetched from an HTTP API.
    // fetch(MY_API_URL).then((response) => response.json()).then((data) => { return data; }

    // In this example, for simplicity, we generate a list of 25 SKUs.
    return Array.from({ length: 25 }, (_, i) => ({ 
        name: `WKND-${String(i + 1).padStart(3, '0')}`, 
        id: `WKND-${String(i + 1).padStart(3, '0')}` 
    }));
  }

  /**
   * When the fields changes, update the value in the Content Fragment Editor
   * @param {*} value the selected value in the picker
   */
  const onSelectionChange = async (value) => {
    // This sets the value in the React state of the custom field
    setValue(value);
    // This calls the setValue method on the host (AEM's Content Fragment Editor)
    connection.host.field.onChange(value);
  };

  /**
   * Some widgets, like the Picker, have a variable height.
   * In these cases adjust the Content Fragment Editor's iframe's height so the field doesn't get cut off.     *
   * @param {*} isOpen true if the picker is open, false if it's closed
   */
  const onOpenChange = async (isOpen) => {
    if (isOpen) {
      // Calculate the height of the picker box and its label, and surrounding padding.
      const pickerHeight = Number(document.body.clientHeight.toFixed(0));
      // Calculate the height of the picker options dropdown, and surrounding padding.
      // We do this  by multiplying the number of items by the height of each item (32px) and adding 12px for padding.
      const optionsHeight = items.length * 32 + 12;

      // Set the height of the iframe to the height of the picker + the height of the options, or 400px, whichever is smaller.
      // The options will scroll if they they cannot fit into 400px
      const height = Math.min(pickerHeight + optionsHeight, 400);

      // Set the height of the iframe in the Content Fragment Editor
      await connection.host.field.setHeight(height);
    } else {
      // Set the height of the iframe in the Content Fragment Editor to the height of the closed picker.
      await connection.host.field.setHeight(
        Number(document.body.clientHeight.toFixed(0))
      );
    }
  };

  useEffect(() => {
    const init = async () => {
      // Connect to the host (AEM's Content Fragment Editor)
      const conn = await attach({ id: extensionId });
      setConnection(conn);

      // get the Content Fragment Model
      setModel(await conn.host.field.getModel());

      // Share the validation state back to the client.
      // When conn.host.field.setValue(value) is called, the
      await conn.host.field.onValidationStateChange((state) => {
        // state can be `valid` or `invalid`.
        setValidationState(state);
      });
      // Get default value from the Content Fragment Editor
      // (either the default value set in the model, or a perviously set value)
      setValue(await conn.host.field.getDefaultValue());

      // Get the list of items for the picker; in this case its a list of adventure SKUs 
      // This could come from elsewhere in AEM or from an external system.
      setItems(await getItems());
    };

    init().catch(console.error);
  }, []);

  // If the component is not yet initialized, return a loading state.
  if (!connection || !model || !items) {
    // Put whatever loader you like here...
    return <Provider theme={lightTheme}>Loading custom field...</Provider>;
  }

  // Wrap the Spectrum UI component in a Provider theme, such that it is styled appropriately.
  // Render the picker, and bind to the data and custom event handlers.

  // Set as much of the model as we can, to allow maximum authoring flexibility without developer support.
  return (
    <Provider theme={lightTheme}>
      <View width="100%">
        <Picker
          label={model.fieldLabel}
          isRequired={model.required}
          placeholder={model.emptyText}
          errorMessage={model.customErrorMsg}
          selectedKey={value}
          necessityIndicator="icon"
          shouldFlip={false}
          width={"90%"}
          items={items}
          isInvalid={validationState === "invalid"}
          onSelectionChange={onSelectionChange}
          onOpenChange={onOpenChange}
        >
          {(item) => <Item key={item.value}>{item.name}</Item>}
        </Picker>
      </View>
    </Provider>
  );
};

export default SkuField;
```