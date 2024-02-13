---
title: Événements AEM Assets pour l’intégration PIM
description: Découvrez comment intégrer AEM Assets et les systèmes de gestion des informations sur les produits (PIM) pour les mises à jour des métadonnées des ressources.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-02-13T00:00:00Z
jira: KT-14901
thumbnail: KT-14901.jpeg
source-git-commit: f150a2517c4cafe55917e1aa50dca297c9bb3bc5
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 1%

---


# Événements AEM Assets pour l’intégration PIM

Découvrez comment intégrer AEM Assets et le système de gestion des informations sur les produits (PIM) pour mettre à jour les métadonnées des ressources **utilisation d’AEM Eventing**. Lors de la réception d’un événement AEM Assets, les métadonnées de la ressource peuvent être mises à jour dans AEM, PIM ou les deux systèmes, en fonction des besoins de l’entreprise. Cependant, dans cet exemple, mettons à jour les métadonnées de la ressource dans AEM.

Pour exécuter la mise à jour des métadonnées de ressource **code en dehors de l’AEM**, la variable [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) une plateforme sans serveur est utilisée. Le flux de traitement des événements est le suivant :

![Événements AEM Assets pour l’intégration PIM](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. Le service AEM Author déclenche une _Traitement des ressources terminé_ lorsqu’un chargement de ressource est terminé.
1. L’événement est envoyé au [Événements d’Adobe I/O](https://developer.adobe.com/events/) service.
1. Le service Adobe I/O Events transmet l’événement à la variable [Action Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) pour le traitement.
1. L’action Adobe I/O Runtime appelle l’API PIM moquée pour récupérer des métadonnées supplémentaires telles que le SKU, les informations sur les fournisseurs.
1. Les métadonnées supplémentaires récupérées par PIM sont ensuite mises à jour dans AEM Assets à l’aide de la variable [API de création de ressources](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

## Conditions préalables

Pour suivre ce tutoriel, vous devez :

- AEM environnement as a Cloud Service avec [AEM Eventing activé](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). L’exemple [Sites WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) doit être déployé sur .

- Accès à [Console Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Interface de ligne de commande d’Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) installé sur votre ordinateur local.

## Étapes de développement

Les étapes de développement de haut niveau sont les suivantes :

1. [Créer un projet dans la console Adobe Developer (ADC)](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [Initialisation du projet pour le développement local](./runtime-action.md#initialize-project-for-local-development)
1. Configuration d’un projet dans ADC
1. Configuration du service d’auteur AEM pour activer la communication du projet ADC
1. Développer une action d’exécution qui orchestre la récupération et la mise à jour des métadonnées
1. Chargement d’une ressource dans AEM service Auteur et vérification de la mise à jour des métadonnées

Pour obtenir des instructions détaillées sur 1-2, reportez-vous à la section [Action Adobe I/O Runtime et événements AEM](./runtime-action.md#) pour les versions 3 à 6, reportez-vous aux sections suivantes.

### Configuration d’un projet dans la console Adobe Developer (ADC)

Pour recevoir des événements AEM Assets et exécuter l’action Adobe I/O Runtime créée à l’étape précédente, configurez le projet dans ADC.

- Dans ADC, accédez à la [project](https://developer.adobe.com/console/projects). Sélectionnez la variable `Stage` workspace, c’est là que l’action d’exécution a été déployée.

- Cliquez sur **Ajouter un service** et sélectionnez **Événement** . Dans le **Ajout d’événements** boîte de dialogue, sélectionnez **Experience Cloud** > **AEM Assets**, puis cliquez sur **Suivant**. Suivez d’autres étapes de configuration, sélectionnez l’instance AEMCS, _Traitement des ressources terminé_ , type d’authentification OAuth serveur à serveur et autres détails.

  ![Événement AEM Assets - événement add](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- Enfin, dans la section **Comment recevoir des événements** étape, développer **Action d’exécution** et sélectionnez l’option _générique_ action créée à l’étape précédente. Cliquez sur **Enregistrement des événements configurés**.

  ![Événement AEM Assets - événement de réception](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- De même, cliquez sur **Ajouter un service** et sélectionnez **API** . Dans le **Ajout d’une API** modal, sélectionnez **Experience Cloud** > **API AS A CLOUD SERVICE AEM** et cliquez sur **Suivant**.

  ![Ajout AEM API as a Cloud Service - Configuration du projet](../assets/examples/assets-pim-integration/add-aem-api.png)

- Sélectionnez **OAuth serveur à serveur** pour le type d’authentification et cliquez sur **Suivant**.

- Sélectionnez ensuite le **AEM Administrateurs-XXX** profil de produit et cliquez sur **Enregistrer l’API configurée**. Pour accéder à des fonctionnalités granulaires et à des autorisations, le profil de produit sélectionné doit être associé à l’événement AEM Assets produisant l’environnement AEMCS.

  ![Ajout AEM API as a Cloud Service - Configuration du projet](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

### Configuration du service d’auteur AEM pour activer la communication du projet ADC

Pour mettre à jour les métadonnées de la ressource dans AEM à partir du projet ADC ci-dessus, configurez AEM service Auteur avec l’ID client du projet ADC. La variable _client id_ est ajouté en tant que variable d’environnement à l’aide de la variable [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html#add-variables) Interface utilisateur.

- Connexion à [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/), sélectionnez **Programme** > **Environnement** > **Ellipse** > **Afficher les détails** > **Configuration** .

  ![Adobe Cloud Manager - Configuration de l’environnement](../assets/examples/assets-pim-integration/cloud-manager-environment-configuration.png)

- Alors **Ajouter une configuration** et saisissez les détails de la variable sous la forme

  | Nom | Valeur | Service AEM | Type |
  | ----------- | ----------- | ----------- | ----------- |
  | ADOBE_PROVIDED_CLIENT_ID | &lt;COPY_FROM_ADC_PROJECT_CREDENTIALS> | Création | Variable |

  ![Adobe Cloud Manager - Configuration de l’environnement](../assets/examples/assets-pim-integration/add-environment-variable.png)

- Cliquez sur **Ajouter** et **Enregistrer** la configuration.

### Action Développer l’exécution

Pour effectuer la récupération et la mise à jour des métadonnées, commencez par mettre à jour la création automatique. _générique_ code d’action dans `src/dx-excshell-1/actions/generic` dossier.

Consultez la section [WKND-Assets-PIM-Integration.zip](../assets/examples/assets-pim-integration/WKND-Assets-PIM-Integration.zip) pour le code complet et la section ci-dessous met en surbrillance les fichiers clés.

- La variable `src/dx-excshell-1/actions/generic/mockPIMCommunicator.js` file moque l’appel de l’API PIM pour récupérer des métadonnées supplémentaires telles que le SKU et le nom du fournisseur.

  ```javascript
  /**
   * Mock PIM API to get the product data such as SKU, Supplier, etc.
   *
   * In a real-world scenario, this function would call the PIM API to get the product data.
   * For this example, we are returning mock data.
   *
   * @param {string} assetId - The assetId to get the product data.
   */
  module.exports = {
      async getPIMData(assetId) {
          if (!assetId) {
          throw new Error('Invalid assetId');
          }
          // Mock response data for demo purposes
          const data = {
          SKUID: 'MockSKU 123',
          SupplierName: 'mock-supplier',
          // ... other product data
          };
          return data;
      },
  };
  ```

- La variable `src/dx-excshell-1/actions/generic/aemCommunicator.js` met à jour les métadonnées de la ressource dans AEM à l’aide de la fonction [API de création de ressources](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

  ```javascript
  const fetch = require('node-fetch');
  
  ...
  
  /**
  *  Get IMS Access Token using Client Credentials Flow
  *
  * @param {*} clientId - IMS Client ID from ADC project's OAuth Server-to-Server Integration
  * @param {*} clientSecret - IMS Client Secret from ADC project's OAuth Server-to-Server Integration
  * @param {*} scopes - IMS Meta Scopes from ADC project's OAuth Server-to-Server Integration as comma separated strings
  * @returns {string} - Returns the IMS Access Token
  */
  async function getIMSAccessToken(clientId, clientSecret, scopes) {
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';
  
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
    };
  
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();
  
    return responseJSON.access_token;
  }    
  
  async function updateAEMAssetMetadata(metadataDetails, aemAssetEvent, params) {
    ...
    // Transform the metadata details to JSON Patch format,
    // see https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/patchAssetMetadata
    const transformedMetadata = Object.keys(metadataDetails).map((key) => ({
      op: 'add',
      path: `wknd-${key.toLowerCase()}`,
      value: metadataDetails[key],
    }));
  
    ...
  
    // Get ADC project's OAuth Server-to-Server Integration credentials
    const clientId = params.ADC_CECREDENTIALS_CLIENTID;
    const clientSecret = params.ADC_CECREDENTIALS_CLIENTSECRET;
    const scopes = params.ADC_CECREDENTIALS_METASCOPES;
  
    // Get IMS Access Token using Client Credentials Flow
    const access_token = await getIMSAccessToken(clientId, clientSecret, scopes);
  
    // Call AEM Author service to update the metadata using Assets Author API
    // See https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/
    const res = await fetch(`${aemAuthorHost}/adobe/assets/${assetId}/metadata`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json-patch+json',
        'If-Match': '*',
        'X-Adobe-Accept-Experimental': '1',
        'X-Api-Key': 'aem-assets-management-api', // temporary value
        Authorization: `Bearer ${access_token}`,
      },
      body: JSON.stringify(transformedMetadata),
    });
  
    ...
  }
  
  module.exports = { updateAEMAssetMetadata };
  ```

  La variable `.env` stocke les détails des informations d’identification OAuth Server-to-Server du projet ADC et elles sont transmises en tant que paramètres à l’action à l’aide de `ext.config.yaml` fichier . Voir [Fichiers de configuration du générateur d’applications](https://developer.adobe.com/app-builder/docs/guides/configuration/) pour la gestion des secrets et des paramètres d’action.

- La variable `src/dx-excshell-1/actions/model` Le dossier contient `aemAssetEvent.js` et `errors.js` fichiers, qui sont utilisés par l’action pour analyser l’événement reçu et gérer les erreurs, respectivement.

- La variable `src/dx-excshell-1/actions/generic/index.js` utilise les modules susmentionnés pour orchestrer la récupération et la mise à jour des métadonnées.

  ```javascript
  ...
  
  let responseMsg;
  // handle the challenge probe request, they are sent by I/O to verify the action is valid
  if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
  } else {
    logger.info('AEM Asset Event request received');
  
    // create AEM Asset Event object from request parameters
    const aemAssetEvent = new AEMAssetEvent(params);
  
    // Call mock PIM API to get the product data such as SKU, Supplier, etc.
    const mockPIMData = await mockPIMAPI.getPIMData(
      aemAssetEvent.getAssetName(),
    );
    logger.info('Mock PIM API response', mockPIMData);
  
    // Update PIM received data in AEM as Asset metadata
    const aemUpdateStatus = await updateAEMAssetMetadata(
      mockPIMData,
      aemAssetEvent,
      params,
    );
    logger.info('AEM Asset metadata update status', aemUpdateStatus);
  
    if (aemUpdateStatus) {
      // create response message
      responseMsg = JSON.stringify({
        message:
          'AEM Asset Event processed successfully, updated the asset metadata with PIM data.',
        assetdata: {
          assetName: aemAssetEvent.getAssetName(),
          assetPath: aemAssetEvent.getAssetPath(),
          assetId: aemAssetEvent.getAssetId(),
          aemHost: aemAssetEvent.getAEMHost(),
          pimdata: mockPIMData,
        },
      });
    } 
  
    // response object
    const response = {
      statusCode: 200,
      body: responseMsg,
    };
  
    // Return the response to the caller
    return response;
  
    ...
  }
  ```

Déployez l’action mise à jour sur Adobe I/O Runtime à l’aide de la commande suivante :

```bash
$ aio app deploy
```

### Chargement de ressources et vérification des métadonnées

Pour vérifier l’intégration AEM Assets et PIM, procédez comme suit :

- Pour afficher les métadonnées fournies par le modèle PIM comme le SKU et le nom du fournisseur, créez un schéma de métadonnées dans AEM Assets, voir [Schémas de métadonnées](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/configuring/metadata-schemas.html) qui affiche les propriétés de métadonnées du SKU et du nom du fournisseur.

- Chargez une ressource dans AEM service Auteur et vérifiez la mise à jour des métadonnées.

  ![Mise à jour des métadonnées AEM Assets](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## Concept et principales leçons

La synchronisation des métadonnées des ressources entre AEM et d’autres systèmes tels que PIM est souvent requise dans l’entreprise. L’utilisation d’AEM Eventing de telles exigences peut être réalisée.

- Le code de métadonnées de la ressource est exécuté en dehors d’AEM, ce qui permet d’éviter la charge sur AEM service Auteur et donc une architecture pilotée par un événement qui se met à l’échelle indépendamment.
- L’API d’auteur de ressources nouvellement introduite est utilisée pour mettre à jour les métadonnées de ressources dans AEM.
- L’authentification API utilise OAuth serveur à serveur (ou flux d’informations d’identification du client), voir [Guide de mise en oeuvre des informations d’identification OAuth Server-to-Server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/).
- Au lieu d’actions Adobe I/O Runtime, d’autres webhooks ou Amazon EventBridge peuvent être utilisés pour recevoir l’événement AEM Assets et traiter la mise à jour des métadonnées.
- Événements de ressources par le biais d’AEM Eventing permet aux entreprises d’automatiser et de rationaliser les processus critiques, ce qui favorise l’efficacité et la cohérence dans l’écosystème de contenu.

