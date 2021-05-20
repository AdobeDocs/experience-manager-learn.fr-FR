---
title: Outil de développement Asset compute
description: L’outil de développement d’Asset compute est un outil web local qui permet aux développeurs de configurer et d’exécuter localement les objets Worker Asset Compute, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset compute dans Adobe I/O Runtime.
feature: Microservices Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Intégrations, développement
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%

---


# Outil de développement Asset compute

L’outil de développement d’Asset compute est un outil web local qui permet aux développeurs de configurer et d’exécuter localement les objets Worker Asset Compute, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset compute dans Adobe I/O Runtime.

## Exécution de l’outil de développement d’Asset compute

L’outil de développement d’Asset compute peut être exécuté à partir de la racine du projet d’Asset compute via la commande de terminal :

```
$ aio app run
```

L’outil de développement démarre alors à l’adresse __http://localhost:9000__ et s’ouvre automatiquement dans une fenêtre de navigateur. Pour que l’outil de développement s’exécute, [un devToolToken valide généré automatiquement doit être fourni via un paramètre de requête](#troubleshooting__devtooltoken).

## Présentation de l’interface des outils de développement Asset compute{#interface}

![Outil de développement Asset compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Fichier source :__ la sélection du fichier source est utilisée pour :
   + Sélectionnez le binaire de ressource qui sera le binaire `source` transmis au programme de travail d’Asset compute.
   + Chargement des fichiers source
1. __Définition des profils d’Asset compute :__ définit le programme de travail d’Asset compute à exécuter, y compris les paramètres : y compris le point de terminaison de l’URL du programme de travail, le nom du rendu résultant et tous les paramètres.
1. __Exécuter :__ le bouton Exécuter exécute le profil d’Asset compute tel que défini dans l’éditeur de profil de configuration d’Asset compute.
1. __Abandonner :__ le bouton Abandonner annule une exécution lancée à partir du bouton Exécuter.
1. __Requête/Réponse :__ fournit la requête HTTP et la réponse à/du programme de travail d’Asset compute s’exécutant dans Adobe I/O Runtime. Cela peut s’avérer utile pour le débogage.
1. __Journaux d’activation :__  journaux décrivant l’exécution du programme de travail d’Asset compute, ainsi que les erreurs éventuelles. Ces informations sont également disponibles dans la norme `aio app run`
1. __Rendus :__ affiche tous les rendus générés par l’exécution du programme de travail Asset compute.
1. ____ Le paramètre de requête devToolToken : le jeton de l’outil de développement d’Asset compute nécessite la présence d’un paramètre  `devToolToken` de requête valide. Ce jeton est automatiquement généré chaque fois qu’un nouvel outil de développement est généré.

### Exécution d’un programme de travail personnalisé

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clic publicitaire de l’exécution d’un travail d’Asset compute dans l’outil de développement (sans audio)_

1. Assurez-vous que l’outil de développement d’Asset compute est démarré à partir de la racine de votre projet à l’aide de la commande `aio app run`.
1. Dans l’outil de développement d’Asset compute, téléchargez ou sélectionnez un [fichier d’image d’exemple](../assets/samples/sample-file.jpg).
   + Assurez-vous que le fichier est sélectionné dans la liste déroulante __Fichier source__
1. Examinez la __zone de texte Définition du profil d’Asset compute__
   + La clé `worker` définit l’URL du programme de travail d’Asset compute déployé.
   + La clé `name` définit le nom du rendu à générer.
   + D’autres clés/valeurs peuvent être fournies dans cet objet JSON et seront disponibles dans le programme de travail sous l’objet `rendition.instructions`
      + Vous pouvez éventuellement ajouter des valeurs pour `size`, `contrast` et `brightness` :

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

1. Appuyez sur le bouton __Exécuter__
1. La __section Rendus__ est renseignée avec un espace réservé de rendu.
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
