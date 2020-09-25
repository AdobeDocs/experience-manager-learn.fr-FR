---
title: Test d'un intervenant de calcul de ressources
description: Le projet Asset Compute définit un modèle permettant de créer et d’exécuter facilement des tests sur les employés d’Asset Compute.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 06632b90e5cdaf80b9343e5a69ab9c735d4a70f1
workflow-type: tm+mt
source-wordcount: '792'
ht-degree: 0%

---


# Test d&#39;un intervenant de calcul de ressources

Le projet Asset Compute définit un modèle permettant de créer et d&#39;exécuter facilement des [tests pour les employés](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)d&#39;Asset Compute.

## Anatomie d’un test de travail

Les tests du personnel de calcul des ressources sont divisés en suites de tests et, dans chaque suite de tests, un ou plusieurs cas de test qui affirment une condition à tester.

La structure des tests dans un projet Asset Compute est la suivante :

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

Chaque test peut comporter les fichiers suivants :

+ `file.<extension>`
   + Fichier source à tester (l&#39;extension peut être n&#39;importe quoi sauf `.link`le cas échéant)
   + Requis
+ `rendition.<extension>`
   + Rendu attendu
   + Obligatoire, à l’exception des tests d’erreur
+ `params.json`
   + Instructions JSON de rendu unique
   + Facultatif
+ `validate`
   + Script qui obtient les chemins d’accès au fichier de rendu réel et attendu en tant qu’arguments et qui doit renvoyer le code de sortie 0 si le résultat est ok, ou un code de sortie non nul si la validation ou la comparaison a échoué.
   + Facultatif, la `diff` commande est utilisée par défaut.
   + Utilisez un script shell qui encapsule une commande docker run pour utiliser différents outils de validation.
+ `mock-<host-name>.json`
   + Les réponses HTTP au format JSON pour [masquer les appels](https://www.mock-server.com/mock_server/creating_expectations.html)de service externe.
   + Facultatif, utilisé uniquement si le code de travail effectue lui-même des requêtes HTTP

## Ecriture d’un cas de test

Ce cas de test affirme que l’entrée paramétrée (`params.json`) pour le fichier d’entrée (`file.jpg`) génère le rendu PNG attendu (`rendition.png`).

1. Commencez par supprimer le cas de `simple-worker` tests générés automatiquement `/test/asset-compute/simple-worker` car il n’est pas valide, car notre collaborateur ne copie plus simplement la source dans le rendu.
1. Créez un dossier de cas de test sur `/test/asset-compute/worker/success-parameterized` pour tester une exécution réussie du programme de travail qui génère un rendu PNG.
1. Dans le `success-parameterized` dossier, ajoutez le fichier [d’](./assets/test/success-parameterized/file.jpg) entrée de test pour ce cas de test et nommez-le `file.jpg`.
1. Dans le `success-parameterized` dossier, ajoutez un nouveau fichier nommé `params.json` qui définit les paramètres d’entrée du programme de travail :

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   Il s&#39;agit des mêmes clés/valeurs transmises dans la définition [du profil](../develop/development-tool.md)Asset Compute de l&#39;outil de `worker` développement, moins laclé.
1. ajoutez le fichier [de](./assets/test/success-parameterized/rendition.png) rendu attendu à ce cas de test et nommez-le `rendition.png`. Ce fichier représente la sortie attendue du travailleur pour l’entrée donnée `file.jpg`.
1. Dans la ligne de commande, exécutez les tests de la racine du projet en exécutant `aio app test`
   + Assurez-vous que [Docker Desktop](../set-up/development-environment.md#docker) et les images Docker prises en charge sont installées et démarrées.
   + Arrêt de toute instance de l’outil de développement en cours d’exécution

![Test - Réussite ](./assets/test/success-parameterized/result.png)

## Ecriture d’un cas de test de vérification d’erreur

Ce cas de test est testé pour s’assurer que le programme de travail génère l’erreur appropriée lorsque le `contrast` paramètre est défini sur une valeur non valide.

1. Créez un dossier de cas de test à `/test/asset-compute/worker/error-contrast` pour tester une exécution erronée du programme de travail en raison d&#39;une valeur de `contrast` paramètre non valide.
1. Dans le `error-contrast` dossier, ajoutez le fichier [d’](./assets/test/error-contrast/file.jpg) entrée de test pour ce cas de test et nommez-le `file.jpg`. Le contenu de ce fichier n&#39;est pas important pour ce test, il doit juste exister pour passer la vérification &quot;source corrompue&quot;, afin d&#39;atteindre les vérifications de `rendition.instructions` validité, que ce cas de test valide.
1. Dans le `error-contrast` dossier, ajoutez un nouveau fichier nommé `params.json` qui définit les paramètres d’entrée du programme de travail avec le contenu :

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Définissez `contrast` les paramètres sur `10`, une valeur non valide, car le contraste doit être compris entre -1 et 1, pour lancer une `RenditionInstructionsError`valeur.
   + Affirmer que l&#39;erreur appropriée est générée dans les tests en définissant la `errorReason` clé sur la &quot;raison&quot; associée à l&#39;erreur attendue. Ce paramètre de contraste non valide renvoie l&#39;erreur [](../develop/worker.md#errors)personnalisée `RenditionInstructionsError`, par conséquent, définissez la `errorReason` raison de cette erreur ou`rendition_instructions_error` pour affirmer qu&#39;elle est générée.

1. Aucun rendu ne devant être généré lors d’une exécution erronée, aucun `rendition.<extension>` fichier n’est nécessaire.
1. Exécutez la suite de tests à partir de la racine du projet en exécutant la commande `aio app test`
   + Assurez-vous que [Docker Desktop](../set-up/development-environment.md#docker) et les images Docker prises en charge sont installées et démarrées.
   + Arrêt de toute instance de l’outil de développement en cours d’exécution

![Test - Contraste d’erreur](./assets/test/error-contrast/result.png)

## Résolution des incidents

### Aucun rendu généré

Le cas de test échoue sans générer de rendu.

+ __Erreur :__ Échec : Aucun rendu généré.
+ __Cause :__ Le programme de travail n&#39;a pas pu générer de rendu en raison d&#39;une erreur inattendue, telle qu&#39;une erreur de syntaxe JavaScript.
+ __Résolution :__ Examinez l&#39;exécution du test `test.log` à `/build/test-results/test-worker/test.log`. Localisez la section de ce fichier correspondant au cas de test d’échec et recherchez les erreurs.

   ![Dépannage - Aucun rendu généré](./assets/test/troubleshooting__no-rendition-generated.png)

### Le test génère un rendu incorrect

Le cas de test ne génère pas un rendu incorrect.

+ __Erreur :__ Échec : Rendition &#39;rendition.xxx&#39; pas comme prévu.
+ __Cause :__ Le programme de travail génère un rendu différent de celui `rendition.<extension>` fourni dans le cas de test.
   + Si le `rendition.<extension>` fichier attendu n&#39;est pas créé exactement de la même manière que le rendu généré localement dans le cas du test, le test peut échouer car il peut y avoir une différence dans les bits. Si le rendu attendu dans le cas de test est enregistré à partir de l’outil de développement, c’est-à-dire généré dans Adobe I/O Runtime, les bits peuvent être techniquement différents, ce qui peut entraîner l’échec du test, même si, d’un point de vue humain, les fichiers de rendu attendus et réels sont identiques.
+ __Résolution :__ Examinez la sortie du rendu du test en accédant à `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`et comparez-la au fichier de rendu attendu dans le cas du test.
