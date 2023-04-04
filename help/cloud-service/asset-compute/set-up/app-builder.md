---
title: Configuration d’App Builder pour une extensibilité des Assets compute
description: Les projets Asset compute sont des projets App Builder spécialement définis et, en tant que tels, nécessitent l’accès à App Builder dans la console Adobe Developer pour les configurer et les déployer.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# Configuration d’App Builder

Les projets Asset compute sont des projets App Builder spécialement définis et, en tant que tels, nécessitent l’accès à App Builder dans la console Adobe Developer pour les configurer et les déployer.

## Création et configuration d’App Builder dans la console Adobe Developer{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Clic publicitaire de configuration du générateur d’applications (sans audio)_

1. Connectez-vous à [Console Adobe Developer](https://console.adobe.io) à l’aide de l’Adobe ID associée à la configuration [comptes et services](./accounts-and-services.md). Vérifiez que vous êtes __Administrateur système__ ou dans la variable __Rôle de développeur__ pour l’organisation d’Adobe appropriée.
1. Créez un projet App Builder en appuyant sur __Créer un projet > Projet à partir d’un modèle > Créateur d’applications__

   _Si__ Créer un projet __ou le bouton__ App Builder __type non disponible, ce qui signifie que votre organisation d’Adobe n’est pas [configuré avec App Builder](#request-adobe-project-app-builder)._

   + __Titre du projet__: `WKND AEM Asset Compute`
   + __Nom de l’application__: `wkndAemAssetCompute<YourName>`
      + Le __Nom de l’application__ doit être unique dans tous les projets du créateur d’applications (FApp) et ne peut pas être modifié plus tard. Ajouter un préfixe au nom de votre entreprise ou organisation et ajouter un suffixe significatif est une bonne approche, par exemple : `wkndAemAssetCompute`.
      + Pour l’auto-activation, il est souvent préférable de placer votre nom sur la page __Nom de l’application__, par exemple `wkndAemAssetComputeJaneDoe` pour éviter les collisions avec d’autres projets App Builder.
   + Sous __Espaces de travail__ ajouter un nouvel environnement nommé `Development`
   + Sous __Adobe I/O Runtime__ veiller à __Inclure l’exécution à chaque espace de travail__ est sélectionné
   + Appuyer __Enregistrer__ enregistrement du projet
1. Dans le projet App Builder, sélectionnez `Development` à partir du sélecteur de l’espace de travail
1. Appuyer __+ Ajouter un service > API__ pour ouvrir le __Ajout d’une API__ , utilisez cette méthode pour ajouter les API suivantes :

   + __Experience Cloud > Asset compute__
      + Sélectionner __Générer une paire de clés__ et appuyez sur __Générer une paire de clés__ et enregistrez le fichier téléchargé. `config.zip` à un emplacement sécurisé pour [utilisation ultérieure](#private-key)
      + Appuyez sur __Suivant__
      + Sélection du profil de produit __Intégrations - Cloud Service__ et appuyez sur __Enregistrer l’API configurée__
   + __Services Adobe > Événements I/O__ et appuyez sur __Enregistrer l’API configurée__
   + __Services Adobe > API de gestion I/O__ et appuyez sur __Enregistrer l’API configurée__

## Accès à private.key{#private-key}

Lors de la configuration de la variable [Intégration de l’API d’Asset compute](#set-up) une nouvelle paire de clés a été générée et une `config.zip` a été automatiquement téléchargé. Ceci `config.zip` contient le certificat public généré et la correspondance `private.key` fichier .

1. Décompresser `config.zip` à un emplacement sécurisé de votre système de fichiers en tant que `private.key` is [utilisé ultérieurement](../develop/environment-variables.md)
   + Les secrets et les clés privées ne doivent jamais être ajoutés à Git pour des raisons de sécurité.

## Vérification des informations d’identification du compte de service (JWT)

Les informations d’identification de ce projet d’Adobe I/O sont utilisées par le [Outil de développement Asset compute](../develop/development-tool.md) pour interagir avec Adobe I/O Runtime et doivent être intégrés au projet Asset compute. Familiarisez-vous avec les informations d’identification du compte de service (JWT).

![Informations d’identification du compte de service Adobe Developer](./assets/app-builder/service-account.png)

1. Dans le projet Adobe I/O Project App Builder, assurez-vous que la variable `Development` espace de travail sélectionné
1. Appuyez sur __Compte de service (JWT)__ under __Informations d’identification__
1. Vérifiez les informations d’identification de l’Adobe I/O affichées.
   + Le __clé publique__ est répertorié en bas de la page. __private.key__ dans le `config.zip` téléchargé lorsque la variable __API Asset compute__ a été ajouté à ce projet.
      + Si la clé privée est perdue ou compromise, la clé publique correspondante peut être supprimée et une nouvelle paire de clés générée dans ou chargée vers Adobe I/O à l’aide de cette interface.
