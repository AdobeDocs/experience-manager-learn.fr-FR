---
title: Traitement des événements AEM à l’aide de l’action Adobe I/O Runtime
description: Découvrez comment traiter les événements AEM reçus à l’aide de l’action Adobe I/O Runtime.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---


# Traitement des événements AEM à l’aide de l’action Adobe I/O Runtime

Découvrez comment traiter les événements AEM reçus à l’aide de [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Action. Cet exemple améliore l’exemple précédent [Action Adobe I/O Runtime et événements AEM](runtime-action.md), assurez-vous que vous l’avez terminé avant de poursuivre avec celui-ci.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

Dans cet exemple, le traitement des événements stocke les données d’événement d’origine et l’événement reçu comme message d’activité dans le stockage Adobe I/O Runtime. Cependant, si l’événement est défini sur _Fragment de contenu modifié_ saisissez , puis il appelle également AEM service de création pour trouver les détails de modification. Enfin, il affiche les détails de l’événement dans une application d’une seule page (SPA).

## Prérequis

Pour suivre ce tutoriel, vous devez :

- AEM environnement as a Cloud Service avec [AEM Eventing activé](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). L’exemple [Sites WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) doit être déployé sur .

- Accès à [Console Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Interface de ligne de commande d’Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) installé sur votre ordinateur local.

- Projet initialisé localement à partir de l’exemple précédent [Action Adobe I/O Runtime et événements AEM](./runtime-action.md#initialize-project-for-local-development).


## Action du processeur d’événements AEM

Dans cet exemple, le responsable du traitement des événements [action](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) effectue les tâches suivantes :

- Analyse l’événement reçu dans un message d’activité.
- Si l’événement reçu est _Fragment de contenu modifié_ saisissez , revenez au service de création d’AEM pour rechercher les détails de modification.
- Conserve les données d’événement d’origine, le message d’activité et les détails de modification (le cas échéant) dans le stockage Adobe I/O Runtime.

Pour effectuer les tâches ci-dessus, commençons par ajouter une action au projet, développez des modules JavaScript pour exécuter les tâches ci-dessus, puis mettez à jour le code d’action afin d’utiliser les modules développés.

Consultez la section [WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) pour le code complet et la section ci-dessous met en surbrillance les fichiers clés.

### Ajouter une action

- Pour ajouter une action, exécutez la commande suivante :

  ```bash
  aio app add action
  ```

- Sélectionner `@adobe/generator-add-action-generic` en tant que modèle d’action, nommez l’action comme `aem-event-processor`.

  ![Ajouter une action](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### Développement de modules JavaScript

Pour effectuer les tâches mentionnées ci-dessus, développez les modules JavaScript suivants.

- La variable `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` détermine si l’événement reçu est _Fragment de contenu modifié_ type.

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- La variable `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` module appelle AEM service de création pour trouver les détails de la modification.

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  Voir [Tutoriel sur les informations d’identification du service AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en) pour en savoir plus. En outre, la variable [Fichiers de configuration du générateur d’applications](https://developer.adobe.com/app-builder/docs/guides/configuration/) pour la gestion des secrets et des paramètres d’action.

- La variable `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` stocke les données d’événement d’origine, le message d’activité et les détails de modification (le cas échéant) dans le stockage Adobe I/O Runtime.

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### Mettre à jour le code d’action

Enfin, mettez à jour le code d’action à l’adresse `src/dx-excshell-1/actions/aem-event-processor/index.js` pour utiliser les modules développés.

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## Ressources supplémentaires

- La variable `src/dx-excshell-1/actions/model` Le dossier contient `aemEvent.js` et `errors.js` fichiers, qui sont utilisés par l’action pour analyser l’événement reçu et gérer les erreurs, respectivement.
- La variable `src/dx-excshell-1/actions/load-processed-aem-events` contient du code d’action. Cette action est utilisée par la SPA pour charger les événements AEM traités à partir du stockage Adobe I/O Runtime.
- La variable `src/dx-excshell-1/web-src` contient le code SPA, qui affiche les événements AEM traités.
- La variable `src/dx-excshell-1/ext.config.yaml` contient la configuration des actions et des paramètres.

## Concept et principales leçons

Les exigences de traitement des événements diffèrent d’un projet à l’autre, mais les principales leçons de cet exemple sont les suivantes :

- Le traitement des événements peut être effectué à l’aide de l’action Adobe I/O Runtime.
- L’action d’exécution peut communiquer avec des systèmes tels que vos applications internes, des solutions tierces et des solutions d’Adobe.
- L’action d’exécution sert de point d’entrée à un processus d’entreprise conçu autour d’un changement de contenu.





