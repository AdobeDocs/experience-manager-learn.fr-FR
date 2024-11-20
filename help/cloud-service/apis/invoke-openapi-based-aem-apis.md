---
title: Comment appeler les API d’AEM OpenAPI
description: Découvrez comment appeler des API AEM OpenAPI à partir de votre application.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Tutorial
jira: KT-16516
thumbnail: KT-16516.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
source-git-commit: 6b8a8dc5cdcddfa2d8572bfd195bc67906882f67
workflow-type: tm+mt
source-wordcount: '1751'
ht-degree: 1%

---


# Comment appeler les API d’AEM OpenAPI{#invoke-openapi-based-aem-apis}

Découvrez comment appeler des API d’AEM OpenAPI sur AEM as a Cloud Service à partir d’applications personnalisées.

>[!AVAILABILITY]
>
>Les API d’AEM OpenAPI sont disponibles dans le cadre d’un programme d’accès anticipé. Si vous souhaitez y accéder, nous vous encourageons à envoyer un email [aem-apis@adobe.com](mailto:aem-apis@adobe.com) avec une description de votre cas d’utilisation.

Dans ce tutoriel, vous apprenez à :

- Activez l’accès aux API d’AEM OpenAPI pour votre environnement AEM as a Cloud Service.
- Créez et configurez un projet Adobe Developer Console (ADC) pour accéder aux API AEM à l’aide de l’authentification OAuth serveur à serveur.
- Développez un exemple d’application NodeJS qui appelle l’API d’auteur Assets pour récupérer les métadonnées d’une ressource spécifique.

Avant de commencer, veillez à consulter la section [Accès aux API d’Adobe et aux concepts associés](overview.md#accessing-adobe-apis-and-related-concepts) .

## Conditions préalables

Les éléments suivants sont requis afin de terminer ce tutoriel :

- Environnement AEM as a Cloud Service modernisé avec les éléments suivants :
   - AEM version `2024.10.18459.20241031T210302Z` ou ultérieure.
   - Nouveau style de profils de produit (si l’environnement a été créé avant novembre 2024)

- L’exemple de projet [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) doit être déployé sur celui-ci.

- Accès à [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- Installez [Node.js](https://nodejs.org/fr/) sur votre ordinateur local pour exécuter l’exemple d’application NodeJS.

## Étapes de développement

Les étapes de développement générales sont les suivantes :

1. Modernisation de l’environnement AEM as a Cloud Service.
1. Activez l’accès aux API d’AEM.
1. Créez un projet Adobe Developer Console (ADC).
1. Configuration du projet ADC
   1. Ajout des API d’AEM souhaitées
   1. Configurer son authentification
   1. Association du profil de produit à la configuration de l’authentification
1. Configuration de l’instance AEM pour activer la communication du projet ADC
1. Développement d’un exemple d’application NodeJS
1. Vérification du flux de bout en bout

## Modernisation de l’environnement AEM as a Cloud Service

Commençons par moderniser l’environnement AEM as a Cloud Service. Cette étape n’est nécessaire que si l’environnement n’est pas modernisé.

La modernisation de l&#39;environnement AEM as a Cloud Service est un processus en deux étapes,

- Mise à jour vers la dernière version AEM
- Ajoutez-y de nouveaux profils de produit.

### Mise à jour de l’instance AEM

Pour mettre à jour l’instance AEM, dans la section _Environments_ de l’Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/), sélectionnez l’icône _ellipsis_ en regard du nom de l’environnement et sélectionnez l’option **Mettre à jour** .

![Mettre à jour l’instance AEM](assets/update-aem-instance.png)

Cliquez ensuite sur le bouton **Submit** et exécutez le pipeline Fullstack suggéré.

![Sélectionner la dernière version AEM version](assets/select-latest-aem-release.png)

Dans mon cas, le nom du pipeline Fullstack est _Dev :: Fullstack-Deploy_ et le nom de l’environnement d’AEM est _wknd-program-dev_, il peut varier dans votre cas.

### Ajouter de nouveaux profils de produit

Pour ajouter de nouveaux profils de produit à l’instance AEM, dans la section _Environments_ de l’Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/), sélectionnez l’icône _ellipsis_ en regard du nom de l’environnement et sélectionnez l’option **Ajouter des profils de produit**.

![Ajouter de nouveaux profils de produit](assets/add-new-product-profiles.png)

Vous pouvez passer en revue les profils de produit nouvellement ajoutés en cliquant sur l’icône _ellipsis_ en regard du nom de l’environnement et en sélectionnant **Gérer l’accès** > **Profils d’auteur**.

La fenêtre _Admin Console_ affiche les profils de produit nouvellement ajoutés.

![Vérification de nouveaux profils de produit](assets/review-new-product-profiles.png)

Les étapes ci-dessus terminent la modernisation de l’environnement AEM as a Cloud Service.

## Activation de l’accès aux API d’AEM

Les nouveaux profils de produit permettent l’accès aux API d’AEM OpenAPI dans Adobe Developer Console (ADC).

Les profils de produit nouvellement ajoutés sont associés aux _Services_ qui représentent AEM groupes d’utilisateurs avec des listes de contrôle d’accès prédéfinies (ACL). Les _Services_ sont utilisés pour contrôler le niveau d’accès aux API AEM.

Vous pouvez également sélectionner ou désélectionner les _Services_ associés au profil de produit pour réduire ou augmenter le niveau d’accès.

Vérifiez l’association en cliquant sur l’icône _Afficher les détails_ en regard du nom du profil du produit.

![Services de révision associés au profil de produit](assets/review-services-associated-with-product-profile.png)

Par défaut, le service **Utilisateurs de l’API AEM Assets** n’est associé à aucun profil de produit. Associons-le au tout nouveau **AEM Administrateurs - Auteur - Programme XXX - Environnement XXX** Profil produit. Après cette association, l’ _API d’auteur de ressources_ du projet ADC peut configurer l’authentification OAuth serveur à serveur et associer le compte d’authentification au profil de produit.

![Associer le service utilisateurs de l’API AEM Assets au profil de produit](assets/associate-aem-assets-api-users-service-with-product-profile.png)

Il est important de noter qu’avant la modernisation, dans l’instance d’auteur AEM, deux profils de produit étaient disponibles, **AEM Administrateurs-XXX** et **Utilisateurs-XXX**. Il est également possible d’associer ces profils de produit existants aux nouveaux services.

## Créer un projet Adobe Developer Console (ADC)

Créez ensuite un projet ADC pour accéder aux API AEM.

1. Connectez-vous à [Adobe Developer Console](https://developer.adobe.com/console) à l’aide de votre Adobe ID.

   ![Adobe Developer Console](assets/adobe-developer-console.png)

1. Dans la section _Démarrage rapide_ , cliquez sur le bouton **Créer un projet** .

   ![Créer un projet](assets/create-new-project.png)

1. Il crée un projet portant le nom par défaut.

   ![Nouveau projet créé](assets/new-project-created.png)

1. Modifiez le nom du projet en cliquant sur le bouton **Modifier le projet** dans le coin supérieur droit. Attribuez un nom significatif et cliquez sur **Enregistrer**.

   ![Modifier le nom du projet](assets/edit-project-name.png)

## Configuration du projet ADC

Ensuite, configurez le projet ADC pour ajouter AEM API, configurer son authentification et associer le profil de produit.

1. Pour ajouter des API AEM, cliquez sur le bouton **Ajouter une API**.

   ![Ajouter une API](assets/add-api.png)

1. Dans la boîte de dialogue _Ajouter une API_, filtrez par _Experience Cloud_ et sélectionnez la carte **AEM Assets Author API** et cliquez sur **Suivant**.

   ![Ajouter une API AEM](assets/add-aem-api.png)

1. Ensuite, dans la boîte de dialogue _Configurer l&#39;API_, sélectionnez l&#39;option d&#39;authentification **Serveur à serveur** et cliquez sur **Suivant**. L’authentification serveur à serveur est idéale pour les services principaux nécessitant un accès à l’API sans interaction de l’utilisateur.

   ![Sélectionner l’authentification](assets/select-authentication.png)

1. Renommez les informations d’identification pour une identification plus facile (si nécessaire) et cliquez sur **Suivant**. À des fins de démonstration, le nom par défaut est utilisé.

   ![Renommer credential](assets/rename-credential.png)

1. Sélectionnez le profil de produit **AEM Administrateurs - Auteur - Programme XXX - Environnement XXX** et cliquez sur **Enregistrer**. Comme vous pouvez le constater, seul le profil de produit associé au service Utilisateurs de l’API AEM Assets peut être sélectionné.

   ![Sélectionner le profil de produit](assets/select-product-profile.png)

1. Passez en revue l’API AEM et la configuration de l’authentification.

   ![AEM configuration de l’API](assets/aem-api-configuration.png)

   ![Configuration de l’authentification](assets/authentication-configuration.png)


## Configuration de l’instance AEM pour activer la communication du projet ADC

Pour activer l’ID d’identification client OAuth du projet ADC pour communiquer avec l’instance AEM, vous devez configurer l’instance AEM.

Pour ce faire, définissez la configuration dans le fichier `config.yaml` du projet AEM. Déployez ensuite le fichier `config.yaml` à l’aide du pipeline de configuration dans Cloud Manager.

1. Dans AEM Projet, recherchez ou créez le fichier `config.yaml` à partir du dossier `config`.

   ![Localisation de la configuration YAML](assets/locate-config-yaml.png)

1. Ajoutez la configuration suivante au fichier `config.yaml`.

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's OAuth Server-to-Server credential ClientID>"
   ```

   Remplacez `<ADC Project's OAuth Server-to-Server credential ClientID>` par l’ID client réel des informations d’identification OAuth Server-to-Server du projet ADC. Le point de terminaison d’API utilisé dans ce tutoriel est disponible uniquement sur le niveau auteur, mais pour les autres API, la configuration yaml peut également avoir un noeud _publish_ ou _preview_ .

1. Validez les modifications de configuration dans le référentiel Git et poussez les modifications dans le référentiel distant.

1. Déployez les modifications ci-dessus à l’aide du pipeline de configuration dans Cloud Manager. Notez que le fichier `config.yaml` peut également être installé dans un RDE, à l’aide de l’outil de ligne de commande.

   ![Déployer config.yaml](assets/config-pipeline.png)

## Développement d’un exemple d’application NodeJS

Développons un exemple d’application NodeJS qui appelle l’API d’auteur Assets.

Vous pouvez utiliser d’autres langages de programmation tels que Java, Python, etc., pour développer l’application.

À des fins de test, vous pouvez utiliser [Postman](https://www.postman.com/), [curl](https://curl.se/) ou tout autre client REST pour appeler les API AEM.

### Vérification de l’API

Avant de développer l’application, examinons la [diffusion du point de terminaison de métadonnées](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata) de la ressource spécifiée à partir de l’ _API d’auteur Assets_. La syntaxe de l’API est la suivante :

```http
GET https://{bucket}.adobeaemcloud.com/adobe/assets/{assetId}/metadata
```

Pour récupérer les métadonnées d’une ressource spécifique, vous avez besoin des valeurs `bucket` et `assetId` . `bucket` est le nom de l’instance AEM sans le nom de domaine de l’Adobe (.adobeaemcloud.com), par exemple, `author-p63947-e1420428`.

`assetId` est l’UUID JCR de la ressource avec le préfixe `urn:aaid:aem:`, par exemple `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da`. Il existe plusieurs méthodes pour obtenir le `assetId` :

- Ajoutez l’extension AEM chemin d’accès aux ressources `.json` pour obtenir les métadonnées des ressources. Par exemple, `https://author-p63947-e1420429.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg.json` et recherchez la propriété `jcr:uuid`.

- Vous pouvez également obtenir le `assetId` en examinant la ressource dans l’Inspecteur d’éléments du navigateur. Recherchez l’attribut `data-id="urn:aaid:aem:..."` .

  ![Ressource Inspect](assets/inspect-asset.png)

### Appel de l’API à l’aide du navigateur

Avant de développer l’application, appelez l’API à l’aide de la fonction **Essayer** de la [documentation de l’API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata).

1. Ouvrez la [documentation de l’API d’auteur Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author) dans le navigateur.

1. Développez la section _Métadonnées_ et cliquez sur l’option **Diffuse les métadonnées de la ressource spécifiée** .

1. Dans le volet de droite, cliquez sur le bouton **Essayer** .
   ![Documentation de l’API](assets/api-documentation.png)

1. Saisissez les valeurs suivantes :
   1. La valeur `bucket` est le nom de l’instance AEM sans le nom de domaine de l’Adobe (.adobeaemcloud.com), par exemple, `author-p63947-e1420428`.

   1. Les valeurs **Security** associées `Bearer Token` et `X-Api-Key` sont obtenues à partir des informations d’identification OAuth Server-to-Server du projet ADC. Cliquez sur **Générer le jeton d’accès** pour obtenir la valeur `Bearer Token` et utiliser la valeur `ClientID` comme `X-Api-Key`.
      ![Générer un jeton d’accès](assets/generate-access-token.png)

   1. La valeur **Parameters** associée `assetId` est l’identifiant unique de la ressource dans AEM. `X-Adobe-Accept-Experimental` est défini sur 1.

      ![Invoke API - valeurs d’entrée](assets/invoke-api-input-values.png)

1. Cliquez sur **Send** pour appeler l’API.

1. Passez en revue l’onglet **Réponse** pour voir la réponse de l’API.

   ![Invoke API - response](assets/invoke-api-response.png)

Les étapes ci-dessus confirment la modernisation de l’environnement AEM as a Cloud Service, ce qui permet l’accès aux API d’AEM. Cela confirme également la configuration réussie du projet ADC et la communication d’identifiant client ID OAuth serveur à serveur avec l’instance d’auteur AEM.

### Exemple d’application NodeJS

Développons un exemple d’application NodeJS.

Pour développer l’application, vous pouvez utiliser les instructions _Run-the-sample-application_ ou _Step-by-step-development_.


>[!BEGINTABS]

>[!TAB Run-the-sample-application]

1. Téléchargez l’exemple de fichier zip d’application [demo-nodejs-app-to-invoke-aem-openapi](assets/demo-nodejs-app-to-invoke-aem-openapi.zip) et extrayez-le.

1. Accédez au dossier extrait et installez les dépendances.

   ```bash
   $ npm install
   ```

1. Remplacez les espaces réservés dans le fichier `.env` par les valeurs réelles des informations d’identification OAuth Server-to-Server du projet ADC.

1. Remplacez `<BUCKETNAME>` et `<ASSETID>` dans le fichier `src/index.js` par les valeurs réelles.

1. Exécutez l’application NodeJS.

   ```bash
   $ node src/index.js
   ```

>[!TAB Développement pas à pas]

1. Créez un projet NodeJS.

   ```bash
   $ mkdir demo-nodejs-app-to-invoke-aem-openapi
   $ cd demo-nodejs-app-to-invoke-aem-openapi
   $ npm init -y
   ```

1. Installez la bibliothèque _fetch_ et _dotenv_ pour effectuer des requêtes HTTP et lire respectivement les variables d’environnement.

   ```bash
   $ npm install node-fetch
   $ npm install dotenv
   ```

1. Ouvrez le projet dans votre éditeur de code préféré et mettez à jour le fichier `package.json` pour ajouter `type` à `module`.

   ```json
   {
       ...
       "version": "1.0.0",
       "type": "module",
       "main": "index.js",
       ...
   }
   ```

1. Créez le fichier `.env` et ajoutez la configuration suivante. Remplacez les espaces réservés par les valeurs réelles des informations d’identification OAuth Server-to-Server du projet ADC.

   ```properties
   CLIENT_ID=<ADC Project OAuth Server-to-Server credential ClientID>
   CLIENT_SECRET=<ADC Project OAuth Server-to-Server credential Client Secret>
   SCOPES=<ADC Project OAuth Server-to-Server credential Scopes>
   ```

1. Créez le fichier `src/index.js` et ajoutez le code suivant, puis remplacez `<BUCKETNAME>` et `<ASSETID>` par les valeurs réelles.

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
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
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

Une fois l’exécution terminée, la réponse de l’API s’affiche dans la console. La réponse contient les métadonnées de la ressource spécifiée.

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

Félicitations. Vous avez correctement appelé les API AEM OpenAPI de votre application personnalisée à l’aide de l’authentification OAuth serveur à serveur.

### Vérification du code de l’application

Les légendes clés de l’exemple de code d’application NodeJS sont les suivantes :

1. **Authentification IMS** : récupère un jeton d’accès à l’aide des informations d’identification OAuth Server-to-Server configurées dans le projet ADC.

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

1. **Appel d’API** : appelle l’API d’auteur Assets pour récupérer les métadonnées d’une ressource spécifique en fournissant le jeton d’accès pour autorisation.

   ```javascript
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
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

## Résumé

Dans ce tutoriel, vous avez appris à appeler des API AEM OpenAPI à partir d’applications personnalisées. Vous avez activé AEM accès aux API, créé et configuré un projet Adobe Developer Console (ADC).
Dans le projet ADC, vous avez ajouté les API AEM, configuré son type d’authentification et associé le profil de produit. Vous avez également configuré l’instance AEM pour activer la communication du projet ADC et développé un exemple d’application NodeJS qui appelle l’API d’auteur Assets.

