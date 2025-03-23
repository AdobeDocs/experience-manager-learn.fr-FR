---
title: Vérification du webhook Github.com
description: Découvrez comment vérifier une requête webhook de Github.com dans une action App Builder.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
exl-id: 5492dc7b-f034-4a7f-924d-79e083349e26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 100%

---

# Vérification du webhook Github.com

Les webhooks vous permettent de créer ou de configurer des intégrations qui s’abonnent à certains événements sur GitHub.com. Lorsque l’un de ces événements est déclenché, GitHub envoie une payload HTTP POST à l’URL configurée du webhook. Cependant, pour des raisons de sécurité, il est important de vérifier que la requête webhook entrante provient bien de GitHub et non d’un acteur malveillant. Ce tutoriel vous guide tout au long des étapes pour vérifier une requête de webhook GitHub.com dans une action App Builder Adobe à l’aide d’un secret partagé.

## Configuration d’un secret Github dans AppBuilder

1. **Ajoutez un secret au fichier `.env` :**

   Dans le fichier `.env` du projet App Builder, ajoutez une clé personnalisée pour le secret de webhook GitHub.com :

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **Mettez à jour le fichier `ext.config.yaml` :**

   Le fichier `ext.config.yaml` doit être mis à jour pour vérifier la requête de webhook GitHub.com.

   - Définissez la configuration de l’action AppBuilder `web` sur `raw` pour recevoir le corps de la requête brute de GitHub.com.
   - Sous `inputs` dans la configuration de l’action AppBuilder, ajoutez la clé `GITHUB_SECRET` en la mappant au champ `.env` contenant le secret. La valeur de cette clé est le nom du champ `.env` précédé du préfixe `$`.
   - Définissez l’annotation `require-adobe-auth` dans la configuration de l’action AppBuilder sur `false` pour permettre l’appel de l’action sans nécessiter d’authentification d’Adobe.

   Le fichier `ext.config.yaml` obtenu doit ressembler à celui-ci :

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## Ajout du code de vérification à l’action AppBuilder

Ajoutez ensuite le code JavaScript fourni ci-dessous (copié à partir de la [documentation de GitHub.com](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)) à votre action AppBuilder. Veillez à exporter la fonction `verifySignature`.

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## Mise en œuvre de la vérification dans l’action AppBuilder

Ensuite, vérifiez que la requête provient de GitHub en comparant la signature dans l’en-tête de la requête à celle générée par la fonction `verifySignature`.

Dans l’action `index.js` d’AppBuilder, ajoutez le code suivant à la fonction `main` :


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## Configuration du webhook dans GitHub

De retour dans GitHub.com, indiquez la même valeur secrète à GitHub.com lors de la création du webhook. Utilisez la valeur secrète spécifiée dans la clé `GITHUB_SECRET` du fichier `.env`.

Dans GitHub.com, accédez aux paramètres du référentiel et modifiez le webhook. Dans les paramètres webhook, indiquez la valeur secrète dans le champ `Secret`. Cliquez sur __Mettre à jour le webhook__ en bas pour enregistrer les modifications.

![Secret de webhook Github](./assets/github-webhook-verification/github-webhook-settings.png)

En suivant ces étapes, vous vous assurez que votre action App Builder peut vérifier en toute sécurité que les requêtes webhook entrantes proviennent bien de votre webhook GitHub.com.
