---
title: Débogage d’un Asset compute Worker
description: Les objets Worker Asset compute peuvent être débogués de plusieurs façons, depuis des instructions de journal de débogage simples, jusqu’au code VS joint en tant que débogueur distant, jusqu’à l’extraction des journaux pour les activations dans Adobe I/O Runtime initiée à partir d’AEM en tant que Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Débogage d’un Asset compute Worker

Les objets Worker Asset compute peuvent être débogués de plusieurs façons, depuis des instructions de journal de débogage simples, jusqu’au code VS joint en tant que débogueur distant, jusqu’à l’extraction des journaux pour les activations dans Adobe I/O Runtime initiée à partir d’AEM en tant que Cloud Service.

## Journalisation

La forme la plus élémentaire de débogage des objets Worker Asset compute utilise les instructions `console.log(..)` traditionnelles dans le code du programme de travail. L’objet JavaScript `console` est un objet global implicite. Il n’est donc pas nécessaire de l’importer ni de l’exiger, car il est toujours présent dans tous les contextes.

Ces instructions de journal peuvent être examinées différemment selon la manière dont le programme de travail d’Asset compute est exécuté :

+ À partir de `aio app run`, consigne l’impression en sortie standard et les [journaux d’activation de l’outil de développement ](../develop/development-tool.md)
   ![l’application aio run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ À partir de `aio app test`, consigne l’impression sur `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ À l’aide de `wskdebug`, consigne les instructions imprimées dans la console de débogage du code VS (Vue > Console de débogage), en standard
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ À l’aide de `aio app logs`, les instructions du journal sont imprimées dans la sortie du journal d’activation.

## Débogage à distance via le débogueur joint

>[!WARNING]
>
>Utilisation de Microsoft Visual Studio Code 1.48.0 ou version ultérieure pour la compatibilité avec wskdebug

Le module npm [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) prend en charge l’association d’un débogueur aux objets Worker Asset compute, notamment la possibilité de définir des points d’arrêt dans VS Code et de parcourir le code.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Clic publicitaire de débogage d’un objet Worker Asset compute à l’aide de wskdebug (pas d’audio)_

1. Vérifiez que les modules npm [wskdebug](../set-up/development-environment.md#wskdebug) et [ngrok](../set-up/development-environment.md#ngork) et npm sont installés.
1. Vérifiez que [Docker Desktop et les images Docker prises en charge](../set-up/development-environment.md#docker) sont installés et en cours d’exécution.
1. Fermez toutes les instances principales de l’outil de développement.
1. Déployez le code le plus récent à l’aide de `aio app deploy` et enregistrez le nom de l’action déployée (nom entre `[...]`). Elle sera utilisée pour mettre à jour `launch.json` à l’étape 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Démarrez une nouvelle instance de l’outil de développement d’Asset compute à l’aide de la commande `npx adobe-asset-compute devtool`
1. Dans VS Code, appuyez sur l’icône Débogage dans le volet de navigation de gauche.
   + Si vous y êtes invité, appuyez sur __créer un fichier launch.json > Node.js__ pour créer un fichier `launch.json`.
   + Sinon, appuyez sur l’icône __Engrenage__ à droite du menu déroulant __Lancer le programme__ pour ouvrir la balise `launch.json` existante dans l’éditeur.
1. Ajoutez la configuration d’objet JSON suivante au tableau `configurations` :

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

1. Sélectionnez le nouveau __wskdebug__ dans la liste déroulante.
1. Appuyez sur le bouton vert __Exécuter__ à gauche de la liste déroulante __wskdebug__ .
1. Ouvrez `/actions/worker/index.js` et appuyez à gauche des numéros de ligne pour ajouter les points de rupture 1. Accédez à la fenêtre du navigateur Web de l’outil de développement Asset compute ouverte à l’étape 6.
1. Appuyez sur le bouton __Exécuter__ pour exécuter le programme de travail.
1. Revenez au code VS, à `/actions/worker/index.js` et parcourez le code.
1. Pour quitter l’outil de développement débogage, appuyez sur `Ctrl-C` dans le terminal qui a exécuté la commande `npx adobe-asset-compute devtool` à l’étape 6.

## Accès aux journaux à partir de Adobe I/O Runtime{#aio-app-logs}

[AEM en tant que Cloud Service exploite les objets Worker d’Asset compute via les ](../deploy/processing-profiles.md) profils de traitement en les appelant directement dans Adobe I/O Runtime. Comme ces appels n’impliquent pas de développement local, leurs exécutions ne peuvent pas être déboguées à l’aide d’outils locaux tels que l’outil de développement d’Asset compute ou wskdebug. Au lieu de cela, l’interface de ligne de commande de l’Adobe I/O peut être utilisée pour récupérer les journaux du programme de travail exécuté dans un espace de travail particulier de Adobe I/O Runtime.

1. Vérifiez que les [variables d’environnement spécifiques à l’espace de travail](../deploy/runtime.md) sont définies via `AIO_runtime_namespace` et `AIO_runtime_auth`, en fonction de l’espace de travail nécessitant un débogage.
1. Sur la ligne de commande, exécutez `aio app logs`
   + Si l’espace de travail génère un trafic important, développez le nombre de journaux d’activation à l’aide de l’indicateur `--limit` :
      `$ aio app logs --limit=25`
1. Les journaux d’activation les plus récents (jusqu’aux `--limit` fournis) seront renvoyés comme sortie de la commande pour révision.

   ![journaux de l’application aio](./assets/debug/aio-app-logs.png)

## Résolution des problèmes

+ [Debugger ne se joint pas](../troubleshooting.md#debugger-does-not-attach)
+ [Points d’arrêt en pause](../troubleshooting.md#breakpoints-no-pausing)
+ [Débogueur de code VS non joint](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Débogueur VS Code attaché après le début de l’exécution du programme de travail](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Le traitement expire lors du débogage](../troubleshooting.md#worker-times-out-while-debugging)
+ [Impossible d’arrêter le processus de débogage](../troubleshooting.md#cannot-terminate-debugger-process)
