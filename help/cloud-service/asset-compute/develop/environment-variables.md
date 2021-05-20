---
title: Configuration des variables d’environnement pour l’extensibilité des Assets compute
description: Les variables d’environnement sont conservées dans le fichier .env pour le développement local et sont utilisées pour fournir des informations d’identification d’Adobe I/O et des informations d’identification de l’espace de stockage requises pour le développement local.
feature: Microservices Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Intégrations, développement
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# Configuration des variables d’environnement 

![fichier env de point](assets/environment-variables/dot-env-file.png)

Avant de commencer le développement des objets Worker Asset compute, assurez-vous que le projet est configuré avec les informations d’Adobe I/O et de stockage dans le cloud. Ces informations sont stockées dans la balise `.env` du projet, qui n’est utilisée que pour le développement local, et non dans Git. Le fichier `.env` permet d’exposer facilement des paires clé/valeur dans l’environnement de développement local de l’Asset compute local. Lorsque [déployant](../deploy/runtime.md) Asset compute des objets Worker à Adobe I/O Runtime, le fichier `.env` n’est pas utilisé, mais un sous-ensemble de valeurs est transmis par le biais de variables d’environnement. D’autres paramètres et secrets personnalisés peuvent également être stockés dans le fichier `.env`, tels que les informations d’identification de développement pour les services web tiers.

## Référencez `private.key`

![clé privée](assets/environment-variables/private-key.png)

Ouvrez le fichier `.env`, annulez la mise en commentaire de la clé `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` et indiquez le chemin absolu sur votre système de fichiers vers la balise `private.key` qui correspond au certificat public ajouté à votre projet FireFly d’Adobe I/O.

+ Si votre paire de clés a été générée par Adobe I/O, elle a été automatiquement téléchargée dans le cadre de la fonction `config.zip`.
+ Si vous avez fourni la clé publique à Adobe I/O, vous devez également être en possession de la clé privée correspondante.
+ Si vous ne disposez pas de ces paires de clés, vous pouvez générer de nouvelles paires de clés ou charger de nouvelles clés publiques au bas de :
   [https://console.adobe.com](https://console.adobe.io)  > Votre projet Asset compute Firefly > Espaces de travail @ Développement > Compte de service (JWT).

Souvenez-vous que le fichier `private.key` ne doit pas être archivé dans Git, car il contient des secrets. Il doit plutôt être stocké dans un emplacement sécurisé en dehors du projet.

Par exemple, sur macOS, cela peut se présenter comme suit :

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configuration des informations d’identification de Cloud Storage

Le développement local des employés d’Asset compute nécessite l’accès à l’[espace de stockage dans le cloud](../set-up/accounts-and-services.md#cloud-storage). Les informations d’identification de l’espace de stockage dans le cloud utilisées pour le développement local sont fournies dans le fichier `.env` .

Ce tutoriel préfère l’utilisation du stockage Azure Blob. Toutefois, Amazon S3 et ses clés correspondantes dans le fichier `.env` peuvent être utilisées à la place.

### Utilisation du stockage Azure Blob

Supprimez les commentaires et renseignez les clés suivantes dans le fichier `.env` et renseignez-les avec les valeurs de l’espace de stockage cloud configuré sur Azure Portal.

![Stockage Azure Blob](./assets/environment-variables/azure-portal-credentials.png)

1. Valeur de la clé `AZURE_STORAGE_CONTAINER_NAME`
1. Valeur de la clé `AZURE_STORAGE_ACCOUNT`
1. Valeur de la clé `AZURE_STORAGE_KEY`

Par exemple, cela peut ressembler à (valeurs pour l’illustration uniquement) :

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Le fichier `.env` obtenu se présente comme suit :

![Informations d’identification Azure Blob Storage](assets/environment-variables/cloud-storage-credentials.png)

Si vous n’utilisez PAS le stockage Blob Microsoft Azure, supprimez ou laissez les commentaires (en ajoutant un préfixe `#`).

### Utilisation de l’espace de stockage dans le cloud Amazon S3{#amazon-s3}

Si vous utilisez l’espace de stockage dans le cloud Amazon S3, supprimez les commentaires et renseignez les clés suivantes dans le fichier `.env` .

Par exemple, cela peut ressembler à (valeurs pour l’illustration uniquement) :

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validation de la configuration du projet

Une fois le projet d’Asset compute généré configuré, validez la configuration avant d’apporter des modifications au code pour vous assurer que les services de prise en charge sont configurés, dans les fichiers `.env`.

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

+ [L’outil de développement ne peut pas démarrer en raison d’une valeur private.key manquante.](../troubleshooting.md#missing-private-key)
