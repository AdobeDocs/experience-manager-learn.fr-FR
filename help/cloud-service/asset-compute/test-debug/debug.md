---
title: Débogage d’un intervenant de calcul de ressources
description: Les agents Asset Compute peuvent être débogués de plusieurs manières, depuis les instructions de journal de débogage simples, jusqu'au code VS joint en tant que débogueur distant, jusqu'à l'extraction des journaux pour les activations dans Adobe I/O Runtime initiée à partir d'AEM en tant que Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# Débogage d’un intervenant de calcul de ressources

Les agents Asset Compute peuvent être débogués de plusieurs manières, depuis les instructions de journal de débogage simples, jusqu&#39;au code VS joint en tant que débogueur distant, jusqu&#39;à l&#39;extraction des journaux pour les activations dans Adobe I/O Runtime initiée à partir d&#39;AEM en tant que Cloud Service.

## Journalisation

La forme la plus élémentaire de débogage des travailleurs de Asset Compute utilise des `console.log(..)` instructions traditionnelles dans le code de travail. L’objet `console` JavaScript est un objet implicite global. Il n’est donc pas nécessaire de l’importer ou de l’exiger, car il est toujours présent dans tous les contextes.

Les instructions de journal suivantes peuvent faire l’objet d’une révision différente selon la manière dont le programme de travail Asset Compute est exécuté :

+ De `aio app run`, les journaux s&#39;impriment en standard et les [journaux d&#39;Activation de l&#39;outil de](../develop/development-tool.md) développement
   ![application aio exécuter console.log(..)](./assets/debug/console-log__aio-app-run.png)
+ A partir de `aio app test`, les journaux s&#39;impriment en `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ A l&#39;aide `wskdebug`de, consigne les instructions imprimées dans la console de débogage du code VS (Vue > Console de débogage), sortie standard
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Les instructions de journal `aio app logs`s’impriment dans la sortie du journal des activations.

## Débogage à distance via un débogueur joint

>[!WARNING]
>
>Utiliser Microsoft Visual Studio Code 1.48.0 ou version ultérieure pour la compatibilité avec wskdebug

Le module npm [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) prend en charge l&#39;association d&#39;un débogueur aux agents Asset Compute, y compris la possibilité de définir des points d&#39;arrêt dans le code VS et de parcourir le code.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Clic publicitaire de débogage d’un intervenant Asset Compute à l’aide de wskdebug (sans audio)_

1. Vérifier que les modules [wskdebug](../set-up/development-environment.md#wskdebug) et [ngrok](../set-up/development-environment.md#ngork) npm sont installés
1. Assurez-vous que [Docker Desktop et les images](../set-up/development-environment.md#docker) Docker prises en charge sont installées et en cours d&#39;exécution.
1. Fermez toutes les instances principales en cours d’exécution de l’outil de développement.
1. Déployez le code le plus récent à l’aide `aio app deploy` et enregistrez le nom de l’action déployée (nom entre le `[...]`). Elle sera utilisée pour mettre à jour la `launch.json` à l’étape 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Début d’une nouvelle instance de Asset Compute Development Tool à l’aide de la commande `npx adobe-asset-compute devtool`
1. Dans le code VS, appuyez sur l’icône Débogage dans le volet de navigation de gauche.
   + Si vous y êtes invité, appuyez sur __créer un fichier launch.json > Node.js__ pour créer un nouveau `launch.json` fichier.
   + Sinon, appuyez sur l’icône __Engrenage__ à droite de la liste déroulante Programme __de__ lancement pour ouvrir l’élément existant `launch.json` dans l’éditeur.
1. Ajoutez la configuration d’objet JSON suivante sur le `configurations` tableau :

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

1. Sélectionnez le nouveau __débogage__ dans la liste déroulante.
1. Appuyez sur le bouton vert __Exécuter__ à gauche de la liste déroulante __wskdebug__ .
1. Ouvrez `/actions/worker/index.js` et appuyez sur à gauche des numéros de ligne pour ajouter des points de rupture 1. Accédez à la fenêtre du navigateur Web de l&#39;outil de développement de calcul des actifs ouverte à l&#39;étape 6.
1. Appuyez sur le bouton __Exécuter__ pour exécuter le programme de travail.
1. Revenez au code VS, au code `/actions/worker/index.js` et parcourez-le.
1. Pour quitter l&#39;outil de développement débogage, appuyez sur `Ctrl-C` le terminal qui a exécuté `npx adobe-asset-compute devtool` la commande à l&#39;étape 6.

## Accès aux journaux à partir de Adobe I/O Runtime{#aio-app-logs}

[aem en tant que Cloud Service exploite les employés d&#39;Asset Compute via les Profils](../deploy/processing-profiles.md) de traitement en les appelant directement dans Adobe I/O Runtime. Comme ces appels n’impliquent pas de développement local, leurs exécutions ne peuvent pas être déboguées à l’aide d’outils locaux tels que Asset Compute Development Tool ou wskdebug. A la place, l&#39;interface de ligne de commande E/S Adobe peut être utilisée pour récupérer les journaux du collaborateur exécuté dans un espace de travail particulier de Adobe I/O Runtime.

1. Assurez-vous que les variables [d’environnement propres à](../deploy/runtime.md) l’espace de travail sont définies via `AIO_runtime_namespace` et `AIO_runtime_auth`, en fonction de l’espace de travail nécessitant un débogage.
1. Dans la ligne de commande, exécutez `aio app logs`
   + Si l’espace de travail génère un trafic important, augmentez le nombre de journaux d’activation via l’ `--limit` indicateur :
      `$ aio app logs --limit=25`
1. Les journaux d&#39;activations les plus récents (jusqu&#39;aux `--limit`journaux fournis) seront renvoyés en tant que sortie de la commande pour révision.

   ![journaux de l’application aio](./assets/debug/aio-app-logs.png)

## Résolution des incidents

+ [Le débogueur n’est pas joint](../troubleshooting.md#debugger-does-not-attach)
+ [Points d’arrêt non suspendus](../troubleshooting.md#breakpoints-no-pausing)
+ [Débogueur de code VS non joint](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Débogueur de code VS attaché après le début de l&#39;exécution du programme de travail](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Le programme de travail expire lors du débogage](../troubleshooting.md#worker-times-out-while-debugging)
+ [Impossible d&#39;arrêter le processus de débogage](../troubleshooting.md#cannot-terminate-debugger-process)
