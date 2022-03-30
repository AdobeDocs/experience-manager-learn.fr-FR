---
title: Création d’un projet d’Asset compute pour l’extensibilité des Assets compute
description: Les projets Asset compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande d’Adobe I/O, qui adhèrent à une certaine structure leur permettant d’être déployés dans Adobe I/O Runtime et intégrés à AEM as a Cloud Service.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# Création d’un projet d’Asset compute

Les projets Asset compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande d’Adobe I/O, qui adhèrent à une certaine structure qui leur permet d’être déployés dans Adobe I/O Runtime et intégrés à AEM as a Cloud Service. Un projet d’Asset compute unique peut contenir un ou plusieurs objets Worker d’Asset compute, chacun ayant un point de terminaison HTTP distinct pouvant être référencé à partir d’un profil de traitement as a Cloud Service AEM.

## Génération d’un projet

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Clic publicitaire pour la génération d’un projet d’Asset compute (sans audio)_

Utilisez la variable [Module externe d’Asset compute d’interface de ligne de commande Adobe I/O](../set-up/development-environment.md#aio-cli) pour générer un nouveau projet d’Asset compute vide.

1. Dans la ligne de commande, accédez au dossier contenant le projet.
1. Sur la ligne de commande, exécutez `aio app init` pour commencer l’interface de ligne de commande de génération de projet interactif.
   + Cette commande peut générer un navigateur Web invitant à l’Adobe I/O de l’authentification. Si tel est le cas, indiquez vos informations d’identification d’Adobe associées à la variable [services et produits Adobe requis](../set-up/accounts-and-services.md). Si vous ne parvenez pas à vous connecter, suivez [ces instructions sur la génération d’un projet ;](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Sélectionner une organisation__
   + Sélectionnez l’organisation de l’Adobe qui a AEM as a Cloud Service, et App Builder est enregistré auprès de
1. __Sélectionner le projet__
   + Recherchez et sélectionnez le projet. Il s’agit de la variable [Titre du projet](../set-up/app-builder.md) créé à partir du modèle de projet App Builder, dans ce cas `WKND AEM Asset Compute`
1. __Sélectionner Workspace__
   + Sélectionnez la `Development` workspace
1. __Quelles fonctionnalités d’application Adobe I/O voulez-vous activer pour ce projet ? Sélectionner les composants à inclure__
   + Sélectionner `Actions: Deploy runtime actions`
   + Utilisez les touches de flèches pour sélectionner et libérer de l’espace pour désélectionner/sélectionner, et la touche Entrée pour confirmer la sélection.
1. __Sélectionner le type d’actions à générer__
   + Sélectionner `DX Asset Compute Worker v1`
   + Utilisez les touches de flèches pour sélectionner, libérer de l’espace pour désélectionner/sélectionner et Entrée pour confirmer la sélection.
1. __Comment souhaitez-vous nommer cette action ?__
   + Utiliser le nom par défaut `worker`.
   + Si votre projet contient plusieurs programmes de travail qui effectuent différents calculs de ressources, nommez-les sémantiquement.

## Générer console.json

L’outil de développement nécessite un fichier nommé `console.json` qui contient les informations d’identification nécessaires pour se connecter à Adobe I/O. Ce fichier est téléchargé à partir de la console Adobe I/O.

1. Ouvrez le champ Asset compute worker [Adobe I/O](https://console.adobe.io) project
1. Sélectionnez l’espace de travail du projet à télécharger. `console.json` informations d’identification pour, dans ce cas, sélectionnez `Development`
1. Accédez à la racine du projet Adobe I/O et appuyez sur __Tout télécharger__ dans le coin supérieur droit.
1. Un fichier est téléchargé sous la forme d’un `.json` avec le préfixe du projet et de l’espace de travail, par exemple : `wkndAemAssetCompute-81368-Development.json`
1. Vous pouvez effectuer l&#39;une des opérations suivantes :
   + Renommez le fichier en `config.json` et déplacez-le à la racine de votre projet Asset compute Worker. Il s’agit de l’approche préconisée dans ce tutoriel.
   + Déplacez-le dans un dossier arbitraire ET référencez-le à partir de votre `.env` fichier avec une entrée de configuration `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Le chemin d’accès au fichier peut être absolu ou relatif à la racine de votre projet. Par exemple :
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Ou
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> REMARQUE
> Le fichier  contient des informations d’identification. Si vous stockez le fichier dans votre projet, veillez à l’ajouter à votre `.gitignore` pour empêcher le partage. Il en va de même pour la variable `.env` fichier : ces fichiers d’identification ne doivent pas être partagés ni stockés dans Git.

## asset compute d’un projet sur GitHub

Le projet d’Asset compute final est disponible sur GitHub à l’adresse :

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contient l’état final du projet, entièrement renseigné avec les cas de travail et de test, mais ne contient aucune information d’identification, c’est-à-dire : `.env`, `console.json` ou `.aio`._
