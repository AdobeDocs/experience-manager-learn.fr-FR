---
title: Développement d’un Asset compute Worker
description: Les objets Worker Asset compute sont au coeur des projets d’Asset compute, car ils fournissent des fonctionnalités personnalisées qui exécutent ou orchestrent le travail effectué sur une ressource pour créer un rendu.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# Développement d’un Asset compute Worker

Les objets Worker Asset compute sont au coeur d’un projet Asset compute, car ils fournissent des fonctionnalités personnalisées qui exécutent ou orchestrent le travail effectué sur une ressource pour créer un rendu.

Le projet Asset compute génère automatiquement un programme de travail simple qui copie le binaire d’origine de la ressource dans un rendu nommé, sans aucune conversion. Dans ce tutoriel, nous allons modifier ce programme de travail pour créer un rendu plus intéressant, afin d’illustrer la puissance des Assets compute.

Nous allons créer un programme de travail d’Asset compute qui génère un nouveau rendu d’image horizontal qui couvre l’espace vide à gauche et à droite du rendu de la ressource avec une version floue de la ressource. La largeur, la hauteur et le flou du rendu final sont paramétrés.

## Flux logique de l’appel d’un ouvrier d’Asset compute

Les agents d’Asset compute implémentent le contrat d’API de traitement du SDK Asset compute, dans la variable `renditionCallback(...)` , qui est conceptuellement :

+ __Entrée :__ Le binaire d’origine d’une ressource AEM et les paramètres de profil de traitement
+ __Sortie :__ Un ou plusieurs rendus à ajouter à la ressource AEM

![Flux logique Asset compute Worker](./assets/worker/logical-flow.png)

1. Le service AEM Author appelle le programme de travail d’Asset compute, en fournissant le __(1a)__ binaire d’origine (`source` ) et __(1b)__ tous les paramètres définis dans le profil de traitement (`rendition.instructions` ).
1. Le SDK Asset compute orchestre l’exécution du programme de travail de métadonnées d’Asset compute personnalisé `renditionCallback(...)` , génération d’un nouveau rendu binaire, en fonction du fichier binaire d’origine de la ressource. __(1a)__ et tout paramètre __(1b)__.

   + Dans ce tutoriel, le rendu est créé &quot;en cours&quot;, ce qui signifie que le programme de travail compose le rendu, mais le binaire source peut également être envoyé à d’autres API de service Web pour la génération du rendu.

1. Asset compute Worker enregistre les données binaires du nouveau rendu dans `rendition.path`.
1. Les données binaires écrites dans `rendition.path` est transporté via le SDK Asset compute vers le service d’auteur AEM et exposé sous la forme __(4a)__ un rendu texte et __(4b)__ persistait dans le noeud de métadonnées de la ressource.

Le diagramme ci-dessus exprime les préoccupations des développeurs Assets compute et le flux logique vers l’appel des employés d’Asset compute. Pour les curieux, le [Détails internes de l’exécution de l’Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) sont disponibles, mais seuls les contrats d’API SDK d’Asset compute public peuvent être dépendants.

## Anatomie d&#39;un travailleur

Tous les employés d’Asset compute suivent la même structure de base et le même contrat d’entrée/sortie.

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

## Ouverture du fichier worker index.js

![Index.js généré automatiquement](./assets/worker/autogenerated-index-js.png)

1. Assurez-vous que le projet d’Asset compute est ouvert dans VS Code.
1. Accédez au `/actions/worker` folder
1. Ouvrez le `index.js` fichier

Il s’agit du fichier JavaScript Worker que nous allons modifier dans ce tutoriel.

## Installer et importer les modules npm de prise en charge

Basés sur Node.js, les projets d’Asset compute bénéficient de la puissance [écosystème de module npm](https://npmjs.com). Pour tirer parti des modules npm, nous devons d’abord les installer dans notre projet Asset compute.

Dans ce travailleur, nous exploitons la variable [jimp](https://www.npmjs.com/package/jimp) pour créer et manipuler l’image de rendu directement dans le code Node.js.

>[!WARNING]
>
>Tous les modules npm pour la manipulation de ressources ne sont pas pris en charge par Asset compute. Les modules npm qui reposent sur l’existence d’applications telles qu’ImageMagick ou d’autres bibliothèques dépendantes du système d’exploitation ne sont pas pris en charge. Il est préférable de limiter l’utilisation des modules npm JavaScript uniquement.

1. Ouvrez la ligne de commande à la racine de votre projet Asset compute (cela peut être effectué dans VS Code via __Terminal > Nouveau terminal__) et exécutez la commande :

   ```
   $ npm install jimp
   ```

1. Importez la variable `jimp` dans le code de travail afin qu’il puisse être utilisé via le `Jimp` Objet JavaScript.
Mettez à jour le `require` directives en haut de l’objet Worker’s `index.js` pour importer la variable `Jimp` à partir de l’objet `jimp` module :

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

## Lecture des paramètres

Les objets Worker Asset compute peuvent lire les paramètres qui peuvent être transmis via les profils de traitement définis dans AEM service de création as a Cloud Service. Les paramètres sont transmis au programme de travail via le `rendition.instructions` .

Ils peuvent être lus en accédant à `rendition.instructions.<parameterName>` dans le code de travail.

Nous allons lire ici le rapport `SIZE`, `BRIGHTNESS` et `CONTRAST`, fournissant des valeurs par défaut si aucune valeur n’a été fournie via le profil de traitement. Notez que `renditions.instructions` sont transmis en tant que chaînes lors de l’appel à partir AEM profils de traitement as a Cloud Service. Veillez donc à ce qu’ils soient transformés en types de données corrects dans le code de travail.

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

Les objets Worker Asset compute peuvent rencontrer des situations qui génèrent des erreurs. Le SDK Adobe Asset compute fournit [une suite d&#39;erreurs prédéfinies](https://github.com/adobe/asset-compute-commons#asset-compute-errors) qui peuvent être générés en cas de situation de ce type. Si aucun type d’erreur spécifique n’est appliqué, la variable `GenericError` peut être utilisé, ou personnalisé spécifique. `ClientErrors` peuvent être définies.

Avant de commencer le traitement du rendu, vérifiez que tous les paramètres sont valides et pris en charge dans le contexte de ce programme de travail :

+ Assurez-vous que les paramètres d’instruction de rendu pour `SIZE`, `CONTRAST`, et `BRIGHTNESS` sont valides. Si ce n’est pas le cas, envoyez une erreur personnalisée. `RenditionInstructionsError`.
   + Une `RenditionInstructionsError` qui étend `ClientError` est défini au bas de ce fichier. L’utilisation d’une erreur personnalisée spécifique est utile lorsque [écriture de tests](../test-debug/test.md) pour le travailleur.

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

## Création du rendu

Avec les paramètres lus, assainis et validés, le code est écrit pour générer le rendu. Le pseudo-code pour la génération du rendu est le suivant :

1. Créer `renditionImage` zone de travail aux dimensions carrées spécifiées via `size` .
1. Créez un `image` à partir du binaire de la ressource source
1. Utilisez la variable __Jimp__ pour transformer l’image :
   + Recadrer l’image d’origine sur un carré centré
   + Couper un cercle à partir du centre de l’image &quot;carrée&quot;
   + Echelle à adapter aux dimensions définies par la variable `SIZE` parameter value
   + Ajustez le contraste en fonction des `CONTRAST` parameter value
   + Ajuster la luminosité en fonction des `BRIGHTNESS` parameter value
1. Placez le `image` dans le centre de `renditionImage` avec un arrière-plan transparent
1. Ecrire le composé, `renditionImage` to `rendition.path` afin qu’il puisse être réenregistré dans AEM en tant que rendu de ressource.

Ce code utilise la variable [API Jimp](https://github.com/oliver-moran/jimp#jimp) pour effectuer ces transformations d’image.

Les Assets compute doivent terminer leur travail de manière synchrone, et le `rendition.path` doit être entièrement réécrit à avant l’expiration du délai de `renditionCallback` se termine. Cela nécessite que les appels des fonctions asynchrones soient effectués de manière synchrone à l’aide de la variable `await` de l’opérateur. Si vous ne connaissez pas les fonctions asynchrones JavaScript et que vous ne savez pas comment les exécuter de manière synchrone, familiarisez-vous avec [Opérateur d’attente de JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

Le travailleur terminé `index.js` doit ressembler à :

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

## Exécution du programme de travail

Maintenant que le code de travail est terminé, il a été précédemment enregistré et configuré dans la variable [manifest.yml](./manifest.md), il peut être exécuté à l’aide de l’outil de développement d’Asset compute local pour afficher les résultats.

1. À partir de la racine du projet Asset compute
1. Exécuter `aio app run`
1. Attendez que l’outil de développement des Assets compute s’ouvre dans une nouvelle fenêtre.
1. Dans le __Sélectionnez un fichier...__ , sélectionnez un exemple d’image à traiter
   + Sélectionner un fichier image d’exemple à utiliser comme binaire de ressource source
   + S’il n’en existe pas encore, appuyez sur la __(+)__ à gauche, puis chargez un [exemple d’image](../assets/samples/sample-file.jpg) et actualisez la fenêtre du navigateur des outils de développement.
1. Mettre à jour `"name": "rendition.png"` comme objet Worker pour générer un fichier PNG transparent.
   + Notez que ce paramètre &quot;name&quot; n’est utilisé que pour l’outil de développement et ne doit pas être utilisé.

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

1. Appuyer __Exécuter__ et attendez que le rendu soit généré.
1. Le __Rendus__ prévisualise le rendu généré. Appuyez sur l’aperçu du rendu pour télécharger le rendu complet.

   ![Rendu PNG par défaut](./assets/worker/default-rendition.png)

### Exécution du programme de travail avec des paramètres

Les paramètres, transmis via les configurations de profil de traitement, peuvent être simulés dans les outils de développement d’Asset compute en les fournissant en tant que paires clé/valeur sur le paramètre de rendu JSON.

>[!WARNING]
>
>Lors du développement local, les valeurs peuvent être transmises à l’aide de différents types de données, lorsqu’elles sont transmises d’AEM en tant que profils de traitement de Cloud Service sous forme de chaînes. Assurez-vous donc que les types de données appropriés sont analysés si nécessaire.
> Par exemple, Jimp `crop(width, height)` nécessite que ses paramètres soient `int`&quot;s&quot;. If `parseInt(rendition.instructions.size)` n’est pas analysé à un entier, puis l’appel à `jimp.crop(SIZE, SIZE)` échoue, car les paramètres sont incompatibles avec le type &quot;String&quot;.

Notre code accepte les paramètres pour :

+ `size` définit la taille du rendu (hauteur et largeur en tant qu’entiers) ;
+ `contrast` définit l’ajustement du contraste, qui doit être compris entre -1 et 1, sous la forme de flotteurs.
+ `brightness`  définit l’ajustement de la luminosité, qui doit être compris entre -1 et 1, sous la forme de flotteurs.

Celles-ci sont lues dans le programme de travail `index.js` via :

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

1. Appuyer __Exécuter__ again
1. Appuyez sur l’aperçu du rendu pour télécharger et consulter le rendu généré. Notez ses dimensions et comment le contraste et la luminosité ont été modifiés par rapport au rendu par défaut.

   ![Rendu PNG paramétré](./assets/worker/parameterized-rendition.png)

1. Chargement d’autres images dans la __Fichier source__ et essayez d’exécuter l’objet Worker par rapport à eux avec des paramètres différents.

## Worker index.js sur Github

La finale `index.js` est disponible sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Résolution des problèmes

+ [Rendu renvoyé partiellement tracé/corrompu](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
