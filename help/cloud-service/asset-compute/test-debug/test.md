---
title: Test d’un Asset compute Worker
description: Le projet Asset compute définit un modèle permettant de créer et d’exécuter facilement des tests sur les employés d’Asset compute.
feature: Microservices Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: Intégrations, développement
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 1%

---


# Test d’un Asset compute Worker

Le projet Asset compute définit un modèle permettant de créer et d’exécuter facilement des [tests des agents d’Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## Anatomie d’un test de travail

Les tests des agents d’Asset compute sont divisés en suites de tests et, dans chaque suite de tests, un ou plusieurs cas de test affirmant une condition à tester.

La structure des tests dans un projet d’Asset compute est la suivante :

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

Chaque test peut comporter les fichiers suivants :

+ `file.<extension>`
   + Fichier source à tester (l’extension peut être tout sauf `.link`)
   + Requis
+ `rendition.<extension>`
   + Rendu attendu
   + Obligatoire, à l’exception des tests d’erreur
+ `params.json`
   + Instructions JSON sur le rendu unique
   + Facultatif
+ `validate`
   + Script qui récupère les chemins d’accès aux fichiers de rendu prévus et réels sous forme d’arguments et qui doit renvoyer le code de sortie 0 si le résultat est correct, ou un code de sortie non nul si la validation ou la comparaison a échoué.
   + Facultatif, la commande `diff` est utilisée par défaut.
   + Utilisez un script shell qui encapsule une commande docker run pour utiliser différents outils de validation.
+ `mock-<host-name>.json`
   + Réponses HTTP au format JSON pour [moquer les appels de service externes](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Facultatif, utilisé uniquement si le code de traitement effectue ses propres requêtes HTTP

## Ecriture d’un cas de test

Ce cas de test affirme que l’entrée paramétrée (`params.json`) pour le fichier d’entrée (`file.jpg`) génère le rendu PNG attendu (`rendition.png`).

1. Supprimez d’abord le cas de tests `simple-worker` générés automatiquement à l’adresse `/test/asset-compute/simple-worker`, car il n’est pas valide, car notre programme de travail ne copie plus simplement la source dans le rendu.
1. Créez un dossier de cas de test à l’adresse `/test/asset-compute/worker/success-parameterized` pour tester une exécution réussie du programme de travail qui génère un rendu PNG.
1. Dans le dossier `success-parameterized` , ajoutez le [fichier d’entrée](./assets/test/success-parameterized/file.jpg) test pour ce cas de test et nommez-le `file.jpg`.
1. Dans le dossier `success-parameterized` , ajoutez un nouveau fichier nommé `params.json` qui définit les paramètres d’entrée du programme de travail :

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Il s’agit des mêmes clés/valeurs transmises dans la définition de profil d’Asset compute ](../develop/development-tool.md) de l’outil de développement, moins la clé `worker`.[

1. Ajoutez le [fichier de rendu attendu](./assets/test/success-parameterized/rendition.png) à ce cas de test et nommez-le `rendition.png`. Ce fichier représente la sortie attendue du programme de travail pour l’entrée `file.jpg` donnée.
1. Sur la ligne de commande, exécutez le test de la racine du projet en exécutant `aio app test`
   + Vérifiez que [Docker Desktop](../set-up/development-environment.md#docker) et les images Docker prises en charge sont installées et démarrées.
   + Arrêtez toutes les instances de l’outil de développement en cours d’exécution.

![Test - Réussite  ](./assets/test/success-parameterized/result.png)

## Ecriture d’un cas de test de vérification d’erreur

Ce cas de test teste afin de s’assurer que le programme de travail renvoie l’erreur appropriée lorsque le paramètre `contrast` est défini sur une valeur non valide.

1. Créez un nouveau dossier de cas de test à l’adresse `/test/asset-compute/worker/error-contrast` pour tester une exécution erronée du programme de travail en raison d’une valeur de paramètre `contrast` non valide.
1. Dans le dossier `error-contrast` , ajoutez le [fichier d’entrée](./assets/test/error-contrast/file.jpg) test pour ce cas de test et nommez-le `file.jpg`. Le contenu de ce fichier n&#39;est pas important pour ce test, il doit simplement exister pour passer la vérification &quot;Source corrompue&quot;, afin d&#39;atteindre les `rendition.instructions` vérifications de validité, que ce cas de test valide.
1. Dans le dossier `error-contrast` , ajoutez un nouveau fichier nommé `params.json` qui définit les paramètres d’entrée du programme de travail avec le contenu :

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Définissez les paramètres `contrast` sur `10`, une valeur non valide, car le contraste doit être compris entre -1 et 1, pour générer une valeur `RenditionInstructionsError`.
   + Vérifiez que l’erreur appropriée est générée dans les tests en définissant la clé `errorReason` sur la &quot;raison&quot; associée à l’erreur attendue. Ce paramètre de contraste non valide renvoie l’[erreur personnalisée](../develop/worker.md#errors), `RenditionInstructionsError`, et définit donc la `errorReason` sur la raison de cette erreur, ou `rendition_instructions_error` pour affirmer qu’elle est générée.

1. Comme aucun rendu ne doit être généré lors d’une exécution erronée, aucun fichier `rendition.<extension>` n’est nécessaire.
1. Exécutez la suite de tests à partir de la racine du projet en exécutant la commande `aio app test`
   + Vérifiez que [Docker Desktop](../set-up/development-environment.md#docker) et les images Docker prises en charge sont installées et démarrées.
   + Arrêtez toutes les instances de l’outil de développement en cours d’exécution.

![Test - Contraste d’erreur](./assets/test/error-contrast/result.png)

## Cas de test sur Github

Les derniers cas de test sont disponibles sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Résolution des problèmes

+ [Aucun rendu généré lors de l’exécution du test](../troubleshooting.md#test-no-rendition-generated)
+ [Le test génère un rendu incorrect](../troubleshooting.md#tests-generates-incorrect-rendition)
