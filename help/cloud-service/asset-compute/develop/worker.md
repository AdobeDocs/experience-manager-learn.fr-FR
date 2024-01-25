---
title: Développer un programme de travail d’Asset Compute
description: Les programmes de travail Asset Compute sont au cœur des projets Asset Compute, car ils fournissent des fonctionnalités personnalisées qui exécutent ou orchestrent le travail effectué sur une ressource pour créer un rendu.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 572
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 100%

---

# Développer un programme de travail d’Asset Compute

Les programmes de travail Asset Compute sont l’essence d’un projet Asset Compute, car ils fournissent des fonctionnalités personnalisées qui exécutent ou orchestrent le travail effectué sur une ressource pour créer un rendu.

Le projet Asset Compute génère automatiquement un programme de travail simple qui copie le contenu binaire d’origine de la ressource dans un rendu nommé, sans aucune transformation. Dans ce tutoriel, nous allons modifier ce programme de travail afin de créer un rendu plus intéressant, qui tire parti de la puissance d’Asset Compute.

Nous allons créer un programme de travail Asset Compute qui génère un nouveau rendu d’image horizontal qui couvre l’espace vide à gauche et à droite du rendu de la ressource avec une version floue de la ressource. La largeur, la hauteur et le flou du rendu final peuvent être ajustés.

## Flux logique de l’appel d’un programme de travail Asset Compute

Les programmes de travail Asset Compute implémentent l’API Worker du SDK Asset Compute dans la fonction `renditionCallback(...)` :

+ __Entrée :__ fichier binaire d’origine d’une ressource AEM et paramètres de profil de traitement.
+ __Sortie :__ un ou plusieurs rendus à ajouter à la ressource AEM.

![Flux logique des programmes de travail Asset Compute](./assets/worker/logical-flow.png).

1. Le service de création AEM appelle le programme de travail Asset Compute, fournissant ainsi le contenu binaire d’origine __(1a)__ de la ressource (paramètre `source`) et tous les paramètres __(1b)__ définis dans le profil de traitement (paramètre `rendition.instructions`).
1. Le SDK Asset Compute orchestre l’exécution de la fonction `renditionCallback(...)` du programme de travail de métadonnées personnalisé Asset Compute et génère un nouveau rendu binaire, basé sur le fichier binaire d’origine de la ressource __(1a)__ et tous les paramètres __(1b)__.

   + Dans ce tutoriel, le rendu est créé « en cours », ce qui signifie que le programme de travail compose le rendu, mais le fichier binaire source peut également être envoyé à d’autres API de service web pour la génération du rendu.

1. Le programme de travail Asset Compute enregistre les données binaires du nouveau rendu dans `rendition.path`.
1. Les données binaires écrites dans `rendition.path` sont transmises au service de création AEM via le SDK Asset Compute. Elles sont affichées sous la forme d’un rendu texte __(4a)__ et __(4b)__ est conservé dans le nœud de métadonnées de la ressource.

Le diagramme ci-dessus présente les préoccupations des développeurs et développeuses Asset Compute et le flux logique de l’appel des programmes de travail Asset Compute. Pour épancher votre soif de connaissances inextinguible, les [détails internes de l’exécution d’Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html?lang=fr) sont disponibles, mais seuls les contrats publics de l’API de SDK Asset Compute peuvent être utilisés.

## En quoi consistent les programmes de travail ?

Tous les programmes de travail Asset Compute suivent la même structure de base et le même contrat d’entrée/sortie.

```javascript
'use strict';

// Any npm module imports used by the worker
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

/**
Exports the worker implemented by a custom rendition callback function, which parametrizes the input/output contract for the worker.
 + `source` represents the asset's original binary used as the input for the worker.
 + `rendition` represents the worker's output, which is the creation of a new asset rendition.
 + `params` are optional parameters, which map to additional key/value pairs, including a sub `auth` object that contains Adobe I/O access credentials.
**/
exports.main = worker(async (source, rendition, params) => {
    // Perform any necessary source (input) checks
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        // Throw appropriate errors whenever an erring condition is met
        throw new SourceCorruptError('source file is empty');
    }

    // Access any custom parameters provided via the Processing Profile configuration
    let param1 = rendition.instructions.exampleParam;

    /** 
    Perform all work needed to transform the source into the rendition.
    
    The source data can be accessed:
        + In the worker via a file available at `source.path`
        + Or via a presigned GET URL at `source.url`
    **/
    if (success) {
        // A successful worker must write some data back to `renditions.path`. 
        // This example performs a trivial 1:1 copy of the source binary to the rendition
        await fs.copyFile(source.path, rendition.path);
    } else {
        // Upon failure an Asset Compute Error (exported by @adobe/asset-compute-commons) should be thrown.
        throw new GenericError("An error occurred!", "example-worker");
    }
});

/**
Optionally create helper classes or functions the worker's rendition callback function invokes to help organize code.

Code shared across workers, or to complex to be managed in a single file, can be broken out across supporting JavaScript files in the project and imported normally into the worker. 
**/
function customHelperFunctions() { ... }
```

## Ouvrir le programme de travail index.js

![Fichier Index.js généré automatiquement](./assets/worker/autogenerated-index-js.png).

1. Assurez-vous que le projet Asset Compute est ouvert dans VS Code.
1. Accédez au dossier `/actions/worker`.
1. Ouvrez le fichier `index.js`.

Il s’agit du fichier JavaScript du programme de travail que nous allons modifier dans ce tutoriel.

## Installer et importer les modules npm de prise en charge

Basés sur Node.js, les projets Asset Compute bénéficient de la puissance de l’[écosystème des modules npm](https://npmjs.com). Pour tirer parti des modules npm, nous devons d’abord les installer dans notre projet Asset Compute.

Dans ce programmes de travail, nous utilisons [jimp](https://www.npmjs.com/package/jimp) pour créer et manipuler l’image de rendu directement dans le code Node.js.

>[!WARNING]
>
>Tous les modules npm pour la manipulation de ressources ne sont pas pris en charge par Asset Compute. Les modules npm qui reposent sur l’existence d’applications telles qu’ImageMagick ou d’autres bibliothèques dépendantes du système d’exploitation ne sont pas pris en charge. Il est recommandé de limiter l’utilisation des modules npm à JavaScript uniquement.

1. Ouvrez la ligne de commande à la racine de votre projet Asset Compute (cela peut être effectué dans VS Code via __Terminal > Nouveau terminal__) et exécutez la commande suivante :

   ```
   $ npm install jimp
   ```

1. Importez le module `jimp` dans le code du programme de travail afin qu’il puisse être utilisé via l’objet JavaScript `Jimp`.
Mettez à jour les directives `require` écrites au début de l’`index.js` du programme de travail pour importer l’objet `Jimp` à partir du module `jimp` :

   ```javascript
   'use strict';
   
   const Jimp = require('jimp');
   const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
   const fs = require('fs').promises;
   
   exports.main = worker(async (source, rendition, params) => {
       // Check handle a corrupt input source
       const stats = await fs.stat(source.path);
       if (stats.size === 0) {
           throw new SourceCorruptError('source file is empty');
       }
   
       // Do work here
   });
   ```

## Lire les paramètres

Les programmes de travail Asset Compute peuvent lire les paramètres qui peuvent être transmis via les profils de traitement définis dans le service de création AEM as a Cloud Service. Les paramètres sont transmis au programme de travail via l’objet `rendition.instructions`.

Ils peuvent être lus en accédant à `rendition.instructions.<parameterName>` dans le code du programme de travail.

Ici, nous lirons les valeurs `SIZE`, `BRIGHTNESS` et `CONTRAST` du rendu configurable, en fournissant des valeurs par défaut si aucune n’a été fournie par le profil de traitement. Notez que les `renditions.instructions` sont transmises en tant que chaînes lors de l’appel à partir des profils de traitement AEM as a Cloud Service. Veillez donc à ce qu’elles soient transformées en types de données corrects dans le code du programme de travail.

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    // Processing Profiles pass in instructions as Strings, so make sure to parse to correct data types
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    // Do work here
}
```

## Erreurs de génération{#errors}

Les programmes de travail Asset Compute peuvent rencontrer des situations qui génèrent des erreurs. Le SDK Asset Compute d’Adobe fournit [une suite d’erreurs prédéfinies](https://github.com/adobe/asset-compute-commons#asset-compute-errors) qui peuvent être générées dans ce genre de situation. Si aucun type d’erreur spécifique n’est appliqué, l’`GenericError` peut être utilisée, ou des `ClientErrors` personnalisées spécifiques peuvent être définies.

Avant de commencer le traitement du rendu, vérifiez que tous les paramètres sont valides et pris en charge dans le contexte de ce programme de travail :

+ Assurez-vous que les paramètres d’instruction de rendu de `SIZE`, `CONTRAST`, et `BRIGHTNESS` sont valides. Si ce n’est pas le cas, générez une erreur personnalisée `RenditionInstructionsError`.
   + Une classe `RenditionInstructionsError` personnalisée qui étend `ClientError` est définie au bas de ce fichier. L’utilisation d’une erreur personnalisée spécifique est utile lors de l’[écriture de tests](../test-debug/test.md) pour le programme de travail.

```javascript
'use strict';

const Jimp = require('jimp');
// Import the Asset Compute SDK provided `ClientError` 
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        // Ensure size is within allowable bounds
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Do work here
}

// Create a new ClientError to handle invalid rendition.instructions values
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        // Provide a:
        // + message: describing the nature of this erring condition
        // + name: the name of the error; usually same as class name
        // + reason: a short, searchable, unique error token that identifies this error
        super(message, "RenditionInstructionsError", "rendition_instructions_error");

        // Capture the strack trace
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Créer le rendu

Avec les paramètres lus, assainis et validés, le code est écrit pour générer le rendu. Le pseudo-code pour la génération du rendu est le suivant :

1. Créez de nouveaux canevas `renditionImage` aux dimensions carrées spécifiées via le paramètre `size`.
1. Créez un objet `image` à partir du fichier binaire de la ressource source.
1. Utilisez la bibliothèque __Jimp__ pour transformer l’image :
   + Recadrez l’image d’origine sur un carré centré.
   + Coupez un cercle à partir du centre de l’image « rendue carrée ».
   + Adaptez aux dimensions définies par la valeur de paramètre `SIZE`.
   + Ajustez le contraste en fonction de la valeur de paramètre `CONTRAST`.
   + Ajustez la luminosité en fonction de la valeur de paramètre `BRIGHTNESS`.
1. Placez l’`image` au milieu de l’`renditionImage` avec un arrière-plan transparent.
1. Écrivez la composition, `renditionImage` vers `rendition.path`, afin qu’elle puisse être réenregistrée dans AEM en tant que rendu de ressource.

Ce code utilise les [API Jimp](https://github.com/oliver-moran/jimp#jimp) pour effectuer ces transformations d’image.

Les programmes de travail Assets Compute doivent terminer leur travail de manière synchrone, et le `rendition.path` doit être entièrement réécrit avant la fin de l’`renditionCallback` du programme de travail. Cela nécessite que les appels des fonctions asynchrones soient effectués de manière synchrone à l’aide de l’opérateur `await`. Si vous ne connaissez pas les fonctions asynchrones JavaScript et que vous ne savez pas comment les exécuter de manière synchrone, familiarisez-vous avec l’[opérateur d’attente de JavaScript](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/await).

L’`index.js` du programme de travail terminé doit ressembler à ce qui suit :

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read/parse and validate parameters
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image 
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness
    image.crop(
            image.bitmap.width < image.bitmap.height ? 0 : (image.bitmap.width - image.bitmap.height) / 2,
            image.bitmap.width < image.bitmap.height ? (image.bitmap.height - image.bitmap.width) / 2 : 0,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height
        )   
        .circle()
        .scaleToFit(SIZE, SIZE)
        .contrast(CONTRAST)
        .brightness(BRIGHTNESS);

    // Place the transformed image onto the transparent renditionImage to save as PNG
    renditionImage.composite(image, 0, 0)

    // Write the final transformed image to the asset's rendition
    await renditionImage.writeAsync(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Exécuter le programme de travail

Maintenant que le code du programme de travail est terminé, et qu’il a été enregistré et configuré dans le [manifest.yml](./manifest.md), il peut être exécuté à l’aide de l’outil de développement d’Asset Compute local pour afficher les résultats.

1. À partir de la racine du projet Asset Compute
1. Exécuter `aio app run`
1. Attendez que l’outil de développement Asset Compute s’ouvre dans une nouvelle fenêtre.
1. Dans le menu déroulant __Sélectionnez un fichier...__, sélectionnez un exemple d’image à traiter.
   + Sélectionnez un exemple de fichier image à utiliser comme fichier binaire de ressource source.
   + S’il n’en existe aucun, appuyez sur le __(+)__ à gauche, puis chargez un fichier d’[exemple d’image](../assets/samples/sample-file.jpg) et actualisez la fenêtre du navigateur des outils de développement.
1. Mettez à jour `"name": "rendition.png"` comme programme de travail pour générer un fichier PNG transparent.
   + Notez que ce paramètre « name » n’est utilisé que pour l’outil de développement et ne doit pas être utilisé.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png"
           }
       ]
   }
   ```

1. Appuyez sur __Exécuter__ et attendez que le rendu soit généré.
1. La section __Rendus__ prévisualise le rendu généré. Appuyez sur l’aperçu du rendu pour télécharger le rendu complet.

   ![Rendu PNG par défaut.](./assets/worker/default-rendition.png)

### Exécuter le programme de travail avec des paramètres

Les paramètres, transmis via les configurations de profil de traitement, peuvent être simulés dans les outils de développement Asset Compute en les fournissant en tant que paires clé/valeur sur le paramètre de rendu JSON.

>[!WARNING]
>
>Lors du développement local, les valeurs peuvent être transmises à l’aide de différents types de données, lorsqu’elles sont transmises à partir des profils de traitement AEM as a Cloud Service sous forme de chaînes. Assurez-vous donc que les types de données appropriés sont analysés si nécessaire.
> Par exemple, la fonction `crop(width, height)` de Jimp nécessite que ses paramètres soient des `int`. Si `parseInt(rendition.instructions.size)` n’est pas transformé en entier, l’appel à `jimp.crop(SIZE, SIZE)` échoue, car les paramètres sont incompatibles avec le type « Chaîne ».

Notre code accepte les paramètres pour les éléments suivants :

+ `size` définit la taille du rendu (hauteur et largeur en tant qu’entiers) ;
+ `contrast` définit l’ajustement du contraste, qui doit être compris entre -1 et 1, sous la forme de flotteurs ;
+ `brightness` définit l’ajustement de la luminosité, qui doit être compris entre -1 et 1, sous la forme de flotteurs.

Ceux-ci sont lus dans le programme de travail `index.js` via :

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Mettez à jour les paramètres de rendu pour personnaliser la taille, le contraste et la luminosité.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png",
               "size": "450",
               "contrast": "0.30",
               "brightness": "0.15"
           }
       ]
   }
   ```

1. Appuyez à nouveau sur __Exécuter__.
1. Appuyez sur l’aperçu du rendu pour télécharger et consulter le rendu généré. Notez ses dimensions et la manière dont le contraste et la luminosité ont été modifiés par rapport au rendu par défaut.

   ![Rendu PNG paramétré.](./assets/worker/parameterized-rendition.png)

1. Chargez d’autres images dans le menu déroulant __Fichier source__ et essayez d’exécuter le programme de travail avec des paramètres différents.

## index.js de programme de travail sur Github

L’`index.js` final est disponible sur Github à :

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Résolution des problèmes

+ [Rendu renvoyé partiellement tracé/corrompu](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
