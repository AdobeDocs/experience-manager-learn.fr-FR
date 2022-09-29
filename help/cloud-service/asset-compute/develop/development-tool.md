---
title: Outil de développement Asset compute
description: L’outil de développement d’Asset compute est un outil web local qui permet aux développeurs de configurer et d’exécuter localement les objets Worker Asset Compute, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset compute dans Adobe I/O Runtime.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 0%

---

# Outil de développement Asset compute

L’outil de développement d’Asset compute est un outil web local qui permet aux développeurs de configurer et d’exécuter localement les objets Worker Asset Compute, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset compute dans Adobe I/O Runtime.

## Exécution de l’outil de développement d’Asset compute

L’outil de développement d’Asset compute peut être exécuté à partir de la racine du projet d’Asset compute via la commande de terminal :

```
$ aio app run
```

L’outil de développement démarre alors à l’adresse __http://localhost:9000__, puis ouvrez-la automatiquement dans une fenêtre de navigateur. Pour que l’outil de développement s’exécute, [un devToolToken valide généré automatiquement doit être fourni via un paramètre de requête](#troubleshooting__devtooltoken).

## Présentation de l’interface des outils de développement Asset compute{#interface}

![Outil de développement Asset compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Fichier source :__ La sélection du fichier source est utilisée pour :
   + Sélection du binaire de ressource qui agit comme le `source` binaire transmis au programme de travail d’Asset compute
   + Chargement des fichiers source
1. __Définition des profils d’Asset compute :__ Définit le programme de travail de l’Asset compute à exécuter, y compris les paramètres : y compris le point de terminaison de l’URL du programme de travail, le nom du rendu résultant et tous les paramètres.
1. __Exécutez :__ Le bouton Exécuter exécute le profil d’Asset compute tel que défini dans l’éditeur de profil de configuration d’Asset compute
1. __Abandonner :__ Le bouton Abandonner annule une exécution lancée à partir du bouton Exécuter
1. __Requête/Réponse :__ Fournit la requête et la réponse HTTP au/à l’Asset compute Worker en cours d’exécution dans Adobe I/O Runtime. Cela peut s’avérer utile pour le débogage.
1. __Journaux d’activation :__ Les journaux décrivant l’exécution du programme de travail de l’Asset compute, ainsi que les erreurs éventuelles. Ces informations sont également disponibles dans la `aio app run` standard out
1. __Rendus :__ Affiche tous les rendus générés par l’exécution du programme de travail Asset compute.
1. __devToolToken paramètre de requête :__ Le jeton de l’outil de développement d’Assets compute nécessite un `devToolToken` paramètre de requête à être présent. Ce jeton est automatiquement généré chaque fois qu’un nouvel outil de développement est généré.

### Exécution d’un programme de travail personnalisé

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clic publicitaire de l’exécution d’un travail d’Asset compute dans l’outil de développement (sans audio)_

1. Assurez-vous que l’outil de développement d’Asset compute est démarré à partir de la racine de votre projet à l’aide de la fonction `aio app run` .
1. Dans l’outil de développement Asset compute, téléchargez ou sélectionnez une [exemple de fichier image](../assets/samples/sample-file.jpg)
   + Assurez-vous que le fichier est sélectionné dans la variable __Fichier source__ menu déroulant
1. Consultez la section __Définition d’un profil d’Asset compute__ zone de texte
   + Le `worker` clé définit l’URL du programme de travail d’Asset compute déployé.
   + Le `name` clé définit le nom du rendu à générer.
   + D’autres clés/valeurs peuvent être fournies dans cet objet JSON et sont disponibles dans le programme de travail sous `rendition.instructions` objet
      + Vous pouvez éventuellement ajouter des valeurs pour `size`, `contrast` et `brightness`:

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

1. Appuyez sur le bouton __Exécuter__ button
1. Le __Section Rendus__ est renseigné avec un espace réservé de rendu
1. Une fois le programme de travail terminé, l’espace réservé du rendu affiche le rendu généré.

Apporter des modifications au code du programme de travail pendant l’exécution de l’outil de développement permet de &quot;déployer&quot; les modifications à chaud. Le &quot;déploiement à chaud&quot; prend plusieurs secondes, donc laissez le déploiement se terminer avant de réexécuter le programme de travail à partir de l’outil de développement.

## Résolution des problèmes

+ [Retrait YAML incorrect](../troubleshooting.md#incorrect-yaml-indentation)
+ [La limite memorySize est définie trop basse](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [L’outil de développement ne peut pas démarrer en raison d’une valeur private.key manquante.](../troubleshooting.md#missing-private-key)
+ [Fichier source : liste déroulante incorrect](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Paramètre de requête devToolToken absent ou non valide](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Impossible de supprimer les fichiers source](../troubleshooting.md#unable-to-remove-source-files)
+ [Rendu renvoyé partiellement tracé/corrompu](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
