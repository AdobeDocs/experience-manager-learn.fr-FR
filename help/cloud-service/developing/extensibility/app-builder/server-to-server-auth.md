---
title: Générer un jeton d’accès serveur à serveur dans l’action App Builder
description: Découvrez comment générer un jeton d’accès à l’aide des informations d’identification OAuth Server-to-Server à utiliser dans une action App Builder.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: null
source-git-commit: c77dd9c2872e7e43863d83837cedbff50a7d3c1a
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 8%

---

# Générer un jeton d’accès serveur à serveur dans l’action App Builder

Les actions du créateur d’applications peuvent nécessiter une interaction avec les API d’Adobe qui prennent en charge **Identifiants OAuth serveur à serveur** et sont associés aux projets Adobe Developer Console que l’application App Builder est déployée.

Ce guide explique comment générer un jeton d’accès à l’aide de _Identifiants OAuth serveur à serveur_ à utiliser dans une action du créateur d’applications.

>[!IMPORTANT]
>
> Les informations d’identification du compte de service (JWT) ont été abandonnées au profit des informations d’identification OAuth serveur à serveur. Cependant, certaines API d’Adobe prennent toujours en charge uniquement les informations d’identification du compte de service (JWT) et la migration vers OAuth Server-to-Server est en cours. Consultez la documentation de l’API d’Adobe pour savoir quelles informations d’identification sont prises en charge.

## Configurations de projet de la console Adobe Developer

Lors de l’ajout de l’API d’Adobe souhaitée au projet de console Adobe Developer, dans la variable _Configuration de l’API_ , sélectionnez la **OAuth serveur à serveur** type d’authentification.

![Console Adobe Developer - OAuth serveur à serveur](./assets/s2s-auth/oauth-server-to-server.png)

Pour attribuer le compte de service d’intégration créé automatiquement ci-dessus, sélectionnez le profil de produit souhaité. Par conséquent, via le profil de produit, les autorisations du compte de service sont contrôlées.

![Console Adobe Developer - Profil du produit](./assets/s2s-auth/select-product-profile.png)

## Fichier .env

Dans le fichier du projet App Builder `.env` , ajoutez des clés personnalisées pour les informations d’identification OAuth Server-to-Server du projet Console Adobe Developer. Les valeurs d’informations d’identification OAuth Server-to-Server peuvent être obtenues à partir du __Informations d’identification__ > __OAuth serveur à serveur__ pour un espace de travail donné.

![Informations d’identification de serveur à serveur OAuth de la console Adobe Developer](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

Les valeurs de `OAUTHS2S_CLIENT_ID`, `OAUTHS2S_CLIENT_SECRET`, `OAUTHS2S_CECREDENTIALS_METASCOPES` peut être directement copié à partir de l’écran Informations d’identification OAuth Server-to-Server du projet Console Adobe Developer.

## Mapper des entrées

Avec la valeur d’identification OAuth Server-to-Server définie dans la variable `.env` , ils doivent être mappés aux entrées d’action AppBuilder afin qu’ils puissent être lus dans l’action elle-même. Pour ce faire, ajoutez des entrées pour chaque variable dans les `inputs` de l’action `ext.config.yaml` au format : `PARAMS_INPUT_NAME: $ENV_KEY`.

Par exemple :

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

Les clés définies sous `inputs` sont disponibles sur l’objet `params` fourni à l’action Créateur d’applications.

## Identifiants OAuth serveur à serveur pour accéder au jeton

Dans l’action App Builder, les informations d’identification OAuth serveur à serveur sont disponibles dans la variable `params` . Grâce à ces informations d’identification, le jeton d’accès peut être généré à l’aide de [Bibliothèques OAuth 2.0](https://oauth.net/code/). Vous pouvez également utiliser la variable [Bibliothèque de récupération des noeuds](https://www.npmjs.com/package/node-fetch) pour envoyer une requête de POST au point de terminaison du jeton Adobe IMS afin d’obtenir le jeton d’accès.

L’exemple suivant montre comment utiliser la variable `node-fetch` pour envoyer une requête de POST au point de terminaison du jeton Adobe IMS afin d’obtenir le jeton d’accès.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = responseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const res = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
