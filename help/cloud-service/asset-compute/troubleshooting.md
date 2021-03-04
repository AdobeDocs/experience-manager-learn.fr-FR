---
title: Résolution des problèmes d'extensibilité des Assets compute pour AEM Assets
description: Vous trouverez ci-dessous un index des problèmes et erreurs courants, ainsi que des résolutions, qui peuvent se produire lors du développement et du déploiement de travailleurs d'Asset compute personnalisés pour AEM Assets.
feature: Microservices Asset compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Intégrations, développement
role: Développeur
level: Intermédiaire, expérimenté
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1249'
ht-degree: 0%

---


# Résolution des problèmes d&#39;extensibilité des Assets compute

Vous trouverez ci-dessous un index des problèmes et erreurs courants, ainsi que des résolutions, qui peuvent se produire lors du développement et du déploiement de travailleurs d&#39;Asset compute personnalisés pour AEM Assets.

## Développer{#develop}

### Le rendu est renvoyé partiellement dessiné/corrompu{#rendition-returned-partially-drawn-or-corrupt}

+ __Erreur__ : Le rendu est rendu incomplet (lorsqu’une image) ou endommagé et ne peut pas être ouvert.

   ![Le rendu est renvoyé partiellement tracé](./assets/troubleshooting/develop__await.png)

+ __Cause__ : La  `renditionCallback` fonction du collaborateur s’arrête avant que le rendu ne puisse être entièrement écrit  `rendition.path`.
+ __Résolution__ : Vérifiez le code de travail personnalisé et assurez-vous que tous les appels asynchrones sont effectués de manière synchrone à l’aide de  `await`.

## Outil de développement{#development-tool}

### Fichier Console.json manquant dans le projet d&#39;Asset compute{#missing-console-json}

+ __Erreur :__ Erreur : Fichiers requis manquants lors de la validation (.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) sur async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __Cause :__ Le  `console.json` fichier est absent de la racine du projet d&#39;Asset compute
+ __Résolution :__ Télécharger un nouveau  `console.json` formulaire de votre projet d&#39;Adobe I/O
   1. Dans console.adobe.io, ouvrez le projet d’Adobe I/O que le projet d’Asset compute est configuré pour utiliser
   1. Appuyez sur le bouton __Télécharger__ en haut à droite.
   1. Enregistrez le fichier téléchargé à la racine de votre projet d’Asset compute en utilisant le nom de fichier `console.json`.

### Retrait YAML incorrect dans manifest.yml{#incorrect-yaml-indentation}

+ __Erreur :__ YAMLException : indentation incorrecte d&#39;une entrée de mappage à la ligne X, colonne Y : (via standard out from  `aio app run` command)
+ __Cause : les fichiers__ Yaml sont sensibles à l’espacement blanc ; il est probable que votre mise en retrait soit incorrecte.
+ __Résolution :__ Vérifiez votre mise en retrait  `manifest.yml` et assurez-vous que toutes les mises en retrait sont correctes.

### La limite memorySize est définie sur too low{#memorysize-limit-is-set-too-low}

+ __Erreur : OpenWhiskError du serveur de développement__  local : PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true renvoyé HTTP 400 (Requête incorrecte) —> &quot;Le contenu de la requête n’est pas correct : l’exigence a échoué : mémoire 64 Mo en dessous du seuil autorisé de 134217728 B&quot;
+ __Cause :__ Une  `memorySize` limite pour le collaborateur dans le  `manifest.yml` a été définie en dessous du seuil minimum autorisé, comme indiqué par le message d&#39;erreur en octets.
+ __Résolution :__  Vérifiez les  `memorySize` limites dans le  `manifest.yml` et assurez-vous qu&#39;elles sont toutes supérieures au seuil minimum autorisé.

### L&#39;outil de développement ne peut pas être début en raison de l&#39;absence de private.key{#missing-private-key}

+ __Erreur : serveur de développement__ localErreur : Fichiers requis manquants à validatePrivateKeyFile.... (via standard out from `aio app run` command)
+ __Cause :__ La  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valeur du  `.env` fichier ne pointe pas vers  `private.key` ou  `private.key` n&#39;est pas lisible par l&#39;utilisateur actuel.
+ __Résolution :__ Vérifiez la  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valeur du  `.env` fichier et assurez-vous qu&#39;elle contient le chemin d&#39;accès complet et absolu au fichier  `private.key` sur votre système de fichiers.

### Liste déroulante des fichiers source incorrecte{#source-files-dropdown-incorrect}

L&#39;outil de développement d&#39;Asset compute peut entrer un état où il extrait les données obsolètes et est le plus visible dans la liste déroulante __Fichier source__ affichant des éléments incorrects.

+ __Erreur : la liste déroulante du fichier__ source affiche des éléments incorrects.
+ __Cause : l&#39;état__ du navigateur mis en cache est obsolète et entraîne la
+ __Résolution :__ dans votre navigateur, effacez complètement l&#39;&quot;état d&#39;application&quot; de l&#39;onglet du navigateur, le cache du navigateur, l&#39;enregistrement local et le travailleur de services.

### Paramètre de requête devToolToken manquant ou non valide{#missing-or-invalid-devtooltoken-query-parameter}

+ __Erreur : notification__ &quot;non autorisée&quot; dans l&#39;outil de développement d&#39;Asset compute
+ __Cause :__ `devToolToken` est manquante ou non valide
+ __Résolution :__ fermez la fenêtre du navigateur de l&#39;outil de développement d&#39;Asset compute, arrêtez tout processus d&#39;outil de développement en cours d&#39;exécution initié par l&#39;intermédiaire de la  `aio app run` commande, puis redébut Development Tool (à l&#39;aide  `aio app run`).

### Impossible de supprimer les fichiers source{#unable-to-remove-source-files}

+ __Erreur :__ il n’existe aucun moyen de supprimer les fichiers source ajoutés de l’interface utilisateur des outils de développement.
+ __Cause :__ Cette fonctionnalité n&#39;a pas été implémentée
+ __Résolution :__ connectez-vous à votre fournisseur d&#39;enregistrements cloud à l&#39;aide des informations d&#39;identification définies dans  `.env`. Recherchez le conteneur utilisé par les outils de développement (également spécifié dans `.env`), accédez au dossier __source__ et supprimez les images source. Vous devrez peut-être exécuter les étapes décrites dans la liste déroulante [Fichiers source incorrecte](#source-files-dropdown-incorrect) si les fichiers source supprimés continuent à s’afficher dans la liste déroulante car ils peuvent être mis en cache localement dans l’&quot;état de l’application&quot; des outils de développement.

   ![Stockage Microsoft Azure Blob](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Test{#test}

### Aucun rendu généré lors de l&#39;exécution du test{#test-no-rendition-generated}

+ __Erreur :__ échec : Aucun rendu généré.
+ __Cause :__ le programme de travail n&#39;a pas pu générer de rendu en raison d&#39;une erreur inattendue, telle qu&#39;une erreur de syntaxe JavaScript.
+ __Résolution :__ passez en revue l&#39;exécution du test  `test.log` à  `/build/test-results/test-worker/test.log`. Localisez la section de ce fichier correspondant au cas de test d’échec et recherchez les erreurs.

   ![Dépannage - Aucun rendu généré](./assets/troubleshooting/test__no-rendition-generated.png)

### Le test génère un rendu incorrect provoquant l’échec du test{#tests-generates-incorrect-rendition}

+ __Erreur :__ échec : Rendition &#39;rendition.xxx&#39; pas comme prévu.
+ __Cause :__ le collaborateur génère un rendu différent de celui  `rendition.<extension>` fourni dans le cas de test.
   + Si le fichier `rendition.<extension>` attendu n&#39;est pas créé exactement de la même manière que le rendu généré localement dans le cas de test, le test peut échouer car il peut y avoir une différence dans les bits. Par exemple, si le programme de travail des Assets compute modifie le contraste à l’aide d’API et que le résultat attendu est créé en ajustant le contraste dans Adobe Photoshop CC, les fichiers peuvent apparaître de la même manière, mais des variations mineures dans les bits peuvent être différentes.
+ __Résolution :__ Examinez la sortie du rendu du test en naviguant vers  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`et comparez-la au fichier de rendu attendu dans le cas du test. Pour créer une ressource attendue exacte, procédez comme suit :
   + Utilisez l’outil de développement pour générer un rendu, le valider et l’utiliser comme fichier de rendu attendu.
   + Ou, validez le fichier généré par le test sur `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, validez-le correctement et utilisez-le comme fichier de rendu attendu.

## Déboguer

### Le débogueur ne joint pas {#debugger-does-not-attach}

+ __Erreur__ : Erreur de traitement du lancement : Erreur : Impossible de se connecter à la cible de débogage à...
+ __Cause__ : Docker Desktop n&#39;est pas en cours d&#39;exécution sur le système local. Vérifiez ce point en examinant la console de débogage du code VS (Vue > Console de débogage), en confirmant que cette erreur est signalée.
+ __Résolution__ : Début  [Docker Desktop et vérifiez que les images Docker requises sont installées](./set-up/development-environment.md#docker).

### Points d’arrêt en pause {#breakpoints-no-pausing}

+ __Erreur__ : Lors de l&#39;exécution du programme de travail des Assets compute à partir de l&#39;outil de développement débogage, le code VS ne s&#39;arrête pas aux points d&#39;arrêt.

#### Débogueur de code VS non joint{#vs-code-debugger-not-attached}

+ __Cause :__ Le débogueur du code VS a été arrêté/déconnecté.
+ __Résolution :__ Redémarrez le débogueur du code VS et vérifiez qu&#39;il est joint en regardant la console de sortie de débogage du code VS (Vue > Console de débogage)

#### Débogueur de code VS attaché après le début de l&#39;exécution du programme de travail{#vs-code-debugger-attached-after-worker-execution-began}

+ __Cause :__ Le débogueur du code VS n&#39;était pas joint avant d&#39;appuyer sur l&#39;outil de développement  ____ d&#39;exécution.
+ __Résolution :__ assurez-vous que le débogueur est joint en examinant la Console de débogage de VS Code (Vue > Console de débogage), puis relancez l&#39;agent d&#39;Asset compute à partir de l&#39;outil de développement.

### Le programme de travail expire lors du débogage{#worker-times-out-while-debugging}

+ __Erreur__ : La console de débogage signale que &quot;l&#39;action va expirer dans -XXX millisecondes&quot; ou que la prévisualisation de rendu de l&#39;outil de développement d&#39; [Asset compute ](./develop/development-tool.md) tourne indéfiniment ou indéfiniment
+ __Cause__ : Le délai d&#39;expiration du programme de travail défini dans le fichier  [manifest.](./develop/manifest.md) ymlis a été dépassé pendant le débogage.
+ __Résolution__ : Augmentez temporairement le délai d’attente du collaborateur dans le  [manifest.](./develop/manifest.md) ymlor accélère les activités de débogage.

### Impossible d&#39;arrêter le processus de débogage{#cannot-terminate-debugger-process}

+ __Erreur__ :  `Ctrl-C` sur la ligne de commande n&#39;arrête pas le processus de débogage (`npx adobe-asset-compute devtool`).
+ __Cause__ : Un bogue de la  `@adobe/aio-cli-plugin-asset-compute` version 1.3.x  `Ctrl-C` n&#39;est pas reconnu comme une commande d&#39;arrêt.
+ __Résolution__ : Mise  `@adobe/aio-cli-plugin-asset-compute` à jour vers la version 1.4.1+

   ```
   $ aio update
   ```

   ![Dépannage - Mise à jour d’aio](./assets/troubleshooting/debug__terminate.png)

## Déploiement{#deploy}

### Rendu personnalisé manquant de l’élément dans AEM{#custom-rendition-missing-from-asset}

+ __Erreur :__ Le traitement des ressources nouvelles et retraitées a réussi, mais le rendu personnalisé est absent

#### Profil de traitement non appliqué au dossier ancêtre

+ __Cause :__ Le fichier n&#39;existe pas sous un dossier avec le Profil de traitement qui utilise le programme de travail personnalisé
+ __Résolution :__ Application du Profil de traitement à un dossier ancêtre de la ressource

#### Profil de traitement remplacé par Profil de traitement inférieur

+ __Cause :__ La ressource existe sous un dossier auquel est appliqué le Profil de traitement personnalisé du programme de travail. Toutefois, un autre Profil de traitement qui n’utilise pas le programme de travail client a été appliqué entre ce dossier et la ressource.
+ __Résolution :__ combiner ou réconcilier les deux Profils de traitement et supprimer le Profil de traitement intermédiaire

### Échec du traitement des ressources dans AEM{#asset-processing-fails}

+ __Erreur : badge__ Échec du traitement des ressources affiché sur la ressource
+ __Cause :__ une erreur s&#39;est produite dans l&#39;exécution du programme de travail personnalisé
+ __Résolution :__ Suivez les instructions relatives au  [débogage de l’](./test-debug/debug.md#aio-app-logs) activation de Adobe I/O Runtime à l’aide  `aio app logs`de.


