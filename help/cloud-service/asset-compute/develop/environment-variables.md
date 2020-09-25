---
title: Configuration des variables d’environnement pour l’extensibilité Asset Compute
description: Les variables d'Environnement sont conservées dans le fichier .env pour le développement local et sont utilisées pour fournir des informations d'identification d'E/S d'Adobe et des informations d'enregistrement de cloud requises pour le développement local.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 1%

---


# Configuration des variables d’environnement 

![fichier env](assets/environment-variables/dot-env-file.png)

Avant de commencer le développement des agents d&#39;Asset Compute, assurez-vous que le projet est configuré avec les informations d&#39;E/S d&#39;Adobe et d&#39;enregistrement de cloud. Ces informations sont stockées dans le projet `.env` qui n&#39;est utilisé que pour le développement local, et non pas sauvegardé dans Git. Le `.env` fichier fournit un moyen pratique d’exposer les paires clé/valeur à l’environnement de développement local Asset Compute. Lors du [déploiement](../deploy/runtime.md) des agents Asset Compute sur Adobe I/O Runtime, le `.env` fichier n’est pas utilisé, mais un sous-ensemble de valeurs est transmis par le biais de variables d’environnement. D’autres paramètres et secrets personnalisés peuvent également être stockés dans le `.env` fichier, tels que les informations d’identification de développement pour les services Web tiers.

## Référencez la variable `private.key`

![clé privée](assets/environment-variables/private-key.png)

Ouvrez le `.env` fichier, annulez la mise en commentaire de la `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` clé et fournissez le chemin absolu sur votre système de fichiers vers le `private.key` qui correspond au certificat public ajouté à votre projet d&#39;E/S FireFly d&#39;Adobe.

+ Si votre paire de clés a été générée par E/S d&#39;Adobe, elle a été automatiquement téléchargée dans le cadre du `config.zip`programme.
+ Si vous avez fourni la clé publique pour les E/S d&#39;Adobe, vous devriez également être en possession de la clé privée correspondante.
+ Si vous ne disposez pas de ces paires de clés, vous pouvez générer de nouvelles paires de clés ou télécharger de nouvelles clés publiques au bas de :
   [https://console.adobe.com](https://console.adobe.io) > Your Asset Compute Firefly project > Workspaces @ Development > Service Account (JWT).

Rappelez-vous que le `private.key` fichier ne doit pas être archivé dans Git car il contient des secrets, mais qu&#39;il doit être stocké dans un endroit sûr en dehors du projet.

Par exemple, sur macOS, ceci peut se présenter comme suit :

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configuration des informations d’identification de l’Enregistrement Cloud

Le développement local des employés d&#39;Asset Compute nécessite l&#39;accès à l&#39;enregistrement [](../set-up/accounts-and-services.md#cloud-storage)cloud. Les informations d’identification de l’enregistrement cloud utilisées pour le développement local sont fournies dans le `.env` fichier.

Ce tutoriel préfère l&#39;utilisation de l&#39;Enregistrement Azure Blob, cependant Amazon S3 et ses clés correspondantes dans le `.env` fichier peuvent être utilisés à la place.

### Utilisation de l&#39;Enregistrement Azure Blob

Supprimez les commentaires et renseignez les clés suivantes dans le `.env` fichier, puis renseignez-les avec les valeurs de l&#39;enregistrement de cloud mis en service sur Azure Portal.

![Enregistrement Blob Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valeur de la `AZURE_STORAGE_CONTAINER_NAME` clé
1. Valeur de la `AZURE_STORAGE_ACCOUNT` clé
1. Valeur de la `AZURE_STORAGE_KEY` clé

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

Si vous utilisez l’enregistrement cloud Amazon S3, annulez la mise en commentaire et renseignez les clés suivantes dans le `.env` fichier.

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

Une fois que le projet Asset Compute généré a été configuré, validez la configuration avant de modifier le code pour vous assurer que les services de prise en charge sont configurés, dans les `.env` fichiers.

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

### Les outils de développement local Asset Compute ne peuvent pas être débuts en raison de l’absence de private.key

+ __Erreur :__ Erreur du serveur de développement local : Fichiers requis manquants à validatePrivateKeyFile.... (via standard out from `aio app run` command)
+ __Cause :__ La `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valeur dans `.env` le fichier, ne pointe pas vers `private.key` ou `private.key` n&#39;est pas lisible par l&#39;utilisateur actuel.
+ __Résolution :__ Vérifiez la `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valeur du `.env` fichier et assurez-vous qu’elle contient le chemin d’accès complet et absolu au fichier `private.key` sur votre système de fichiers.
