---
title: Appeler des API AEM basées sur OpenAPI à l’aide de l’authentification de serveur à serveur OAuth
description: Découvrez comment appeler les API AEM basées sur OpenAPI sur AEM as a Cloud Service à partir d’applications personnalisées à l’aide de l’authentification de serveur à serveur OAuth.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Tutorial
jira: KT-16516
thumbnail: KT-16516.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 8338a905-c4a2-4454-9e6f-e257cb0db97c
source-git-commit: 610fe6fc91a400baa9d7f5d40a6a5c2084f93ed0
workflow-type: tm+mt
source-wordcount: '1687'
ht-degree: 3%

---

# Appeler des API AEM basées sur OpenAPI à l’aide de l’authentification de serveur à serveur OAuth

Découvrez comment appeler les API AEM basées sur OpenAPI sur AEM as a Cloud Service à partir d’applications personnalisées à l’aide de l’authentification _OAuth de serveur à serveur_.

L’authentification de serveur à serveur OAuth est idéale pour les services principaux nécessitant un accès à l’API sans interaction de l’utilisateur. Il utilise le type d’octroi OAuth 2.0 _client_credentials_ pour authentifier l’application cliente.

## Ce que vous apprenez.{#what-you-learn}

Dans ce tutoriel, vous apprendrez à :

- Configurez un projet Adobe Developer Console (ADC) pour accéder à l’API de création Assets à l’aide de l’authentification _OAuth de serveur à serveur_.

- Développez un exemple d’application NodeJS qui appelle l’API de création Assets pour récupérer les métadonnées d’une ressource spécifique.

Avant de commencer, vérifiez les points suivants :

- [Accès aux API Adobe et aux concepts associés](../overview.md#accessing-adobe-apis-and-related-concepts) section.
- Article [Configuration des API AEM basées sur OpenAPI](../setup.md).

## Conditions préalables

Les éléments suivants sont requis afin de terminer ce tutoriel :

- Environnement AEM as a Cloud Service modernisé avec les éléments suivants :
   - Version AEM `2024.10.18459.20241031T210302Z` ou ultérieure.
   - Nouveaux profils de produit de style (si l’environnement a été créé avant novembre 2024)

  Consultez l’article [ Configuration d’API AEM basées sur OpenAPI ](../setup.md) pour plus d’informations.

- L’exemple de projet [Sites WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) doit y être déployé.

- Accès à [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- Installez [Node.js](https://nodejs.org/fr/) sur votre ordinateur local pour exécuter l’exemple d’application NodeJS.

## Étapes de développement

Les étapes de développement générales sont les suivantes :

1. Configurer le projet ADC
   1. Ajouter l’API de création Assets
   1. Configurez sa méthode d’authentification en tant que OAuth de serveur à serveur
   1. Associer le profil de produit à la configuration d’authentification
1. Configuration de l’instance AEM pour activer la communication de projet ADC
1. Développement d’un exemple d’application NodeJS
1. Vérifier le flux de bout en bout

## Configurer le projet ADC

L’étape Configurer le projet ADC est _répétée_ à partir de l’[Configurer les API AEM basées sur OpenAPI](../setup.md). Il est répété pour ajouter l’API de création Assets et configurer sa méthode d’authentification comme OAuth de serveur à serveur.

>[!TIP]
>
>Assurez-vous d’avoir terminé l’étape **Activer l’accès aux API AEM** de l’article [Configurer les API AEM basées sur OpenAPI](../setup.md#enable-aem-apis-access). Sans cela, l’option d’authentification de serveur à serveur n’est pas disponible.


1. Ouvrez le projet souhaité à partir de [Adobe Developer Console](https://developer.adobe.com/console/projects).

1. Pour ajouter des API AEM, cliquez sur le bouton **Ajouter une API**.

   ![Ajouter une API](../assets/s2s/add-api.png)

1. Dans la boîte de dialogue _Ajouter une API_, filtrez par _Experience Cloud_ et sélectionnez la vignette **API de création AEM Assets** puis cliquez sur **Suivant**.

   ![Ajouter une API AEM](../assets/s2s/add-aem-api.png)

1. Ensuite, dans la boîte de dialogue _Configurer l’API_, sélectionnez l’option d’authentification **Serveur à serveur** et cliquez sur **Suivant**. L’authentification de serveur à serveur est idéale pour les services principaux nécessitant un accès à l’API sans interaction de l’utilisateur.

   ![Sélectionner l’authentification](../assets/s2s/select-authentication.png)

1. Renommez les informations d’identification pour une identification plus facile (si nécessaire), puis cliquez sur **Suivant**. À des fins de démonstration, le nom par défaut est utilisé.

   ![Renommer les informations d’identification](../assets/s2s/rename-credential.png)

1. Sélectionnez le profil de produit **Utilisateurs AEM Assets Collaborator - auteur - Programme XXX - Environnement XXX** et cliquez sur **Enregistrer**. Comme vous pouvez le constater, seul le profil de produit associé au service Utilisateurs de l’API AEM Assets peut être sélectionné.

   ![Sélectionner le profil de produit](../assets/s2s/select-product-profile.png)

1. Examinez l’API AEM et la configuration de l’authentification.

   ![Configuration de l’API AEM](../assets/s2s/aem-api-configuration.png)

   ![Configuration de l’authentification](../assets/s2s/authentication-configuration.png)

## Configurer l’instance AEM pour activer la communication de projet ADC

Suivez les instructions de l’article [Configurer les API AEM basées sur OpenAPI](../setup.md#configure-the-aem-instance-to-enable-adc-project-communication) pour configurer l’instance AEM afin d’activer la communication de projet ADC.

## Développement d’un exemple d’application NodeJS

Développons un exemple d’application NodeJS qui appelle l’API de création Assets.

Vous pouvez utiliser d’autres langages de programmation tels que Java, Python, etc., pour développer l’application.

À des fins de test, vous pouvez utiliser [Postman](https://www.postman.com/), [curl](https://curl.se/) ou tout autre client REST pour appeler les API AEM.

### Vérifier l’API

Avant de développer l’application, examinons [diffuser le point d’entrée des métadonnées de la ressource spécifiée](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/../assets/author/#operation/getAssetMetadata) à partir de l’API de création _Assets_. La syntaxe API est la suivante :

```http
GET https://{bucket}.adobeaemcloud.com/adobe/../assets/{assetId}/metadata
```

Pour récupérer les métadonnées d’une ressource spécifique, vous avez besoin des valeurs `bucket` et `assetId`. Le `bucket` est le nom de l’instance AEM sans le nom de domaine Adobe (.adobeaemcloud.com), par exemple `author-p63947-e1420428`.

L’`assetId` est l’UUID JCR de la ressource avec le préfixe `urn:aaid:aem:`, par exemple `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da`. Il existe plusieurs façons d’obtenir le `assetId` :

- Ajoutez l’extension de `.json` Chemin d’accès à la ressource AEM pour obtenir les métadonnées de la ressource. Par exemple, `https://author-p63947-e1420429.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg.json` et recherchez la propriété `jcr:uuid` .

- Vous pouvez également obtenir le `assetId` en examinant la ressource dans l’inspecteur d’éléments du navigateur. Recherchez l’attribut `data-id="urn:aaid:aem:..."` .

  ![Inspecter la ressource](../assets/s2s/inspect-asset.png)

### Appeler l’API à l’aide du navigateur

Avant de développer l’application, appelons l’API à l’aide de la fonctionnalité **Essayer** de la documentation de l’API [](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

1. Ouvrez la documentation de l’API de création [Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) dans le navigateur.

1. Développez la section _Métadonnées_ et cliquez sur l’option **Diffuse les métadonnées de la ressource spécifiée**.

1. Dans le volet de droite, cliquez sur le bouton **Essayer**.
   ![Documentation de l’API](../assets/s2s/api-documentation.png)

1. Saisissez les valeurs suivantes :

   | Section | Paramètre | Valeur  |
   | --- | --- | --- |
   |  | seau | Le nom de l’instance AEM sans le nom de domaine Adobe (.adobeaemcloud.com), par exemple `author-p63947-e1420428`. |
   | **Sécurité** | Jeton du porteur | Utilisez le jeton d’accès à partir des informations d’identification de serveur à serveur OAuth du projet ADC. |
   | **Sécurité** | X-Api-Key | Utilisez la valeur `ClientID` des informations d’identification OAuth de serveur à serveur du projet ADC. |
   | **Paramètres** | assetId | L’identifiant unique de la ressource dans AEM, par exemple `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da` |
   | **Paramètres** | X-Adobe-Accept-Experimental | 1 |

   ![Appeler l’API - Jeton d’accès](../assets/s2s/generate-access-token.png)

   ![Appeler l’API - valeurs d’entrée](../assets/s2s/invoke-api-input-values.png)

1. Cliquez sur **Envoyer** pour appeler l’API et passer en revue la réponse dans l’onglet **Réponse**.

   ![Appeler l’API - réponse](../assets/s2s/invoke-api-response.png)

Les étapes ci-dessus confirment la modernisation de l’environnement AEM as a Cloud Service en permettant l’accès aux API AEM. Cela confirme également la réussite de la configuration du projet ADC et de la communication des informations d’identification de serveur à serveur OAuth ClientID avec l’instance d’auteur AEM.

### Exemple d’application NodeJS

Développons un exemple d’application NodeJS.

Pour développer l’application, vous pouvez utiliser les instructions _Run-the-sample-application_ ou _Step-by-development_.

>[!BEGINTABS]

>[!TAB Exécuter-l’-exemple-d’application]

1. Téléchargez l’exemple de fichier zip de l’application [demo-nodejs-app-to-invoke-aem-openapi](../assets/s2s/demo-nodejs-app-to-invoke-aem-openapi.zip) et extrayez-le.

1. Accédez au dossier extrait et installez les dépendances.

   ```bash
   $ npm install
   ```

1. Remplacez les espaces réservés dans le fichier `.env` par les valeurs réelles des informations d’identification OAuth de serveur à serveur du projet ADC.

1. Remplacez les `<BUCKETNAME>` et les `<ASSETID>` du fichier `src/index.js` par les valeurs réelles.

1. Exécutez l’application NodeJS.

   ```bash
   $ node src/index.js
   ```

>[!TAB Développement détaillé]

1. Créez un projet NodeJS.

   ```bash
   $ mkdir demo-nodejs-app-to-invoke-aem-openapi
   $ cd demo-nodejs-app-to-invoke-aem-openapi
   $ npm init -y
   ```

1. Installez la bibliothèque _fetch_ et _dotenv_ pour effectuer respectivement des requêtes HTTP et lire les variables d’environnement.

   ```bash
   $ npm install node-fetch
   $ npm install dotenv
   ```

1. Ouvrez le projet dans votre éditeur de code préféré et mettez à jour le fichier `package.json` pour ajouter le `type` à `module`.

   ```json
   {
       ...
       "version": "1.0.0",
       "type": "module",
       "main": "index.js",
       ...
   }
   ```

1. Créez `.env` fichier et ajoutez la configuration suivante. Remplacez les espaces réservés par les valeurs réelles des informations d’identification de serveur à serveur OAuth du projet ADC.

   ```properties
   CLIENT_ID=<ADC Project OAuth Server-to-Server credential ClientID>
   CLIENT_SECRET=<ADC Project OAuth Server-to-Server credential Client Secret>
   SCOPES=<ADC Project OAuth Server-to-Server credential Scopes>
   ```

1. Créez `src/index.js` fichier et ajoutez le code suivant, puis remplacez les `<BUCKETNAME>` et `<ASSETID>` par les valeurs réelles.

   ```javascript
   // Import the dotenv configuration to load environment variables from the .env file
   import "dotenv/config";
   
   // Import the fetch function to make HTTP requests
   import fetch from "node-fetch";
   
   // REPLACE THE FOLLOWING VALUES WITH YOUR OWN
   const bucket = "<BUCKETNAME>"; // Bucket name is the AEM instance name (e.g. author-p63947-e1420428)
   const assetId = "<ASSETID>"; // Asset ID is the unique identifier for the asset in AEM (e.g. urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da). You can get it by inspecting the asset in browser's element inspector, look for data-id="urn:aaid:aem:..."
   
   // Load environment variables for authentication
   const clientId = process.env.CLIENT_ID; // Adobe IMS client ID
   const clientSecret = process.env.CLIENT_SECRET; // Adobe IMS client secret
   const scopes = process.env.SCOPES; // Scope for the API access
   
   // Adobe IMS endpoint for obtaining an access token
   const adobeIMSV3TokenEndpointURL =
   "https://ims-na1.adobelogin.com/ims/token/v3";
   
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
       console.log("Getting access token from IMS"); // Log process initiation
       //console.log("Client ID: " + clientId); // Display client ID for debugging purposes
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       //console.log("Response status: " + response.status); // Log the HTTP status for debugging
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       console.log("Access token received"); // Log success message
   
       // Return the access token
       return responseJSON.access_token;
   };
   
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/../assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   
   // Call the getAssets function to start the process
   getAssetMetadat();
   ```

1. Exécutez l’application NodeJS.

   ```bash
   $ node src/index.js
   ```

>[!ENDTABS]

### Réponse de l’API

Une fois l’exécution réussie, la réponse de l’API s’affiche dans la console. La réponse contient les métadonnées de la ressource spécifiée.

```json
{
  "assetId": "urn:aaid:aem:9c09ff70-9ee8-4b14-a5fa-ec37baa0d1b3",
  "assetMetadata": {    
    ...
    "dc:title": "A Young Mountain Biking Couple Takes A Minute To Take In The Scenery",
    "xmp:CreatorTool": "Adobe Photoshop Lightroom Classic 7.5 (Macintosh)",
    ...
  },
  "repositoryMetadata": {
    ...
    "repo:name": "adobestock-221043703.jpg",
    "repo:path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg",
    "repo:state": "ACTIVE",
    ...
  }
}
```

Félicitations. Vous avez correctement appelé les API AEM basées sur OpenAPI à partir de votre application personnalisée à l’aide de l’authentification de serveur à serveur OAuth.

### Vérifier le code de l’application

Les principales légendes de l’exemple de code d’application NodeJS sont les suivantes :

1. **Authentification IMS** : récupère un jeton d’accès à l’aide des informations d’identification OAuth de serveur à serveur configurées dans le projet ADC.

   ```javascript
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token from Adobe IMS token endpoint https://ims-na1.adobelogin.com/ims/token/v3
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       // Return the access token
       return responseJSON.access_token;
   };
   ...
   ```

1. **Appel de l’API** : appelle l’API de création Assets pour récupérer les métadonnées d’une ressource spécifique en fournissant le jeton d’accès pour l’autorisation.

   ```javascript
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/../assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   ...
   ```

## Ce qui se passe.

Une fois l’appel de l’API réussi, un utilisateur représentant les informations d’identification OAuth de projet ADC est créé dans le service de création AEM, avec les groupes d’utilisateurs correspondant à la configuration du profil de produit et des services . Le _utilisateur du compte technique_ est associé au groupe d’utilisateurs Profil de produit et _Services_, qui dispose des autorisations nécessaires pour _LIRE_ les métadonnées de la ressource.

Pour vérifier la création de l’utilisateur du compte technique et du groupe d’utilisateurs, procédez comme suit :

- Dans le projet ADC, accédez à la configuration des informations d’identification **OAuth de serveur à serveur**. Notez la valeur **E-mail du compte technique**.

  ![E-mail du compte technique](../assets/s2s/technical-account-email.png)

- Dans le service de création AEM, accédez à **Outils** > **Sécurité** > **Utilisateurs** et recherchez la valeur **E-mail du compte technique**.

  ![Utilisateur du compte technique](../assets/s2s/technical-account-user.png)

- Cliquez sur l’utilisateur du compte technique pour afficher les détails de l’utilisateur, tels que l’abonnement **Groupes**. Comme illustré ci-dessous, l’utilisateur du compte technique est associé aux groupes d’utilisateurs **Utilisateurs de l’espace de travail AEM Assets - auteur - Programme XXX - Environnement XXX** et **Utilisateurs de l’espace de travail AEM Assets - Service**.

  ![Abonnement de l’utilisateur du compte technique](../assets/s2s/technical-account-user-membership.png)

- Notez que l’utilisateur du compte technique est associé au profil de produit **Utilisateurs AEM Assets Collaborator - auteur - Programme XXX - Environnement XXX**. Le profil de produit est associé aux **utilisateurs de l’API AEM Assets** et aux **utilisateurs du collaborateur AEM Assets** services.

  ![ Profil de produit de l’utilisateur du compte technique ](../assets/s2s/technical-account-user-product-profile.png)

- Le profil de produit et l’association des utilisateurs et utilisatrices du compte technique peuvent être vérifiés dans l’onglet **Profils de produit** **Informations d’identification de l’API**.

  ![Informations d’identification de l’API Profil de produit](../assets/s2s/product-profile-api-credentials.png)

## Erreur 403 pour les requêtes non GET

Pour _LIRE_ les métadonnées de la ressource, l’utilisateur du compte technique créé pour les informations d’identification OAuth de serveur à serveur dispose des autorisations nécessaires via le groupe d’utilisateurs Services (par exemple, Utilisateurs AEM Assets Collaborator - Service).

Toutefois, pour _Créer, mettre à jour, supprimer_ (CUD) les métadonnées de la ressource, l’utilisateur du compte technique a besoin d’autorisations supplémentaires. Vous pouvez le vérifier en appelant l’API avec une requête non GET (par exemple, PATCH, DELETE) et en observant la réponse d’erreur 403.

Appelons la requête _PATCH_ pour mettre à jour les métadonnées de la ressource et observons la réponse d’erreur 403.

- Ouvrez la documentation de l’API de création [Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) dans le navigateur.

- Saisissez les valeurs suivantes :

  | Section | Paramètre | Valeur  |
  | --- | --- | --- |
  | **compartiment** |  | Le nom de l’instance AEM sans le nom de domaine Adobe (.adobeaemcloud.com), par exemple `author-p63947-e1420428`. |
  | **Sécurité** | Jeton du porteur | Utilisez le jeton d’accès à partir des informations d’identification de serveur à serveur OAuth du projet ADC. |
  | **Sécurité** | X-Api-Key | Utilisez la valeur `ClientID` des informations d’identification OAuth de serveur à serveur du projet ADC. |
  | **Corps** |  | `[{ "op": "add", "path": "foo","value": "bar"}]` |
  | **Paramètres** | assetId | L’identifiant unique de la ressource dans AEM, par exemple `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da` |
  | **Paramètres** | X-Adobe-Accept-Experimental | * |
  | **Paramètres** | X-Adobe-Accept-Experimental | 1 |

- Cliquez sur **Envoyer** pour appeler la requête _PATCH_ et observer la réponse d’erreur 403.

  ![Appeler l’API - Requête PATCH](../assets/s2s/invoke-api-patch-request.png)

Pour corriger l’erreur 403, vous disposez de deux options :

- Dans le projet ADC, mettez à jour le profil de produit associé aux informations d’identification de serveur à serveur OAuth avec un profil de produit approprié disposant des autorisations nécessaires pour _Créer, mettre à jour, supprimer_ (CUD) les métadonnées de la ressource, par exemple **Administrateurs AEM - auteur - Programme XXX - Environnement XXX**. Pour plus d’informations, consultez l’article [ Comment : informations d’identification connectées de l’API et gestion des profils de produit ](../how-to/credentials-and-product-profile-management.md).

- À l’aide du projet AEM, mettez à jour les autorisations du groupe d’utilisateurs du service AEM associé (par exemple, Utilisateurs de l’espace de travail AEM Assets - Service) dans l’instance de création AEM pour autoriser _Créer, Mettre à jour, Supprimer_ (CUD) des métadonnées de la ressource. Pour plus d’informations, voir l’article [Comment - Gestion des autorisations des groupes d’utilisateurs d’AEM Service ](../how-to/services-user-group-permission-management.md).

## Résumé

Dans ce tutoriel, vous avez appris à appeler des API AEM basées sur OpenAPI à partir d’applications personnalisées. Vous avez activé l’accès aux API AEM, créé et configuré un projet Adobe Developer Console (ADC).
Dans le projet ADC, vous avez ajouté les API AEM, configuré son type d’authentification et associé le profil de produit. Vous avez également configuré l’instance AEM pour activer la communication de projet ADC et développé un exemple d’application NodeJS qui appelle l’API de création Assets.

## Ressources supplémentaires

- [Guide de mise en œuvre des informations d’identification OAuth de serveur à serveur](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/)
