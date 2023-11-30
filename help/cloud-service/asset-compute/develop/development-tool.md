---
title: Outil de développement d’Asset Compute
description: L’outil de développement d’Asset Compute est un outil web local qui permet aux personnes chargées du développement de configurer et d’exécuter localement les programme de travail d’Asset Compute, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset Compute dans Adobe I/O Runtime.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 100%

---

# Outil de développement d’Asset Compute

L’outil de développement d’Asset Compute est un outil web local qui permet aux personnes chargées du développement de configurer et d’exécuter localement les programme de travail d’Asset Compute, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset Compute dans Adobe I/O Runtime.

## Exécuter l’outil de développement d’Asset Compute

L’outil de développement d’Asset Compute peut être exécuté à partir de la racine du projet Asset Compute via la commande de terminal :

```
$ aio app run
```

L’outil de développement démarre alors à l’adresse __http://localhost:9000__, et s’ouvre automatiquement dans une fenêtre de navigateur. Pour que l’outil de développement s’exécute, [un devToolToken valide généré automatiquement doit être fourni via un paramètre de requête](#troubleshooting__devtooltoken).

## Comprendre l’interface des outils de développement d’Asset Compute{#interface}

![Outil de développement d’Asset Compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Fichier source :__ la sélection du fichier source est utilisée pour :
   + Sélectionner le binaire de ressource qui agit comme le binaire `source` transmis au programme de travail d’Asset Compute
   + Charger des fichiers source
1. __Définition des profils d’Asset Compute :__ définit le programme de travail d’Asset Compute à exécuter, y compris les paramètres : notamment le point d’entrée de l’URL du programme de travail, le nom du rendu résultant et tous les paramètres.
1. __Exécuter :__ le bouton Exécuter exécute le profil d’Asset Compute tel que défini dans l’éditeur de profil de configuration d’Asset Compute.
1. __Abandonner :__ le bouton Abandonner annule une exécution lancée à partir du bouton Exécuter.
1. __Requête/Réponse :__ fournit la requête et la réponse HTTP au/du programme de travail d’Asset Compute s’exécutant dans Adobe I/O Runtime. Cela peut s’avérer utile pour le débogage.
1. __Journaux d’activation :__ les journaux décrivant l’exécution du programme de travail d’Asset Compute, ainsi que les erreurs éventuelles. Ces informations sont également disponibles dans Standard Out `aio app run`.
1. __Rendus :__ affiche tous les rendus générés par l’exécution du programme de travail d’Asset Compute.
1. __Paramètre de requête devToolToken :__ le jeton de l’outil de développement d’Assets Compute nécessite la présence d’un paramètre de requête `devToolToken` valide. Ce jeton est automatiquement généré chaque fois qu’un nouvel outil de développement est généré.

### Exécuter un programme de travail personnalisé

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clic publicitaire de l’exécution d’un programme de travail d’Asset Compute dans l’outil de développement (sans audio)_

1. Assurez-vous que l’outil de développement d’Asset Compute est lancé depuis la racine de votre projet à l’aide de la commande `aio app run`.
1. Dans l’outil de développement d’Asset Compute, chargez ou sélectionnez un [exemple de fichier image](../assets/samples/sample-file.jpg).
   + Assurez-vous que le fichier est sélectionné dans le menu déroulant __Fichier source__.
1. Consultez la zone de texte __Définition du profil d’Asset Compute__.
   + La clé `worker` définit l’URL du programme de travail d’Asset Compute déployé
   + La clé `name` définit le nom du rendu à générer.
   + D’autres clés/valeurs peuvent être fournies dans cet objet JSON, et sont disponibles dans le programme de travail sous l’objet `rendition.instructions`.
      + Vous pouvez éventuellement ajouter des valeurs pour `size`, `contrast` et `brightness` :

        ```json
        {
            "renditions": [
                {
                    "worker": "...",
                    "name": "rendition.png",
                    "size":"800",
                    "contrast": "0.30",
                    "brightness": "-0.15"
                }
            ]
        }
        ```

1. Appuyez sur le bouton __Exécuter__.
1. La __section Rendus__ est renseignée avec un espace réservé de rendu.
1. Une fois le programme de travail terminé, l’espace réservé du rendu affiche le rendu généré.

Apporter des modifications au code du programme de travail pendant l’exécution de l’outil de développement permet de déployer rapidement les modifications. Le déploiement rapide prend plusieurs secondes. Laissez le déploiement se terminer avant de réexécuter le programme de travail à partir de l’outil de développement.

## Résolution des problèmes

+ [Mise en retrait incorrecte de YAML](../troubleshooting.md#incorrect-yaml-indentation)
+ [La limite memorySize est définie trop basse](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [L’outil de développement ne peut pas démarrer en raison d’une clé privée manquante](../troubleshooting.md#missing-private-key)
+ [Fichier source : menu déroulant incorrect](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Paramètre de requête devToolToken absent ou non valide](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Impossible de supprimer les fichiers source](../troubleshooting.md#unable-to-remove-source-files)
+ [Rendu renvoyé partiellement tracé/corrompu](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
