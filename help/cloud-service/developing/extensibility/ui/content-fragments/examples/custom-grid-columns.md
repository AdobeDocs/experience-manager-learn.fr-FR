---
title: Colonnes de grille personnalisées dans la console de fragments de contenu
description: Découvrez comment ajouter une colonne de grille personnalisée à la console de fragments de contenu.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13453
thumbnail: KT-13453.jpeg
doc-type: article
last-substantial-update: 2023-06-07T00:00:00Z
exl-id: 87143cf9-e932-4ad6-afe2-cce093c520f4
duration: 305
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 100%

---

# Colonnes de grille personnalisées

![Colonne de grille personnalisée de la console de fragments de contenu.](./assets/custom-grid-columns/hero.png){align="center"}

Vous pouvez ajouter des colonnes de grille personnalisées à la console de fragments de contenu à l’aide du point d’extension `contentFragmentGrid`. Cet exemple montre comment ajouter, dans un format lisible, une colonne personnalisée qui affiche l’âge des fragments de contenu en fonction de sa date de dernière modification.

## Point d’extension

Cet exemple s’étend jusqu’au point d’extension `contentFragmentGrid` pour ajouter une colonne personnalisée à la console de fragments de contenu.

| Interface utilisateur AEM étendue | Point d’extension |
| ------------------------ | --------------------- | 
| [Console Fragments de contenu](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [Colonnes de grille](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/) |

## Exemple d’extension

L’exemple suivant crée une colonne personnalisée `Age` qui affiche l’âge du fragment de contenu dans un format lisible. L’âge est calculé à partir de la date de dernière modification du fragment de contenu.

Le code indique comment obtenir les métadonnées du fragment de contenu dans le fichier d’enregistrement de l’extension et comment transformer et exporter le contenu JSON du fragment de contenu.

Cet exemple utilise la bibliothèque [Luxon](https://moment.github.io/luxon/) pour calculer l’âge du fragment de contenu, installée via `npm i luxon`.

### Enregistrement d’une extension

`ExtensionRegistration.js`, mappé à l’itinéraire index.html, est le point d’entrée de l’extension AEM et définit les éléments suivants :

+ L’emplacement de l’extension (`contentFragmentGrid`) s’insère dans l’expérience de création AEM.
+ La définition de la colonne personnalisée, dans la fonction `getColumns()`.
+ Les valeurs de chaque colonne personnalisée, par ligne.

```javascript
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";
import { Duration } from "luxon";

/**
 * Set up a in-memory cache of the custom column data.
 * This is important if the work to contain column data is expensive, such as making HTTP requests to obtain the value.
 * 
 * The caching of computed value is optional, but recommended if the work to compute the column data is expensive.
 */
const COLUMN_AGE_ID = "age";
const cache = {
  [COLUMN_AGE_ID]: {},
};

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        contentFragmentGrid: {
          getColumns() {
            return [
              {
                id: COLUMN_AGE_ID,          // Give the column a unique ID.
                label: "Age",               // The label to display
                render: async function (fragments) {
                  // This function is run for each row in the grid.
                  // The fragments parameter is an array of all the fragments (aka each row) in the grid.
                  const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();

                  // Iterate over each fragment in the grid
                  for (const fragment of fragments) {
                    // Check if a previous pass has computed the value for this fragment.id. If it has, we can skip it.
                    if (!cache[COLUMN_AGE_ID][fragment.id]) {
                      // If the fragment.id has not been computed and cached, then compute the value and cache it.
                      cache[COLUMN_AGE_ID][fragment.id] = await computeAgeColumnValue(fragment, context);
                    }
                  }
                  // Return the populated cache of the custom column data.
                  return cache[COLUMN_AGE_ID];
                }
              },
              // Add other custom columns here...
            ];
          },
        },
      },
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

async function computeAgeColumnValue(fragment, context) {
  // Various data is available in the sharedContext, such as the AEM host, the IMS token, and the fragment itself.
  
  // Accessing AEM APIs requires the IMS token, which is available in the sharedContext, and also supporting CORS configurations deployed to AEM Author.
  //const aemHost = context.aemHost;
  //const accessToken = context.auth.imsToken;
  
  // Get the user's locale to format the value of the column.
  const locale = context.locale;

  // Compute the value of the column, in this case we are computing the age of the fragment from its last modified date.
  const duration = Duration.fromMillis(Date.now() - fragment.modifiedDate).rescale();

  let largestUnit = {};

  if (duration.years > 0) {
    largestUnit = {years: duration.years};
  } else if (duration.months > 0) {
    largestUnit = {months: duration.months};
  } else if (duration.days > 0) {
    largestUnit = {days: duration.days};
  } else if (duration.hours > 0) {
    largestUnit = {hours: duration.hours};
  } else if (duration.minutes > 0) {
    largestUnit = {minutes: duration.minutes};
  } else if (duration.seconds > 0) {
    largestUnit = {seconds: duration.seconds};
  }

  // Convert the largest unit of the age to human readable format.
  const columnValue = Duration.fromObject(largestUnit, {locale: locale}).toHuman();

  // Return the value of the column.
  return columnValue;
}

export default ExtensionRegistration;
```

#### Données du fragment de contenu

La méthode `render(..)` dans `getColumns()` est transmise par un tableau de fragments. Chaque objet du tableau représente une ligne dans la grille et contient les métadonnées suivantes sur le fragment de contenu. Ces métadonnées peuvent être utilisées pour renseigner les colonnes personnalisées dans la grille.


```javascript
render: async function (fragments) {
    for (const fragment of fragments) {
        // An example value from this console log is displayed below.
        console.log(fragment);
    }
}
```

Exemple de fichier JSON de fragment de contenu disponible en tant qu’élément du paramètre `fragments` dans la méthode `render(..)`.

```json
{
    "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
    "name": "alaskan-adventures",
    "title": "Alaskan Adventures",
    "status": "draft",
    "statusPreview": null,
    "model": {
        "name": "Article",
        "id": "/conf/wknd-shared/settings/dam/cfm/models/article"
    },
    "folderId": "/content/dam/wknd-shared/en/magazine/alaska-adventure",
    "folderName": "Alaska Adventure",
    "createdBy": "admin",
    "createdDate": 1684875665786,
    "modifiedBy": "admin",
    "modifiedDate": 1654104774889,
    "publishedBy": "",
    "publishedDate": 0,
    "locale": "",
    "main": true,
    "translations": {
        "locale": "en",
        "languageCopies": [
            {
                "path": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
                "title": "Alaskan Adventures",
                "locale": "en",
                "model": "Article",
                "status": "DRAFT",
                "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures"
            }
        ]
    },
    "transientStatus": {},
    "extensions": {
        "age": "1 year old"
    }
}
```

Si d’autres données sont requises pour remplir la colonne personnalisée, des requêtes HTTP peuvent être envoyées à l’instance de création AEM pour récupérer les données.

>[!IMPORTANT]
>
> Assurez-vous que l’instance de creation AEM est configurée pour autoriser les [demandes entre origines multiples](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=fr) à partir des origines de l’application AppBuilder en cours d’exécution. Les origines autorisées incluent `https://localhost:9080`, l’origine de l’évaluation AppBuilder et l’origine de production AppBuilder.
>
> L’extension peut également appeler une [action AppBuilder](../../runtime-action.md) personnalisée qui envoie la requête à l’instance de création AEM au nom de l’extension.


```javascript
const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();
...
// Fetch the Content Fragment JSON from AEM Author using the AEM Assets HTTP API
const response = await fetch(`${context.aemHost}${fragment.id.slice('/content/dam'.length)}.json`, {
    headers: {
        'Authorization': `Bearer ${context.auth.imsToken}`
    }
})
...
```

#### Définition des colonnes

Le résultat de la méthode de rendu est un objet JavaScript dont les clés sont le chemin d’accès du fragment de contenu (ou `fragment.id`) et la valeur s’affiche dans la colonne.

Par exemple, les résultats de cette extension pour la colonne `age` sont les suivants :

```json
{
    "/content/dam/wknd-shared/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks": "22 minutes",
    "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp": "1 hour",
    "/content/dam/wknd-shared/en/magazine/western-australia/western-australia-by-camper-van": "1 hour",
    "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/climbing-new-zealand": "10 months",
    "/content/dam/wknd-shared/en/magazine/skitouring/skitouring": "1 year",
    "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland": "1 year",
    "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures": "1 year",
    "/content/dam/wknd-shared/en/magazine/arctic-surfing/aloha-spirits-in-northern-norway": "1 year",
    "/content/dam/wknd-shared/en/magazine/san-diego-surf-spots/san-diego-surfspots": "1 year",
    "/content/dam/wknd-shared/en/magazine/fly-fishing-amazon/fly-fishing": "1 year",
    "/content/dam/wknd-shared/en/adventures/napa-wine-tasting/napa-wine-tasting": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah": "1 year",
    "/content/dam/wknd-shared/en/adventures/gastronomic-marais-tour/gastronomic-marais-tour": "1 year",
    "/content/dam/wknd-shared/en/adventures/tahoe-skiing/tahoe-skiing": "1 year",
    "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica": "1 year",
    "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking": "1 year",
    "/content/dam/wknd-shared/en/adventures/whistler-mountain-biking/whistler-mountain-biking": "1 year",
    "/content/dam/wknd-shared/en/adventures/colorado-rock-climbing/colorado-rock-climbing": "1 year",
    "/content/dam/wknd-shared/en/adventures/ski-touring-mont-blanc/ski-touring-mont-blanc": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany": "1 year",
    "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling": "1 year",
    "/content/dam/wknd-shared/en/adventures/downhill-skiing-wyoming/downhill-skiing-wyoming": "1 year",
    "/content/dam/wknd-shared/en/adventures/riverside-camping-australia/riverside-camping-australia": "1 year",
    "/content/dam/wknd-shared/en/contributors/ian-provo": "1 year",
    "/content/dam/wknd-shared/en/contributors/sofia-sj-berg": "1 year",
    "/content/dam/wknd-shared/en/contributors/justin-barr": "1 year",
    "/content/dam/wknd-shared/en/contributors/jake-hammer": "1 year",
    "/content/dam/wknd-shared/en/contributors/jacob-wester": "1 year",
    "/content/dam/wknd-shared/en/contributors/stacey-roswells": "1 year",
    "/content/dam/wknd-shared/en/contributors/kumar-selveraj": "1 year"
}
```
