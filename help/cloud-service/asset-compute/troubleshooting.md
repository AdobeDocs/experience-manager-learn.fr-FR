---
title: Résolution des problèmes d'extensibilité de l'Asset Compute pour AEM Assets
description: Vous trouverez ci-dessous un index des problèmes courants et des erreurs, ainsi que des résolutions, qui peuvent se produire lors du développement et du déploiement de travailleurs de l’informatique personnalisée pour AEM Assets.
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# Dépannage de l’extensibilité de Asset Compute

Vous trouverez ci-dessous un index des problèmes courants et des erreurs, ainsi que des résolutions, qui peuvent se produire lors du développement et du déploiement de travailleurs de l’informatique personnalisée pour AEM Assets.

## Développer{#develop}

### Le rendu est renvoyé partiellement tracé/endommagé{#rendition-returned-partially-drawn-or-corrupt}

+ __Erreur__: Le rendu est rendu incomplet (lorsqu’une image) ou endommagé et ne peut pas être ouvert.

   ![Le rendu est renvoyé partiellement tracé](./assets/troubleshooting/develop__await.png)

+ __Cause__: La `renditionCallback` fonction du collaborateur s’arrête avant que le rendu ne puisse être entièrement écrit `rendition.path`.
+ __Résolution__: Vérifiez le code de travail personnalisé et assurez-vous que tous les appels asynchrones sont effectués de manière synchrone à l’aide de `await`.

## Outil de développement{#development-tool}

### Retrait YAML incorrect dans manifest.yml{#incorrect-yaml-indentation}

+ __Erreur :__ YAMLException : indentation incorrecte d&#39;une entrée de mappage à la ligne X, colonne Y : (via standard out from `aio app run` command)
+ __Cause :__ Les fichiers Yaml sont sensibles à l’espacement blanc, il est probable que votre mise en retrait soit incorrecte.
+ __Résolution :__ Vérifiez votre mise en retrait `manifest.yml` et assurez-vous que toutes les mises en retrait sont correctes.

### MemorySize limit est trop faible{#memorysize-limit-is-set-too-low}

+ __Erreur :__  OpenWhiskError du serveur de développement local : PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true renvoyé HTTP 400 (Requête incorrecte) —> &quot;Le contenu de la requête n’est pas correct : l’exigence a échoué : mémoire 64 Mo en dessous du seuil autorisé de 134217728 B&quot;
+ __Cause :__ Une `memorySize` limite pour le collaborateur dans le `manifest.yml` a été définie en deçà du seuil minimum autorisé, tel qu&#39;indiqué par le message d&#39;erreur en octets.
+ __Résolution :__  Examinez les `memorySize` limites dans le `manifest.yml` et assurez-vous qu’elles sont toutes supérieures au seuil minimum autorisé.

### L&#39;outil de développement ne peut pas être début en raison de l&#39;absence de private.key{#missing-private-key}

+ __Erreur :__ Erreur du serveur de développement local : Fichiers requis manquants à validatePrivateKeyFile.... (via standard out from `aio app run` command)
+ __Cause :__ La `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valeur dans `.env` le fichier, ne pointe pas vers `private.key` ou `private.key` n&#39;est pas lisible par l&#39;utilisateur actuel.
+ __Résolution :__ Vérifiez la `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valeur du `.env` fichier et assurez-vous qu’elle contient le chemin d’accès complet et absolu au fichier `private.key` sur votre système de fichiers.

### Liste déroulante des fichiers source incorrecte{#source-files-dropdown-incorrect}

L’outil de développement de calcul de ressources peut entrer un état dans lequel il extrait les données obsolètes et est le plus visible dans la liste déroulante des fichiers ____ source affichant des éléments incorrects.

+ __Erreur :__ La liste déroulante des fichiers source affiche des éléments incorrects.
+ __Cause :__ L’état du navigateur en cache obsolète provoque la
+ __Résolution :__ Dans votre navigateur, effacez complètement l’état de l’application de l’onglet du navigateur, le cache du navigateur, l’enregistrement local et l’agent de service.

### Paramètre de requête devToolToken manquant ou non valide{#missing-or-invalid-devtooltoken-query-parameter}

+ __Erreur :__ Notification &quot;non autorisée&quot; dans l’outil de développement d’Asset Compute
+ __Cause :__ `devToolToken` est absent ou non valide
+ __Résolution :__ Fermez la fenêtre du navigateur de l’outil de développement Asset Compute, arrêtez les processus de développement d’outil lancés via la `aio app run` commande et rédébut de l’outil de développement (à l’aide `aio app run`).

### Impossible de supprimer les fichiers source{#unable-to-remove-source-files}

+ __Erreur :__ Il n’existe aucun moyen de supprimer les fichiers source ajoutés de l’interface des outils de développement.
+ __Cause :__ Cette fonctionnalité n&#39;a pas été implémentée
+ __Résolution :__ Connectez-vous à votre fournisseur d’enregistrements cloud à l’aide des informations d’identification définies dans `.env`. Recherchez le conteneur utilisé par les outils de développement (également indiqués dans `.env`), accédez au dossier __source__ et supprimez les images source. Vous devrez peut-être exécuter les étapes décrites dans la liste déroulante des fichiers [source incorrectes](#source-files-dropdown-incorrect) si les fichiers source supprimés continuent à s’afficher dans la liste déroulante car ils peuvent être mis en cache localement dans l’&quot;état de l’application&quot; des outils de développement.

   ![Stockage Microsoft Azure Blob](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Test{#test}

### Aucun rendu généré lors de l’exécution du test{#test-no-rendition-generated}

+ __Erreur :__ Échec : Aucun rendu généré.
+ __Cause :__ Le programme de travail n&#39;a pas pu générer de rendu en raison d&#39;une erreur inattendue, telle qu&#39;une erreur de syntaxe JavaScript.
+ __Résolution :__ Examinez l&#39;exécution du test `test.log` à `/build/test-results/test-worker/test.log`. Localisez la section de ce fichier correspondant au cas de test d’échec et recherchez les erreurs.

   ![Dépannage - Aucun rendu généré](./assets/troubleshooting/test__no-rendition-generated.png)

### Le test génère un rendu incorrect provoquant l’échec du test.{#tests-generates-incorrect-rendition}

+ __Erreur :__ Échec : Rendition &#39;rendition.xxx&#39; pas comme prévu.
+ __Cause :__ Le programme de travail génère un rendu différent de celui `rendition.<extension>` fourni dans le cas de test.
   + Si le `rendition.<extension>` fichier attendu n&#39;est pas créé exactement de la même manière que le rendu généré localement dans le cas du test, le test peut échouer car il peut y avoir une différence dans les bits. Par exemple, si le programme de travail Asset Compute modifie le contraste à l’aide d’API et que le résultat attendu est créé en ajustant le contraste dans Adobe Photoshop CC, les fichiers peuvent apparaître de la même manière, mais des variations mineures dans les bits peuvent être différentes.
+ __Résolution :__ Examinez la sortie du rendu du test en accédant à `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`et comparez-la au fichier de rendu attendu dans le cas du test. Pour créer une ressource attendue exacte, procédez comme suit :
   + Utilisez l’outil de développement pour générer un rendu, le valider et l’utiliser comme fichier de rendu attendu.
   + Ou, validez le fichier généré par le test `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, validez-le et utilisez-le comme fichier de rendu attendu.

## Déboguer


### Le débogueur n’est pas joint{#debugger-does-not-attach}

+ __Erreur__: Erreur de traitement du lancement : Erreur : Impossible de se connecter à la cible de débogage à...
+ __Cause__: Docker Desktop n&#39;est pas en cours d&#39;exécution sur le système local. Vérifiez ce point en examinant la console de débogage du code VS (Vue > Console de débogage), en confirmant que cette erreur est signalée.
+ __Résolution__: Début [Docker Desktop et vérifiez que les images Docker requises sont installées](./set-up/development-environment.md#docker).

### Points d’arrêt non suspendus{#breakpoints-no-pausing}

+ __Erreur__: Lors de l&#39;exécution du programme de travail Asset Compute à partir de l&#39;outil de développement débogage, VS Code ne s&#39;arrête pas aux points d&#39;arrêt.

#### Débogueur de code VS non joint{#vs-code-debugger-not-attached}

+ __Cause :__ Le débogueur du code VS a été arrêté/déconnecté.
+ __Résolution :__ Redémarrez le débogueur de code VS et vérifiez qu&#39;il est joint en regardant la console de sortie de débogage du code VS (Vue > Console de débogage).

#### Débogueur de code VS attaché après le début de l&#39;exécution du programme de travail{#vs-code-debugger-attached-after-worker-execution-began}

+ __Cause :__ Le débogueur du code VS n&#39;était pas joint avant d&#39;appuyer sur __Exécuter__ dans l&#39;outil de développement.
+ __Résolution :__ Assurez-vous que le débogueur est joint en examinant la Console de débogage de VS Code (Vue > Console de débogage), puis réexécutez le programme de travail Asset Compute à partir de l&#39;outil de développement.

### Le programme de travail expire lors du débogage{#worker-times-out-while-debugging}

+ __Erreur__: La console de débogage signale que &quot;l&#39;action va expirer dans -XXX millisecondes&quot; ou que la prévisualisation de rendu de l&#39;outil de développement [Asset Compute](./develop/development-tool.md) tourne indéfiniment ou
+ __Cause__: Le délai d’expiration du programme de travail défini dans [manifest.yml](./develop/manifest.md) est dépassé pendant le débogage.
+ __Résolution__: Augmentez temporairement le délai d’attente du collaborateur dans [manifest.yml](./develop/manifest.md) ou accélérez les activités de débogage.

### Impossible d&#39;arrêter le processus de débogage{#cannot-terminate-debugger-process}

+ __Erreur__: `Ctrl-C` sur la ligne de commande n&#39;arrête pas le processus de débogage (`npx adobe-asset-compute devtool`).
+ __Cause__: Un bogue de la `@adobe/aio-cli-plugin-asset-compute` version 1.3.x `Ctrl-C` n&#39;est pas reconnu comme une commande d&#39;arrêt.
+ __Résolution__: Mise à jour `@adobe/aio-cli-plugin-asset-compute` de la version 1.4.1+

   ```
   $ aio update
   ```

   ![Dépannage - Mise à jour d’aio](./assets/troubleshooting/debug__terminate.png)

## Déploiement{#deploy}

### Rendu personnalisé absent de la ressource dans AEM{#custom-rendition-missing-from-asset}

+ __Erreur :__ Le nouveau traitement et le nouveau traitement des ressources ont réussi, mais le rendu personnalisé est absent

#### Profil de traitement non appliqué au dossier ancêtre

+ __Cause :__ La ressource n’existe pas sous un dossier avec le Profil de traitement qui utilise le programme de travail personnalisé.
+ __Résolution :__ Appliquer le Profil de traitement à un dossier ancêtre de la ressource

#### Profil de traitement remplacé par Profil de traitement inférieur

+ __Cause :__ Le fichier se trouve sous un dossier auquel est appliqué le Profil de traitement du programme de travail personnalisé. Toutefois, un autre Profil de traitement qui n’utilise pas le programme de travail client a été appliqué entre ce dossier et le fichier.
+ __Résolution :__ Combiner, ou réconcilier, les deux Profils de traitement et supprimer le Profil de traitement intermédiaire

### Échec du traitement des ressources dans AEM{#asset-processing-fails}

+ __Erreur :__ Balise Échec du traitement des ressources affichée sur la ressource
+ __Cause :__ Une erreur s&#39;est produite dans l&#39;exécution du programme de travail personnalisé
+ __Résolution :__ Suivez les instructions relatives au [débogage des activations](./test-debug/debug.md#aio-app-logs) Adobe I/O Runtime à l’aide de `aio app logs`.


