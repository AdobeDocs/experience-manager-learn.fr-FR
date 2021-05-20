---
title: Configuration de Adobe Project Firefly pour l’extensibilité des Assets compute
description: Les projets Asset compute sont des projets Adobe Project Firefly spécialement définis. Par conséquent, ils nécessitent l’accès à Adobe Project Firefly dans Adobe Developer Console pour les configurer et les déployer.
feature: Microservices Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Intégrations, développement
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 0%

---


# Configuration de Adobe Project Firefly

Les projets Asset compute sont des projets Adobe Project Firefly spécialement définis. Par conséquent, ils nécessitent l’accès à Adobe Project Firefly dans Adobe Developer Console pour les configurer et les déployer.

## Créez et configurez Adobe Project Firefly dans Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Clic publicitaire de configuration de Adobe Project Firefly (Pas d’audio)_

1. Connectez-vous à [Adobe Developer Console](https://console.adobe.io) à l’aide de l’Adobe ID associé aux [comptes et services configurés](./accounts-and-services.md). Vérifiez que vous êtes __administrateur système__ ou dans le __rôle de développeur__ de l’organisation d’Adobe appropriée.
1. Créez un projet Firefly en appuyant sur __Créer un projet > Projet à partir d’un modèle > Projet Firefly__

   _Si__ Create new __project button ou__ Project __Fireflytype n’est pas disponible, cela signifie que votre organisation d’Adobe n’est pas  [configurée avec Project Firefly](#request-adobe-project-firefly)._

   + __Titre__ du projet :  `WKND AEM Asset Compute`
   + __Nom__ de l’application :  `wkndAemAssetCompute<YourName>`
      + Le __nom de l’application__ doit être unique pour tous les projets Firefly et ne peut pas être modifié plus tard. Ajouter un préfixe au nom de votre entreprise ou organisation et ajouter un suffixe significatif est une bonne approche, par exemple : `wkndAemAssetCompute`.
      + Pour l’auto-activation, il est souvent préférable de placer votre nom sur le __nom de l’application__, par exemple `wkndAemAssetComputeJaneDoe` pour éviter les collisions avec d’autres projets Project Firefly.
   + Sous __Espaces de travail__, ajoutez un nouvel environnement nommé `Development`
   + Sous __Adobe I/O Runtime__ assurez-vous que l’option __Inclure l’exécution à chaque espace de travail__ est sélectionnée.
   + Appuyez sur __Save__ (Enregistrer) pour enregistrer le projet.
1. Dans le projet Adobe Firefly, sélectionnez `Development` dans le sélecteur d’espace de travail.
1. Appuyez sur __+ Ajouter un service > API__ pour ouvrir l’assistant __Ajouter une API__. Utilisez cette approche pour ajouter les API suivantes :

   + __Experience Cloud > Asset compute__
      + Sélectionnez __Générer une paire de clés__ et appuyez sur le bouton __Générer la paire de clés__, puis enregistrez la balise `config.zip` téléchargée à un emplacement sécurisé pour [utiliser ultérieurement](#private-key).
      + Appuyez sur __Suivant__
      + Sélectionnez le profil de produit __Intégrations - Cloud Service__ et appuyez sur __Enregistrer l’API configurée__
   + __Adobe Services > I/O__ Events et appuyez sur  __Save configured API (Enregistrer l’API configurée)__
   + __Adobe Services >__ API de gestion I/O et appuyez sur  __Enregistrer l’API configurée__

## Accédez à private.key{#private-key}

Lors de la configuration de l’[intégration de l’API d’Asset compute](#set-up), une nouvelle paire de clés a été générée et un fichier `config.zip` a été automatiquement téléchargé. Ce `config.zip` contient le certificat public généré et le fichier `private.key` correspondant.

1. Décompressez `config.zip` à un emplacement sécurisé de votre système de fichiers, car `private.key` est [utilisé ultérieurement](../develop/environment-variables.md).
   + Les secrets et les clés privées ne doivent jamais être ajoutés à Git pour des raisons de sécurité.

## Vérification des informations d’identification du compte de service (JWT)

Les informations d’identification de ce projet d’Adobe I/O sont utilisées par l’[outil de développement d’Asset compute](../develop/development-tool.md) local pour interagir avec Adobe I/O Runtime et doivent être intégrées au projet d’Asset compute. Familiarisez-vous avec les informations d’identification du compte de service (JWT).

![Informations d’identification du compte du service de développement Adobe](./assets/firefly/service-account.png)

1. Dans le projet Adobe I/O Project Firefly, assurez-vous que l’espace de travail `Development` est sélectionné.
1. Appuyez sur __Compte de service (JWT)__ sous __Informations d’identification__.
1. Vérifiez les informations d’identification de l’Adobe I/O affichées.
   + La __clé publique__ répertoriée en bas a sa __contrepartie private.key__ dans la `config.zip` téléchargée lorsque l’ __API d’Asset compute__ a été ajoutée à ce projet.
      + Si la clé privée est perdue ou compromise, la clé publique correspondante peut être supprimée et une nouvelle paire de clés générée dans ou chargée vers Adobe I/O à l’aide de cette interface.
