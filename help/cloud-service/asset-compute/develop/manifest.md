---
title: Configuration du fichier manifest.yml d’un projet Asset Compute
description: Le fichier manifest.yml du projet Asset Compute décrit tous les travailleurs de ce projet à déployer.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Configuration du fichier manifest.yml

Le `manifest.yml`, situé à la racine du projet Asset Compute, décrit tous les travailleurs de ce projet à déployer.

![manifest.yml](./assets/manifest/manifest.png)

## Définition de travailleur par défaut

Les travailleurs sont définis comme des entrées d&#39;action Adobe I/O Runtime sous `actions`et sont constitués d&#39;un ensemble de configurations.

Les travailleurs qui accèdent à d&#39;autres intégrations d&#39;E/S d&#39;Adobe doivent définir la `annotations -> require-adobe-auth` propriété sur `true` car cela [expose les informations d&#39;identification](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) d&#39;E/S d&#39;Adobe du travailleur via l&#39; `params.auth` objet. Cela est généralement requis lorsque le travailleur appelle des API d&#39;E/S d&#39;Adobe, telles que les API Adobe Photoshop, Lightroom ou Sensei, et peut être basculé par employé.

1. Ouvrez et passez en revue le travailleur généré automatiquement `manifest.yml`. Les projets qui contiennent plusieurs programmes de travail Asset Compute doivent définir une entrée pour chaque programme de travail sous la `actions` baie.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## Définir des limites

Chaque collaborateur peut configurer les [limites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) de son contexte d’exécution dans Adobe I/O Runtime. Ces valeurs doivent être affinées pour fournir un dimensionnement optimal au travailleur, en fonction du volume, du taux et du type de ressources qu&#39;il calculera, ainsi que du type de travail qu&#39;il effectue.

Examinez les conseils [relatifs au dimensionnement des](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) Adobes avant de fixer des limites. Les employés de Asset Compute peuvent manquer de mémoire lors du traitement des ressources, ce qui entraîne la mort de l’exécution Adobe I/O Runtime. Assurez-vous donc que le collaborateur est dimensionné de manière appropriée pour gérer toutes les ressources candidates.

1. Ajoutez une `inputs` section à la nouvelle entrée `wknd-asset-compute` Actions. Cela permet d&#39;ajuster les performances globales et l&#39;allocation des ressources du travailleur Asset Compute.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## Le manifeste terminé.yml

La finale `manifest.yml` ressemble à ceci :

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## manifest.yml sur Github

Le dernier `.manifest.yml` est disponible sur Github à l&#39;adresse :

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validation du fichier manifest.yml

Une fois que le calcul d’actifs généré `manifest.yml` est mis à jour, exécutez l’outil de développement local et vérifiez que les débuts ont bien accès aux `manifest.yml` paramètres mis à jour.

Pour début à Asset Compute Development Tool pour le projet Asset Compute :

1. Ouvrez une ligne de commande dans la racine du projet Asset Compute (dans le code VS, vous pouvez l&#39;ouvrir directement dans l&#39;IDE via Terminal > New Terminal), puis exécutez la commande :

   ```
   $ aio app run
   ```

1. L’outil de développement de calcul de ressources local s’ouvre dans votre navigateur Web par défaut à l’adresse __http://localhost:9000__.

   ![exécution de l’application aio](assets/environment-variables/aio-app-run.png)

1. Recherchez les messages d’erreur dans la sortie de ligne de commande et dans le navigateur Web au fur et à mesure que l’outil de développement s’initialise.
1. Pour arrêter l’outil de développement de calcul de ressources, appuyez sur `Ctrl-C` la fenêtre qui s’est exécutée `aio app run` pour arrêter le processus.

## Résolution des incidents

+ [Retrait YAML incorrect](../troubleshooting.md#incorrect-yaml-indentation)
+ [MemorySize limit est trop faible](../troubleshooting.md#memorysize-limit-is-set-too-low)
