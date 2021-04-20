---
title: Configuration des variables d'environnement pour l'extensibilité de l'Asset compute
description: Les variables d’Environnement sont conservées dans le fichier .env pour le développement local et sont utilisées pour fournir les informations d’identification d’Adobe I/O et d’enregistrement cloud requises pour le développement local.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---


# Configuration des variables d’environnement 

![fichier env](assets/environment-variables/dot-env-file.png)

Avant de commencer le développement des travailleurs d’Asset compute, assurez-vous que le projet est configuré avec les informations d’Adobe I/O et d’enregistrement de cloud. Ces informations sont stockées dans `.env` du projet, qui n&#39;est utilisé que pour le développement local, et non dans Git. Le fichier `.env` fournit un moyen pratique d’exposer les paires clé/valeur à l’environnement de développement local de l’Asset compute local. Lorsque [déployait ](../deploy/runtime.md) les travailleurs d’Asset compute vers Adobe I/O Runtime, le fichier `.env` n’est pas utilisé, mais un sous-ensemble de valeurs est transmis par le biais de variables d’environnement. D’autres paramètres et secrets personnalisés peuvent également être stockés dans le fichier `.env`, tels que les informations d’identification de développement pour les services Web tiers.

## Référencer `private.key`

![clé privée](assets/environment-variables/private-key.png)

Ouvrez le fichier `.env`, annulez la mise en commentaire de la clé `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` et indiquez le chemin absolu sur votre système de fichiers vers le `private.key` qui correspond au certificat public ajouté à votre projet FireFly Adobe I/O.

+ Si votre paire de clés a été générée par l&#39;Adobe I/O, elle a été automatiquement téléchargée dans le cadre du `config.zip`.
+ Si vous avez fourni la clé publique à l&#39;Adobe I/O, vous devriez également être en possession de la clé privée correspondante.
+ Si vous ne disposez pas de ces paires de clés, vous pouvez générer de nouvelles paires de clés ou télécharger de nouvelles clés publiques au bas de :
   [https://console.adobe.com](https://console.adobe.io) > Votre projet de luciole d&#39;Asset compute > Workspaces @ Development > Service Account (JWT).

Rappelez-vous que le fichier `private.key` ne doit pas être archivé dans Git car il contient des secrets, mais qu&#39;il doit être stocké dans un endroit sûr en dehors du projet.

Par exemple, sur macOS, ceci peut se présenter comme suit :

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configuration des informations d’identification de l’Enregistrement Cloud

Le développement local des employés d&#39;Asset compute nécessite l&#39;accès à l&#39;[enregistrement de nuage](../set-up/accounts-and-services.md#cloud-storage). Les informations d’identification de l’enregistrement cloud utilisées pour le développement local sont fournies dans le fichier `.env`.

Ce tutoriel préfère l&#39;utilisation de l&#39;Enregistrement Azure Blob, cependant Amazon S3 et ses clés correspondantes dans le fichier `.env` peuvent être utilisées à la place.

### Utilisation de l&#39;Enregistrement Azure Blob

Supprimez les commentaires et renseignez les clés suivantes dans le fichier `.env`, puis renseignez-les avec les valeurs de l&#39;enregistrement de cloud mis en service sur Azure Portal.

![Enregistrement Blob Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valeur de la clé `AZURE_STORAGE_CONTAINER_NAME`
1. Valeur de la clé `AZURE_STORAGE_ACCOUNT`
1. Valeur de la clé `AZURE_STORAGE_KEY`

Par exemple, ceci peut ressembler à (valeurs pour l’illustration uniquement) :

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Le fichier `.env` résultant se présente comme suit :

![Informations d&#39;identification de l&#39;Enregistrement Azure Blob](assets/environment-variables/cloud-storage-credentials.png)

Si vous n&#39;utilisez PAS l&#39;Enregistrement Blob Microsoft Azure, supprimez ou laissez-les en commentaire (en préfixe avec `#`).

### Utilisation de l’enregistrement cloud Amazon S3{#amazon-s3}

Si vous utilisez l’enregistrement cloud Amazon S3 sans commentaire et que vous renseignez les clés suivantes dans le fichier `.env`.

Par exemple, ceci peut ressembler à (valeurs pour l’illustration uniquement) :

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validation de la configuration du projet

Une fois le projet d&#39;Asset compute généré configuré, validez la configuration avant d&#39;apporter des modifications au code pour vous assurer que les services de prise en charge sont configurés, dans les fichiers `.env`.

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

+ [L&#39;outil de développement ne peut pas être début en raison de l&#39;absence de private.key](../troubleshooting.md#missing-private-key)
