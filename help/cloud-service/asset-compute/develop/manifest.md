---
title: Configuration du fichier manifest.yml d'un projet d'Asset compute
description: Le manifeste.yml du projet Asset compute décrit tous les travailleurs de ce projet à déployer.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 3%

---


# Configuration du fichier manifest.yml

Le `manifest.yml`, situé à la racine du projet d&#39;Asset compute, décrit tous les travailleurs de ce projet à déployer.

![manifest.yml](./assets/manifest/manifest.png)

## Définition de travailleur par défaut

Les travailleurs sont définis comme des entrées d&#39;action Adobe I/O Runtime sous `actions` et sont constitués d&#39;un ensemble de configurations.

Les utilisateurs qui accèdent à d&#39;autres intégrations d&#39;Adobe I/O doivent définir la propriété `annotations -> require-adobe-auth` sur `true`, dans la mesure où cette [exposition les informations d&#39;identification de l&#39;Adobe I/O du travailleur](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) via l&#39;objet `params.auth`. Cela est généralement requis lorsque le travailleur appelle des API d’Adobe I/O telles que les API Adobe Photoshop, Lightroom ou Sensei, et peut être basculé par employé.

1. Ouvrez et passez en revue le programme de travail généré automatiquement `manifest.yml`. Les projets qui contiennent plusieurs travailleurs d&#39;Asset compute doivent définir une entrée pour chaque travailleur sous la baie `actions`.

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

Chaque collaborateur peut configurer les [limites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) pour son contexte d’exécution dans Adobe I/O Runtime. Ces valeurs doivent être affinées pour fournir un dimensionnement optimal au travailleur, en fonction du volume, du taux et du type de ressources qu&#39;il calculera, ainsi que du type de travail qu&#39;il effectue.

Examinez [les conseils relatifs au dimensionnement des Adobes](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) avant de définir des limites. Les employés d’Asset compute peuvent manquer de mémoire lors du traitement des actifs, ce qui entraîne la mort de l’exécution de Adobe I/O Runtime. Assurez-vous donc que le collaborateur est dimensionné de manière appropriée pour gérer tous les actifs candidats.

1. Ajoutez une section `inputs` à la nouvelle entrée d&#39;actions `wknd-asset-compute`. Cela permet d&#39;ajuster la performance globale du travailleur d&#39;Asset compute et l&#39;allocation des ressources.

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

Le `manifest.yml` final ressemble à ceci :

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

Le `.manifest.yml` final est disponible sur Github à l&#39;adresse suivante :

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validation du fichier manifest.yml

Une fois que l&#39;Asset compute `manifest.yml` généré est mis à jour, exécutez l&#39;outil de développement local et assurez-vous que les débuts correspondent aux paramètres `manifest.yml` mis à jour.

Outil de développement de l&#39;Asset compute début pour le projet d&#39;Asset compute :

1. Ouvrez une ligne de commande dans la racine du projet d&#39;Asset compute (dans le code VS, vous pouvez l&#39;ouvrir directement dans l&#39;IDE via Terminal > New Terminal) et exécutez la commande :

   ```
   $ aio app run
   ```

1. L&#39;outil de développement d&#39;Asset compute local s&#39;ouvre dans votre navigateur Web par défaut à l&#39;adresse __http://localhost:9000__.

   ![exécution de l’application aio](assets/environment-variables/aio-app-run.png)

1. Recherchez les messages d’erreur dans la sortie de ligne de commande et dans le navigateur Web au fur et à mesure que l’outil de développement s’initialise.
1. Pour arrêter l&#39;outil de développement d&#39;Asset compute, appuyez sur `Ctrl-C` dans la fenêtre qui a exécuté `aio app run` pour arrêter le processus.

## Résolution des incidents

+ [Retrait YAML incorrect](../troubleshooting.md#incorrect-yaml-indentation)
+ [MemorySize limit est trop faible](../troubleshooting.md#memorysize-limit-is-set-too-low)
