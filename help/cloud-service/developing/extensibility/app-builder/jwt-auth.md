---
title: Générer un jeton d’accès dans une action Créateur d’applications
description: Découvrez comment générer un jeton d’accès à l’aide des informations d’identification JWT à utiliser dans une action Créateur d’applications.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 161
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '452'
ht-degree: 100%

---

# Générer un jeton d’accès dans une action Créateur d’applications

Les actions Créateur d’applications peuvent avoir besoin d’interagir avec les API d’Adobe associées aux projets Adobe Developer Console sur lesquels l’application Créateur d’applications est déployée.

Cela peut nécessiter que l’action Créateur d’applications génère son propre jeton d’accès associé au projet Adobe Developer Console souhaité.

>[!IMPORTANT]
>
> Référez-vous à la [documentation de sécurité du Créateur d’applications](https://developer.adobe.com/app-builder/docs/guides/security/) pour comprendre quand il est approprié de générer des jetons d’accès plutôt que d’utiliser des jetons d’accès fournis.
>
> L’action personnalisée peut avoir besoin de fournir ses propres contrôles de sécurité pour s’assurer que seuls les consommateurs et consommatrices autorisés peuvent accéder à l’action Créateur d’applications et aux services Adobe en arrière-plan.


## Fichier .env

Dans le fichier `.env` du projet Créateur d’applications, ajoutez des clés personnalisées pour chacune des informations d’identification JWT du projet Adobe Developer Console. Les valeurs des informations d’identification JWT peuvent être obtenues à partir des __Informations d’identification__ > __Compte de service (JWT)__ du projet Adobe Developer Console pour un espace de travail donné.

![Informations d’identification du service JWT Adobe Developer Console](./assets/jwt-auth/jwt-credentials.png).

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

Les valeurs de `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` peuvent être directement copiées à partir de l’écran Informations d’identification JWT du projet Adobe Developer Console.

### Métascopes

Déterminez les API d’Adobe et leurs métascopes avec lesquels l’action Créateur d’applications interagit. Répertoriez les métascopes à l’aide de délimiteurs de virgules dans la clé `JWT_METASCOPES`. Les métascopes valides sont répertoriés dans la [documentation du métascope JWT d’Adobe](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


Par exemple, la valeur suivante peut être ajoutée à la clé `JWT_METASCOPES` dans le fichier `.env` :

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Clé privée

La `JWT_PRIVATE_KEY` doit être spécialement formatée, car il s’agit d’une valeur multiligne native, qui n’est pas prise en charge dans les fichiers `.env`. Le moyen le plus simple est de coder la clé privée en base64. L’encodage de la clé privée en Base64 (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) peut être réalisé à l’aide d’outils natifs fournis par votre système d’exploitation.

>[!BEGINTABS]

>[!TAB macOS]

1. Ouvrez `Terminal`.
1. Exécutez la commande `base64 -i /path/to/private.key | pbcopy`.
1. La sortie en base64 est automatiquement copiée dans le presse-papiers.
1. Collez-la dans `.env` en tant que valeur de la clé correspondante.

>[!TAB Windows]

1. Ouvrez `Command Prompt`.
1. Exécutez la commande `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`.
1. Exécutez la commande `findstr /v CERTIFICATE C:\path\to\encoded-private.key`.
1. Copiez la sortie en base64 dans le presse-papiers.
1. Collez-la dans `.env` en tant que valeur de la clé correspondante.

>[!TAB Linux®]

1. Ouvrez le terminal.
1. Exécutez la commande `base64 private.key`.
1. Copiez la sortie en base64 dans le presse-papiers.
1. Collez-la dans `.env` en tant que valeur de la clé correspondante.

>[!ENDTABS]

Par exemple, la clé privée encodée en base64 suivante peut être ajoutée à la clé `JWT_PRIVATE_KEY` dans le fichier `.env` :

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Mapper des entrées

Avec la valeur d’identification JWT définie dans le fichier `.env`, celles-ci doivent être mappées aux entrées d’action Créateur d’applications pour pouvoir être lues dans l’action elle-même. Pour ce faire, ajoutez des entrées pour chaque variable dans les `inputs` de l’action `ext.config.yaml` au format : `PARAMS_INPUT_NAME: $ENV_KEY`.

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

Les clés définies sous `inputs` sont disponibles sur l’objet `params` fourni à l’action Créateur d’applications.


## Informations d’identification JWT pour le jeton d’accès

Dans l’action Créateur d’applications, les informations d’identification JWT sont disponibles dans l’objet `params` et utilisables par [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) pour générer un jeton d’accès, qui peut à son tour accéder à d’autres API et services Adobe.

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
