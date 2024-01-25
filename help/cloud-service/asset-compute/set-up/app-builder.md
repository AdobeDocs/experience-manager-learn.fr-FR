---
title: Configurer le créateur d’applications pour l’extensibilité d’Asset Compute
description: Les projets Asset Compute sont des projets App Builder définis spécialement et, à ce titre, ils nécessitent un accès à App Builder dans l’Adobe Developer Console pour être configurés et déployés.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 216
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 100%

---

# Configurer App Builder

Les projets Asset Compute sont des projets du Créateur d’applications définis spécialement et, à ce titre, ils nécessitent un accès au Créateur d’applications dans l’Adobe Developer Console pour être configurés et déployés.

## Créer et configurer le créateur d’applications dans Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Clic publicitaire de configuration du créateur d’applications (sans audio)_

1. Connectez-vous à [Adobe Developer Console](https://console.adobe.io) à l’aide de l’Adobe ID associé aux [comptes et services](./accounts-and-services.md) configurés. Vérifiez que vous êtes __administrateur ou administratrice système__, ou avez le __rôle de développeur ou développeuse__ pour l’organisation Adobe appropriée.
1. Créez un projet de créateur d’applications en appuyant sur __Créer un projet > Projet à partir d’un modèle > Créateur d’applications__.

   _Si le bouton__ Créer un projet __ou le type__ Créateur d’applications __n’est pas disponible, cela signifie que votre organisation Adobe n’est pas [configurée avec le créateur d’applications](#request-adobe-project-app-builder)._

   + __Titre du projet__ : `WKND AEM Asset Compute`
   + __Nom de l’application__ : `wkndAemAssetCompute<YourName>`
      + Le __nom de l’application__ doit être unique dans tous les projets Firefly du créateur d’applications et ne peut pas être modifié plus tard. Ajouter un préfixe au nom de votre entreprise ou organisation et le postfixer avec un suffixe significatif est une bonne approche, par exemple : `wkndAemAssetCompute`.
      + Pour l’auto-activation, il est souvent préférable de postfixer votre nom au __nom de l’application__, par exemple `wkndAemAssetComputeJaneDoe`, pour éviter les collisions avec d’autres projets du créateurs d’applications.
   + Sous __Espaces de travail__, ajoutez un nouvel environnement nommé `Development`.
   + Sous __Adobe I/O Runtime__, assurez-vous que __Inclure l’exécution avec chaque espace de travail__ est sélectionné.
   + Appuyez sur __Enregistrer__ pour enregistrer le projet.
1. Dans le projet du créateur d’applications, sélectionnez `Development` à partir du sélecteur de l’espace de travail.
1. Appuyez sur __+ Ajouter un service > API__ pour ouvrir l’assistant __Ajouter une API__, utilisez cette méthode pour ajouter les API suivantes :

   + __Experience Cloud > Asset Compute__
      + Sélectionnez __Générer une paire de clés__ et appuyez sur le bouton __Générer une paire de clés__ et enregistrez le fichier téléchargé `config.zip` à un emplacement sécurisé pour [l’utiliser ultérieurement](#private-key).
      + Appuyez sur __Suivant__.
      + Sélectionnez le profil de produit __Intégrations - Cloud Service__ et appuyez sur __Enregistrer l’API configurée__.
   + __Services Adobe > Événements I/O__ et appuyez sur __Enregistrer l’API configurée__.
   + __Services Adobe > API I/O Management__ et appuyez sur __Enregistrer l’API configurée__.

## Accès au fichier private.key{#private-key}

Lors de la configuration de l’[Intégration de l’API Asset Compute](#set-up), une nouvelle paire de clés a été générée et un fichier `config.zip` a été automatiquement téléchargé. Ce fichier `config.zip` contient le certificat public généré et le fichier `private.key` correspondant.

1. Décompressez `config.zip` à un emplacement sécurisé de votre système de fichiers car la `private.key` est [utilisée ultérieurement](../develop/environment-variables.md).
   + Les secrets et les clés privées ne doivent jamais être ajoutés à Git pour des raisons de sécurité.

## Vérifier les informations d’identification du compte de service (JWT)

Les informations d’identification de ce projet Adobe I/O sont utilisées par l&#39;[outil de développement Asset Compute](../develop/development-tool.md) pour interagir avec Adobe I/O Runtime et doivent être intégrées au projet Asset Compute. Familiarisez-vous avec les informations d’identification du compte de service (JWT).

![Informations d’identification du compte de service Adobe Developer](./assets/app-builder/service-account.png)

1. Dans le projet de créateurs d’applications de projet Adobe I/O, assurez-vous que l’espace de travail `Development` est sélectionné.
1. Appuyez sur __Compte de service (JWT)__ sous __Informations d’identification__.
1. Vérifiez les informations d’identification Adobe I/O affichées.
   + La __clé publique__ répertoriée en bas de la page a son équivalent __private.key__ dans le fichier `config.zip` téléchargé lorsque l’__API Asset Compute__ a été ajoutée à ce projet.
      + Si la clé privée est perdue ou compromise, la clé publique correspondante peut être supprimée et une nouvelle paire de clés peut être générée ou chargée vers Adobe I/O à l’aide de cette interface.
