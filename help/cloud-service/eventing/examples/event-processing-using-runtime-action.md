---
title: Traitement des événements AEM à l’aide de l’action Adobe I/O Runtime
description: Découvrez comment traiter les événements AEM reçus à l’aide de l’action Adobe I/O Runtime.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 558
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
exl-id: c362011e-89e4-479c-9a6c-2e5caa3b6e02
source-git-commit: efa0a16649c41fab8309786a766483cfeab98867
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 100%

---

# Traitement des événements AEM à l’aide de l’action Adobe I/O Runtime

Découvrez comment traiter les événements AEM reçus à l’aide de l’action [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/). Comme cet exemple améliore l’exemple précédent d’[action Adobe I/O Runtime et d’événements AEM](runtime-action.md), assurez-vous de l’avoir terminé avant de passer à celui-ci.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

Dans cet exemple, le traitement des événements stocke les données d’événement d’origine et l’événement reçu comme message d’activité dans le stockage Adobe I/O Runtime. Cependant, si l’événement est du type _Fragment de contenu modifié_, alors il appelle également le service de création AEM pour trouver les détails de modification. Enfin, il affiche les détails de l’événement dans une application monopage (SPA).

## Prérequis

Les éléments suivants sont requis afin de terminer ce tutoriel :

- Environnement AEM as a Cloud Service avec [AEM Eventing activé](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). L’exemple de projet [Sites WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) doit être déployé dessus.

- Accès à [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Interface de ligne de commande d’Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) installée sur votre ordinateur local.

- Projet initialisé localement à partir de l’exemple précédent d’[action Adobe I/O Runtime et d’événements AEM](./runtime-action.md#initialize-project-for-local-development).

## Action du processeur des événements AEM

Dans cet exemple, l’[action](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) du processeur des événements effectue les tâches suivantes :

- Analyse l’événement reçu dans un message d’activité.
- Si l’événement reçu est du type _Fragment de contenu modifié_, rappelez le service de création AEM pour rechercher les détails de modification.
- Conserve les données d’événement d’origine, le message d’activité et les détails de modification (le cas échéant) dans le stockage Adobe I/O Runtime.

Pour effectuer les tâches ci-dessus, commençons par ajouter une action au projet, développons des modules JavaScript pour exécuter les tâches ci-dessus, puis mettons à jour le code d’action afin d’utiliser les modules développés.

Consultez le fichier [WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) ci-joint pour obtenir le code complet. La section ci-dessous met en évidence les fichiers clés.

### Ajouter une action

- Pour ajouter une action, exécutez la commande suivante :

  ```bash
  aio app add action
  ```

- Sélectionnez `@adobe/generator-add-action-generic` en tant que modèle d’action, nommez l’action `aem-event-processor`.

  ![Ajouter une action](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### Développer des modules JavaScript

Pour effectuer les tâches mentionnées ci-dessus, développons les modules JavaScript ci-après.

- Le module `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` détermine si l’événement reçu est du type _Fragment de contenu modifié_.

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

- Le module `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` appelle le service de création AEM pour trouver les détails de modification.

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

  Reportez-vous au [tutoriel sur les informations d’identification du service AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=fr) pour en savoir plus. Consultez également les [fichiers de configuration du créateur d’applications](https://developer.adobe.com/app-builder/docs/guides/configuration/) pour la gestion des secrets et des paramètres d’action.

- Le module `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` stocke les données d’événement d’origine, le message d’activité et les détails de modification (le cas échéant) dans le stockage Adobe I/O Runtime.

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

Enfin, mettez à jour le code d’action de `src/dx-excshell-1/actions/aem-event-processor/index.js` pour utiliser les modules développés.

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

- Le dossier `src/dx-excshell-1/actions/model` contient les fichiers `aemEvent.js` et `errors.js`, qui sont utilisés par l’action pour analyser l’événement reçu et gérer les erreurs, respectivement.
- Le dossier `src/dx-excshell-1/actions/load-processed-aem-events` contient du code d’action. Cette action est utilisée par la SPA pour charger les événements AEM traités à partir du stockage Adobe I/O Runtime.
- Le dossier `src/dx-excshell-1/web-src` contient le code de la SPA, qui affiche les événements AEM traités.
- Le fichier `src/dx-excshell-1/ext.config.yaml` contient la configuration des actions et des paramètres.

## Concept et principaux points à retenir

Les exigences de traitement des événements diffèrent d’un projet à l’autre, mais les principaux points à rentenir de cet exemple sont les suivants :

- Le traitement des événements peut être effectué à l’aide de l’action Adobe I/O Runtime.
- L’action Runtime peut communiquer avec des systèmes tels que vos applications internes, des solutions tierces et des solutions Adobe.
- L’action Runtime sert de point d’entrée à un processus métier conçu autour d’un changement de contenu.
