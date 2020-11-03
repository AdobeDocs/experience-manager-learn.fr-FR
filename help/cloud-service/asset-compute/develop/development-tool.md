---
title: Outil de développement de calcul des ressources
description: Asset Compute Development Tool est un outil Web local qui permet aux développeurs de configurer et d’exécuter localement les travailleurs d’Asset Computer, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset Compute de Adobe I/O Runtime.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---


# Outil de développement de calcul des ressources

Asset Compute Development Tool est un outil Web local qui permet aux développeurs de configurer et d’exécuter localement les travailleurs d’Asset Computer, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset Compute de Adobe I/O Runtime.

## Exécution de l&#39;outil de développement de calcul des ressources

L&#39;outil de développement Asset Compute peut être exécuté à partir de la racine du projet Asset Compute via la commande terminal :

```
$ aio app run
```

L’outil de développement est alors début à l’adresse __http://localhost:9000__ et s’ouvre automatiquement dans une fenêtre de navigateur. Pour que l&#39;outil de développement s&#39;exécute, [un devToolToken valide généré automatiquement doit être fourni par le biais d&#39;un paramètre](#troubleshooting__devtooltoken)de requête.

## Comprendre l’interface des outils de développement d’Asset Compute{#interface}

![Outil de développement de calcul des ressources](./assets/development-tool/asset-compute-dev-tool.png)

1. __Fichier source :__ La sélection du fichier source est utilisée pour :
   + Sélection du binaire de ressources qui sera le `source` binaire transmis au programme de travail de calcul de ressources
   + Téléchargement de fichiers source
1. __Définition du ou des profils de calcul des ressources :__ Définit le programme de travail Asset Compute à exécuter, y compris les paramètres : incluant le point de terminaison de l’URL du collaborateur, le nom du rendu résultant et tous les paramètres
1. __Exécuter :__ Le bouton Exécuter exécute le profil de calcul du fichier tel que défini dans l’éditeur de profil de configuration de Asset Compute.
1. __Abandonner :__ Le bouton Abandonner annule une exécution initiée lorsque vous appuyez sur le bouton Exécuter.
1. __Demande/réponse :__ Fournit la requête et la réponse HTTP au/à l’agent Asset Compute s’exécutant dans Adobe I/O Runtime. Cela peut s’avérer utile pour le débogage
1. __Journaux des Activations :__ Journaux décrivant l’exécution du programme de travail Asset Compute, ainsi que les erreurs éventuelles. Ces informations sont également disponibles dans la `aio app run` norme
1. __Rendus :__ Affiche tous les rendus générés par l&#39;exécution du programme de travail Asset Compute
1. __devToolToken, paramètre de requête :__ Le jeton Asset Compute Development Tool nécessite la présence d’un paramètre de `devToolToken` requête valide. Ce jeton est généré automatiquement chaque fois qu’un nouvel outil de développement est généré.

### Exécution d’un travailleur personnalisé

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clic publicitaire pour exécuter un travail de calcul des ressources dans l’outil de développement (sans audio)_

1. Assurez-vous que l’outil de développement d’Asset Compute est démarré à partir de la racine du projet à l’aide de la `aio app run` commande.
1. Dans l’outil de développement de calcul des ressources, téléchargez ou sélectionnez un [exemple de fichier image.](../assets/samples/sample-file.jpg)
   + Vérifier que le fichier est sélectionné dans la liste déroulante des fichiers ____ source
1. Consultez la zone de texte Définition __du profil de calcul de__ l&#39;actif
   + La `worker` clé définit l’URL du collaborateur de calcul d’actifs déployé.
   + La `name` clé définit le nom du rendu à générer.
   + D’autres clés/valeurs peuvent être fournies dans cet objet JSON et seront disponibles dans le programme de travail sous l’ `rendition.instructions` objet
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

1. Tap the __Run__ button
1. La section ____ Rendus est renseignée avec un espace réservé de rendu
1. Une fois le collaborateur terminé, l’espace réservé pour le rendu affiche le rendu généré.

Si vous modifiez le code du collaborateur pendant l’exécution de l’outil de développement, les modifications seront &quot;déployées à chaud&quot;. Le &quot;déploiement à chaud&quot; prend plusieurs secondes, donc laissez le déploiement se terminer avant de réexécuter le programme de travail à partir de l’outil de développement.

## Résolution des incidents

+ [Retrait YAML incorrect](../troubleshooting.md#incorrect-yaml-indentation)
+ [MemorySize limit est trop faible](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [L&#39;outil de développement ne peut pas être début en raison de l&#39;absence de private.key](../troubleshooting.md#missing-private-key)
+ [Liste déroulante des fichiers source incorrecte](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Paramètre de requête devToolToken manquant ou non valide](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Impossible de supprimer les fichiers source](../troubleshooting.md#unable-to-remove-source-files)
+ [Le rendu est renvoyé partiellement dessiné/corrompu](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
