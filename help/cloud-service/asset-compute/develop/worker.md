---
title: Développer un intervenant en calcul des ressources
description: Les travailleurs de Asset Compute sont au coeur des projets Asset Compute, car ils fournissent des fonctionnalités personnalisées qui exécutent, ou orchestrent, le travail effectué sur une ressource pour créer un rendu.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '1508'
ht-degree: 0%

---


# Développer un intervenant en calcul des ressources

Les travailleurs de Asset Compute sont au coeur d’un projet Asset Compute, car ils fournissent des fonctionnalités personnalisées qui exécutent, ou orchestrent, le travail effectué sur une ressource pour créer un nouveau rendu.

Le projet Asset Compute génère automatiquement un programme de travail simple qui copie le fichier binaire d’origine dans un rendu nommé, sans aucune conversion. Dans ce tutoriel, nous allons modifier ce collaborateur pour créer un rendu plus intéressant, pour illustrer la puissance des collaborateurs d&#39;Asset Compute.

Nous allons créer un programme de travail Asset Compute qui génère un nouveau rendu d’image horizontal, qui couvre l’espace vide à gauche et à droite du rendu de fichier avec une version floue de la ressource. La largeur, la hauteur et le flou du rendu final seront paramétrés.

## Flux logique d&#39;un appel de travailleur Asset Compute

Les agents d’Asset Compute implémentent le contrat d’API de travail SDK Asset Compute dans la `renditionCallback(...)` fonction, qui est conceptuellement :

+ __Input :__ Fichier binaire et paramètres d’origine d’une ressource AEM
+ __Output :__ Un ou plusieurs rendus à ajouter à la ressource AEM

![Flux logique de travail de calcul des ressources](./assets/worker/logical-flow.png)

1. Lorsqu’un intervenant Asset Compute est appelé à partir du service Auteur AEM, il est affecté à une ressource AEM via un Profil de traitement. Le binaire d’origine de la ressource __(1a)__ est transmis au programme de travail par le biais du `source` paramètre de la fonction de rappel de rendu et __(1b)__ tout paramètre défini dans le Profil de traitement par le biais du `rendition.instructions` jeu de paramètres.
1. La couche SDK Asset Compute accepte la demande du profil de traitement et orchestre l’exécution de la `renditionCallback(...)` fonction personnalisée du programme de travail Asset Compute, en transformant le fichier binaire source fourni dans __(1a)__ en fonction des paramètres fournis par __(1b)__ pour générer un rendu du fichier binaire source.
   + Dans ce didacticiel, le rendu est créé &quot;en cours&quot;, ce qui signifie que le collaborateur compose le rendu. Toutefois, le binaire source peut être envoyé à d’autres API de service Web pour la génération de rendu.
1. Le programme de travail Asset Compute enregistre la représentation binaire du rendu dans `rendition.path` laquelle il peut être enregistré dans le service Auteur AEM.
1. Une fois l’opération terminée, les données binaires écrites sur `rendition.path` sont transportées via le SDK Asset Compute et exposées via le service d’auteur AEM en tant que rendu disponible dans l’interface utilisateur AEM.

Le diagramme ci-dessus exprime les préoccupations des développeurs d’Asset Compute et le flux logique vers l’appel des employés d’Asset Compute. Pour les curieux, les détails [internes de l’exécution](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html) d’Asset Compute sont disponibles, mais seuls les contrats publics d’API SDK d’Asset Compute doivent être pris en compte.

## Anatomie d&#39;un travailleur

Tous les employés de Asset Compute suivent la même structure de base et le même contrat d&#39;entrée/sortie.

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

## Ouverture du fichier index.js du collaborateur

![index.js généré automatiquement](./assets/worker/autogenerated-index-js.png)

1. Vérifier que le projet Asset Compute est ouvert dans VS Code
1. Navigate to the `/actions/worker` folder
1. Open the `index.js` file

Il s&#39;agit du fichier JavaScript de l&#39;intervenant que nous allons modifier dans ce didacticiel.

## Installation et importation des modules npm de prise en charge

Basés sur Node.js, les projets Asset Compute bénéficient de l’écosystème [robuste des modules](https://npmjs.com)npm. Pour exploiter les modules npm, nous devons d&#39;abord les installer dans notre projet Asset Compute.

Dans ce programme de travail, nous utilisons le [jimp](https://www.npmjs.com/package/jimp) pour créer et manipuler l’image de rendu directement dans le code Node.js.

>[!WARNING]
>
>Tous les modules npm pour la manipulation de ressources ne sont pas pris en charge par Asset Compute. modules npm reposant sur les applications existantes telles que ImageMagick ou les bibliothèques dépendantes du système d’exploitation. Il est préférable de limiter l’utilisation des modules npm JavaScript uniquement.

1. Ouvrez la ligne de commande à la racine du projet Asset Compute (vous pouvez le faire dans le code VS via __Terminal > New Terminal__) et exécutez la commande :

   ```
   $ npm install jimp
   ```

1. Importez le `jimp` module dans le code de travail afin de l’utiliser via l’objet `Jimp` JavaScript.
Mettez à jour les `require` directives en haut de l&#39;outil `index.js` de travail pour importer l&#39; `Jimp` objet à partir du `jimp` module :

   ```javascript
   'use strict';
   
   const { Jimp } = require('jimp');
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

## Paramètres de lecture

Les collaborateurs de Asset Compute peuvent lire dans des paramètres qui peuvent être transmis via des Profils de traitement définis dans AEM en tant que service d’auteur Cloud Service. Les paramètres sont transmis au programme de travail via l’ `rendition.instructions` objet.

Vous pouvez les lire en accédant `rendition.instructions.<parameterName>` au code de travail.

Ici, nous allons lire dans les rendus configurables `SIZE``BRIGHTNESS` et `CONTRAST`en fournissant des valeurs par défaut si aucune n’a été fournie par le Profil de traitement. Notez que `renditions.instructions` les chaînes sont transmises en tant que chaînes lorsqu’elles sont appelées à partir d’AEM en tant que Profils de traitement Cloud Service. Assurez-vous donc qu’elles sont transformées en types de données corrects dans le code de travail.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

## Erreurs de déclenchement{#errors}

Les employés de Asset Compute peuvent rencontrer des situations qui génèrent des erreurs. Le SDK Adobe Asset Compute fournit [une suite d’erreurs](https://github.com/adobe/asset-compute-commons#asset-compute-errors) prédéfinies qui peuvent être générées lorsque de telles situations se produisent. Si aucun type d’erreur spécifique ne s’applique, le `GenericError` paramètre peut être utilisé ou des paramètres personnalisés spécifiques `ClientErrors` peuvent être définis.

Avant de commencer à traiter le rendu, vérifiez que tous les paramètres sont valides et pris en charge dans le contexte de ce programme de travail :

+ Assurez-vous que les paramètres d’instruction de rendu pour `SIZE`, `CONTRAST`et `BRIGHTNESS` sont valides. Dans le cas contraire, renvoyez une erreur personnalisée `RenditionInstructionsError`.
   + Une `RenditionInstructionsError` classe personnalisée qui s&#39;étend `ClientError` est définie au bas de ce fichier. L’utilisation d’une erreur personnalisée spécifique est utile lors de l’ [écriture de tests](../test-debug/test.md) pour le collaborateur.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

Lorsque les paramètres sont lus, expurgés et validés, le code est écrit pour générer le rendu. Le pseudo-code pour la génération de rendu est le suivant :

1. Créez un nouveau `renditionImage` canevas aux dimensions carrées spécifiées via le `size` paramètre.
1. Création d’un `image` objet à partir du fichier binaire de l’actif source
1. Utilisez la bibliothèque __Jimp__ pour transformer l’image :
   + Recadrer l’image d’origine sur un carré centré
   + Coupe un cercle à partir du centre de l’image &quot;carrée&quot;
   + Ajuster à l&#39;échelle selon les dimensions définies par la valeur du `SIZE` paramètre
   + Ajuster le contraste en fonction de la valeur du `CONTRAST` paramètre
   + Ajuster la luminosité en fonction de la valeur du `BRIGHTNESS` paramètre
1. Placer le transformé `image` au centre du `renditionImage` qui possède un arrière-plan transparent
1. Ecrivez le composé, `renditionImage` afin `rendition.path` qu’il puisse être enregistré à nouveau en AEM en tant que rendu de ressource.

Ce code utilise les API [](https://github.com/oliver-moran/jimp#jimp) Jimp pour effectuer ces transformations d&#39;image.

Les employés de Asset Compute doivent terminer leur travail de manière synchrone et le `rendition.path` travail doit être entièrement réécrit avant que le travailleur ne `renditionCallback` termine son travail. Pour ce faire, les appels de fonctions asynchrones doivent être effectués de manière synchrone à l’aide de l’ `await` opérateur. Si vous ne connaissez pas les fonctions asynchrones de JavaScript et comment les exécuter de manière synchrone, familiarisez-vous avec l’opérateur [d’attente de](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)JavaScript.

Le travailleur fini `index.js` doit se présenter comme suit :

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

    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 10,000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image of the target size with a transparent background (0x0)
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness using Jimp
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
    renditionImage.write(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Exécution du collaborateur

Maintenant que le code de travail est terminé et qu&#39;il a été précédemment enregistré et configuré dans [manifest.yml](./manifest.md), il peut être exécuté à l&#39;aide de l&#39;outil de développement de l&#39;informatique d&#39;actifs local pour afficher les résultats.

1. A partir de la racine du projet Asset Compute
1. Exécuter `app aio run`
1. Attente de l’ouverture de l’outil de développement Asset Compute dans une nouvelle fenêtre
1. Dans la __liste déroulante Sélectionner un fichier...__ , sélectionnez un exemple d’image à traiter.
   + Sélectionnez un exemple de fichier image à utiliser comme fichier binaire source
   + S’il n’en existe pas encore, appuyez sur le bouton __(+)__ à gauche, téléchargez un [exemple de fichier d’image](../assets/samples/sample-file.jpg) , puis actualisez la fenêtre du navigateur des outils de développement.
1. Mettez à jour `"name": "rendition.png"` en tant que collaborateur pour générer un fichier PNG transparent.
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
1. Appuyez sur __Exécuter__ et attendez que le rendu génère
1. La section __Rendus__ prévisualisation le rendu généré. Appuyez sur la prévisualisation de rendu pour télécharger le rendu complet.

   ![Rendu PNG par défaut](./assets/worker/default-rendition.png)

### Exécution du programme de travail avec des paramètres

Les paramètres transmis via les configurations de Profil de traitement peuvent être simulés dans les outils de développement de calcul d’actifs en les fournissant sous forme de paires clé/valeur sur le paramètre de rendu JSON.

>[!WARNING]
>
>Lors du développement local, les valeurs peuvent être transmises à l’aide de différents types de données, lorsqu’elles sont transmises d’AEM en tant que Profils de traitement Cloud Service en tant que chaînes. Assurez-vous donc que les types de données appropriés sont analysés si nécessaire.
> Par exemple, la `crop(width, height)` fonction de Jimp requiert que ses paramètres soient `int`ceux de Jimp. Si `parseInt(rendition.instructions.size)` n&#39;est pas analysé à un entier, l&#39;appel à `jimp.crop(SIZE, SIZE)` échoue car les paramètres sont incompatibles avec le type &quot;String&quot;.

Notre code accepte les paramètres pour :

+ `size` définit la taille du rendu (hauteur et largeur en tant qu’entiers)
+ `contrast` définit l’ajustement du contraste, doit être compris entre -1 et 1, sous forme de flotteurs
+ `brightness`  définit le réglage de la luminosité, doit être compris entre -1 et 1, sous forme de flotteurs

Ils sont lus dans le travailleur `index.js` via :

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

1. Appuyez à nouveau sur __Exécuter__ .
1. Appuyez sur la prévisualisation de rendu pour télécharger et consulter le rendu généré. Notez ses dimensions et comment le contraste et la luminosité ont été modifiés par rapport au rendu par défaut.

   ![Rendu PNG paramétré](./assets/worker/parameterized-rendition.png)

1. Téléchargez d’autres images dans la liste déroulante des fichiers ____ source, puis essayez d’exécuter le programme de travail avec d’autres paramètres !

## Worker index.js sur Github

Le dernier `index.js` est disponible sur Github à l&#39;adresse :

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Résolution des incidents

### Le rendu est renvoyé partiellement tracé

+ __Erreur__: Le rendu est rendu incomplet lorsque la taille totale du fichier de rendu est importante.

   ![Dépannage - Le rendu est renvoyé partiellement](./assets/worker/troubleshooting__await.png)

+ __Cause__: La `renditionCallback` fonction du collaborateur s’arrête avant que le rendu ne puisse être entièrement écrit `rendition.path`.
+ __Résolution__: Vérifiez le code de travail personnalisé et assurez-vous que tous les appels asynchrones sont effectués de manière synchrone.
