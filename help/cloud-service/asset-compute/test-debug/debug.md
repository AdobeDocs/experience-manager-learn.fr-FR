---
title: Déboguer un programme de travail Asset Compute
description: Les programmes de travail Asset Compute peuvent être débogués de plusieurs façons, depuis des instructions de journal de débogage simples, jusqu’au VS Code joint en tant que débogueur à distance en passant par l’extraction des journaux pour les activations dans Adobe I/O Runtime initiées à partir d’AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 268
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '618'
ht-degree: 100%

---

# Déboguer un programme de travail Asset Compute

Les programmes de travail Asset Compute peuvent être débogués de plusieurs façons, depuis des instructions de journal de débogage simples, jusqu’au VS Code joint en tant que débogueur à distance en passant par l’extraction des journaux pour les activations dans Adobe I/O Runtime initiées à partir d’AEM as a Cloud Service.

## Journalisation

La forme la plus élémentaire de débogage des programmes de travail Asset Compute utilise les instructions `console.log(..)` traditionnelles dans le code du programme de travail. L’objet `console` JavaScript est un objet global implicite. Il n’est donc pas nécessaire de l’importer ni de l’exiger, car il est toujours présent dans tous les contextes.

Ces instructions de journal peuvent être examinées différemment selon la manière dont le programme de travail d’Asset Compute est exécuté :

+ Depuis `aio app run`, les journaux impriment vers Standard Out et les journaux d’activation des [outil de développement](../develop/development-tool.md).
  ![Exécution de l’application console.log(...) dans AIO.](./assets/debug/console-log__aio-app-run.png)
+ Depuis `aio app test`, les journaux impriment vers `/build/test-results/test-worker/test.log`.
  ![Test de l’application console.log(...) AIO.](./assets/debug/console-log__aio-app-test.png)
+ À l’aide de `wskdebug`, les instructions de journal impriment vers la console de débogage de VS Code (Affichage > Console de débogage), Standard Out.
  ![wskdebug console.log(...).](./assets/debug/console-log__wskdebug.png)
+ À l’aide de `aio app logs`, les instructions de journal impriment vers la sortie du journal d’activation.

## Déboguer à distance via un débogueur joint

>[!WARNING]
>
>Utilisez Microsoft Visual Studio Code 1.48.0 ou version ultérieure pour la compatibilité avec wskdebug.

Le module npm [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) prend en charge un débogueur joint aux programmes de travail d’Asset Compute, y compris la possibilité de définir des points d’arrêt dans VS Code et de parcourir le code.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Déboguer par clic un programme de travail d’Asset Compute à l’aide de wskdebug (pas d’audio)_

1. Assurez-vous que les modules npm [wskdebug](../set-up/development-environment.md#wskdebug) et [ngrok](../set-up/development-environment.md#ngork) sont installés.
1. Assurez-vous que [l’application Docker Desktop et les images Docker prises en charge](../set-up/development-environment.md#docker) sont installées et en cours d’exécution.
1. Fermez toutes les instances actives en cours d’exécution de l’outil de développement.
1. Déployez le code le plus récent à l’aide de `aio app deploy` et enregistrez le nom de l’action déployée (nom entre `[...]`). Cela permet de mettre à jour le `launch.json` à l’étape 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Démarrez une nouvelle instance de l’outil de développement d’Asset Compute à l’aide de la commande `npx adobe-asset-compute devtool`.
1. Dans VS Code, appuyez sur l’icône Débogage dans le volet de navigation de gauche.
   + Si on vous y invite, appuyez sur __Créer un fichier launch.json > Node.js__ pour créer un nouveau fichier `launch.json`.
   + Sinon, appuyez sur l’icône d’__engrenage__ à droite du menu déroulant __Programme Launch__ pour ouvrir le `launch.json` existant dans l’éditeur.
1. Ajoutez la configuration d’objet JSON suivante au tableau `configurations` :

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. Sélectionnez le nouveau __wskdebug__ dans le menu déroulant.
1. Appuyez sur le bouton vert __Exécuter__ à gauche du menu déroulant __wskdebug__.
1. Ouvrez `/actions/worker/index.js` et appuyez sur la partie gauche des numéros de ligne pour ajouter les points d’arrêt 1. Accédez à la fenêtre du navigateur web de l’outil de développement Asset Compute ouverte à l’étape 6.
1. Appuyez sur le bouton __Exécuter__ pour exécuter le programme de travail.
1. Revenez à VS Code, à `/actions/worker/index.js` et parcourez le code.
1. Pour quitter l’outil de développement de débogage, appuyez sur `Ctrl-C` dans le terminal qui a exécuté la commande `npx adobe-asset-compute devtool` à l’étape 6.

## Accéder aux journaux à partir d’Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service exploite les programmes de travail d’Asset Compute via les profils de traitement](../deploy/processing-profiles.md) en les appelant directement dans Adobe I/O Runtime. Comme ces appels n’impliquent pas de développement local, leurs exécutions ne peuvent pas être déboguées à l’aide d’outils locaux tels que l’outil de développement Asset Compute ou wskdebug. Au lieu de cela, l’interface de ligne de commande d’Adobe I/O peut être utilisée pour récupérer les journaux du programme de travail exécuté dans un espace de travail particulier d’Adobe I/O Runtime.

1. Assurez-vous que les [variables d’environnement spécifiques à l’espace de travail](../deploy/runtime.md) sont définies via `AIO_runtime_namespace` et `AIO_runtime_auth`, en fonction de l’espace de travail nécessitant un débogage.
1. Depuis la ligne de commande, exécutez `aio app logs`.
   + Si l’espace de travail génère un trafic important, développez le nombre de journaux d’activation via l’indicateur `--limit` :
     `$ aio app logs --limit=25`
1. Les journaux d’activation les plus récents (jusqu’à la valeur `--limit` fournie) sont renvoyés en tant que sortie de la commande pour révision.

   ![Journaux de l’Application AIO.](./assets/debug/aio-app-logs.png)

## Résolution des problèmes

+ [Impossible de joindre le débogueur.](../troubleshooting.md#debugger-does-not-attach)
+ [Impossible de mettre les points d’arrêt en pause.](../troubleshooting.md#breakpoints-no-pausing)
+ [Débogueur de VS Code non joint.](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Débogueur VS Code joint après le début de l’exécution du programme de travail.](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Le programme de travail expire lors du débogage.](../troubleshooting.md#worker-times-out-while-debugging)
+ [Impossible d’arrêter le processus de débogage.](../troubleshooting.md#cannot-terminate-debugger-process)
