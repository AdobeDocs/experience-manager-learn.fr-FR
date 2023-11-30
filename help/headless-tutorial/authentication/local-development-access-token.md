---
title: Jeton d’accès au développement local
description: Les jetons d’accès au développement local d’AEM sont utilisés pour accélérer le développement des intégrations avec AEM as a Cloud Service qui interagissent par programmation avec les services de création et de publication d’AEM via HTTP.
version: Cloud Service
topics: Development, Security
feature: APIs
activity: develop
audience: developer
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 100%

---

# Jeton d’accès au développement local

Les développeurs et développeuses qui créent des intégrations nécessitant un accès par programmation à AEM as a Cloud Service ont besoin d’un moyen simple et rapide d’obtenir des jetons d’accès temporaires pour AEM afin de faciliter les activités de développement local. Pour répondre à ce besoin, la Developer Console d’AEM permet aux développeurs et développeuses de générer eux-mêmes des jetons d’accès temporaires qui peuvent être utilisés pour accéder par programmation à AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## Générer un jeton d’accès au développement local

![Obtention d’un jeton d’accès au développement local.](assets/local-development-access-token/getting-a-local-development-access-token.png)

Le jeton d’accès au développement local permet d’accéder aux services de création et de publication d’AEM en tant que personne ayant généré le jeton, ainsi qu’à ses autorisations. Bien qu’il s’agisse d’un jeton de développement, ne le partagez pas et ne le stockez pas dans le contrôle de code source.

1. Dans [Adobe Admin Console](https://adminconsole.adobe.com/), assurez-vous, en tant que personne responsable du développement, que vous appartenez aux profils suivants :
   + Profil de produit IMS __Cloud Manager - Développement__ (accorde l’accès à la Developer Console d’AEM).
   + Profil de produit IMS __Administration d’AEM__ ou __Utilisateurs et utilisatrices d’AEM__ pour le service de l’environnement AEM auquel le jeton d’accès s’intègre.
   + L’environnement sandbox AEM as a Cloud Service ne nécessite qu’une adhésion à un profil de produit, __Administration d’AEM__ ou __Utilisateurs et utilisatrices d’AEM__.
1. Connectez-vous à [Adobe Cloud Manager](https://my.cloudmanager.adobe.com).
1. Ouvrez le programme contenant l’environnement AEM as a Cloud Service auquel s’intégrer.
1. Appuyez sur les __points de suspension__ en regard de l’environnement dans la section __Environnements__ et sélectionnez __Developer Console__.
1. Appuyez sur l’onglet __Intégrations__.
1. Appuyez sur l’onglet __Jeton local__.
1. Appuyez sur le bouton __Obtenir le jeton de développement local__.
1. Appuyez sur le __bouton de téléchargement__ dans le coin supérieur gauche pour télécharger le fichier JSON contenant la valeur `accessToken` et enregistrez le fichier JSON à un emplacement sécurisé sur votre ordinateur de développement.
   + Il s’agit de votre jeton d’accès de développement valable 24 heures permettant d’accéder à l’environnement d’AEM as a Cloud Service.

![Developer Console d’AEM - Intégrations - Obtention d’un jeton de développement local.](./assets/local-development-access-token/developer-console.png)

## Utiliser le jeton d’accès au développement local{#use-local-development-access-token}

![Jeton d’accès au développement local - Application externe.](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Téléchargez le jeton d’accès au développement local temporaire depuis la Developer Console d’AEM.
   + Le jeton d’accès au développement local expire toutes les 24 heures. Les développeurs et développeuses doivent donc télécharger de nouveaux jetons d’accès tous les jours.
1. Une application externe est en cours de développement, qui interagit par programmation avec AEM as a Cloud Service.
1. L’application externe lit dans le jeton d’accès au développement local.
1. L’application externe crée des requêtes HTTP pour AEM as a Cloud Service en ajoutant le jeton d’accès au développement local en tant que jeton porteur à l’en-tête d’autorisation des requêtes HTTP.
1. AEM as a Cloud Service reçoit la requête HTTP, authentifie la requête et effectue le travail demandé par la requête HTTP, puis renvoie une réponse HTTP à l’application externe.

### Exemple d’application externe

Nous allons créer une application JavaScript externe simple pour illustrer comment accéder par programmation à AEM as a Cloud Service via HTTPS à l’aide du jeton d’accès au développement local. Cette méthode illustre la manière dont _tout(e)_ application ou système s’exécutant en dehors d’AEM, indépendamment de la structure ou de la langue, peut utiliser le jeton d’accès pour s’authentifier par programmation et accéder à AEM as a Cloud Service. Dans la [section suivante](./service-credentials.md), nous mettrons à jour ce code d’application afin de prendre en charge l’approche de génération d’un jeton en vue d’une utilisation en production.

Cet exemple d’application est exécuté à partir de la ligne de commande et met à jour les métadonnées des ressources d’AEM à l’aide des API HTTP d’AEM Assets, à l’aide du flux suivant :

1. Lit les paramètres de la ligne de commande (`getCommandLineParams()`).
1. Obtient le jeton d’accès utilisé pour s’authentifier sur AEM as a Cloud Service (`getAccessToken(...)`).
1. Répertorie toutes les ressources d’un dossier de ressources d’AEM spécifié dans des paramètres de ligne de commande (`listAssetsByFolder(...)`).
1. Met à jour les métadonnées des ressources répertoriées avec les valeurs spécifiées dans les paramètres de ligne de commande (`updateMetadata(...)`).

L’élément clé sur lequel repose l’authentification par programmation à AEM à l’aide du jeton d’accès consiste en l’ajout d’un en-tête de requête HTTP d’autorisation à toutes les requêtes HTTP envoyées à AEM, au format suivant :

+ `Authorization: Bearer ACCESS_TOKEN`

## Exécuter l’application externe

1. Assurez-vous que [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) est installé sur votre ordinateur de développement local chargé d’exécuter l’application.
1. Téléchargez et décompressez l’[exemple d’application externe](./assets/aem-guides_token-authentication-external-application.zip).
1. Sur la ligne de commande, dans le dossier de ce projet, exécutez `npm install`.
1. Copiez le [jeton d’accès au développement local téléchargé](#download-local-development-access-token) dans un fichier nommé `local_development_token.json` à la racine du projet.
   + Souvenez-vous cependant de ne jamais enregistrer vos informations d’identification sur Git.
1. Ouvrez `index.js` et examinez le code et les commentaires de l’application externe.

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   Examinez les appels `fetch(..)` dans `listAssetsByFolder(...)` et `updateMetadata(...)`, puis notez que l’en-tête `headers` définit l’en-tête de requête HTTP `Authorization` avec la valeur `Bearer ACCESS_TOKEN`. La requête HTTP provenant de l’application externe s’authentifie de cette manière auprès d’AEM as a Cloud Service.

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   Toute requête HTTP à AEM as a Cloud Service doit définir le jeton d’accès porteur dans l’en-tête d’autorisation. Souvenez-vous que chaque environnement AEM as a Cloud Service nécessite son propre jeton d’accès. Le jeton d’accès n’est valable que dans son propre environnement (le jeton d’accès de l’environnement de développement n’est pas valable sur l’environnement d’évaluation ou de production etc.).

1. Utilisez la ligne de commande à partir de la racine du projet et exécutez l’application en transmettant les paramètres suivants :

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Les paramètres suivants sont transmis :

   + `aem` : le schéma et le nom d’hôte de l’environnement AEM as a Cloud Service avec lequel l’application interagit (`https://author-p1234-e5678.adobeaemcloud.com`, par exemple).
   + `folder` : le chemin d’accès au dossier de ressources dont les ressources sont mises à jour avec la `propertyValue`. N’ajoutez pas le préfixe `/content/dam` (`/wknd-shared/en/adventures/napa-wine-tasting`, par exemple).
   + `propertyName` : le nom de la propriété de la ressource à mettre à jour, par rapport à `[dam:Asset]/jcr:content` (ex. `metadata/dc:rights`).
   + `propertyValue` : la valeur de la propriété `propertyName`. Les valeurs comportant des espaces doivent être encapsulées avec `"` (par exemple, `"WKND Limited Use"`).
   + `file` : le chemin d’accès au fichier JSON téléchargé à partir de la Developer Console d’AEM.

   Les résultats des ressources mises à jour s’affichent en cas de réussite de l’application :

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Vérifier la mise à jour des métadonnées dans AEM

Vérifiez la mise à jour des métadonnées en vous connectant à l’environnement AEM as a Cloud Service (assurez-vous d’accéder au même hôte que celui transmis dans le paramètre de ligne de commande `aem`).

1. Connectez-vous à l’environnement AEM as a Cloud Service avec lequel l’application externe a interagi (utilisez le même hôte que celui fourni dans le paramètre de ligne de commande `aem`).
1. Accédez à __Ressources__ > __Fichiers__.
1. Accédez au dossier de ressources spécifié par le paramètre de ligne de commande `folder`, par exemple __WKND__ > __English__ > __Adventures__ > __Napa Wine Tasting__.
1. Ouvrez les __Propriétés__ de n’importe quelle ressource (autre qu’un fragment de contenu) présente dans le dossier.
1. Cliquez sur l’onglet __Avancé__.
1. Vérifiez la valeur de la propriété mise à jour, par exemple __Copyright__, qui est mappée à la propriété JCR `metadata/dc:rights` mise à jour, laquelle reflète la valeur fournie dans le paramètre `propertyValue` (par exemple, __WKND Limited Use__).

![Mise à jour des métadonnées de WKND Limited Use.](./assets/local-development-access-token/asset-metadata.png)

## Étapes suivantes

Vous avez réussi à accéder par programmation à AEM as a Cloud Service à l’aide du jeton de développement local. Dans la section suivante, vous allez mettre à jour l’application afin de prendre en charge l’utilisation des informations d’identification de service, de sorte que cette application puisse être utilisée dans un environnement de production.

+ [Utiliser les informations d’identification de service](./service-credentials.md)
