---
title: Configurer la luciole du projet Adobe pour l'extensibilité de l'Asset compute
description: Les projets d'Asset compute sont des projets de lucioles d'Adobe spécialement définis et, en tant que tels, nécessitent l'accès à la luciole de projet d'Adobe dans la console de développement des Adobes pour les configurer et les déployer.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---


# Configurer la luciole du projet d&#39;Adobe

Les projets d&#39;Asset compute sont des projets de lucioles d&#39;Adobe spécialement définis et, en tant que tels, nécessitent l&#39;accès à la luciole de projet d&#39;Adobe dans la console de développement des Adobes pour les configurer et les déployer.

## Création et configuration d’une luciole de projet d’Adobe dans la console de développement d’Adobe{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Clic publicitaire de configuration de la luciole du projet d&#39;Adobe (sans audio)_

1. Connectez-vous à [Adobe Developer Console](https://console.adobe.io) à l&#39;aide de l&#39;Adobe ID associé aux [comptes et services ](./accounts-and-services.md) configurés. Vérifiez que vous êtes un __administrateur système__ ou dans le __rôle du développeur__ pour l&#39;organisation d&#39;Adobe appropriée.
1. Créez un projet Firefly en appuyant sur __Créer un projet > Projet à partir d’un modèle > Project Firefly__.

   _Si le bouton__ Créer un nouveau __projet ou le__ type __Project Firefytype n&#39;est pas disponible, cela signifie que votre organisation d&#39;Adobes n&#39;est pas  [configurée avec Project Firefly](#request-adobe-project-firefly)._

   + __Titre__ du projet :  `WKND AEM Asset Compute`
   + __Nom__ de l&#39;application :  `wkndAemAssetCompute<YourName>`
      + Le __nom de l&#39;application__ doit être unique pour tous les projets de lucioles et ne peut pas être modifié ultérieurement. La préfixation du nom de votre société ou de votre organisation et la postfixation avec un suffixe significatif constituent une bonne approche, par exemple : `wkndAemAssetCompute`.
      + Pour l’auto-activation, il est souvent préférable de reporter votre nom sur le __nom de l’application__, par exemple `wkndAemAssetComputeJaneDoe` pour éviter les collisions avec d’autres projets Project Firefly.
   + Sous __Espaces de travail__, ajoutez un nouvel environnement nommé `Development`.
   + Sous __Adobe I/O Runtime__ vérifiez que __Inclure l&#39;exécution avec chaque espace de travail__ est sélectionné
   + Appuyez sur __Enregistrer__ pour enregistrer le projet.
1. Dans le projet Adobe luciole, sélectionnez `Development` dans le sélecteur d’espace de travail.
1. Appuyez sur __+ Ajoute Service > API__ pour ouvrir l&#39;Assistant __Ajouter une API__, utilisez cette approche pour ajouter les API suivantes :

   + __Experience Cloud > Asset compute__
      + Sélectionnez __Générer une paire de clés__ et appuyez sur le bouton __Générer la paire de clés__, puis enregistrez le `config.zip` téléchargé à un emplacement sûr pour [l&#39;utiliser ultérieurement](#private-key).
      + Appuyez sur __Suivant__
      + Sélectionnez le profil de produits __Intégrations - Cloud Service__ et appuyez sur __Enregistrer l&#39;API configurée__.
   + __Adobe Services >__ Evénements d&#39;E/S et appuyez sur  __Enregistrer l&#39;API configurée__
   + __Adobe Services >__ API de gestion des E/S et appuyez sur  __Enregistrer l’API configurée__

## Accédez au fichier private.key{#private-key}

Lors de la configuration de l&#39;[intégration de l&#39;API d&#39;Asset compute](#set-up), une nouvelle paire de clés a été générée et un fichier `config.zip` a été automatiquement téléchargé. Ce `config.zip` contient le certificat public généré et le fichier `private.key` correspondant.

1. Décompressez `config.zip` dans un emplacement sécurisé de votre système de fichiers, car `private.key` est [utilisé ultérieurement](../develop/environment-variables.md).
   + Les secrets et les clés privées ne devraient jamais être ajoutés à Git pour des raisons de sécurité.

## Vérification des informations d’identification du compte de service (JWT)

Les informations d&#39;identification de ce projet d&#39;Adobe I/O sont utilisées par l&#39;[outil de développement d&#39;Asset compute](../develop/development-tool.md) local pour interagir avec Adobe I/O Runtime et devront être intégrées au projet d&#39;Asset compute. Familiarisez-vous avec les informations d’identification du compte de service (JWT).

![Informations d’identification du compte Adobe Developer Service](./assets/firefly/service-account.png)

1. Dans le projet Adobe I/O Project Firefly, vérifiez que l&#39;espace de travail `Development` est sélectionné.
1. Appuyez sur __Compte de service (JWT)__ sous __Identifiants__.
1. Vérification des informations d’identification de l’Adobe I/O affichées
   + La __clé publique__ répertoriée au bas de la liste est __private.key__ équivalent dans `config.zip` téléchargée lorsque l&#39;API d&#39;Asset compute ____ a été ajoutée à ce projet.
      + Si la clé privée est perdue ou compromise, la clé publique correspondante peut être supprimée et une nouvelle paire de clés générée ou téléchargée vers l&#39;Adobe I/O à l&#39;aide de cette interface.
