---
title: Utilisation de jeux de résultats volumineux dans AEM sans affichage
description: Découvrez comment utiliser des jeux de résultats volumineux avec AEM sans affichage.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
source-git-commit: 9eb706e49f12a3ebd5222e733f540db4cf2c8748
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 2%

---


# Jeux de résultats volumineux dans AEM sans affichage

AEM requêtes GraphQL sans affichage peuvent renvoyer des résultats volumineux. Cet article décrit comment travailler avec des résultats volumineux dans AEM sans affichage afin d’optimiser les performances de votre application.

AEM Headless prend en charge une [offset/limit](#list-query) et [pagination basée sur le curseur](#paginated-query) requêtes à des sous-ensembles plus petits d’un plus grand jeu de résultats. Plusieurs demandes peuvent être effectuées pour collecter autant de résultats que nécessaire.

Les exemples ci-dessous utilisent de petits sous-ensembles de résultats (quatre enregistrements par requête) pour démontrer les techniques. Dans une application réelle, vous utiliseriez un plus grand nombre d’enregistrements par requête pour améliorer les performances. 50 enregistrements par requête est une bonne ligne de base.

## Modèle de fragment de contenu

La pagination et le tri peuvent être utilisés par rapport à n’importe quel modèle de fragment de contenu.

## Requêtes persistantes GraphQL

Lors de l’utilisation de jeux de données volumineux, la pagination décalée, limitée et basée sur le curseur peut être utilisée pour récupérer un sous-ensemble spécifique des données. Cependant, il existe des différences entre les deux techniques, qui peuvent rendre l&#39;une plus appropriée que l&#39;autre dans certaines situations.

### Décalage/limite

Liste des requêtes, à l’aide de `limit` et `offset` proposer une approche simple qui spécifie le point de départ (`offset`) et le nombre d’enregistrements à récupérer (`limit`). Cette approche permet de sélectionner un sous-ensemble de résultats à partir de n’importe quel emplacement du jeu de résultats complet, comme accéder à une page de résultats spécifique. Bien qu’il soit facile à mettre en oeuvre, il peut être lent et inefficace lorsque vous traitez de gros résultats, car la récupération de nombreux enregistrements nécessite une analyse de tous les enregistrements précédents. Cette approche peut également entraîner des problèmes de performances lorsque la valeur de décalage est élevée, car elle peut nécessiter la récupération et le rejet de nombreux résultats.

#### Requête GraphQL

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### Variables de requête

```json
{
  "offset": 1,
  "limit": 4
}
```

#### Réponse GraphQL

La réponse JSON résultante contient les 2e, 3e, 4e et 5e aventures les plus coûteuses. Les deux premières aventures dans les résultats ont le même prix (`4500` Ainsi, la variable [requête de liste](#list-queries) indique que les aventures au prix identique sont ensuite triées par titre dans l’ordre croissant.)

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### Requête paginée

La pagination basée sur un curseur, disponible dans les requêtes paginées, implique l’utilisation d’un curseur (une référence à un enregistrement spécifique) pour récupérer l’ensemble de résultats suivant. Cette approche est plus efficace, car elle évite d’analyser tous les enregistrements précédents pour récupérer le sous-ensemble de données requis. Les requêtes paginées sont idéales pour itérer à travers de grands ensembles de résultats du début à un certain point au milieu ou à la fin. Liste des requêtes, à l’aide de `limit` et `offset` proposer une approche simple qui spécifie le point de départ (`offset`) et le nombre d’enregistrements à récupérer (`limit`). Cette approche permet de sélectionner un sous-ensemble de résultats à partir de n’importe quel emplacement du jeu de résultats complet, comme accéder à une page de résultats spécifique. Bien qu’il soit facile à mettre en oeuvre, il peut être lent et inefficace lorsque vous traitez de gros résultats, car la récupération de nombreux enregistrements nécessite une analyse de tous les enregistrements précédents. Cette approche peut également entraîner des problèmes de performances lorsque la valeur de décalage est élevée, car elle peut nécessiter la récupération et le rejet de nombreux résultats.

#### Requête GraphQL

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### Variables de requête

```json
{
  "first": 3
}
```

#### Réponse GraphQL

La réponse JSON résultante contient les 2e, 3e, 4e et 5e aventures les plus coûteuses. Les deux premières aventures dans les résultats ont le même prix (`4500` Ainsi, la variable [requête de liste](#list-queries) indique que les aventures au prix identique sont ensuite triées par titre dans l’ordre croissant.)

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### Prochain jeu de résultats paginés

L’ensemble de résultats suivant peut être récupéré à l’aide de la variable `after` et le paramètre `endCursor` de la requête précédente. S’il n’y a plus de résultats à récupérer, `hasNextPage` is `false`.

##### Variables de requête

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## Exemples React

Voici des exemples React qui montrent comment utiliser [décalage et limite](#offset-and-limit) et [pagination basée sur le curseur](#cursor-based-pagination) approches. En règle générale, le nombre de résultats par requête est plus élevé. Toutefois, pour les besoins de ces exemples, la limite est définie sur 5.

### Exemple de décalage et de limite

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

En utilisant le décalage et la limite, les sous-ensembles de résultats peuvent facilement être récupérés et affichés.

#### useEffect hook

Le `useEffect` hook appelle une requête persistante (`adventures-by-offset-and-limit`) qui récupère une liste d’aventures. La requête utilise la variable `offset` et `limit` pour spécifier le point de départ et le nombre de résultats à récupérer. Le `useEffect` hook est appelé lorsque la fonction `page` change.


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### Composant

Le composant utilise la variable `useOffsetLimitAdventures` pour récupérer une liste d’aventures. Le `page` est incrémentée et décrémentée pour récupérer le jeu de résultats suivant et précédent. Le `hasMore` est utilisée pour déterminer si le bouton de page suivante doit être activé.

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### Exemple paginé

![Exemple paginé](./assets/large-results/paginated-example.png)

_Chaque zone rouge représente une requête HTTP GraphQL paginée discrète._

À l’aide de la pagination placée sur le curseur, les jeux de résultats volumineux peuvent facilement être récupérés et affichés, en collectant progressivement les résultats et en les concaténant aux résultats existants.


#### Hochet UseEffect

Le `useEffect` hook appelle une requête persistante (`adventures-by-paginated`) qui récupère une liste d’aventures. La requête utilise la variable `first` et `after` pour indiquer le nombre de résultats à récupérer et le début du curseur. `fetchData` boucle en continu, collecte le prochain ensemble de résultats paginés, jusqu’à ce qu’il n’y ait plus de résultats à récupérer.

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### Composant

Le composant utilise la variable `usePaginatedAdventures` pour récupérer une liste d’aventures. Le `queryCount` est utilisée pour afficher le nombre de requêtes HTTP effectuées pour récupérer la liste des aventures.

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```