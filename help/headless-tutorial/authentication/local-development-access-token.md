---
title: Jeton d’accès au développement local
description: AEM Les jetons d’accès au développement local sont utilisés pour accélérer le développement des intégrations avec AEM en tant que Cloud Service qui interagissent par programmation avec les services AEM Author ou Publish sur HTTP.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 0%

---

# Jeton d’accès au développement local

Les développeurs qui créent des intégrations nécessitant un accès programmatique à AEM en tant que Cloud Service ont besoin d’un moyen simple et rapide d’obtenir des jetons d’accès temporaires pour AEM faciliter les activités de développement local. Pour répondre à ce besoin, AEM Developer Console permet aux développeurs de générer eux-mêmes des jetons d’accès temporaires qui peuvent être utilisés pour accéder par programmation à AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## Génération d’un jeton d’accès au développement local

![Obtention d’un jeton d’accès au développement local](assets/local-development-access-token/getting-a-local-development-access-token.png)

Le jeton d’accès au développement local permet d’accéder aux services AEM Author et Publish en tant qu’utilisateur ayant généré le jeton, ainsi que ses autorisations. Bien qu’il s’agisse d’un jeton de développement, ne partagez pas ce jeton, ni ne le stockez dans le contrôle de code source.

1. Dans [Adobe Admin Console](https://adminconsole.adobe.com/), assurez-vous que vous, développeur, êtes membre de :
   + __Cloud Manager - Profil produit__ DeveloperIMS (accorde l’accès à AEM Developer Console)
   + __AEM Administrateurs__ ou __AEM Utilisateurs__ Profil de produit IMS pour le service de l’environnement de l’AEM auquel le jeton d’accès s’intègre.
   + Les environnements Sandbox AEM en tant qu’environnements de Cloud Service ne nécessitent d’adhésion qu’aux __Administrateurs AEM__ ou __UtilisateursAEM__ Profil de produits
1. Connectez-vous à [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Ouvrez le programme contenant l’AEM en tant qu’environnement de Cloud Service à intégrer à
1. Appuyez sur les __points de suspension__ en regard de l’environnement dans la section __Environnements__, puis sélectionnez __Developer Console__
1. Appuyez sur l’onglet __Intégrations__ .
1. Appuyez sur le bouton __Obtenir le jeton de développement local__ .
1. Appuyez sur le __bouton de téléchargement__ dans le coin supérieur gauche pour télécharger le fichier JSON contenant la valeur `accessToken`, puis enregistrez le fichier JSON à un emplacement sécurisé sur votre ordinateur de développement.
   + Il s’agit de votre jeton d’accès développeur 24 heures sur 24 à l’AEM en tant qu’environnement de Cloud Service.

![AEM Developer Console - Intégrations - Obtenir un jeton de développement local](./assets/local-development-access-token/developer-console.png)

## Utilisation du jeton d’accès au développement local{#use-local-development-access-token}

![Jeton d’accès au développement local - Application externe](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Téléchargez le jeton d’accès au développement local temporaire depuis AEM Developer Console
   + Le jeton d’accès au développement local expire toutes les 24 heures. Les développeurs devront donc télécharger de nouveaux jetons d’accès tous les jours.
1. Une application externe est en cours de développement qui interagit par programmation avec AEM en tant que Cloud Service.
1. L’application externe se lit dans le jeton d’accès au développement local.
1. L’application externe crée des requêtes HTTP pour AEM en tant que Cloud Service, en ajoutant le jeton d’accès au développement local en tant que jeton porteur à l’en-tête d’autorisation des requêtes HTTP.
1. AEM en tant que Cloud Service reçoit la requête HTTP, authentifie la requête et effectue le travail demandé par la requête HTTP, puis renvoie une réponse HTTP à l’application externe.

### Exemple d’application externe

Nous allons créer une application JavaScript externe simple pour illustrer comment accéder par programmation à AEM en tant que Cloud Service via HTTPS à l’aide du jeton d’accès développeur local. Cela illustre la manière dont _toute application ou tout système s’exécutant en dehors d’AEM, indépendamment de la structure ou de la langue, peut utiliser le jeton d’accès pour s’authentifier par programmation et accéder à AEM en tant que Cloud Service._ Dans la [section suivante](./service-credentials.md), nous mettrons à jour ce code d’application afin de prendre en charge l’approche de génération d’un jeton à des fins d’utilisation en production.

Cet exemple d’application est exécuté à partir de la ligne de commande et met à jour AEM métadonnées des ressources à l’aide des API HTTP AEM Assets, à l’aide du flux suivant :

1. Lectures dans les paramètres de la ligne de commande (`getCommandLineParams()`)
1. Obtient le jeton d’accès utilisé pour s’authentifier auprès d’AEM en tant que Cloud Service (`getAccessToken(...)`)
1. Répertorie toutes les ressources d’un dossier de ressources AEM spécifié dans les paramètres de ligne de commande (`listAssetsByFolder(...)`)
1. Mise à jour des métadonnées des ressources répertoriées avec les valeurs spécifiées dans les paramètres de ligne de commande (`updateMetadata(...)`)

L’élément clé de l’authentification par programmation pour AEM à l’aide du jeton d’accès est l’ajout d’un en-tête de requête HTTP d’autorisation à toutes les requêtes HTTP envoyées à AEM, au format suivant :

+ `Authorization: Bearer ACCESS_TOKEN`

## Exécution de l’application externe

1. Assurez-vous que [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) est installé sur votre ordinateur de développement local, qui sera utilisé pour exécuter l’application externe.
1. Téléchargez et décompressez l’[exemple d’application externe](./assets/aem-guides_token-authentication-external-application.zip)
1. Sur la ligne de commande, dans le dossier de ce projet, exécutez `npm install`
1. Copiez le [jeton d’accès au développement local](#download-local-development-access-token) téléchargé dans un fichier nommé `local_development_token.json` à la racine du projet.
   + Mais rappelez-vous, ne validez jamais vos identifiants à Git !
1. Ouvrez `index.js` et passez en revue le code et les commentaires de l’application externe.

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
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
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
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
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

   Examinez les `fetch(..)` appels dans les `listAssetsByFolder(...)` et `updateMetadata(...)`, et notez que `headers` définissez l’en-tête de requête HTTP `Authorization` avec la valeur `Bearer ACCESS_TOKEN`. C’est ainsi que la requête HTTP provenant de l’application externe s’authentifie auprès d’AEM en tant que Cloud Service.

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

   Toute requête HTTP à AEM en tant que Cloud Service doit définir le jeton d’accès du porteur dans l’en-tête Authorization. Souvenez-vous que chaque AEM en tant qu’environnement de Cloud Service nécessite son propre jeton d’accès. Le jeton d’accès du développement ne fonctionnera pas dans l’environnement intermédiaire ou de production, l’environnement intermédiaire ne fonctionnera pas dans le cadre du développement ou de la production et l’environnement de production ne fonctionnera pas dans l’environnement de développement ou d’évaluation.

1. A l’aide de la ligne de commande, à partir de la racine du projet, exécutez l’application, en transmettant les paramètres suivants :

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Les paramètres suivants sont transmis :

   + `aem`: Le schéma et le nom d’hôte de l’AEM en tant qu’environnement de Cloud Service avec lequel l’application interagira (par exemple,  `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: Chemin d’accès au dossier de ressources dont les ressources seront mises à jour avec  `propertyValue` ; N’ajoutez PAS le  `/content/dam` préfixe (ex.  `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`: Nom de la propriété de la ressource à mettre à jour, par rapport à  `[dam:Asset]/jcr:content` (ex.  `metadata/dc:rights`).
   + `propertyValue`: La valeur à définir sur  `propertyName` ; les valeurs avec espaces doivent être encapsulées avec  `"` (ex.  `"WKND Limited Use"`)
   + `file`: Chemin d’accès au fichier JSON téléchargé depuis AEM Developer Console.

   Une exécution réussie de l’application donne des résultats pour chaque ressource mise à jour :

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Vérification de la mise à jour des métadonnées dans AEM

Vérifiez que les métadonnées ont été mises à jour en vous connectant à l’environnement AEM as a Cloud Service (assurez-vous que le même hôte transmis dans le paramètre de ligne de commande `aem` est accessible).

1. Connectez-vous à l’AEM en tant qu’environnement de Cloud Service avec lequel l’application externe a interagi (utilisez le même hôte fourni dans le paramètre de ligne de commande `aem`).
1. Accédez à __Ressources__ > __Fichiers__
1. Accédez-y le dossier de ressources spécifié par le paramètre de ligne de commande `folder`, par exemple __WKND__ __Anglais__ > __Aventures__ > __Goûts du vin de Napa__
1. Ouvrez les __Propriétés__ pour toute ressource (autre que le fragment de contenu) dans le dossier.
1. Appuyez sur l’onglet __Avancé__ .
1. Vérifiez la valeur de la propriété mise à jour, par exemple __Copyright__ mappé à la propriété `metadata/dc:rights` JCR mise à jour, qui reflète la valeur fournie dans le paramètre `propertyValue`, par exemple __WKND Limited Use__

![Mise à jour des métadonnées d’utilisation limitée WKND](./assets/local-development-access-token/asset-metadata.png)

## Étapes suivantes

Maintenant que nous avons accédé par programmation à AEM en tant que Cloud Service à l’aide du jeton de développement local, nous devons mettre à jour l’application pour la gérer à l’aide des informations d’identification du service, afin que cette application puisse être utilisée dans un contexte de production.

+ [Utilisation des informations d’identification du service](./service-credentials.md)
