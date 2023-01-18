---
title: Générer un jeton d’accès dans l’action App Builder
description: Découvrez comment générer un jeton d’accès à l’aide des informations d’identification JWT à utiliser dans une action App Builder.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
kt: 11743
last-substantial-update: 2023-01-17T00:00:00Z
source-git-commit: 0990fc230e2a36841380b5b0c6cd94dca24614fa
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 2%

---


# Générer un jeton d’accès dans l’action App Builder

Les actions du créateur d’applications peuvent avoir besoin d’interagir avec les API d’Adobe associées aux projets de la console Adobe Developer sur lesquels l’application du créateur d’applications est déployée.

Cela peut nécessiter que l’action App Builder génère son propre jeton d’accès associé au projet de console Adobe Developer souhaité.

>[!IMPORTANT]
>
> Réviser [Documentation de sécurité d’App Builder](https://developer.adobe.com/app-builder/docs/guides/security/) pour comprendre quand il est approprié de générer des jetons d’accès plutôt que d’utiliser des jetons d’accès fournis.
>
> L’action personnalisée peut nécessiter de fournir ses propres contrôles de sécurité pour s’assurer que seuls les clients autorisés peuvent accéder à l’action App Builder et aux services Adobe qui la sous-tendent.


## fichier .env

Dans le fichier du projet App Builder `.env` , ajoutez des clés personnalisées pour chacune des informations d’identification JWT du projet Adobe Developer Console. Les valeurs d’informations d’identification JWT peuvent être obtenues à partir du __Informations d’identification__ > __Compte de service (JWT)__ pour un espace de travail donné.

![Informations d’identification du service JWT de la console Adobe Developer](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

Les valeurs de `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` peut être directement copié à partir de l’écran Informations d’identification JWT du projet Adobe Developer Console.

### Métascopes

Déterminez les API d’Adobe et leurs métadonnées avec lesquelles l’action App Builder interagit. Répertorier les métadonnées à l’aide de délimiteurs de virgules dans la variable `JWT_METASCOPES` clé. Les métascopes valides sont répertoriés dans la section [Documentation du métascope JWT d’Adobe](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


Par exemple, la valeur suivante peut être ajoutée à la variable `JWT_METASCOPES` dans la `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Clé privée

Le `JWT_PRIVATE_KEY` doit être spécialement formaté, car il s’agit d’une valeur multi-lignes native, qui n’est pas prise en charge dans `.env` fichiers . Le moyen le plus simple est de coder la clé privée base64. Base64 encodant la clé privée (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) peut être réalisé à l’aide d’outils natifs fournis par votre système d’exploitation.

>[!BEGINTABS]

>[!TAB macOS]

1. Ouvrez `Terminal`
1. `$ base64 -i /path/to/private.key | pbcopy`

La sortie base64 est automatiquement copiée dans le presse-papiers.

>[!TAB Windows]



1. Ouvrez `Command Prompt`
1. `$ certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. Copiez le contenu de `encoded-private.key` dans le presse-papiers ;

>[!TAB Linux®]

1. Ouvrir le terminal
1. `$ base64 private.key`
1. Copiez la sortie base64 dans le Presse-papiers.

>[!ENDTABS]

Par exemple, la valeur suivante peut être ajoutée à la variable `JWT_PRIVATE_KEY` dans la `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Configuration de l’extension

Avec la valeur d’identification JWT définie dans la variable `.env` , ils doivent être mappés aux entrées d’action AppBuilder pour pouvoir être lus dans l’action elle-même. Pour ce faire, ajoutez des entrées pour chaque variable dans la variable `ext.config.yaml` action `inputs` au format : `INPUT_NAME=$ENV_KEY`.

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
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

Les clés définies sous `inputs` sont disponibles sur la `params` fournie à l’action App Builder.


## Conversion des informations d’identification JWT en jeton d’accès

Dans l’action App Builder, les informations d’identification JWT sont disponibles dans la variable `params` et utilisable par [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) pour générer un jeton d’accès, qui peut à son tour accéder à d’autres API et services Adobe.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
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
