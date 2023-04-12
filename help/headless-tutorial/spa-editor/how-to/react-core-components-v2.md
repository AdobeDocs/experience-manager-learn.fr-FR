---
title: Comment utiliser les composants React modifiables v2 d’AEM
description: Découvrez comment utiliser les composants React modifiables v2 d’AEM pour alimenter une application React.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: f02d5e01388ee61228254951b05c37c336423348
workflow-type: ht
source-wordcount: '586'
ht-degree: 100%

---


# Comment utiliser les composants React modifiables v2 d’AEM

AEM fournit les [composants React modifiables v2 d’AEM](https://www.npmjs.com/package/@adobe/aem-react-editable-components), un SDK basé sur Node.js qui permet la création de composants React prenant en charge la modification de composants dans le contexte à l’aide de l’éditeur de SPA d’AEM.

+ [Module npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Projet GitHub](https://github.com/adobe/aem-react-editable-components)
+ [Documentation Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html?lang=fr)


Pour plus d’informations et d’exemples de code pour les composants React modifiables v2 d’AEM, consultez la documentation technique :

+ [Documentation d’intégration à AEM](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentation des composants modifiables](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentation sur les outils d’aide](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## Pages AEM

Les composants React modifiables v2 d’AEM fonctionnent avec l’éditeur de SPA ou les applications React de SPA distante. Le contenu renseignant les composants React modifiables doit être exposé via les pages d’AEM qui étendent le [composant de page de SPA](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html?lang=fr). Les composants AEM, qui mappent les composants React modifiables, doivent implémenter le [cadre de l’exporteur de composants](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html?lang=fr) d’AEM, tels que les [composants principaux de gestion de contenu web d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr).


## Dépendances

Assurez-vous que l’application React utilise Node.js 14+.

L’ensemble minimal de dépendances pour que l’application React utilise les composants React modifiables v2 d’AEM est le suivant : `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping` et `@adobe/aem-spa-page-model-manager`.


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
> [La base de composants principaux React de gestion de contenu web d’AEM](https://github.com/adobe/aem-react-core-wcm-components-base) et les [composants principaux React de gestion de contenu web de SPA d’AEM](https://github.com/adobe/aem-react-core-wcm-components-spa) ne sont pas compatibles avec les composants React modifiables v2 d’AEM.

## Éditeur de SPA

Lors de l’utilisation des composants React modifiables d’AEM avec une application React basée sur l’éditeur de SPA, le SDK `ModelManager` d’AEM, en tant que SDK :

1. Récupère le contenu d’AEM
1. Remplit les composants React modifiables avec du contenu d’AEM

Encapsulez l’application React avec un ModelManager initialisé et effectuez le rendu de l’application React. L’application React doit contenir une instance du composant `<Page>`, composant exporté depuis `@adobe/aem-react-editable-components`. Le composant `<Page>` suit une logique permettant de créer dynamiquement des composants React en fonction du `.model.json` fourni par AEM.

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

Le composant `<Page>` est transmis en tant que représentation de la page AEM au format JSON, via le `pageModel` fourni par `ModelManager`. Le composant `<Page>` crée dynamiquement des composants React pour les objets dans le `pageModel` en faisant correspondre le `resourceType` avec un composant React qui s’enregistre auprès du type de ressource via `MapTo(..)`.

## Composants modifiables

Le composant `<Page>` est transmis en tant que représentation de la page AEM au format JSON, via le `ModelManager`. Le composant `<Page>` crée ensuite dynamiquement des composants React pour chaque objet dans le fichier JSON en faisant correspondre la valeur `resourceType` de l’objet JS avec un composant React qui s’enregistre auprès du type de ressource via l’appel `MapTo(..)` du composant. Par exemple, ce qui suit peut être utilisé pour instancier une instance.

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

## Incorporer des composants

Les composants modifiables peuvent être réutilisés et incorporés les uns dans les autres. Il existe deux points clés à prendre en compte lors de l’incorporation d’un composant modifiable dans un autre :

1. Le contenu JSON d’AEM pour le composant d’incorporation doit inclure le contenu pour satisfaire les composants incorporés. Pour ce faire, créez une boîte de dialogue pour le composant AEM qui collecte les données requises.
1. L’instance « non modifiable » du composant React doit être incorporée, plutôt que l’instance « modifiable » encapsulée avec `<EditableComponent>`. En effet, si le composant incorporé possède le wrapper `<EditableComponent>`, l’éditeur de SPA tente d’habiller le composant interne avec la fonction Modifier de Chrome (zone de survol bleue), plutôt qu’avec le composant d’incorporation externe.

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



