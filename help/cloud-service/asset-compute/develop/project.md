---
title: Création d’un projet Asset Compute pour l’extensibilité Asset Compute
description: Les applications Asset Compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande des E/S Adobe, qui adhèrent à une certaine structure leur permettant d’être déployés dans Adobe I/O Runtime et intégrés à l’AEM en tant que Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: a71c61304bbc9d54490086b3313c823225fbe2e0
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 0%

---


# Création d’un projet Asset Compute

Les applications Asset Compute sont des projets Node.js, générés à l’aide de l’interface de ligne de commande des E/S Adobe, qui adhèrent à une certaine structure qui leur permet d’être déployés dans Adobe I/O Runtime et intégrés à l’AEM en tant que Cloud Service. Un seul projet Asset Compute peut contenir un ou plusieurs agents Asset Compute, chacun ayant un point de terminaison HTTP distinct référencé depuis un AEM en tant que Profil de traitement Cloud Service.

## Générer un projet

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)
_Clic publicitaire de génération d&#39;un projet Asset Compute (sans audio)_


Utilisez le module [d&#39;interface de ligne de commande d&#39;](../set-up/development-environment.md#aio-cli) Adobe Asset Compute pour générer un nouveau projet vide Asset Compute.

1. Dans la ligne de commande, accédez au dossier contenant le projet.
1. Dans la ligne de commande, exécutez `aio app init` pour commencer l’interface de ligne de commande de génération de projet interactive.
   + Cela peut engendrer un navigateur Web invitant à l&#39;authentification des E/S d&#39;Adobe. Si tel est le cas, fournissez vos informations d’identification d’Adobe associées aux produits et services [d’Adobe](../set-up/accounts-and-services.md)requis. Si vous ne pouvez pas vous connecter, suivez ces instructions pour générer un projet.
1. __Sélectionner une organisation__
   + Sélectionnez l&#39;organisation d&#39;Adobes pour laquelle AEM en tant que Cloud Service, Project Firefly est enregistré
1. __Sélectionner le projet__
   + Recherchez et sélectionnez le projet. Titre [du](../set-up/firefly.md) projet créé à partir du modèle de projet Firefly, dans ce cas `WKND AEM Asset Compute`
1. __Sélectionner un espace de travail__
   + Sélectionner l&#39;espace de travail `Development`
1. __Quelles fonctionnalités d&#39;application d&#39;E/S d&#39;Adobe voulez-vous activer pour ce projet ? Sélectionner les composants à inclure__
   + Sélectionner `Actions: Deploy runtime actions`
   + Utilisez les touches de flèches pour sélectionner et espacer pour désélectionner/sélectionner, et la touche Entrée pour confirmer la sélection.
1. __Sélectionner le type d’actions à générer__
   + Sélectionner `Adobe Asset Compute Worker`
   + Utilisez les touches de flèches pour sélectionner, libérer de l’espace pour désélectionner/sélectionner et Entrée pour confirmer la sélection.
1. __Comment souhaitez-vous nommer cette action ?__
   + Utilisez le nom par défaut `worker`.
   + Si votre projet contient plusieurs collaborateurs qui effectuent différents calculs d’actifs, nommez-les sémantiquement.

## Examiner l&#39;anatomie du projet

Le projet Asset Compute généré est un projet Node.js pour une application spécialisée Adobe Project Firefly. Les éléments suivants sont propres au projet Asset Compute :

+ `/actions` contient des sous-dossiers, et chaque sous-dossier définit un agent de traitement d’Asset Compute.
   + `/actions/<worker-name>/index.js` définit le script JavaScript exécuté pour effectuer le travail de ce collaborateur.
      + Le nom de dossier `worker` est un nom par défaut et peut être n’importe quel nom tant qu’il est enregistré dans le `manifest.yml`dossier.
      + Il est possible de définir plusieurs dossiers de travail sous `/actions` le cas échéant ; toutefois, ils doivent être enregistrés dans le `manifest.yml`.
+ `/test/asset-compute` contient les suites de tests pour chaque programme de travail. Tout comme le `/actions` dossier, `/test/asset-compute` peut contenir plusieurs sous-dossiers, chacun correspondant au programme de travail testé.
   + `/test/asset-compute/worker`, qui représente une suite de tests pour un travailleur spécifique, contient des sous-dossiers représentant un cas de test spécifique, ainsi que l’entrée de test, les paramètres et la sortie attendue.
+ `/build` contient la sortie, les journaux et les artefacts des exécutions de cas de test d’Asset Compute.
+ `/manifest.yml` définit les employés Asset Compute fournis par le projet. Chaque implémentation de collaborateur doit être énumérée dans ce fichier pour être mise à la disposition de l&#39;AEM en tant que Cloud Service.
+ `/.aio` contient les configurations utilisées par l’outil d’interface de ligne de commande aio. Ce fichier peut être configuré via la `aio config` commande.
+ `/.env` définit des variables d’environnement dans une `key=value` syntaxe et contient des secrets qui ne doivent pas être partagés. Pour protéger ces secrets, ce fichier ne doit PAS être archivé dans Git et est ignoré par le `.gitignore` fichier par défaut du projet.
   + Les variables définies dans ce fichier peuvent être remplacées en [exportant des variables](../deploy/runtime.md) sur la ligne de commande.

Pour plus de détails sur l&#39;examen de la structure du projet, consultez l&#39;application [](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)Anatomie d&#39;un Adobe Project Firefly.

La majeure partie du développement a lieu dans le `/actions` dossier développant les implémentations des programmes de travail et dans les tests `/test/asset-compute` d’écriture pour les agents informatiques d’actifs personnalisés.
