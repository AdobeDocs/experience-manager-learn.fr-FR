---
title: Création d’un projet d’Asset compute pour l’extensibilité des Assets compute
description: Les projets Asset compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande d’Adobe I/O, qui adhèrent à une certaine structure leur permettant d’être déployés dans Adobe I/O Runtime et intégrés à AEM en tant que Cloud Service.
feature: Microservices Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
topic: Intégrations, développement
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 4%

---


# Création d’un projet d’Asset compute

Les projets Asset compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande d’Adobe I/O, qui adhèrent à une certaine structure qui leur permet d’être déployés dans Adobe I/O Runtime et intégrés à AEM en tant que Cloud Service. Un projet d’Asset compute unique peut contenir un ou plusieurs objets Worker d’Asset compute, chacun ayant un point de terminaison HTTP distinct pouvant être référencé à partir d’un AEM en tant que profil de traitement du Cloud Service.

## Génération d’un projet

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Clic publicitaire pour la génération d’un projet d’Asset compute (sans audio)_

Utilisez le [module Adobe I/O d’Asset compute de l’interface de ligne de commande](../set-up/development-environment.md#aio-cli) pour générer un nouveau projet d’Asset compute vide.

1. Dans la ligne de commande, accédez au dossier contenant le projet.
1. Sur la ligne de commande, exécutez `aio app init` pour lancer l’interface de ligne de commande de génération de projet interactif.
   + Cette commande peut générer un navigateur Web invitant à l’Adobe I/O de l’authentification. Si tel est le cas, fournissez les informations d’identification de votre Adobe associées aux [services et produits d’Adobe requis](../set-up/accounts-and-services.md). Si vous ne parvenez pas à vous connecter, suivez [ces instructions pour générer un projet](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Sélectionner une organisation__
   + Sélectionnez l’organisation d’Adobe qui a AEM en tant que Cloud Service et avec laquelle Project Firefly est enregistré.
1. __Sélectionner le projet__
   + Recherchez et sélectionnez le projet. Il s’agit du [titre du projet](../set-up/firefly.md) créé à partir du modèle de projet Firefly, dans ce cas `WKND AEM Asset Compute`
1. __Sélectionner Workspace__
   + Sélectionnez l’espace de travail `Development`
1. __Quelles fonctionnalités d’application Adobe I/O voulez-vous activer pour ce projet ? Sélectionner les composants à inclure__
   + Sélectionner `Actions: Deploy runtime actions`
   + Utilisez les touches de flèches pour sélectionner et libérer de l’espace pour désélectionner/sélectionner, et la touche Entrée pour confirmer la sélection.
1. __Sélectionner le type d’actions à générer__
   + Sélectionner `Adobe Asset Compute Worker`
   + Utilisez les touches de flèches pour sélectionner, libérer de l’espace pour désélectionner/sélectionner et Entrée pour confirmer la sélection.
1. __Comment souhaitez-vous nommer cette action ?__
   + Utilisez le nom par défaut `worker`.
   + Si votre projet contient plusieurs programmes de travail qui effectuent différents calculs de ressources, nommez-les sémantiquement.

## Générer console.json

L’outil de développement requiert un fichier nommé `console.json` qui contient les informations d’identification nécessaires pour se connecter à Adobe I/O. Ce fichier est téléchargé à partir de la console Adobe I/O.

1. Ouvrez le projet [Adobe I/O](https://console.adobe.io) du travailleur Asset compute.
1. Sélectionnez l’espace de travail du projet pour lequel télécharger les informations d’identification `console.json`. Dans ce cas, sélectionnez `Development`.
1. Accédez à la racine du projet Adobe I/O et appuyez sur __Télécharger tout__ dans le coin supérieur droit.
1. Un fichier est téléchargé sous la forme d’un fichier `.json` précédé du préfixe du projet et de l’espace de travail, par exemple : `wkndAemAssetCompute-81368-Development.json`
1. Vous pouvez effectuer l&#39;une des opérations suivantes :
   + Renommez le fichier `config.json` et déplacez-le à la racine de votre projet Asset compute Worker. Il s’agit de l’approche préconisée dans ce tutoriel.
   + Déplacez-le dans un dossier arbitraire ET référencez ce dossier à partir de votre fichier `.env` avec une entrée de configuration `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Le chemin d’accès au fichier peut être absolu ou relatif à la racine de votre projet. Par exemple :
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Ou
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> REMARQUE
> Le fichier  contient des informations d’identification. Si vous stockez le fichier dans votre projet, veillez à l’ajouter à votre fichier `.gitignore` afin d’éviter tout partage. Il en va de même pour le fichier `.env` : ces fichiers d’identification ne doivent pas être partagés ni stockés dans Git.

## Vérification de l’anatomie du projet

Le projet d’Asset compute généré est un projet Node.js à utiliser comme projet Adobe spécialisé Project Firefly. Les éléments structurels suivants sont idiosyncrasiques pour le projet Asset compute :

+ `/actions` contient des sous-dossiers et chaque sous-dossier définit un objet Worker Asset compute.
   + `/actions/<worker-name>/index.js` définit le code JavaScript utilisé pour effectuer le travail de ce programme de travail.
      + Le nom du dossier `worker` est une valeur par défaut et peut être n’importe quel nom, tant qu’il est enregistré dans la balise `manifest.yml`.
      + Au besoin, plusieurs dossiers de travail peuvent être définis sous `/actions`, mais ils doivent être enregistrés dans la balise `manifest.yml`.
+ `/test/asset-compute` contient les suites de test pour chaque programme de travail. Tout comme le dossier `/actions` , `/test/asset-compute` peut contenir plusieurs sous-dossiers, chacun correspondant au programme de travail testé.
   + `/test/asset-compute/worker`, qui représente une suite de tests pour un programme de travail spécifique, contient des sous-dossiers représentant un cas de test spécifique, ainsi que l’entrée de test, les paramètres et la sortie attendue.
+ `/build` contient la sortie, les journaux et les artefacts des exécutions de cas de test d’Asset compute.
+ `/manifest.yml` définit les employés d’Asset compute fournis par le projet. Chaque implémentation de programme de travail doit être énumérée dans ce fichier afin de le rendre disponible pour AEM en tant que Cloud Service.
+ `/console.json` définit les configurations d’Adobe I/O ;
   + Ce fichier peut être généré/mis à jour à l’aide de la commande `aio app use`.
+ `/.aio` contient les configurations utilisées par l’outil d’interface de ligne de commande aio.
   + Ce fichier peut être généré/mis à jour à l’aide de la commande `aio app use`.
+ `/.env` définit des variables d’environnement dans une  `key=value` syntaxe et contient des secrets qui ne doivent pas être partagés. Pour protéger ces secrets, ce fichier ne doit PAS être archivé dans Git et est ignoré via le fichier `.gitignore` par défaut du projet.
   + Ce fichier peut être généré/mis à jour à l’aide de la commande `aio app use`.
   + Les variables définies dans ce fichier peuvent être remplacées par [l’exportation de variables](../deploy/runtime.md) sur la ligne de commande.

Pour plus d’informations sur la révision de la structure du projet, consultez l’ [Anatomie d’un projet Adobe Project Firefly](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

La majeure partie du développement a lieu dans le dossier `/actions` qui développe des implémentations de programme de travail et dans `/test/asset-compute` les tests d’écriture pour les programmes de travail d’Asset compute personnalisés.

## asset compute d’un projet sur GitHub

Le projet d’Asset compute final est disponible sur GitHub à l’adresse :

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contient l’état final du projet, entièrement renseigné avec les cas de travail et de test, mais ne contient aucune information d’identification, à savoir  `.env`,  `console.json` ou  `.aio`._

