---
title: Configurer les variables d’environnement pour l’extensibilité d’Asset Compute
description: Les variables d’environnement sont conservées dans le fichier .env pour le développement local et sont utilisées pour fournir des informations d’identification d’Adobe I/O et de l’espace de stockage dans le cloud requises pour le développement local.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 156
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 100%

---

# Configurer les variables d’environnement

![Fichier .env.](assets/environment-variables/dot-env-file.png)

Avant de développer des objets de travail Asset Compute, assurez-vous que le projet est configuré avec les informations d’Adobe I/O et de stockage dans le cloud. Ces informations sont stockées dans le fichier `.env` du projet qui est utilisé uniquement pour le développement local, et n’est pas enregistré dans Git. Le fichier `.env` fournit un moyen pratique d’exposer les paires clé/valeur à l’environnement de développement local d’Asset Compute. Lorsque le [déploiement](../deploy/runtime.md) des ressources de travail d’Asset Compute dans Adobe I/O Runtime, le fichier `.env` n’est pas utilisé, mais un sous-ensemble de valeurs est transmis par le biais de variables d’environnement. D’autres paramètres et secrets personnalisés peuvent être stockés dans le fichier `.env`, par exemple les informations d’identification de développement pour les services web tiers.

## Référencer le fichier `private.key`

![Clé privée.](assets/environment-variables/private-key.png)

Ouvrez le fichier `.env`, supprimez les commentaires de la clé `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` et fournissez le chemin d’accès absolu dans votre système de fichiers à la `private.key` qui correspond au certificat public ajouté à votre projet de créateur d’applications Adobe I/O.

+ Si votre paire de clés a été générée par Adobe I/O, elle a été automatiquement téléchargée dans `config.zip`.
+ Si vous avez fourni la clé publique à Adobe I/O, vous devez également être en possession de la clé privée correspondante.
+ Si vous ne disposez pas de ces paires de clés, vous pouvez générer de nouvelles paires de clés ou charger de nouvelles clés publiques au bas de :
  [https://console.adobe.com](https://console.adobe.io) > Votre projet de créateur d’applications Asset Compute > Workspaces @ Développement > Compte de service (JWT).

N’oubliez pas que le fichier `private.key` ne doit pas être archivé dans Git, car il contient des secrets. Il doit plutôt être stocké dans un emplacement sécurisé en dehors du projet.

Par exemple, sur macOS, cela peut se présenter comme suit :

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurer les informations d’identification de l’espace de stockage dans le cloud

Le développement local d’Assets Compute nécessite l’accès à un [espace de stockage dans le cloud](../set-up/accounts-and-services.md#cloud-storage). Les informations d’identification de l’espace de stockage dans le cloud utilisées pour le développement local sont fournies dans le fichier `.env`.

Ce tutoriel privilégie l’utilisation du stockage Azure Blob, mais Amazon S3 et ses clés correspondantes dans le fichier `.env` peuvent être utilisés à la place.

### Utiliser le stockage Azure Blob

Supprimez les commentaires, renseignez les clés suivantes dans le fichier `.env` et renseignez-les avec les valeurs de l’espace de stockage dans le cloud configuré sur Azure Portal.

![Stockage Azure Blob.](./assets/environment-variables/azure-portal-credentials.png)

1. Valeur de la clé `AZURE_STORAGE_CONTAINER_NAME`
1. Valeur de la clé `AZURE_STORAGE_ACCOUNT`
1. Valeur de la clé `AZURE_STORAGE_KEY`

Par exemple, cela peut se présenter comme suit (valeurs à titre d’exemple uniquement) :

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Le fichier `.env` se présente comme suit :

![Informations d’identification du stockage Azure Blob.](assets/environment-variables/cloud-storage-credentials.png)

Si vous n’utilisez PAS le stockage Microsoft Azure Blob, supprimez ou laissez les commentaires (en ajoutant le préfixe `#`).

### Utiliser l’espace de stockage dans le cloud Amazon S3{#amazon-s3}

Si vous utilisez l’espace de stockage dans le cloud Amazon S3, supprimez les commentaires et renseignez les clés suivantes dans le fichier `.env`.

Par exemple, cela peut se présenter comme suit (valeurs à titre d’exemple uniquement) :

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Valider la configuration du projet

Une fois le projet d’Asset Compute généré et configuré, validez la configuration avant d’apporter des modifications au code pour vous assurer que les services de prise en charge sont configurés, dans les fichiers `.env`.

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

+ [L’outil de développement ne peut pas démarrer en raison d’une clé privée manquante](../troubleshooting.md#missing-private-key)
