---
title: Utiliser du texte enrichi avec AEM Headless
description: Découvrez comment créer du contenu et incorporer du contenu référencé à l’aide d’un éditeur de texte enrichi multiligne avec des fragments de contenu d’Adobe Experience Manager. Découvrez en outre comment le texte enrichi est diffusé par les API GraphQL d’AEM sous un format JSON utilisé par les applications découplées.
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '1465'
ht-degree: 100%

---

# Texte enrichi avec AEM Headless

Le champ de texte multiligne est un type de données de fragments de contenu qui permet aux créateurs et créatrices de créer du contenu de texte enrichi. Les références à d’autres contenus, tels que des images ou d’autres fragments de contenu, peuvent être intégrées dynamiquement au sein du flux du texte. Le champ de texte monoligne est un autre type de données de fragments de contenu devant être utilisé pour les éléments de texte simples.

L’API GraphQL d’AEM offre une fonctionnalité robuste permettant de renvoyer du texte enrichi sous forme de HTML, de texte brut ou de JSON pur. La représentation JSON est puissante, car elle donne à l’application cliente un contrôle total sur la manière de rendre le contenu.

## Éditeur multiligne

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

Dans l’éditeur de fragment de contenu, la barre de menu du champ de texte multiligne fournit aux créateurs et créatrices des fonctionnalités standard de mise en forme de texte enrichi, telles que **gras**, *italique* et souligné. L’ouverture d’un champ multiligne en mode plein écran active [d’autres outils de mise en forme, tels que le type de paragraphe, la recherche et le remplacement, la vérification orthographique, etc.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=fr)

>[!NOTE]
>
> Les modules de texte enrichi de l’éditeur multiligne ne peuvent pas être personnalisés.

## Type de données de texte multiligne {#multi-line-data-type}

Utilisez le type de données **Texte multiligne** pour définir votre modèle de fragment de contenu et activer la création de texte enrichi.

![Type de données de texte enrichi multiligne.](assets/rich-text/multi-line-rich-text.png)

Plusieurs propriétés du champ multiligne peuvent être configurées.

La propriété **Rendre en tant que** peut être définie sur :

* Zone de texte : effectue le rendu d’un seul champ multiligne.
* Plusieurs champs : effectue le rendu de plusieurs champs multilignes.


Le **type par défaut** peut être défini sur :

* Texte enrichi
* Texte (Markdown)
* Texte brut

L’option **Type par défaut** influence directement l’expérience de modification et détermine si les outils de texte enrichi sont présents.

Vous pouvez également [activer les références intégrées](#insert-fragment-references) à d’autres fragments de contenu en cochant **Autoriser la référence du fragment** et en configurant les **modèles de fragment de contenu autorisés**.

Cochez la case **Traduisible** si le contenu doit être localisé. Seuls le texte enrichi et le texte brut peuvent être localisés. Voir [Utiliser du contenu localisé pour plus d’informations](./localized-content.md).

## Réponse de texte enrichi avec l’API GraphQL

Lors de la création d’une requête GraphQL, les développeurs et développeuses peuvent choisir différents types de réponse parmi `html`, `plaintext`, `markdown` et `json` à partir d’un champ multiligne.

Les développeurs et développeuses peuvent utiliser la variable [Aperçu JSON](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html?lang=fr) dans l’éditeur de fragment de contenu pour afficher toutes les valeurs du fragment de contenu actuel qui peuvent être renvoyées à l’aide de l’API GraphQL.

## Requête persistante GraphQL

Le fait de sélectionner le format de réponse `json` du champ multiligne offre la plus grande flexibilité lorsque vous utilisez du contenu de texte enrichi. Le contenu de texte enrichi est diffusé sous la forme d’un tableau de types de nœuds JSON qui peuvent être traités de manière unique en fonction de la plateforme cliente.

Voici un type de réponse JSON d’un champ multiligne nommé `main`, qui contient un paragraphe : « *This is a paragraph taht includes **important**content (Il s’agit d’un paragraphe qui comprend du contenu important).* » (Il s’agit d’un paragraphe qui comprend du contenu important) où « important » est marqué comme étant en **gras**.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

La variable `$path` utilisée dans le filtre `_path` nécessite le chemin d’accès complet au fragment de contenu (par exemple, `/content/dam/wknd/en/magazine/sample-article`).

**Réponse GraphQL :**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### Autres exemples

Ci-dessous se trouvent plusieurs exemples de types de réponse d’un champ multiligne nommé `main`, qui contient un paragraphe : « This is a paragraph that includes **important** content. » (Il s’agit d’un paragraphe qui comprend du contenu important) où « important » est marqué comme étant en **gras**.

Exemple HTML

**Requête persistante GraphQL :**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**Réponse GraphQL :**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

Exemple Markdown

**Requête persistante GraphQL :**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**Réponse GraphQL :**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

Exemple de texte brut

**Requête persistante GraphQL :**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**Réponse GraphQL :**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

L’option de rendu `plaintext` supprime toute mise en forme.

+++


## Effectuer le rendu d’une réponse JSON de texte enrichi {#render-multiline-json-richtext}

La réponse JSON en texte enrichi du champ multiligne est structurée sous la forme d’une arborescence hiérarchique. Chaque objet ou nœud représente un bloc HTML différent du texte enrichi.

Vous trouverez ci-dessous un exemple de réponse JSON d’un champ de texte multiligne. Notez que chaque objet ou nœud comprend un `nodeType` qui représente le bloc HTML du texte enrichi comme `paragraph`, `link`, et `text`. Chaque nœud peut contenir `content` qui est un sous-tableau contenant tous les enfants du nœud actuel.

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

La méthode la plus simple pour effectuer le rendu de la réponse `json` multiligne consiste à traiter chaque objet ou nœud dans la réponse, puis à traiter tous les enfants du nœud actuel. Une fonction récursive peut être utilisée pour parcourir l’arborescence JSON.

Vous trouverez ci-dessous un exemple de code qui illustre une approche récursive traversante. Les exemples sont basés sur JavaScript et utilisent le [JSX](https://reactjs.org/docs/introducing-jsx.html) de React. Cependant, les concepts de programmation peuvent être appliqués à n’importe quel langage.

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

`renderNodeList` est une fonction récursive qui utilise un tableau de `childNodes`. Chaque nœud du tableau est ensuite transmis à une fonction `renderNode`, qui à son tour appelle `renderNodeList` si le nœud a des enfants.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

La fonction `renderNode` exige un objet unique nommé `node`. Un nœud peut avoir des enfants qui sont traités de manière récursive à l’aide de la fonction `renderNodeList` décrite ci-dessus. Enfin, un élément `nodeMap` est utilisé pour effectuer le rendu du contenu du nœud en fonction de son `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

L’élément `nodeMap` est un littéral d’objet JavaScript utilisé comme mappage. Chacune des « clés » représente un `nodeType` différent. Les paramètres `node` et `children` peuvent être transmis aux fonctions résultantes qui effectuent le rendu du nœud. Le type de retour utilisé dans cet exemple est JSX, mais l’approche peut être adaptée pour créer un littéral de chaîne représentant du contenu HTML.

### Exemple de code complet

Vous trouverez un utilitaire de rendu de texte enrichi réutilisable dans l’[exemple React GraphQL WKND](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) : utilitaire réutilisable exposant une fonction `mapJsonRichText`. Cet utilitaire peut être utilisé par les composants qui souhaitent effectuer le rendu d’une réponse JSON de texte enrichi en tant que JSX React.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) : exemple de composant qui effectue une requête GraphQL contenant du texte enrichi. Le composant utilise l’utilitaire `mapJsonRichText` pour effectuer le rendu du texte enrichi et des références.


## Ajouter des références intégrées au texte enrichi {#insert-fragment-references}

Le champ multiligne permet aux créateurs et créatrices d’insérer des images ou d’autres ressources numériques d’AEM Assets dans le flux du texte enrichi.

![Insértion de l’image.](assets/rich-text/insert-image.png)

La capture d’écran ci-dessus illustre une image insérée dans le champ multiligne à l’aide du bouton **Insérer une ressource**.

Les références à d’autres fragments de contenu peuvent également être liées ou insérées dans le champ multiligne à l’aide du bouton **Insérer un fragment de contenu**.

![Insertion d’une référence de fragment de contenu.](assets/rich-text/insert-contentfragment.png)

La capture d’écran ci-dessus montre un autre fragment de contenu, Ultimate Guide to LA Skate Parks, inséré dans le champ multiligne. Les types de fragments de contenu qui peuvent être insérés dans le champ sont contrôlés par la configuration des **Modèles de fragment de contenu autorisés** dans le [type de données multiligne](#multi-line-data-type) dans le modèle de fragment de contenu.

## Demander des références intégrées avec GraphQL

L’API GraphQL permet aux développeurs et développpeuses de créer une requête qui inclut des propriétés supplémentaires sur les références insérées dans un champ multiligne. La réponse JSON comprend un objet `_references` séparé qui répertorie ces propriétés supplémentaires. La réponse JSON permet aux développeurs et développeuses de contrôler pleinement la manière d’effectuer le rendu des références ou des liens au lieu d’avoir à gérer du HTML obstiné.

Par exemple, vous pouvez :

* inclure une logique de routage personnalisée pour la gestion des liens vers d’autres fragments de contenu lors de l’implémentation d’une application monopage, comme l’utilisation du routeur React ou de Next.js ;
* effectuer le rendu d’une image intégrée à l’aide du chemin d’accès absolu vers un environnement de publication AEM en tant que valeur `src` ;
* déterminer comment effectuer le rendu d’une référence incorporée sur un autre fragment de contenu avec des propriétés personnalisées supplémentaires.

Utilisez le type de retour `json` et incluez l’objet `_references` lors de la création d’une requête GraphQL :

**Requête persistante GraphQL :**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }  
    }
  }
}
```

Dans la requête ci-dessus, le champ `main` est renvoyé en tant que JSON. L’objet `_references` inclut des fragments pour gérer les références de type `ImageRef` ou `ArticleModel`.

**Réponse JSON :**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

La réponse JSON inclut l’emplacement où la référence a été insérée dans le texte enrichi avec `"nodeType": "reference"`. L’objet `_references` inclut ensuite chaque référence.

## Effectuer le rendu de références intégrées dans du texte enrichi

Pour effectuer le rendu des références intégrées, l’approche récursive décrite à la section [Effectuer le rendu d’une réponse JSON multiligne](#render-multiline-json-richtext) peut être étendue.

Ici, `nodeMap` est le mappage qui effectue le rendu des nœuds JSON.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

L’approche de haut niveau consiste à examiner chaque fois que `nodeType` est égal à `reference` dans la réponse JSON multiligne. Une fonction de rendu personnalisée peut alors être appelée, qui inclut l’objet `_references` renvoyé dans la réponse GraphQL.

Le chemin d’accès à la référence intégrée peut ensuite être comparé à l’entrée correspondante dans l’objet `_references` et un autre mappage personnalisé `renderReference` peut être appelé.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

L’élément `__typename` de l’objet `_references` peut être utilisé pour mapper différents types de référence à différentes fonctions de rendu.

### Exemple de code complet

Vous trouverez un exemple complet d’écriture d’un générateur de rendu de références personnalisé dans [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js), dans le cadre de l’[Exemple React GraphQL WKND](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## Exemple de bout en bout

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> La vidéo ci-dessus utilise `_publishUrl` pour effectuer le rendu de la référence de l’image. Préférez plutôt `_dynamicUrl` comme expliqué dans les [procédures relatives aux images optimisées pour le web](./images.md).


La vidéo précédente présente un exemple de bout en bout :

1. Mise à jour du champ de texte multiligne d’un modèle de fragment de contenu pour autoriser les références de fragment.
2. Utilisation de l’éditeur de fragment de contenu pour inclure une image et une référence à un autre fragment dans un champ de texte multiligne.
3. Création d’une requête GraphQL qui inclut la réponse textuelle multiligne au format JSON et tout élément `_references` utilisé.
4. Écriture d’une SPA React qui effectue le rendu des références intégrées de la réponse de texte enrichi.
