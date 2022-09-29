---
title: Débogage d’un Asset compute Worker
description: Les objets Worker Asset compute peuvent être débogués de plusieurs façons, depuis des instructions de journal de débogage simples, jusqu’au code VS joint en tant que débogueur distant, jusqu’à l’extraction des journaux pour les activations dans Adobe I/O Runtime initiées à partir d’AEM as a Cloud Service.
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# Débogage d’un Asset compute Worker

Les objets Worker Asset compute peuvent être débogués de plusieurs façons, depuis des instructions de journal de débogage simples, jusqu’au code VS joint en tant que débogueur distant, jusqu’à l’extraction des journaux pour les activations dans Adobe I/O Runtime initiées à partir d’AEM as a Cloud Service.

## Journalisation

La forme la plus élémentaire de débogage des objets Worker Asset compute utilise les méthodes traditionnelles `console.log(..)` dans le code de travail. Le `console` L’objet JavaScript est un objet global implicite. Il n’est donc pas nécessaire de l’importer ni de l’exiger, car il est toujours présent dans tous les contextes.

Ces instructions de journal peuvent être examinées différemment selon la manière dont le programme de travail d’Asset compute est exécuté :

+ De `aio app run`, enregistre l’impression en sortie standard et le [Outil de développement](../develop/development-tool.md) Journaux d’activation
   ![l’application aio run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ De `aio app test`, consigne l’impression sur `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Utilisation `wskdebug`, consigne les instructions imprimées dans la console de débogage du code VS (Affichage > Console de débogage), sortie standard
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Utilisation `aio app logs`, les instructions du journal sont imprimées dans la sortie du journal d’activation.

## Débogage à distance via le débogueur joint

>[!WARNING]
>
>Utilisation de Microsoft Visual Studio Code 1.48.0 ou version ultérieure pour la compatibilité avec wskdebug

Le [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) module npm, prend en charge l’association d’un débogueur aux objets Worker Asset compute, y compris la possibilité de définir des points d’arrêt dans VS Code et de parcourir le code.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Clic publicitaire de débogage d’un objet Worker Asset compute à l’aide de wskdebug (pas d’audio)_

1. Assurez-vous que [wskdebug](../set-up/development-environment.md#wskdebug) et [ngrok](../set-up/development-environment.md#ngork) les modules npm sont installés.
1. Assurez-vous que [Docker Desktop et les images Docker prises en charge](../set-up/development-environment.md#docker) sont installés et en cours d’exécution ;
1. Fermez toutes les instances principales de l’outil de développement.
1. Déployez le code le plus récent à l’aide de `aio app deploy`  et enregistrez le nom de l’action déployée (nom entre la variable `[...]`). Elle permet de mettre à jour la variable `launch.json` à l’étape 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Démarrez une nouvelle instance de l’outil de développement d’Asset compute à l’aide de la commande . `npx adobe-asset-compute devtool`
1. Dans VS Code, appuyez sur l’icône Débogage dans le volet de navigation de gauche.
   + Si vous y êtes invité, appuyez sur __créer un fichier launch.json > Node.js ;__ pour créer `launch.json` fichier .
   + Sinon, appuyez sur __Gear__ à droite de l’icône __Programme Launch__ pour ouvrir la `launch.json` dans l’éditeur.
1. Ajoutez la configuration d’objet JSON suivante au `configurations` tableau :

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

1. Sélectionnez la nouvelle __wskdebug__ dans la liste déroulante
1. Appuyez sur le vert __Exécuter__ à gauche de __wskdebug__ menu déroulant
1. Ouvrir `/actions/worker/index.js` et appuyez sur à gauche des numéros de ligne pour ajouter les points de rupture 1. Accédez à la fenêtre du navigateur Web de l’outil de développement Asset compute ouverte à l’étape 6.
1. Appuyez sur le bouton __Exécuter__ pour exécuter le programme de travail
1. Revenez au code VS, pour `/actions/worker/index.js` et parcourir le code
1. Pour quitter l’outil de développement débogage, appuyez sur `Ctrl-C` dans le terminal qui s’est exécuté `npx adobe-asset-compute devtool` commande à l’étape 6

## Accès aux journaux à partir de Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service exploite les objets Worker d’Asset compute via les profils de traitement](../deploy/processing-profiles.md) en les appelant directement dans Adobe I/O Runtime. Comme ces appels n’impliquent pas de développement local, leurs exécutions ne peuvent pas être déboguées à l’aide d’outils locaux tels que l’outil de développement d’Asset compute ou wskdebug. Au lieu de cela, l’interface de ligne de commande de l’Adobe I/O peut être utilisée pour récupérer les journaux du programme de travail exécuté dans un espace de travail particulier de Adobe I/O Runtime.

1. Assurez-vous que la variable [variables d’environnement spécifiques à l’espace de travail](../deploy/runtime.md) sont définis via `AIO_runtime_namespace` et `AIO_runtime_auth`, en fonction de l’espace de travail nécessitant un débogage.
1. Sur la ligne de commande, exécutez `aio app logs`
   + Si l’espace de travail génère un trafic important, développez le nombre de journaux d’activation via la variable `--limit` Indicateur :
      `$ aio app logs --limit=25`
1. La plus récente (jusqu’à la valeur fournie `--limit`) les journaux d’activation sont renvoyés en tant que sortie de la commande pour révision.

   ![journaux de l’application aio](./assets/debug/aio-app-logs.png)

## Résolution des problèmes

+ [Debugger ne se joint pas](../troubleshooting.md#debugger-does-not-attach)
+ [Points d’arrêt en pause](../troubleshooting.md#breakpoints-no-pausing)
+ [Débogueur de code VS non joint](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Débogueur VS Code attaché après le début de l’exécution du programme de travail](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Le traitement expire lors du débogage](../troubleshooting.md#worker-times-out-while-debugging)
+ [Impossible d’arrêter le processus de débogage](../troubleshooting.md#cannot-terminate-debugger-process)
