---
title: Action Adobe I/O Runtime et événements AEM
description: Découvrez comment recevoir des événements AEM à l’aide de l’action Adobe I/O Runtime et consulter les détails de l’événement tels que la charge utile, les en-têtes et les métadonnées.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 1%

---


# Action Adobe I/O Runtime et événements AEM

Découvrez comment recevoir des événements AEM à l’aide de [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Action et révision des détails de l’événement tels que la charge utile, les en-têtes et les métadonnées.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtime est une plateforme sans serveur qui permet l’exécution de code en réponse aux événements d’Adobe I/O. Cela vous aide à créer des applications basées sur des événements sans vous soucier des infrastructures.

Dans cet exemple, vous créez un Adobe I/O Runtime [Action](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) qui reçoit AEM événements et consigne les détails de l’événement.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

Les étapes de haut niveau sont les suivantes :

- Création d’un projet dans la console Adobe Developer
- Initialisation du projet pour le développement local
- Configuration d’un projet dans la console Adobe Developer
- Déclencher AEM événement et vérifier l’exécution de l’action

## Conditions préalables

Pour suivre ce tutoriel, vous devez :

- AEM environnement as a Cloud Service avec [AEM Eventing activé](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- Accès à [Console Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Interface de ligne de commande d’Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) installé sur votre ordinateur local.

## Création d’un projet dans la console Adobe Developer

Pour créer un projet dans la console Adobe Developer, procédez comme suit :

- Accédez à [Console Adobe Developer](https://developer.adobe.com/) et cliquez sur **Console** bouton .

- Dans le **Démarrage rapide** , cliquez sur **Créer un projet à partir d’un modèle**. Ensuite, dans la variable **Parcourir les modèles** boîte de dialogue, sélectionnez **App Builder** modèle.

- Si nécessaire, mettez à jour le titre du projet, le nom de l’application et l’espace de travail Ajouter . Cliquez ensuite sur **Enregistrer**.

  ![Création d’un projet dans la console Adobe Developer](../assets/examples/runtime-action/create-project.png)


## Initialisation du projet pour le développement local

Pour ajouter une action Adobe I/O Runtime au projet, vous devez initialiser le projet pour le développement local. Sur le terminal d’ouverture de l’ordinateur local, accédez à l’emplacement où vous souhaitez initialiser votre projet et procédez comme suit :

- Initialisation du projet en exécutant

  ```bash
  aio app init
  ```

- Sélectionnez la variable `Organization`, la variable `Project` vous avez créé à l’étape précédente et l’espace de travail. Dans `What templates do you want to search for?` étape, sélectionnez `All Templates` .

  ![Organisation-projet-sélection - Initialiser le projet](../assets/examples/runtime-action/all-templates.png)

- Dans la liste des modèles, sélectionnez `@adobe/generator-app-excshell` .

  ![Modèle d’extensibilité - Initialiser le projet](../assets/examples/runtime-action/extensibility-template.png)

- Ouvrez le projet dans votre IDE favori, par exemple VSCode.

- La sélection _Modèle d’extensibilité_ (`@adobe/generator-app-excshell`) fournit une action d’exécution générique, le code se trouve dans `src/dx-excshell-1/actions/generic/index.js` fichier . Mettons-le à jour afin de rester simple, consignons les détails de l’événement et renvoyons une réponse de succès. Cependant, dans l’exemple suivant, il est amélioré pour traiter les événements AEM reçus.

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- Enfin, déployez l’action mise à jour sur Adobe I/O Runtime en exécutant .

  ```bash
  aio app deploy
  ```

## Configuration d’un projet dans la console Adobe Developer

Pour recevoir des événements AEM et exécuter l’action Adobe I/O Runtime créée à l’étape précédente, configurez le projet dans la console Adobe Developer.

- Dans la console Adobe Developer, accédez au [project](https://developer.adobe.com/console/projects) créé à l’étape précédente et cliquez pour l’ouvrir. Sélectionnez la variable `Stage` workspace, c’est là que l’action a été déployée.

- Cliquez sur **Ajouter un service** et sélectionnez **API** . Dans le **Ajout d’une API** modal, sélectionnez **Services Adobe** > **API de gestion I/O** et cliquez sur **Suivant**, suivez les étapes de configuration supplémentaires, puis cliquez sur **Enregistrer l’API configurée**.

  ![Ajouter un service - Configurer le projet](../assets/examples/runtime-action/add-io-management-api.png)

- De même, cliquez sur **Ajouter un service** et sélectionnez **Événement** . Dans le **Ajout d’événements** boîte de dialogue, sélectionnez **Experience Cloud** > **AEM Sites**, puis cliquez sur **Suivant**. Suivez d’autres étapes de configuration, sélectionnez l’instance AEMCS, les types d’événement et d’autres détails.

- Enfin, dans la section **Comment recevoir des événements** étape, développer **Action d’exécution** et sélectionnez l’option _générique_ action créée à l’étape précédente. Cliquez sur **Enregistrement des événements configurés**.

  ![Action d’exécution - Configuration du projet ](../assets/examples/runtime-action/select-runtime-action.png)

- Consultez les détails de l’enregistrement de l’événement, ainsi que la **Suivi du débogage** et vérifiez la variable **Contestation de la propriété** requête et réponse.

  ![Détails de l’enregistrement d’événement](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## Déclencher des événements AEM

Pour déclencher des événements AEM de votre environnement as a Cloud Service AEM qui a été enregistré dans le projet de console Adobe Developer ci-dessus, procédez comme suit :

- Accès et connexion à votre environnement de création as a Cloud Service AEM via [Cloud Manager](https://my.cloudmanager.adobe.com/).

- Selon votre **Événements abonnés**, créer, mettre à jour, supprimer, publier ou annuler la publication d’un fragment de contenu.

## Vérification des détails d’événement

Après avoir effectué les étapes ci-dessus, vous devriez voir les événements AEM distribués à l’action générique.

Vous pouvez consulter les détails de l’événement dans la section **Suivi du débogage** de l’onglet Détails de l’enregistrement d’événement.

![AEM Détails de l’événement](../assets/examples/runtime-action/aem-event-details.png)


## Étapes suivantes

Dans l’exemple suivant, améliorons cette action pour traiter AEM événements, rappelez AEM service de création pour obtenir les détails du contenu, stocker les détails dans le stockage Adobe I/O Runtime et les afficher via l’application d’une seule page (SPA).

