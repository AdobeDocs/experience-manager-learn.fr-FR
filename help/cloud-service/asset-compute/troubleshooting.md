---
title: Résolution des problèmes d’extensibilité des Assets compute pour AEM Assets
description: Vous trouverez ci-dessous un index des problèmes et erreurs courants, ainsi que les résolutions, qui peuvent se produire lors du développement et du déploiement de programmes de travail Asset compute personnalisés pour AEM Assets.
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---

# Résolution des problèmes d’extensibilité des Assets compute

Vous trouverez ci-dessous un index des problèmes et erreurs courants, ainsi que les résolutions, qui peuvent se produire lors du développement et du déploiement de programmes de travail Asset compute personnalisés pour AEM Assets.

## Développer{#develop}

### Le rendu est renvoyé partiellement tracé/corrompu{#rendition-returned-partially-drawn-or-corrupt}

+ __Erreur__ : Le rendu est rendu incomplètement (lorsqu’une image) ou est corrompu et ne peut pas être ouvert.

   ![Le rendu est renvoyé partiellement tracé](./assets/troubleshooting/develop__await.png)

+ __Cause__ : La  `renditionCallback` fonction du programme de travail quitte avant que le rendu ne puisse être entièrement modifié  `rendition.path`.
+ __Résolution__ : Vérifiez le code de traitement personnalisé et assurez-vous que tous les appels asynchrones sont effectués de manière synchrone à l’aide de  `await`.

## Outil de développement{#development-tool}

### Fichier Console.json manquant dans le projet Asset compute{#missing-console-json}

+ __Erreur :__ Erreur : Fichiers requis manquants lors de la validation (.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.:XX:jsYY) à l’adresse async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.:XX:jsYY)
+ __Cause :__ le  `console.json` fichier est absent de la racine du projet Asset compute.
+ __Résolution :__ téléchargez un nouveau  `console.json` formulaire à partir de votre projet Adobe I/O.
   1. Dans console.adobe.io, ouvrez le projet Adobe I/O que le projet Asset compute est configuré pour utiliser
   1. Appuyez sur le bouton __Télécharger__ en haut à droite.
   1. Enregistrez le fichier téléchargé à la racine de votre projet Asset compute à l’aide du nom de fichier `console.json`

### Retrait YAML incorrect dans manifest.yml{#incorrect-yaml-indentation}

+ __Erreur :__ YAMLException : mauvaise mise en retrait d’une entrée de mappage à la ligne X, colonne Y : (via standard out from  `aio app run` command)
+ __Cause :__ les fichiers Yaml sont sensibles aux espaces blancs. Il est probable que votre mise en retrait soit incorrecte.
+ __Résolution :__ vérifiez votre  `manifest.yml` et assurez-vous que toutes les retraits sont corrects.

### La limite memorySize est définie trop basse{#memorysize-limit-is-set-too-low}

+ __Erreur :__  OpenWhiskError du serveur de développement local : PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Renvoyé HTTP 400 (Bad Request) —> &quot;Le contenu de la requête a été incorrect : l’exigence a échoué : mémoire 64 Mo en dessous du seuil autorisé de 134217728 B&quot;
+ __Cause :__ une  `memorySize` limite pour le programme de travail dans  `manifest.yml` a été définie sous le seuil minimal autorisé comme indiqué par le message d’erreur en octets.
+ __Résolution :__  vérifiez les  `memorySize` limites dans  `manifest.yml` et assurez-vous qu’elles sont toutes supérieures au seuil minimal autorisé.

### L’outil de développement ne peut pas démarrer en raison d’une valeur private.key manquante.{#missing-private-key}

+ __Erreur :__ Local Dev ServerError : Fichiers requis manquants à validatePrivateKeyFile.... (via standard, à partir de la commande `aio app run`)
+ __Cause :__ la  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valeur du  `.env` fichier ne pointe pas vers  `private.key` ou  `private.key` n’est pas lisible par l’utilisateur actuel.
+ __Résolution :__ vérifiez la  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valeur du  `.env` fichier et assurez-vous qu’il contient le chemin d’accès absolu et complet à  `private.key` sur votre système de fichiers.

### Fichier source : liste déroulante incorrect{#source-files-dropdown-incorrect}

L’outil de développement d’Asset compute peut entrer un état dans lequel il extrait les données obsolètes et est le plus visible dans la liste déroulante __Fichier source__ affichant des éléments incorrects.

+ __Erreur : la liste déroulante__ Fichier source affiche des éléments incorrects.
+ __Cause :__ l’état du navigateur mis en cache est obsolète.
+ __Résolution :__ dans votre navigateur, effacez complètement l’&quot;état d’application&quot; de l’onglet du navigateur, le cache du navigateur, le stockage local et le service worker.

### Paramètre de requête devToolToken absent ou non valide{#missing-or-invalid-devtooltoken-query-parameter}

+ __Erreur :__  notification &quot;non autorisée&quot; dans l’outil de développement Asset compute
+ __Cause :__ `devToolToken` est absent ou non valide
+ __Résolution :__ fermez la fenêtre du navigateur de l’outil de développement Asset compute, arrêtez les processus de l’outil de développement en cours d’exécution lancés via la  `aio app run` commande et redémarrez l’outil de développement (à l’aide de  `aio app run`).

### Impossible de supprimer les fichiers source{#unable-to-remove-source-files}

+ __Erreur :__ il n’existe aucun moyen de supprimer les fichiers source ajoutés de l’interface utilisateur des outils de développement.
+ __Cause :__ cette fonctionnalité n’a pas été implémentée.
+ __Résolution :__ connectez-vous à votre fournisseur de stockage dans le cloud à l’aide des informations d’identification définies dans  `.env`. Recherchez le conteneur utilisé par les outils de développement (également spécifié dans `.env`), accédez au dossier __source__ et supprimez les images sources. Vous devrez peut-être effectuer les étapes décrites dans la liste déroulante [Fichiers source incorrecte](#source-files-dropdown-incorrect) si les fichiers source supprimés continuent à s’afficher dans la liste déroulante, car ils peuvent être mis en cache localement dans l’&quot;état de l’application&quot; des outils de développement.

   ![Stockage Microsoft Azure Blob](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Test{#test}

### Aucun rendu généré lors de l’exécution du test{#test-no-rendition-generated}

+ __Erreur :__ Échec : Aucun rendu n’a été généré.
+ __Cause :__ le programme de travail n’a pas pu générer de rendu en raison d’une erreur inattendue, telle qu’une erreur de syntaxe JavaScript.
+ __Résolution :__ passez en revue l’exécution du test  `test.log` à l’adresse  `/build/test-results/test-worker/test.log`. Recherchez la section dans ce fichier correspondant au cas de test d’échec et recherchez les erreurs.

   ![Dépannage - Aucun rendu généré](./assets/troubleshooting/test__no-rendition-generated.png)

### Le test génère un rendu incorrect provoquant l’échec du test.{#tests-generates-incorrect-rendition}

+ __Erreur :__ Échec : Rendu &#39;rendition.xxx&#39; pas comme prévu.
+ __Cause :__ le programme de travail génère un rendu différent de celui  `rendition.<extension>` fourni dans le cas de test.
   + Si le fichier `rendition.<extension>` attendu n’est pas créé exactement de la même manière que le rendu généré localement dans le cas de test, le test peut échouer, car il peut y avoir une différence dans les bits. Par exemple, si le programme de travail des Assets compute modifie le contraste à l’aide des API et que le résultat attendu est créé en ajustant le contraste dans Adobe Photoshop CC, les fichiers peuvent apparaître de la même manière, mais des variations mineures dans les bits peuvent être différentes.
+ __Résolution :__ vérifiez la sortie du rendu du test en accédant à  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` et comparez-la au fichier de rendu attendu dans le cas de test. Pour créer une ressource exacte, procédez de l’une des manières suivantes :
   + Utilisez l’outil de développement pour générer un rendu, vérifier qu’il est correct et l’utiliser comme fichier de rendu attendu.
   + Ou, validez le fichier généré par le test à l’adresse `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, validez-le comme correct et utilisez-le comme fichier de rendu attendu.

## Déboguer

### Debugger ne se joint pas{#debugger-does-not-attach}

+ __Erreur__ : Lancement du traitement des erreurs : Erreur : Impossible de se connecter à la cible de débogage à l’adresse ...
+ __Cause__ : Docker Desktop n’est pas en cours d’exécution sur le système local. Vérifiez ce message en consultant la console de débogage du code VS (Affichage > Console de débogage), puis en confirmant que cette erreur est signalée.
+ __Résolution__ : Démarrez  [Docker Desktop et vérifiez que les images Docker requises sont installées](./set-up/development-environment.md#docker).

### Points d’arrêt en pause{#breakpoints-no-pausing}

+ __Erreur__ : Lors de l’exécution du programme de travail Asset compute à partir de l’outil de développement débogage, le code VS ne se met pas en pause aux points d’arrêt.

#### Débogueur de code VS non joint{#vs-code-debugger-not-attached}

+ __Cause :__ le débogueur VS Code a été arrêté/déconnecté.
+ __Résolution :__ Redémarrez le débogueur VS Code et vérifiez qu’il se joint en regardant la console de sortie de débogage VS Code (Affichage > Console de débogage).

#### Débogueur VS Code attaché après le début de l’exécution du programme de travail{#vs-code-debugger-attached-after-worker-execution-began}

+ __Cause :__ le débogueur VS Code ne s’est pas joint avant d’appuyer sur  ____ Runin Development Tool.
+ __Résolution :__ Assurez-vous que le débogueur a joint en examinant la console de débogage de VS Code (Affichage > Console de débogage), puis relancez le programme de travail d’Asset compute à partir de l’outil de développement.

### Le traitement expire lors du débogage{#worker-times-out-while-debugging}

+ __Erreur__ : La console de débogage signale que &quot;l’action expire dans -XXX millisecondes&quot; ou que l’aperçu du rendu de l’outil de développement d’ [Asset compute ](./develop/development-tool.md) s’étend indéfiniment ou
+ __Cause__ : Le délai d’expiration du programme de travail tel que défini dans le  [manifeste.](./develop/manifest.md) ymlis a été dépassé pendant le débogage.
+ __Résolution__ : Augmentez temporairement le délai d’attente du programme de travail dans le  [manifeste.](./develop/manifest.md) ymlor accélère les activités de débogage.

### Impossible d’arrêter le processus de débogage{#cannot-terminate-debugger-process}

+ __Erreur__ :  `Ctrl-C` sur la ligne de commande n’arrête pas le processus de débogage (`npx adobe-asset-compute devtool`).
+ __Cause__ : En cas de bogue dans  `@adobe/aio-cli-plugin-asset-compute` la version 1.3.x,  `Ctrl-C` aucune commande d’arrêt n’est reconnue.
+ __Résolution__ : Mise  `@adobe/aio-cli-plugin-asset-compute` à jour vers la version 1.4.1+

   ```
   $ aio update
   ```

   ![Dépannage - mise à jour aio](./assets/troubleshooting/debug__terminate.png)

## Déploiement{#deploy}

### Rendu personnalisé manquant dans la ressource dans AEM{#custom-rendition-missing-from-asset}

+ __Erreur :__ les ressources nouvelles et retraitées sont traitées avec succès, mais le rendu personnalisé est manquant.

#### Profil de traitement non appliqué au dossier ancêtre

+ __Cause :__  la ressource n’existe pas sous un dossier avec le profil de traitement qui utilise le programme de travail personnalisé.
+ __Résolution :__ Appliquez le profil de traitement à un dossier ancêtre de la ressource.

#### Profil de traitement remplacé par profil de traitement inférieur

+ __Cause :__ la ressource existe sous un dossier auquel est appliqué le profil de traitement personnalisé du programme de travail, mais un autre profil de traitement qui n’utilise pas le programme de travail du client a été appliqué entre ce dossier et la ressource.
+ __Résolution :__ Combinez ou réconciliez les deux profils de traitement et supprimez le profil de traitement intermédiaire.

### Échec du traitement des ressources dans AEM{#asset-processing-fails}

+ __Erreur :__ Badge Échec du traitement des ressources affiché sur la ressource
+ __Cause :__ une erreur s’est produite lors de l’exécution du programme de travail personnalisé
+ __Résolution :__ Suivez les instructions de  [débogage de l’](./test-debug/debug.md#aio-app-logs) activation de Adobe I/O Runtime à l’aide de  `aio app logs`.
