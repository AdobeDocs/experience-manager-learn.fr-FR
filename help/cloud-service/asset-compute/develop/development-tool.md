---
title: Outil de développement Asset compute
description: L’outil de développement d’Assets compute est un outil Web local permettant aux développeurs de configurer et d’exécuter localement les travailleurs d’Asset Computer, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset compute de Adobe I/O Runtime.
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


# Outil de développement Asset compute

L’outil de développement d’Assets compute est un outil Web local permettant aux développeurs de configurer et d’exécuter localement les travailleurs d’Asset Computer, en dehors du contexte du SDK AEM par rapport aux ressources d’Asset compute de Adobe I/O Runtime.

## Exécution de l&#39;outil de développement d&#39;Assets compute

L&#39;outil de développement d&#39;Asset compute peut être exécuté à partir de la racine du projet d&#39;Asset compute via la commande terminal :

```
$ aio app run
```

L’outil de développement sera alors début à l’adresse __http://localhost:9000__ et automatiquement ouvert dans une fenêtre de navigateur. Pour que l&#39;outil de développement s&#39;exécute, [un devToolToken valide généré automatiquement doit être fourni par l&#39;intermédiaire d&#39;un paramètre de requête](#troubleshooting__devtooltoken).

## Comprendre l&#39;interface Outils de développement d&#39;Asset compute{#interface}

![Outil de développement Asset compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Fichier source :__ la sélection du fichier source permet de :
   + Sélection du binaire de ressources qui sera le binaire `source` transmis au travailleur d’Asset compute
   + Téléchargement de fichiers source
1. __Définition du ou des profils d&#39;Asset compute :__ Définit le travailleur d&#39;Asset compute à exécuter, y compris les paramètres : incluant le point de terminaison de l’URL du collaborateur, le nom du rendu résultant et tous les paramètres
1. __Exécuter :__ le bouton Exécuter exécute le profil d&#39;Asset compute tel que défini dans l&#39;éditeur de profil de configuration d&#39;Asset compute
1. __Abandonner :__ le bouton Abandonner annule une exécution initiée lorsque vous appuyez sur le bouton Exécuter.
1. __Demande/réponse :__ fournit la demande et la réponse HTTP au/à l’agent d’Asset compute exécutant Adobe I/O Runtime. Cela peut s’avérer utile pour le débogage
1. __Journaux des Activations :__ Journaux décrivant l’exécution du travailleur d’Asset compute, ainsi que les erreurs éventuelles. Cette information est également disponible dans la norme `aio app run`
1. __Rendus :__ affiche tous les rendus générés par l’exécution de l’Asset compute Worker.
1. __paramètre de requête devToolToken :__ Le jeton Outil de développement d&#39;Asset compute nécessite la présence d&#39;un paramètre de  `devToolToken` requête valide. Ce jeton est généré automatiquement chaque fois qu’un nouvel outil de développement est généré.

### Exécution d’un travailleur personnalisé

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clic publicitaire de l’exécution d’un travail d’Asset compute dans l’outil de développement (sans audio)_

1. Assurez-vous que l&#39;outil de développement d&#39;Asset compute est démarré à partir de la racine du projet à l&#39;aide de la commande `aio app run`.
1. Dans l’outil de développement d’Asset compute, téléchargez ou sélectionnez un [fichier d’image d’exemple](../assets/samples/sample-file.jpg).
   + Assurez-vous que le fichier est sélectionné dans la liste déroulante __Fichier source__.
1. Examiner la zone de texte __Définition du profil d’Asset compute__
   + La clé `worker` définit l&#39;URL de l&#39;agent d&#39;Asset compute déployé.
   + La clé `name` définit le nom du rendu à générer.
   + D’autres clés/valeurs peuvent être fournies dans cet objet JSON et seront disponibles dans le programme de travail sous l’objet `rendition.instructions`
      + Ajoutez éventuellement des valeurs pour `size`, `contrast` et `brightness` :

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
1. La section __Rendus__ est renseignée avec un espace réservé de rendu
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
