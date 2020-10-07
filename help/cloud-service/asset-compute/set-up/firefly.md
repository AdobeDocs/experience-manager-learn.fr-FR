---
title: Configuration d'une luciole de projet d'Adobe pour l'extensibilité de calcul des ressources
description: Les projets Asset Compute sont des projets spécialement définis de Adobe Project Firefly et, en tant que tels, nécessitent l'accès à Adobe Project Firefly dans la console de développement des Adobes pour les configurer et les déployer.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Configurer la luciole du projet d&#39;Adobe

Les projets Asset Compute sont des projets spécialement définis de Adobe Project Firefly et, en tant que tels, nécessitent l&#39;accès à Adobe Project Firefly dans la console de développement des Adobes pour les configurer et les déployer.

## Création et configuration d&#39;une luciole de projet d&#39;Adobe dans la console de développement d&#39;Adobe{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)
_Clic publicitaire de configuration de la luciole du projet d&#39;Adobe (sans audio)_

1. Connectez-vous à [Adobe Developer Console](https://console.adobe.io) à l’aide de l’Adobe ID associé aux [comptes et services](./accounts-and-services.md)configurés. Vérifiez que vous êtes un administrateur ____ système ou que vous êtes dans le rôle __de__ développeur pour l’organisation d’Adobes appropriée.
1. Créez un projet Firefly en appuyant sur __Créer un projet > Projet à partir d’un modèle > Project Firefly__

   _Si le bouton__ Créer un nouveau projet __ou le type__ Project Firefly __n&#39;est pas disponible, cela signifie que votre organisation d&#39;Adobes n&#39;est pas[configurée avec Project Firefly](#request-adobe-project-firefly)._

   + __Titre__ du projet : `WKND AEM Asset Compute`
   + __Nom__ de l&#39;application : `wkndAemAssetCompute<YourName>`
      + Le nom __de l’__ application doit être unique pour tous les projets de luciole et ne peut pas être modifié ultérieurement. La préfixation du nom de votre société ou de votre organisation et la postfixation avec un suffixe significatif constituent une bonne approche, par exemple : `wkndAemAssetCompute`.
      + Pour l’auto-activation, il est souvent préférable de reporter votre nom sur le nom __de l’__ application, par exemple `wkndAemAssetComputeJaneDoe` pour éviter les collisions avec d’autres projets Project Firefly.
   + Sous __Workspaces__ , ajoutez un nouvel environnement nommé `Development`
   + Sous __Adobe I/O Runtime__ , vérifiez que l’option __Inclure l’exécution avec chaque espace de travail__ est sélectionnée.
   + Tap __Save__ to save the project
1. Dans le projet Adobe luciole, sélectionnez `Development` dans le sélecteur d’espace de travail.
1. Appuyez sur __+ Service d’Ajoute > API__ pour ouvrir l’assistant __Ajouter une API__ , utilisez cette approche pour ajouter les API suivantes :

   + __Experience Cloud > Asset Compute__
      + Sélectionnez __Générer une paire__ de clés, appuyez sur le bouton __Générer la paire__ de clés, puis enregistrez le téléchargement `config.zip` à un emplacement sécurisé pour une utilisation [ultérieure.](#private-key)
      + Appuyez sur __Suivant__
      + Sélectionnez __Intégrations du profil de produits - Cloud Service__ et appuyez sur __Enregistrer l’API configurée.__
   + __Adobe Services > Événements__ d’E/S et appuyez sur __Enregistrer l’API configurée__
   + __Adobe Services > API__ de gestion des E/S et appuyez sur __Enregistrer l’API configurée__

## Accès au fichier private.key{#private-key}

Lors de la configuration de l’intégration [de l’API](#set-up) Asset Compute, une nouvelle paire de clés a été générée et un `config.zip` fichier a été automatiquement téléchargé. Contient le certificat public généré et le `config.zip` `private.key` fichier correspondant.

1. Décompressez `config.zip` dans un emplacement sécurisé de votre système de fichiers, car `private.key` il est [utilisé ultérieurement.](../develop/environment-variables.md)
   + Les secrets et les clés privées ne devraient jamais être ajoutés à Git pour des raisons de sécurité.

## Vérification des informations d’identification du compte de service (JWT)

Les informations d&#39;identification de ce projet d&#39;E/S d&#39;Adobe sont utilisées par l&#39;outil [de développement](../develop/development-tool.md) Asset Compute pour interagir avec Adobe I/O Runtime et devront être intégrées au projet Asset Compute. Familiarisez-vous avec les informations d’identification du compte de service (JWT).

![Informations d’identification du compte Adobe Developer Service](./assets/firefly/service-account.png)

1. Dans le projet de luciole de projet d&#39;E/S d&#39;Adobe, vérifiez que l&#39; `Development` espace de travail est sélectionné.
1. Appuyez sur le compte __de service (JWT)__ sous __Informations d’identification.__
1. Vérifiez les informations d&#39;identification d&#39;E/S de l&#39;Adobe affichées.
   + La clé ____ publique répertoriée au bas de la liste est __private.key__ équivalent dans le fichier `config.zip` téléchargé lorsque l’API __de calcul__ des actifs a été ajoutée à ce projet.
      + Si la clé privée est perdue ou compromise, la clé publique correspondante peut être supprimée et une nouvelle paire de clés générée ou téléchargée vers l&#39;E/S d&#39;Adobe à l&#39;aide de cette interface.
