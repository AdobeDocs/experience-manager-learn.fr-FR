---
title: Créer un projet Asset Compute pour l’extensibilité Assets Compute
description: Les projets Asset Compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande d’Adobe I/O, qui adhèrent à une certaine structure qui permet de les déployer dans Adobe I/O Runtime et de les intégrer à AEM as a Cloud Service.
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 100%

---

# Créer un projet Asset Compute

Les projets Asset Compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande d’Adobe I/O, qui adhèrent à une certaine structure qui permet de les déployer dans Adobe I/O Runtime et de les intégrer à AEM as a Cloud Service. Un projet Asset Compute unique peut contenir un ou plusieurs programmes de travail Asset Compute, chacun ayant un point d’entrée HTTP distinct pouvant être référencé à partir d’un profil de traitement AEM as a Cloud Service.

## Générer un projet

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Procédure de génération d’un projet d’Asset Compute (sans audio)_

Utilisez le [plug-in Asset Compute de l’interface de ligne de commande d’Adobe I/O](../set-up/development-environment.md#aio-cli) pour générer un nouveau projet Asset Compute vide.

1. Dans la ligne de commande, accédez au dossier contenant le projet.
1. Dans la ligne de commande, exécutez `aio app init` pour lancer l’interface de ligne de commande interactive de génération de projet.
   + Cette commande peut faire apparaître un navigateur web demandant une authentification à Adobe I/O. Si tel est le cas, indiquez vos informations d’identification Adobe associées aux [services et produits Adobe requis](../set-up/accounts-and-services.md). Si vous ne parvenez pas à vous connecter, suivez [ces instructions sur la génération d’un projet](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Sélectionner une organisation__
   + Sélectionnez l’organisation Adobe qui a AEM as a Cloud Service, et dont le Créateur d’applications est enregistré.
1. __Sélectionner le projet__
   + Recherchez et sélectionnez le projet. Il s’agit du [Titre du projet](../set-up/app-builder.md) créé à partir du modèle de projet du Créateur d’applications, dans ce cas `WKND AEM Asset Compute`.
1. __Sélectionner l’espace de travail__
   + Sélectionnez l’espace de travail `Development`.
1. __Quelles fonctionnalités d’application Adobe I/O voulez-vous activer pour ce projet ? Sélectionner les composants à inclure__
   + Sélectionnez `Actions: Deploy runtime actions`.
   + Utilisez les flèches du clavier pour sélectionner, la touche Espace pour désélectionner/sélectionner, et la touche Entrée pour confirmer la sélection.
1. __Sélectionner le type d’actions à générer__
   + Sélectionnez `DX Asset Compute Worker v1`.
   + Utilisez les flèches du clavier pour sélectionner, la touche Espace pour désélectionner/sélectionner, et la touche Entrée pour confirmer la sélection.
1. __Comment souhaitez-vous nommer cette action ?__
   + Utilisez le nom par défaut `worker`.
   + Si votre projet contient plusieurs programmes de travail qui effectuent différents calculs de ressources, nommez-les de manière sémantique.

## Générer console.json

L’outil de développement nécessite un fichier nommé `console.json` qui contient les informations d’identification nécessaires pour se connecter à Adobe I/O. Vous pouvez télécharger ce fichier à partir de la console Adobe I/O.

1. Ouvrez le projet [Adobe I/O](https://console.adobe.io) du programme de travail Asset Compute.
1. Sélectionnez l’espace de travail du projet pour lequel vous souhaitez télécharger les informations d’identification de `console.json`, dans ce cas, sélectionnez `Development`.
1. Accédez à la racine du projet Adobe I/O et appuyez sur __Tout télécharger__ dans le coin supérieur droit.
1. Un fichier est téléchargé en tant que fichier `.json` avec le préfixe du projet et de l’espace de travail, par exemple : `wkndAemAssetCompute-81368-Development.json`.
1. Vous pouvez :
   + Renommer le fichier en tant que `console.json` et le déplacer dans la racine du projet du programme de travail Asset Compute. Il s’agit de l’approche adoptée dans ce tutoriel.
   + Déplacez-le dans un dossier quelconque ET référencez ce dossier à partir de votre fichier `.env` avec une entrée de configuration `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Le chemin d’accès au fichier peut être absolu ou relatif à la racine de votre projet. Par exemple :
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     Ou
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> Remarque
> Le fichier contient des informations d’identification. Si vous stockez le fichier dans votre projet, veillez à l’ajouter à votre fichier `.gitignore` pour empêcher le partage. Il en va de même pour le fichier `.env` : ces fichiers d’informations d’identification ne doivent pas être partagés ni stockés dans Git.

## Projet Asset Compute sur GitHub

Le projet Asset Compute final est disponible sur GitHub à l’adresse :

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contient le projet dans son état final, entièrement renseigné avec le programme de travail et les cas de test, mais ne contient aucune information d’identification, c’est-à-dire : `.env`, `console.json` ou `.aio`._
