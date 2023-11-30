---
title: Développer un programme de travail de métadonnées Asset Compute
description: Découvrez comment créer un programme de travail de métadonnées Asset Compute qui fournit les couleurs les plus courantes d’une ressource d’image et réécrit les noms des couleurs pour les renvoyer aux métadonnées de la ressource dans AEM.
feature: Asset Compute Microservices
topics: metadata, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 100%

---

# Développer un programme de travail de métadonnées Asset Compute

Les programmes de travail personnalisés Asset Compute peuvent produire des données XMP (XML) qui sont renvoyées à AEM et stockées en tant que métadonnées sur une ressource.

Cas d’utilisation courants :

+ Intégrations à des systèmes tiers, tels qu’un PIM (système Product Information Management), dans lequel des métadonnées supplémentaires doivent être récupérées et stockées sur la ressource.
+ Intégrations à des services Adobe, tels que l’IA de Content and Commerce pour ajouter des métadonnées de ressources à des attributs de machine learning supplémentaires.
+ Dérivation de métadonnées sur la ressource à partir de son fichier binaire et stockage en tant que métadonnées de ressource dans AEM as a Cloud Service.

## Ce que vous devez faire

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

Dans ce tutoriel, nous allons créer un programme de travail de métadonnées Asset Compute qui fournit les couleurs les plus couramment utilisées dans une ressource d’image et qui réécrit les noms des couleurs pour les renvoyer aux métadonnées de la ressource dans AEM. Bien que le programme de travail lui-même soit basique, ce tutoriel l’utilise pour explorer la manière dont les programmes de travail Asset Compute peuvent être utilisés pour écrire des métadonnées sur des ressources dans AEM as a Cloud Service.

## Flux logique de l’appel d’un programme de travail de métadonnées Asset Compute

L’appel des programmes de travail de métadonnées d’Asset Compute est presque identique à celui des [programmes de travail de génération de rendus binaires](../develop/worker.md), la principale différence étant le type de retour, à savoir XMP (XML), dont les valeurs sont également écrites dans les métadonnées de la ressource.

Les programmes de travail Asset Compute implémentent l’API du programme de travail SDK Asset Compute, dans la fonction `renditionCallback(...)`, qui est conceptuellement :

+ __Entrée :__ paramètres binaires et de profil de traitement binaire d’origine d’une ressource AEM.
+ __Sortie :__ un rendu XMP (XML) conservé dans la ressource AEM en tant que rendu et dans les métadonnées de la ressource.

![Flux logique de traitement des métadonnées Asset Compute.](./assets/metadata/logical-flow.png)

1. Le service de création AEM appelle le programme de travail de métadonnées d’Asset Compute en fournissant le format binaire d’origine de la ressource __(1a)__ et __(1b)__ tous les paramètres définis dans le profil de traitement.
1. Le SDK Asset Compute orchestre l’exécution de la fonction `renditionCallback(...)` du programme de travail personnalisé de métadonnées d’Asset Compute, dérivant un rendu XMP (XML), en fonction du format binaire de la ressource __(1a)__ et de tout paramètre de profil de traitement __(1b)__.
1. Le programme de travail Asset Compute enregistre la représentation XMP (XML) dans `rendition.path`.
1. Les données XMP (XML) écrites dans `rendition.path` sont transportées via le SDK Asset Compute vers le service de création AEM, sont exposées comme __(4a)__ un rendu texte et __(4b)__ conservées dans le nœud de métadonnées de la ressource.

## Configurer le fichier manifest.yml{#manifest}

Tous les programmes de travail Assets Compute doivent être enregistrés dans [manifest.yml](../develop/manifest.md).

Ouvrez le fichier `manifest.yml` et ajoutez une entrée de programme de travail qui configure le nouveau programme de travail, dans ce cas, `metadata-colors`.

_Souvenez-vous que `.yml` est sensible aux espaces._

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` pointe vers l’implémentation du programme de travail créée dans [l’étape suivante](#metadata-worker). Nommez les objets du programme de travail sémantiquement (par exemple, `actions/worker/index.js` aurait pu être nommé `actions/rendition-circle/index.js`, ce qui aurait été préférable), comme illustré dans [l’URL du programme de travail](#deploy) et déterminez également le [nom du dossier de la suite de tests du programme de travail](#test).

Les éléments `limits` et `require-adobe-auth` sont configurés discrètement par programme de travail. Dans ce programme de travail, une mémoire de `512 MB` est attribuée lorsque le code inspecte (potentiellement) des données d’image binaire volumineuses. Les `limits` restantes sont supprimées pour utiliser les valeurs par défaut.

## Développer un programme de travail de métadonnées{#metadata-worker}

Créez un fichier JavaScript de traitement des métadonnées dans le projet Asset Compute à l’emplacement [manifest.yml défini pour le nouveau programme de travail](#manifest), à l’emplacement `/actions/metadata-colors/index.js`.

### Installer les modules npm

Installez les modules npm supplémentaires ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors), et [color-namer](https://www.npmjs.com/package/color-namer)) qui sont utilisés dans ce programme de travail Assets Compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Code du programme de travail des métadonnées

Ce programme de travail ressemble beaucoup au [programme de travail de génération des rendus](../develop/worker.md), la principale différence réside dans le fait qu’il écrit des données XMP (XML) dans `rendition.path` pour qu’elles soient enregistrées et renvoyées à AEM.


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces are automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## Exécuter localement le programme de travail des métadonnées{#development-tool}

Une fois le code du programme de travail terminé, il peut être exécuté à l’aide de l’outil de développement d’Asset Compute local.

Parce que notre projet d’Asset Compute contient deux programmes de travail (le [rendu de cercle](../develop/worker.md) précédent et ce programme de travail de `metadata-colors`), la définition du profil de l’[outil de développement Asset Compute](../develop/development-tool.md) répertorie les profils d’exécution pour les deux programmes de travail. La deuxième définition de profil pointe vers le nouveau programme de travail de `metadata-colors`.

![Rendu des métadonnées XML.](./assets/metadata/metadata-rendition.png)

1. À partir de la racine du projet Asset Compute
1. Exécuter `aio app run` pour démarrer l’outil de développement Asset Compute
1. Dans le menu déroulant __Sélectionner un fichier...__, sélectionnez un [exemple d’image](../assets/samples/sample-file.jpg) à traiter.
1. Dans la seconde configuration de définition de profil, qui pointe vers le programme de travail de `metadata-colors`, mettez à jour `"name": "rendition.xml"`, car ce programme de travail génère un rendu XMP (XML). Si vous le souhaitez, vous pouvez ajouter le paramètre `colorsFamily` (valeurs prises en charge : `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```

1. Appuyez sur __Exécuter__ et attendez que le rendu XML soit généré.
   + Les deux programmes de travail étant répertoriés dans la définition du profil, les deux rendus sont générés. Si vous le souhaitez, la définition de profil supérieure pointant vers le [programme de travail de rendu de cercle](../develop/worker.md) peut être supprimée, afin d’éviter de l’exécuter à partir de l’outil de développement.
1. La section __Rendus__ prévisualise le rendu généré. Appuyez sur le fichier `rendition.xml` pour le télécharger, puis ouvrez-le dans VS Code (ou votre éditeur XML/texte favori) pour le consulter.

## Tester le programme de travail{#test}

Les programmes de travail de métadonnées peuvent être testés à l’aide du [même framework de test d’Asset Compute que les rendus binaires](../test-debug/test.md). La seule différence est que le fichier `rendition.xxx` dans le cas de test doit correspondre au rendu XMP (XML) attendu.

1. Créez la structure suivante dans le projet Asset Compute :

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Utilisez l’[exemple de fichier](../assets/samples/sample-file.jpg) comme `file.jpg` du cas de test.
3. Ajoutez le JSON suivant au `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Notez que le `"fmt": "xml"` est requis pour demander à la suite de tests de générer un rendu texte `.xml`.

4. Fournissez le XML attendu dans le fichier `rendition.xml`. Vous pouvez l’obtenir de la manière suivante :
   + Exécutez le fichier d’entrée de test via l’outil de développement et enregistrez le rendu XML (validé).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Exécutez `aio app test` à partir de la racine du projet Asset Compute pour exécuter toutes les suites de test.

### Déployer le programme de travail sur Adobe I/O Runtime{#deploy}

Pour appeler ce nouveau programme de travail des métadonnées à partir d’AEM Assets, il doit être déployé dans Adobe I/O Runtime à l’aide de la commande :

```
$ aio app deploy
```

![Déploiement de l’application AIO.](./assets/metadata/aio-app-deploy.png)

Notez que tous les programmes de travail seront déployés dans le projet. Consultez les [instructions de déploiement complètes](../deploy/runtime.md) pour savoir comment déployer sur les espaces de travail d’évaluation et de production.

### Intégrer aux profils de traitement AEM{#processing-profile}

Appelez le programme de travail à partir d’AEM en créant ou en modifiant un service de profil de traitement personnalisé existant qui appelle ce programme de travail déployé.

![Profil de traitement.](./assets/metadata/processing-profile.png)

1. Connectez-vous au service de création d’AEM as a Cloud Service en tant qu’__Administrateur ou administratrice AEM__.
1. Accédez à __Outils > Ressources > Profils de traitement__.
1. __Créez__ ou __modifiez__ un profil de traitement.
1. Appuyez sur l’onglet __Personnalisé__ et appuyez sur __Ajouter nouveau__.
1. Définir le nouveau service
   + __Créer un rendu de métadonnées__ : appuyez sur le bouton (bascule) pour activer la fonctionnalité.
   + __Point d’entrée :__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`.
      + Il s’agit de l’URL du programme de travail obtenue lors du [deploiement](#deploy) ou à l’aide de la commande `aio app get-url`. Assurez-vous que l’URL pointe vers le bon espace de travail en fonction de l’environnement AEM as a Cloud Service.
   + __Paramètres de service__
      + Appuyez sur __Ajouter un paramètre__.
         + Clé : `colorFamily`
         + Valeur : `pantone`
            + Valeurs prises en charge : `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Types MIME__
      + __Inclut :__, `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Il s’agit des seuls types MIME pris en charge par les modules npm tiers utilisés pour extraire les couleurs.
      + __Exclut :__ `Leave blank`
1. Appuyez sur __Enregistrer__ en haut à droite.
1. Appliquez le profil de traitement à un dossier AEM Assets si ce n’est pas déjà fait.

### Mettre à jour le schéma de métadonnées{#metadata-schema}

Pour passer en revue les métadonnées des couleurs, mappez deux nouveaux champs du schéma de métadonnées de l’image avec les nouvelles propriétés de données de métadonnées renseignées par le programme de travail.

![Schéma de métadonnées.](./assets/metadata/metadata-schema.png)

1. Dans le service de création d’AEM, accédez à __Outils > Ressources > Schémas de métadonnées__.
1. Accédez aux __paramètres par défaut__, sélectionnez et modifiez l’__image__, puis ajoutez des champs de formulaire en lecture seule pour exposer les métadonnées de couleur générées.
1. Ajoutez un __texte monoligne__.
   + __Libellé du champ__ : `Colors Family`
   + __Associer à la propriété__ : `./jcr:content/metadata/wknd:colorsFamily`
   + __Règles > Champ > Désactiver la modification__ : coché.
1. Ajoutez un __texte à plusieurs valeurs__.
   + __Libellé du champ__ : `Colors`
   + __Associer à la propriété__ : `./jcr:content/metadata/wknd:colors`
1. Appuyez sur __Enregistrer__ en haut à droite.

## Traiter des ressources

![Détails de la ressource.](./assets/metadata/asset-details.png)

1. Dans le service de création d’AEM, accédez à __Ressources > Fichiers__.
1. Accédez au dossier, ou sous-dossier, auquel le profil de traitement est appliqué.
1. Chargez une nouvelle image (JPEG, PNG, GIF ou SVG) dans le dossier ou retraitez les images existantes à l’aide du [profil de traitement](#processing-profile) mis à jour.
1. Une fois le traitement terminé, sélectionnez la ressource et appuyez sur __Propriétés__ dans la barre d’actions supérieure pour afficher ses métadonnées.
1. Consultez les [champs de métadonnées](#metadata-schema) `Colors Family` et `Colors` pour les métadonnées écrites à partir du programme de travail de métadonnées d’Asset Compute personnalisé.

Avec les métadonnées de couleur écrites dans les métadonnées de la ressource, sur la ressource `[dam:Asset]/jcr:content/metadata`, ces métadonnées sont indexées, ce qui augmente la capacité de découverte de ressources à l’aide de ces termes par le biais d’une recherche. Elles peuvent même être réécrites dans le fichier binaire de la ressource, si par la suite le workflow __Écriture différée des métadonnées de gestion des ressources numériques__ est appelé dessus.

### Rendu des métadonnées dans AEM Assets

![Fichier de rendu de métadonnées dans AEM Assets.](./assets/metadata/cqdam-metadata-rendition.png)

Le fichier XMP généré par le programme de travail des métadonnées d’Asset Compute est également stocké en tant que rendu distinct sur la ressource. Ce fichier n’est généralement pas utilisé. En revanche, les valeurs appliquées au nœud de métadonnées de la ressource sont utilisées, mais la sortie XML brute du programme de travail est disponible dans AEM.

## Code du programme de travail des couleurs de métadonnées sur Github

Le `metadata-colors/index.js` final est disponible sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js).

La suite de tests du `test/asset-compute/metadata-colors` final est disponible sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
