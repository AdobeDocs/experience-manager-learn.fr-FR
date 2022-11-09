---
title: Utilisation AEM composants React modifiables v2
description: Découvrez comment utiliser AEM React Editable Components v2 pour alimenter une application React.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: f02d5e01388ee61228254951b05c37c336423348
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 5%

---


# Utilisation AEM composants React modifiables v2

AEM fournit [AEM Composants modifiables React v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), un SDK basé sur Node.js qui permet la création de composants React, qui prennent en charge la modification de composants dans le contexte à l’aide d’AEM SPA Editor.

+ [module npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Projet Github](https://github.com/adobe/aem-react-editable-components)
+ [Documentation sur les Adobes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Pour plus d’informations et d’exemples de code pour AEM React Editable Components v2, consultez la documentation technique :

+ [Intégration à la documentation AEM](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentation des composants modifiables](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentation sur l’assistance](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM pages

Les composants React modifiables d’AEM fonctionnent avec SPA éditeur ou les applications à distance SPA React. Le contenu renseignant les composants React modifiables doit être exposé via AEM pages qui étendent la variable [Composant de page SPA](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html). AEM composants, qui mappe les composants React modifiables, doivent implémenter AEM [Structure de l’exportateur de composants](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html?lang=fr) - par exemple [AEM composants WCM principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr).


## Dépendances

Assurez-vous que l’application React est en cours d’exécution sur Node.js 14+.

L’ensemble minimal de dépendances pour que l’application React utilise AEM React Editable Components v2 est le suivant : `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`, et  `@adobe/aem-spa-page-model-manager`.


+ `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [AEM base des composants WCM principaux React](https://github.com/adobe/aem-react-core-wcm-components-base) et [AEM Composants WCM principaux React SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) ne sont pas compatibles avec AEM React Editable Components v2.

## Éditeur SPA

Lors de l’utilisation des AEM composants React modifiables avec une application React SPA basée sur l’éditeur, l’ `ModelManager` SDK, en tant que SDK :

1. Récupère le contenu de AEM
1. Remplit les composants React Edible avec le contenu AEM

Encapsulez l’application React avec un ModelManager initialisé et effectuez le rendu de l’application React. L’application React doit contenir une instance de `<Page>` composant exporté depuis `@adobe/aem-react-editable-components`. Le `<Page>` Le composant a une logique pour créer dynamiquement des composants React en fonction de la variable `.model.json` fourni par AEM.

+ `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

Le `<Page>` est transmis en tant que représentation de la page AEM au format JSON, via la variable `pageModel` fourni par la fonction `ModelManager`. Le `<Page>` crée dynamiquement des composants React pour les objets dans la variable `pageModel` en faisant correspondre la variable `resourceType` avec un composant React qui s’enregistre auprès du type de ressource via `MapTo(..)`.

## Composants modifiables

Le `<Page>` transmet la représentation de la page d’AEM au format JSON, via la fonction `ModelManager`. Le `<Page>` crée ensuite dynamiquement des composants React pour chaque objet dans le fichier JSON en correspondant à l’objet JS. `resourceType` valeur avec un composant React qui s’enregistre auprès du type de ressource via le `MapTo(..)` appel . Par exemple, ce qui suit serait utilisé pour instancier une instance.

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

Le fichier JSON ci-dessus fourni par AEM peut être utilisé pour instancier et renseigner de manière dynamique un composant React modifiable.

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## Incorporation de composants

Les composants modifiables peuvent être réutilisés et incorporés les uns dans les autres. Il existe deux considérations clés lors de l’incorporation d’un composant modifiable dans un autre :

1. Le contenu JSON de l’AEM pour le composant d’intégration doit contenir le contenu pour satisfaire aux composants incorporés. Pour ce faire, créez une boîte de dialogue pour le composant AEM qui collecte les données requises.
1. L’instance &quot;non modifiable&quot; du composant React doit être incorporée, plutôt que l’instance &quot;modifiable&quot; encapsulée avec `<EditableComponent>`. La raison est que, si le composant incorporé possède la propriété `<EditableComponent>` wrapper, l’éditeur de SPA tente d’habiller le composant interne avec la zone de modification (zone de survol bleue), plutôt que avec le composant d’incorporation externe.

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

Le fichier JSON ci-dessus fourni par AEM peut être utilisé pour instancier et renseigner de manière dynamique un composant React modifiable qui incorpore un autre composant React.


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```



