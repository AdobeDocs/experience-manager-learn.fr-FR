---
title: Développement d’un objet Worker de métadonnées d’Asset compute
description: Découvrez comment créer un objet Worker de métadonnées d’Asset compute qui fournit les couleurs les plus courantes d’une ressource d’image et réécrit les noms des couleurs aux métadonnées de la ressource dans AEM.
feature: Asset Compute Microservices
topics: metadata, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 1%

---

# Développement d’un objet Worker de métadonnées d’Asset compute

Les objets Worker Asset compute personnalisés peuvent produire des données XMP (XML) qui sont renvoyées à AEM et stockées en tant que métadonnées sur une ressource.

Cas d’utilisation courants :

+ Intégrations à des systèmes tiers, tels qu’un PIM (Product Information Management system), dans lequel des métadonnées supplémentaires doivent être récupérées et stockées sur la ressource.
+ Intégrations à des services Adobe, tels que Content and Commerce AI pour ajouter des métadonnées de ressources à des attributs d’apprentissage automatique supplémentaires
+ Dérivation de métadonnées sur la ressource à partir de son binaire et stockage en tant que métadonnées de ressource dans AEM as a Cloud Service

## Ce que vous allez faire

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

Dans ce tutoriel, nous allons créer un objet Worker de métadonnées d’Asset compute qui fournit les couleurs les plus couramment utilisées dans une ressource d’image et qui réécrit les noms des couleurs aux métadonnées de la ressource dans AEM. Bien que le programme de travail lui-même soit élémentaire, ce tutoriel l’utilise pour explorer la manière dont les programmes de travail Asset compute peuvent être utilisés pour écrire des métadonnées sur des ressources dans AEM as a Cloud Service.

## Flux logique de l’appel d’un objet Worker de métadonnées d’Asset compute

L’appel des objets Worker de métadonnées d’Asset compute est presque identique à celui des objets [génération de rendus binaires](../develop/worker.md), la Principale différence étant le type de retour, il s’agit d’un rendu XMP (XML) dont les valeurs sont également écrites dans les métadonnées de la ressource.

Les agents d’Asset compute implémentent le contrat d’API de traitement du SDK Asset compute, dans la variable `renditionCallback(...)` , qui est conceptuellement :

+ __Entrée :__ Le binaire d’origine d’une ressource AEM et les paramètres de profil de traitement
+ __Sortie :__ Un rendu XMP (XML) a persisté dans la ressource AEM en tant que rendu et dans les métadonnées de la ressource.

![Flux logique du programme de travail des métadonnées d’Asset compute](./assets/metadata/logical-flow.png)

1. Le service AEM Author appelle l’objet Worker de métadonnées d’Asset compute, en fournissant le __(1a)__ binaire d’origine et __(1b)__ tous les paramètres définis dans le profil de traitement.
1. Le SDK Asset compute orchestre l’exécution du programme de travail de métadonnées d’Asset compute personnalisé `renditionCallback(...)` fonction, dérivant un rendu XMP (XML), en fonction du binaire de la ressource __(1a)__ et tout paramètre de profil de traitement __(1b)__.
1. Le programme de travail d’Asset compute enregistre la représentation XMP (XML) dans `rendition.path`.
1. Les données XMP (XML) écrites dans `rendition.path` est transporté via le SDK Asset compute vers le service d’auteur AEM et l’expose comme __(4a)__ un rendu texte et __(4b)__ persistait dans le noeud de métadonnées de la ressource.

## Configuration du fichier manifest.yml{#manifest}

Tous les Assets compute doivent être enregistrés dans la variable [manifest.yml](../develop/manifest.md).

Ouvrez le `manifest.yml` et ajouter une entrée worker qui configure le nouveau worker, dans ce cas, `metadata-colors`.

_Mémoriser `.yml` est sensible aux espaces._

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

`function` pointe vers l’implémentation du programme de travail créée dans la variable [étape suivante](#metadata-worker). Nommez les objets Worker sémantiquement (par exemple, la fonction `actions/worker/index.js` aurait pu être mieux nommé `actions/rendition-circle/index.js`), comme illustré dans la variable [URL du programme de travail](#deploy) et de déterminer [nom de dossier de la suite de tests du programme de travail](#test).

Le `limits` et `require-adobe-auth` sont configurés discrètement par programme de travail. Dans ce travailleur, `512 MB` de mémoire est allouée lorsque le code inspecte (potentiellement) les données d’image binaire volumineuses. L&#39;autre `limits` sont supprimés pour utiliser les valeurs par défaut.

## Développement d’un objet Worker de métadonnées{#metadata-worker}

Créez un fichier JavaScript de traitement des métadonnées dans le projet Asset compute à l’emplacement [manifest.yml défini pour le nouveau worker](#manifest), à `/actions/metadata-colors/index.js`

### Installation des modules npm

Installez les modules npm supplémentaires ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-couleurs](https://www.npmjs.com/package/get-image-colors), et [color-namer](https://www.npmjs.com/package/color-namer)) qui est utilisé dans ce programme de travail des Assets compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Code de travail des métadonnées

Ce programme ressemble beaucoup à la [worker de génération de rendu](../develop/worker.md), la Principale différence réside dans le fait qu’il écrit XMP données (XML) dans la variable `rendition.path` pour être réenregistré dans AEM.


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

## Exécution locale du traitement des métadonnées{#development-tool}

Une fois le code de travail terminé, il peut être exécuté à l’aide de l’outil de développement d’Asset compute local.

Parce que notre projet d’Asset compute contient deux programmes de travail (la précédente [rendu de cercle](../develop/worker.md) et ceci `metadata-colors` worker), [Outil de développement de l&#39;Asset compute](../develop/development-tool.md) la définition de profil répertorie les profils d’exécution pour les deux programmes de travail. La deuxième définition de profil pointe vers la nouvelle `metadata-colors` worker.

![Rendu des métadonnées XML](./assets/metadata/metadata-rendition.png)

1. À partir de la racine du projet Asset compute
1. Exécuter `aio app run` Démarrage de l’outil de développement d’Asset compute
1. Dans le __Sélectionnez un fichier...__ , sélectionnez une [exemple d’image](../assets/samples/sample-file.jpg) processus
1. Dans la seconde configuration de définition de profil, qui pointe vers la fonction `metadata-colors` worker, update `"name": "rendition.xml"` car ce programme de travail génère un rendu XMP (XML). Si vous le souhaitez, vous pouvez ajouter une `colorsFamily` paramètre (valeurs prises en charge `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

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

1. Appuyer __Exécuter__ et attendez que le rendu XML soit généré.
   + Les deux programmes de travail étant répertoriés dans la définition du profil, les deux rendus sont générés. Si vous le souhaitez, la définition de profil supérieure pointant vers la propriété [worker de rendu de cercle](../develop/worker.md) peut être supprimé afin d’éviter de l’exécuter à partir de l’outil de développement.
1. Le __Rendus__ prévisualise le rendu généré. Appuyez sur le bouton `rendition.xml` pour le télécharger, puis ouvrez-le dans VS Code (ou votre éditeur XML/texte favori) pour le consulter.

## Test du programme de travail{#test}

Les objets Worker de métadonnées peuvent être testés à l’aide du [même structure de test d’Asset compute que les rendus binaires](../test-debug/test.md). La seule différence est que `rendition.xxx` dans le cas de test doit correspondre au rendu XMP (XML) attendu.

1. Créez la structure suivante dans le projet Asset compute :

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Utilisez la variable [exemple de fichier](../assets/samples/sample-file.jpg) comme cas de test `file.jpg`.
3. Ajoutez le fichier JSON suivant à la variable `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Notez que `"fmt": "xml"` est requis pour demander à la suite de tests de générer une `.xml` rendu texte.

4. Fournissez le code XML attendu dans la variable `rendition.xml` fichier . Pour ce faire, vous pouvez :
   + Exécution du fichier d’entrée de test via l’outil de développement et enregistrement du rendu XML (validé).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Exécuter `aio app test` à partir de la racine du projet Asset compute pour exécuter toutes les suites de test.

### Déploiement du programme de travail sur Adobe I/O Runtime{#deploy}

Pour appeler ce nouveau programme de traitement des métadonnées à partir d’AEM Assets, il doit être déployé dans Adobe I/O Runtime à l’aide de la commande :

```
$ aio app deploy
```

![déploiement de l’application aio](./assets/metadata/aio-app-deploy.png)

Notez que tous les programmes de travail seront déployés dans le projet. Consultez la section [instructions de déploiement complètes](../deploy/runtime.md) pour savoir comment déployer sur les espaces de travail Stage (évaluation) et Production.

### Intégration aux profils de traitement AEM{#processing-profile}

Appelez le programme de travail à partir d’AEM en créant ou en modifiant un service de profil de traitement personnalisé existant qui appelle ce programme de travail déployé.

![Profil de traitement](./assets/metadata/processing-profile.png)

1. Connectez-vous à AEM service Auteur as a Cloud Service en tant que __Administrateur AEM__
1. Accédez à __Outils > Ressources > Profils de traitement__
1. __Créer__ d’une nouvelle __edit__ et existant, profil de traitement
1. Appuyez sur le bouton __Personnalisé__ et appuyez sur __Ajouter__
1. Définir le nouveau service
   + __Créer un rendu de métadonnées__: Basculer vers principal
   + __Point d’entrée :__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Il s’agit de l’URL du programme de travail obtenu lors de la [deploy](#deploy) ou à l’aide de la commande `aio app get-url`. Assurez-vous que l’URL pointe vers l’espace de travail correct en fonction de l’environnement as a Cloud Service AEM.
   + __Paramètres de service__
      + Appuyer __Ajouter un paramètre__
         + Clé: `colorFamily`
         + Valeur : `pantone`
            + Valeurs prises en charge : `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Types MIME__
      + __Inclut :__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Il s’agit des seuls types MIME pris en charge par les modules npm tiers utilisés pour dériver les couleurs.
      + __Exclut :__ `Leave blank`
1. Appuyer __Enregistrer__ en haut à droite
1. Application du profil de traitement à un dossier AEM Assets si ce n’est pas déjà fait

### Mise à jour du schéma de métadonnées{#metadata-schema}

Pour passer en revue les métadonnées des couleurs, mappez deux nouveaux champs du schéma de métadonnées de l’image avec les nouvelles propriétés de données de métadonnées renseignées par le programme de travail.

![Schéma de métadonnées](./assets/metadata/metadata-schema.png)

1. Dans le service AEM Author, accédez à __Outils > Ressources > Schémas de métadonnées__
1. Accédez à __default__ et sélectionner et modifier __image__ et ajouter des champs de formulaire en lecture seule pour exposer les métadonnées de couleur générées
1. Ajouter un __Une seule ligne de texte__
   + __Libellé du champ__: `Colors Family`
   + __Associer à la propriété__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Règles > Champ > Désactiver la modification__: Cochée
1. Ajouter un __Texte à plusieurs valeurs__
   + __Libellé du champ__: `Colors`
   + __Associer à la propriété__: `./jcr:content/metadata/wknd:colors`
1. Appuyer __Enregistrer__ en haut à droite

## Traitement des ressources

![Détails de la ressource](./assets/metadata/asset-details.png)

1. Dans le service AEM Author, accédez à __Ressources > Fichiers__
1. Accédez au dossier, ou sous-dossier, auquel le profil de traitement est appliqué.
1. Chargez une nouvelle image (JPEG, PNG, GIF ou SVG) dans le dossier ou retraitez les images existantes à l’aide de la mise à jour [Profil de traitement](#processing-profile)
1. Une fois le traitement terminé, sélectionnez la ressource et appuyez sur __properties__ dans la barre d’actions supérieure pour afficher ses métadonnées ;
1. Consultez la section `Colors Family` et `Colors` [Champs de métadonnées](#metadata-schema) pour les métadonnées écrites à partir du programme de travail de métadonnées d’Asset compute personnalisé.

Avec les métadonnées de couleur écrites dans les métadonnées de la ressource, sur la `[dam:Asset]/jcr:content/metadata` , ces métadonnées sont indexées, ce qui augmente la capacité de découverte de ressources à l’aide de ces termes par le biais d’une recherche, et elles peuvent même être réécrites dans le binaire de la ressource, le cas échéant. __Écriture différée des métadonnées de gestion des actifs numériques__ Le workflow est appelé dessus.

### Rendu des métadonnées dans AEM Assets

![Fichier de rendu de métadonnées AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

Le fichier XMP généré par le programme de travail des métadonnées d’Asset compute est également stocké en tant que rendu distinct sur la ressource. Ce fichier n’est généralement pas utilisé. En revanche, les valeurs appliquées au noeud de métadonnées de la ressource sont utilisées, mais la sortie XML brute du programme de travail est disponible dans AEM.

## code de traitement des couleurs de métadonnées sur Github

La finale `metadata-colors/index.js` est disponible sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

La finale `test/asset-compute/metadata-colors` La suite de tests est disponible sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-couleurs](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
