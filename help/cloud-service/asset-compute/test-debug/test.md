---
title: Tester un programme de travail Asset Compute
description: Le projet Asset Compute définit un modèle permettant de créer et d’exécuter facilement des tests des programmes de travail d’Asset Compute.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 182
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '613'
ht-degree: 100%

---

# Tester un programme de travail Asset Compute

Le projet Asset Compute définit un modèle permettant de créer et d’exécuter facilement des [tests des programmes de travail d’Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html?lang=fr).

## Anatomie du test d’un programme de travail

Les tests des programmes de travail d’Asset Compute sont divisés en suites de tests et, dans chaque suite de tests, un ou plusieurs cas de test affirmant une condition à tester.

La structure des tests dans un projet Asset Compute est la suivante :

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

Chaque cas de test peut comporter les fichiers suivants :

+ `file.<extension>`
   + Fichier source à tester (l’extension peut être n’importe laquelle sauf `.link`).
   + Requis
+ `rendition.<extension>`
   + Rendu attendu.
   + Obligatoire, sauf pour les tests d’erreur.
+ `params.json`
   + Instructions JSON sur le rendu unique.
   + Facultatif
+ `validate`
   + Script qui récupère les chemins d’accès aux fichiers de rendu attendus et réels sous forme d’arguments et qui doit renvoyer le code de sortie 0 si le résultat est OK, ou un code de sortie différent de zéro si la validation ou la comparaison a échoué.
   + Facultatif, par défaut sur la commande `diff`.
   + Utilisez un script shell qui inclut une commande d’exécution Docker pour utiliser différents outils de validation.
+ `mock-<host-name>.json`
   + Réponses HTTP au format JSON pour [simuler les appels de service externes](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Facultatif, utilisé uniquement si le code du programme de travail effectue ses propres requêtes HTTP.

## Écrire un cas de test

Ce cas de test affirme l’entrée paramétrée (`params.json`) pour le fichier d’entrée (`file.jpg`) qui génère le rendu PNG attendu (`rendition.png`).

1. Commencez par supprimer le cas de test `simple-worker` généré automatiquement sur `/test/asset-compute/simple-worker`, car il n’est pas valide, étant donné que notre programme de travail ne copie plus simplement la source dans le rendu.
1. Créez un dossier de cas de test sur `/test/asset-compute/worker/success-parameterized` pour tester une exécution réussie du programme de travail qui génère un rendu PNG.
1. Dans le dossier `success-parameterized`, ajoutez le [fichier d’entrée](./assets/test/success-parameterized/file.jpg) du test pour ce cas de test et nommez-le `file.jpg`.
1. Dans le dossier `success-parameterized`, ajoutez un nouveau fichier nommé `params.json` qui définit les paramètres d’entrée du programme de travail :

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Il s’agit des mêmes clés/valeurs transmises dans la [définition du profil d’Asset Compute de l’outil de développement](../develop/development-tool.md), à l’exception de la clé `worker`.

1. Ajoutez le [fichier de rendu](./assets/test/success-parameterized/rendition.png) attendu à ce cas de test et nommez-le `rendition.png`. Ce fichier représente la sortie attendue du programme de travail pour le fichier `file.jpg` d’entrée donné.
1. Sur la ligne de commande, exécutez le test de la racine du projet en exécutant `aio app test`.
   + Assurez-vous que l’application [Docker Desktop](../set-up/development-environment.md#docker) et les images Docker correspondantes sont installées et démarrées.
   + Arrêtez toutes les instances de l’outil de développement en cours d’exécution.

![Test - Réussite.](./assets/test/success-parameterized/result.png)

## Écrire un cas de test de vérification des erreurs

Ce cas de test teste sert à s’assurer que le programme de travail génère l’erreur appropriée lorsque le paramètre `contrast` est défini sur une valeur non valide.

1. Créez un dossier de cas de test sur `/test/asset-compute/worker/error-contrast` pour tester une exécution erronée du programme de travail en raison d’une valeur non valide du paramètre `contrast`.
1. Dans le dossier `error-contrast`, ajoutez le [fichier d’entrée](./assets/test/error-contrast/file.jpg) du test pour ce cas de test et nommez-le `file.jpg`. Le contenu de ce fichier n’est pas important pour ce test. Il doit simplement exister pour passer le contrôle « Source corrompue », afin d’atteindre les contrôles de validité `rendition.instructions`, que ce cas de test valide.
1. Dans le dossier `error-contrast`, ajoutez un nouveau fichier nommé `params.json` qui définit les paramètres d’entrée du programme de travail avec le contenu :

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Définissez les paramètres `contrast` sur `10`, une valeur non valide, car le contraste doit être compris entre -1 et 1, pour générer une `RenditionInstructionsError`.
   + Affirmez que l’erreur appropriée est générée dans les tests en définissant la clé `errorReason` sur la « raison » associée à l’erreur attendue. Ce paramètre de contraste non valide renvoie l’[erreur personnalisée](../develop/worker.md#errors), `RenditionInstructionsError`, définissez donc `errorReason` sur la raison de cette erreur, ou sur `rendition_instructions_error` pour affirmer qu’elle est générée.

1. Comme aucun rendu ne doit être généré lors d’une exécution erronée, aucun fichier `rendition.<extension>` n’est nécessaire.
1. Exécutez la suite de tests à partir de la racine du projet en exécutant la commande `aio app test`.
   + Assurez-vous que l’application [Docker Desktop](../set-up/development-environment.md#docker) et les images Docker correspondantes sont installées et démarrées.
   + Arrêtez toutes les instances de l’outil de développement en cours d’exécution.

![Test - Contraste d’erreur.](./assets/test/error-contrast/result.png)

## Cas de test sur Github

Les cas de test finaux sont disponibles sur Github :

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Résolution des problèmes

+ [Aucun rendu généré lors de l’exécution du test](../troubleshooting.md#test-no-rendition-generated)
+ [Le test génère un rendu incorrect.](../troubleshooting.md#tests-generates-incorrect-rendition)
