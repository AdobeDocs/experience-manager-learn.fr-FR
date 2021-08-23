---
title: Configuration du fichier manifest.yml d’un projet Asset compute
description: Le fichier manifest.yml du projet Asset compute décrit tous les objets Worker de ce projet à déployer.
feature: Microservices Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Intégrations, développement
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# Configuration du fichier manifest.yml

La section `manifest.yml`, située à la racine du projet Asset compute, décrit tous les programmes de travail de ce projet à déployer.

![manifest.yml](./assets/manifest/manifest.png)

## Définition du programme de travail par défaut

Les programmes de travail sont définis comme des entrées d’action Adobe I/O Runtime sous `actions` et se composent d’un ensemble de configurations.

Les utilisateurs accédant à d’autres intégrations d’Adobe I/O doivent définir la propriété `annotations -> require-adobe-auth` sur `true`, car cette [expose les informations d’identification de l’Adobe I/O du worker](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) via l’objet `params.auth`. Cela est généralement requis lorsque le programme de travail appelle des API d’Adobe I/O telles que les API Adobe Photoshop, Lightroom ou Sensei, et peut être basculé par programme de travail.

1. Ouvrez et passez en revue le programme de travail généré automatiquement `manifest.yml`. Les projets qui contiennent plusieurs objets Worker Asset compute doivent définir une entrée pour chaque objet Worker sous le tableau `actions` .

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

## Définition de limites

Chaque programme de travail peut configurer les [limites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) pour son contexte d’exécution dans Adobe I/O Runtime. Ces valeurs doivent être affinées afin de fournir un dimensionnement optimal pour le programme de travail, en fonction du volume, du taux et du type de ressources qu’il va calculer, ainsi que du type de travail qu’il effectue.

Consultez les [conseils sur le dimensionnement des Adobes](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) avant de définir des limites. Les objets Worker Asset compute peuvent manquer de mémoire lors du traitement des ressources, ce qui entraîne la fin de l’exécution de Adobe I/O Runtime. Assurez-vous donc que le programme de travail est dimensionné de manière appropriée pour gérer toutes les ressources candidates.

1. Ajoutez une section `inputs` à la nouvelle entrée d’actions `wknd-asset-compute`. Cela permet d’ajuster les performances globales et l’allocation des ressources du travailleur d’Asset compute.

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

## manifest.yml terminé

La dernière `manifest.yml` ressemble à ceci :

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

Le dernier `.manifest.yml` est disponible sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validation du fichier manifest.yml

Une fois que l’Asset compute `manifest.yml` généré est mis à jour, exécutez l’outil de développement local et assurez-vous que commence avec les paramètres `manifest.yml` mis à jour.

Pour lancer l’outil de développement d’Asset compute pour le projet d’Asset compute :

1. Ouvrez une ligne de commande dans la racine du projet d’Asset compute (dans VS Code, elle peut être ouverte directement dans l’IDE via Terminal > New Terminal) et exécutez la commande :

   ```
   $ aio app run
   ```

1. L’outil de développement d’Asset compute local s’ouvre dans votre navigateur Web par défaut à l’adresse __http://localhost:9000__.

   ![exécution de l’application aio](assets/environment-variables/aio-app-run.png)

1. Recherchez les messages d’erreur dans la sortie de ligne de commande et dans le navigateur Web à mesure que l’outil de développement s’initialise.
1. Pour arrêter l’outil de développement d’Asset compute, appuyez sur `Ctrl-C` dans la fenêtre qui a exécuté `aio app run` pour arrêter le processus.

## Résolution des problèmes

+ [Retrait YAML incorrect](../troubleshooting.md#incorrect-yaml-indentation)
+ [La limite memorySize est définie trop basse](../troubleshooting.md#memorysize-limit-is-set-too-low)
