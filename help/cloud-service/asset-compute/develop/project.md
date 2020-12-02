---
title: Créer un projet d'Asset compute pour l'extensibilité des Assets compute
description: Les projets d’Asset compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande Adobe I/O, qui adhèrent à une certaine structure leur permettant d’être déployés à Adobe I/O Runtime et intégrés à AEM en tant que Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 3%

---


# Création d’un projet d’Asset compute

Les projets d’Asset compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande Adobe I/O, qui adhèrent à une certaine structure qui leur permet d’être déployés à Adobe I/O Runtime et intégrés à AEM en tant que Cloud Service. Un projet d&#39;Asset compute unique peut contenir un ou plusieurs travailleurs de l&#39;Asset compute, chacun ayant un point de terminaison HTTP distinct référencé à partir d&#39;un AEM en tant que Profil de traitement Cloud Service.

## Générer un projet

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Clic publicitaire de génération d&#39;un projet d&#39;Asset compute (sans audio)_


Utilisez le [module externe d’Asset compute de l’interface de ligne de commande d’Adobe I/O](../set-up/development-environment.md#aio-cli) pour générer un nouveau projet d’Asset compute vide.

1. Dans la ligne de commande, accédez au dossier contenant le projet.
1. Dans la ligne de commande, exécutez `aio app init` pour commencer l&#39;interface de ligne de commande de génération de projet interactive.
   + Cela peut engendrer un navigateur Web invitant à l&#39;authentification auprès d&#39;Adobe I/O. Si tel est le cas, fournissez vos informations d&#39;identification d&#39;Adobe associées aux [produits et services d&#39;Adobe requis](../set-up/accounts-and-services.md). Si vous ne pouvez pas vous connecter, veuillez suivre [ces instructions sur la façon de générer un projet](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Sélectionner une organisation__
   + Sélectionnez l&#39;organisation d&#39;Adobes pour laquelle AEM en tant que Cloud Service, Project Firefly est enregistré
1. __Sélectionner le projet__
   + Recherchez et sélectionnez le projet. Il s&#39;agit du [titre du projet](../set-up/firefly.md) créé à partir du modèle de projet Firefly, dans ce cas `WKND AEM Asset Compute`
1. __Sélectionner un espace de travail__
   + Sélectionnez l&#39;espace de travail `Development`
1. __Quelles fonctionnalités de l’application Adobe I/O souhaitez-vous activer pour ce projet ? Sélectionner les composants à inclure__
   + Sélectionner `Actions: Deploy runtime actions`
   + Utilisez les touches de flèches pour sélectionner et espacer pour désélectionner/sélectionner, et la touche Entrée pour confirmer la sélection.
1. __Sélectionner le type d’actions à générer__
   + Sélectionner `Adobe Asset Compute Worker`
   + Utilisez les touches de flèches pour sélectionner, libérer de l’espace pour désélectionner/sélectionner et Entrée pour confirmer la sélection.
1. __Comment souhaitez-vous nommer cette action ?__
   + Utilisez le nom par défaut `worker`.
   + Si votre projet contient plusieurs collaborateurs qui effectuent différents calculs d’actifs, nommez-les sémantiquement.

## Générer console.json

A partir de la racine du projet d&#39;Asset compute nouvellement créé, exécutez la commande suivante pour générer un `console.json`.

```
$ aio app use
```

Vérifiez que les détails de l&#39;espace de travail actuel sont corrects et joli `Y` ou saisissez pour générer un `console.json`. Si `.env` et `.aio` sont détectés comme existants, appuyez sur `x` pour ignorer leur création.

## Examiner l&#39;anatomie du projet

Le projet d&#39;Asset compute généré est un projet Node.js pour un projet d&#39;Adobe spécialisé Firefly, et les éléments suivants sont propres au projet d&#39;Asset compute :

+ `/actions` contient des sous-dossiers et chaque sous-dossier définit un agent d’Asset compute.
   + `/actions/<worker-name>/index.js` définit le script JavaScript exécuté pour effectuer le travail de ce collaborateur.
      + Le nom de dossier `worker` est un nom par défaut et peut être n’importe quoi, tant qu’il est enregistré dans `manifest.yml`.
      + Au besoin, plusieurs dossiers de travail peuvent être définis sous `/actions`, mais ils doivent être enregistrés dans `manifest.yml`.
+ `/test/asset-compute` contient les suites de tests pour chaque programme de travail. Tout comme le dossier `/actions`, `/test/asset-compute` peut contenir plusieurs sous-dossiers, chacun correspondant au programme de travail testé.
   + `/test/asset-compute/worker`, qui représente une suite de tests pour un travailleur spécifique, contient des sous-dossiers représentant un cas de test spécifique, ainsi que l’entrée de test, les paramètres et la sortie attendue.
+ `/build` contient la sortie, les journaux et les artefacts des exécutions de cas de test d’Asset compute.
+ `/manifest.yml` définit les employés d&#39;Asset compute fournis par le projet. Chaque implémentation de collaborateur doit être énumérée dans ce fichier pour être mise à la disposition de l&#39;AEM en tant que Cloud Service.
+ `/console.json` définit les configurations Adobe I/O
   + Ce fichier peut être généré/mis à jour à l&#39;aide de la commande `aio app use`.
+ `/.aio` contient les configurations utilisées par l’outil d’interface de ligne de commande aio.
   + Ce fichier peut être généré/mis à jour à l&#39;aide de la commande `aio app use`.
+ `/.env` définit des variables d’environnement dans une  `key=value` syntaxe et contient des secrets qui ne doivent pas être partagés. Ceci peut être généré ou Pour protéger ces secrets, ce fichier ne doit PAS être archivé dans Git et est ignoré par le fichier `.gitignore` par défaut du projet.
   + Ce fichier peut être généré/mis à jour à l&#39;aide de la commande `aio app use`.
   + Les variables définies dans ce fichier peuvent être remplacées par [l&#39;exportation de variables](../deploy/runtime.md) sur la ligne de commande.

Pour plus de détails sur l&#39;examen de la structure du projet, consultez l&#39;[Anatomie d&#39;un projet de luciole de projet d&#39;Adobe](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

La majeure partie du développement a lieu dans le dossier `/actions` qui développe les implémentations des opérateurs et dans `/test/asset-compute` les tests d&#39;écriture pour les opérateurs d&#39;Asset compute personnalisés.

## Projet Asset compute sur Github

Le projet d&#39;Asset compute final est disponible sur Github à l&#39;adresse suivante :

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contient l&#39;état final du projet, entièrement renseigné avec les cas de travail et de test, mais ne contient aucune information d&#39;identification, c&#39;est-à-dire. `.env`,  `console.json` ou  `.aio`._

