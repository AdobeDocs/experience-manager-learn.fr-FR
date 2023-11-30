---
title: Configurer le fichier manifest.yml d’un projet Asset Compute
description: Le fichier manifest.yml du projet Asset Compute décrit tous les programmes de travail du projet à déployer.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 100%

---

# Configurer le fichier manifest.yml

Le fichier `manifest.yml`, dans la racine du projet Asset Compute, décrit tous les programmes de travail du projet à déployer.

![manifest.yml](./assets/manifest/manifest.png)

## Définition du programme de travail par défaut

Les programmes de travail sont définis comme des entrées d’action Adobe I/O Runtime sous `actions`. Ils sont composés d’un ensemble de configurations.

Les programmes de travail accédant à d’autres intégrations Adobe I/O doivent définir la propriété `annotations -> require-adobe-auth` sur `true`, car cela [expose les informations d’identification d’Adobe I/O du programme de travail](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=fr#access-adobe-apis) via l’objet `params.auth`. Cette directive est généralement obligatoire lorsque le programme de travail appelle des API d’Adobe I/O telles que les API Adobe Photoshop, Lightroom ou Sensei. L’activation/désactivation est possible pour chaque programme de travail.

1. Ouvrez et examinez le programme de travail `manifest.yml` généré automatiquement. Les projets qui contiennent plusieurs programmes de travail Asset Compute doivent définir une entrée pour chaque programme de travail sous le tableau `actions`.

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

## Définir les limites

Chaque programme de travail peut configurer les [limites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) pour son contexte d’exécution dans Adobe I/O Runtime. Ces valeurs doivent être affinées afin de fournir un dimensionnement optimal pour le programme de travail, en fonction du volume, du taux et du type de ressources qu’il va calculer, ainsi que du type de travail qu’il effectue.

Lisez [Conseils sur le dimensionnement d’Adobe](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=fr#sizing-workers) avant de définir des limites. Les programmes de travail Asset Compute peuvent manquer de mémoire lors du traitement des ressources, ce qui entraîne la fin de l’exécution d’Adobe I/O Runtime. Assurez-vous donc que le programme de travail est dimensionné de manière appropriée pour gérer toutes les ressources candidates.

1. Ajoutez une section `inputs` à la nouvelle entrée d’actions `wknd-asset-compute`. Cela permet d’ajuster les performances globales et l’allocation des ressources du programme de travail Asset Compute.

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

La version finale de `manifest.yml` se présente comme suit :

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

La version finale de `.manifest.yml` est disponible sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Valider le fichier manifest.yml

Une fois le `manifest.yml` Asset Compute généré et mis à jour, exécutez l’outil de développement local et assurez-vous qu’il commence avec la mise à jour des paramètres de `manifest.yml`.

Pour lancer l’outil de développement d’Asset Compute pour le projet d’Asset Compute :

1. Ouvrez une ligne de commande dans la racine du projet d’Asset Compute (dans VS Code, elle peut être ouverte directement dans l’IDE via Terminal > Nouveau terminal) et exécutez la commande :

   ```
   $ aio app run
   ```

1. L’outil de développement d’Asset Compute local s’ouvre dans votre navigateur web par défaut à l’adresse __http://localhost:9000__.

   ![Exécution de l’application AIO.](assets/environment-variables/aio-app-run.png)

1. Recherchez les messages d’erreur dans la sortie de ligne de commande et dans le navigateur web à mesure que l’outil de développement s’initialise.
1. Pour arrêter l’outil de développement d’Asset Compute, appuyez sur `Ctrl-C` dans la fenêtre qui a exécuté `aio app run` pour terminer le processus.

## Résolution des problèmes

+ [Mise en retrait incorrecte de YAML](../troubleshooting.md#incorrect-yaml-indentation)
+ [La limite memorySize est définie trop basse](../troubleshooting.md#memorysize-limit-is-set-too-low)
