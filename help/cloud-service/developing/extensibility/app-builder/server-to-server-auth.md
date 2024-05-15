---
title: Générer un jeton d’accès serveur à serveur dans une action Créateur d’applications
description: Découvrez comment générer un jeton d’accès à l’aide des informations d’identification OAuth serveur à serveur pour l’utiliser dans une action Créateur d’applications.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: 122
exl-id: 919cb9de-68f8-4380-940a-17274183298f
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 100%

---

# Générer un jeton d’accès serveur à serveur dans une action Créateur d’applications

Les actions Créateur d’applications peuvent avoir besoin d’interagir avec les API d’Adobe qui prennent en charge les **informations d’identification OAuth serveur à serveur** et qui sont associées aux projets Adobe Developer Console sur lesquels l’application Créateur d’applications est déployée.

Découvrez comment générer un jeton d’accès à l’aide des _informations d’identification OAuth serveur à serveur_ à utiliser dans une action Créateur d’applications.

>[!IMPORTANT]
>
> Les informations d’identification du compte de service (JWT), ont été rendues obsolètes au profit des informations d’identification OAuth serveur à serveur. Cependant, certaines API Adobe ne prennent toujours en charge que les informations d’identification du compte de service (JWT) et la migration vers OAuth serveur à serveur est en cours. Consultez la documentation des API Adobe pour comprendre quelles informations d’identification sont prises en charge.

## Configurations du projet Adobe Developer Console

Lors de l’ajout de l’API Adobe souhaitée au projet Adobe Developer Console, dans l’étape _Configurer l’API_, sélectionnez le type d’authentification **OAuth serveur à serveur**.

![Adobe Developer Console - OAuth serveur à serveur](./assets/s2s-auth/oauth-server-to-server.png)

Pour attribuer le compte de service d’intégration créé automatiquement ci-dessus, sélectionnez le profil de produit souhaité. Ainsi via le profil produit, les autorisations du compte de service sont contrôlées.

![Adobe Developer Console - profil produit](./assets/s2s-auth/select-product-profile.png)

## Fichier .env

Dans le fichier `.env` du projet Créateur d’applications, ajoutez des clés personnalisées pour chacune des informations d’identification OAuth serveur à serveur du projet Adobe Developer Console. Les valeurs des informations d’identification OAuth serveur à serveur peuvent être obtenues à partir des __Informations d’identification__ > __OAuth serveur à serveur__ du projet Adobe Developer Console pour un espace de travail donné.

![Informations d’identification OAuth serveur à serveur Adobe Developer Console](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

Les valeurs de `OAUTHS2S_CLIENT_ID`, `OAUTHS2S_CLIENT_SECRET` et `OAUTHS2S_CECREDENTIALS_METASCOPES` peuvent être directement copiées à partir de l’écran Informations d’identification OAuth serveur à serveur du projet Adobe Developer Console.

## Mapper des entrées

Avec la valeur d’identification OAuth serveur à serveur définie dans le fichier `.env`, celles-ci doivent être mappées aux entrées d’action Créateur d’applications pour pouvoir être lues dans l’action elle-même. Pour ce faire, ajoutez des entrées pour chaque variable dans les `inputs` de l’action `ext.config.yaml` au format : `PARAMS_INPUT_NAME: $ENV_KEY`.

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

## Informations d’identification OAuth serveur à serveur pour le jeton d’accès

Dans l’action Créateur d’applications, les informations d’identification OAuth serveur à serveur sont disponibles dans l’objet `params`. En utilisant ces informations d’identification, le jeton d’accès peut être généré en utilisant les [bibliothèques OAuth 2.0](https://oauth.net/code/). Vous pouvez également utiliser la [bibliothèque de récupération de nœuds](https://www.npmjs.com/package/node-fetch) pour envoyer une requête POST au point d’entrée du jeton Adobe IMS afin d’obtenir le jeton d’accès.

L’exemple suivant montre comment utiliser la bibliothèque `node-fetch` pour envoyer une requête POST au point d’entrée du jeton Adobe IMS afin d’obtenir le jeton d’accès.

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
    const tokenResponse = await fetch(adobeIMSV3TokenEndpointURL, options);
    const tokenResponseJSON = await tokenResponse.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = tokenResponseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const aemDataResponse = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!aemDataResponse.ok) { throw new Error("Request to API failed with status code " + aemDataResponse.status);}

    // API data
    let data = await aemDataResponse.json();

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
