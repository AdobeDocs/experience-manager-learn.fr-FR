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
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '703'
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
1. __Définition du profil de calcul des ressources :__ Définit le programme de travail Asset Compute à exécuter, y compris les paramètres : incluant le point de terminaison de l’URL du collaborateur, le nom du rendu résultant et tous les paramètres
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

### Liste déroulante des fichiers source incorrecte{#troubleshooting__dev-tool-application-cache}

L’outil de développement de calcul de ressources peut entrer un état dans lequel il extrait les données obsolètes et est le plus visible dans la liste déroulante des fichiers ____ source affichant des éléments incorrects.

+ __Erreur :__ La liste déroulante des fichiers source affiche des éléments incorrects.
+ __Cause :__ L’état du navigateur en cache obsolète provoque la
+ __Résolution :__ Dans votre navigateur, effacez complètement l’état de l’application de l’onglet du navigateur, le cache du navigateur, l’enregistrement local et l’agent de service.

### Paramètre de requête devToolToken manquant ou non valide{#troubleshooting__devtooltoken}

+ __Erreur :__ Notification &quot;non autorisée&quot; dans l’outil de développement de l’informatique d’actifs
+ __Cause :__ `devToolToken` est absent ou non valide
+ __Résolution :__ Fermez la fenêtre du navigateur de l’outil de développement Asset Compute, arrêtez les processus de développement d’outil lancés via la `aio app run` commande et rédébut de l’outil de développement (à l’aide `aio app run`).

### Impossible de supprimer les fichiers source{#troubleshooting__remove-source-files}

+ __Erreur :__ Il n’existe aucun moyen de supprimer les fichiers source ajoutés de l’interface des outils de développement.
+ __Cause :__ Cette fonctionnalité n&#39;a pas été implémentée
+ __Résolution :__ Connectez-vous à votre fournisseur d’enregistrements cloud à l’aide des informations d’identification définies dans `.env`. Recherchez le conteneur utilisé par les outils de développement (également indiqués dans `.env`), accédez au dossier __source__ et supprimez les images source. Vous devrez peut-être exécuter les étapes décrites dans la liste déroulante des fichiers [source incorrectes](#troubleshooting__dev-tool-application-cache) si les fichiers source supprimés continuent à s’afficher dans la liste déroulante car ils peuvent être mis en cache localement dans l’&quot;état de l’application&quot; des outils de développement.

   ![Stockage Microsoft Azure Blob](./assets/development-tool/troubleshooting__remove-source-files.png)
