---
title: Gestion des hôtes AEM pour AEM GraphQL
description: Découvrez comment configurer AEM hôtes dans AEM application sans affichage.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Gestion des hôtes AEM

Le déploiement d’une application AEM sans affichage nécessite une attention particulière à la manière dont AEM URL sont construites pour s’assurer que l’hôte/le domaine d’AEM correct est utilisé. Les Principaux types d’URL/de requêtes à connaître sont les suivants :

+ Requêtes HTTP vers __[AEM API GraphQL](#aem-graphql-api-requests)__
+ __[URL des images](#aem-image-urls)__ pour image des ressources référencées dans les fragments de contenu et diffusées par AEM

En règle générale, une application AEM sans affichage interagit avec un service AEM unique pour l’API GraphQL et les demandes d’image. Le service AEM change en fonction du déploiement de l’application AEM sans affichage :

| AEM type de déploiement sans affichage | Environnement AEM | Service AEM |
|-------------------------------|:---------------------:|:----------------:|
| Production | Production | Publication |
| Aperçu de la création | Production | Aperçu |
| Développement | Développement | Publication |

Pour gérer les permutations de type déploiement, chaque déploiement d’application est créé à l’aide d’une configuration spécifiant le service AEM auquel se connecter. L’hôte/le domaine du service d’AEM configuré est ensuite utilisé pour construire les URL d’API GraphQL AEM et les URL d’image. Pour déterminer l’approche appropriée pour la gestion des configurations dépendantes de la création, reportez-vous à la documentation de la structure de l’application AEM sans affichage (par exemple, React, iOS, Android™, etc.), car l’approche varie selon la structure.

| Type de client | [Application d’une seule page (SPA)](../spa.md) | [Composant Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Serveur à serveur](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuration des hôtes AEM | ✔ | ✔ | ✔ | ✔ |

Vous trouverez ci-dessous des exemples d’approches possibles pour la création d’URL pour [API GraphQL AEM](#aem-graphql-api-requests) et [demandes d’image](#aem-image-requests), pour plusieurs plates-formes et structures sans interface populaires.

## Demandes d’API GraphQL AEM

Les requêtes de GET HTTP de l’application sans interface utilisateur pour AEM les API GraphQL doivent être configurées pour interagir avec le service AEM correct, comme décrit dans la section [tableau ci-dessus](#managing-aem-hosts).

Lors de l’utilisation de [AEM SDK sans affichage](../../how-to/aem-headless-sdk.md) (disponible pour JavaScript basé sur un navigateur, JavaScript basé sur un serveur et Java™), un hôte d’AEM peut initialiser l’objet client AEM sans affichage avec le service à connecter avec lequel se connecter.

Lors du développement d’un client AEM personnalisé sans affichage, assurez-vous que l’hôte du service AEM est paramétrable en fonction des paramètres de génération.

### Exemples

Vous trouverez ci-dessous des exemples d’AEM de la manière dont la valeur d’hôte AEM peut être configurée pour les différentes structures d’application sans interface utilisateur.

+++ Exemple React

Cet exemple, basé vaguement sur la variable [AEM application React sans tête](../../example-apps/react-app.md), illustre la manière dont AEM demandes d’API GraphQL peuvent être configurées pour se connecter à différents services AEM en fonction des variables d’environnement.

Les applications React doivent utiliser la variable [AEM client sans affichage pour JavaScript](../../how-to/aem-headless-sdk.md) pour interagir avec AEM API GraphQL. Le client AEM sans affichage, fourni par AEM client sans affichage pour JavaScript, doit être initialisé avec l’hôte de service auquel il se connecte.

#### Fichier d’environnement React

Utilisation de React [fichiers d’environnement personnalisés](https://create-react-app.dev/docs/adding-custom-environment-variables/)ou `.env` fichiers, stockés à la racine du projet pour définir des valeurs spécifiques à la version. Par exemple, la variable `.env.development` contient les valeurs utilisées pour lors du développement, tandis que `.env.production` contient les valeurs utilisées pour les versions de production.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` des fichiers destinés à d’autres fins [peut être spécifié](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) par postfixage `.env` et un descripteur sémantique, tel que `.env.stage` ou `.env.production`. Différent `.env` Les fichiers peuvent être utilisés lors de l’exécution ou de la création de l’application React, en définissant la variable `REACT_APP_ENV` avant d’exécuter une `npm` .

Par exemple, une application React `package.json` peut contenir les éléments suivants : `scripts` config :

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### AEM client sans interface

Le [AEM client sans affichage pour JavaScript](../../how-to/aem-headless-sdk.md) contient un client AEM sans affichage qui effectue des requêtes HTTP vers AEM API GraphQL. Le client AEM sans affichage doit être initialisé avec l’hôte AEM avec lequel il interagit, à l’aide de la valeur de la principale `.env` fichier .

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### React useEffect(..) hook

Les hooks useEffect personnalisés React appellent le client AEM sans affichage, initialisé avec l’hôte AEM, au nom du composant React qui effectue le rendu de la vue.

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### Composant React

Le point d’extension personnalisé useEffect, `useAdventureByPath` est importé et utilisé pour obtenir les données à l’aide du client AEM sans affichage et, en fin de compte, pour effectuer le rendu du contenu à l’utilisateur final.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ Exemple iOS™

Cet exemple, basé sur la variable [exemple AEM application iOS™ sans affichage](../../example-apps/ios-swiftui-app.md), illustre la manière dont AEM demandes d’API GraphQL peuvent être configurées pour se connecter à différents hôtes AEM en fonction de [variables de configuration spécifiques à la version](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Les applications iOS™ nécessitent un client AEM sans affichage personnalisé pour interagir avec les API GraphQL AEM. Le client AEM sans affichage doit être écrit de sorte que l’hôte de service AEM soit configurable.

#### Configuration de création

Le fichier de configuration Xcode contient les détails de configuration par défaut.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Initialisation du client AEM personnalisé sans interface utilisateur

Le [exemple AEM application iOS sans affichage](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) utilise un client AEM sans tête personnalisé initialisé avec les valeurs de configuration pour `AEM_SCHEME` et `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

Client AEM personnalisé (`api/Aem.swift`) contient une méthode . `makeRequest(..)` qui préfixe les demandes d’API GraphQL AEM avec l’AEM configuré. `scheme` et `host`.

+ `api/Aem.swift`

```swift
/// #makeRequest(..)
/// Generic method for constructing and executing AEM GraphQL persisted queries
private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
    // Encode optional parameters as required by AEM
    let persistedQueryParams = params.map { (param) -> String in
        encode(string: ";\(param.key)=\(param.value)")
    }.joined(separator: "")
    
    // Construct the AEM GraphQL persisted query URL, including optional query params
    let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

    var request = URLRequest(url: URL(string: url)!);
    
    return request;
}
```

[De nouveaux fichiers de configuration de build peuvent être créés.](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) pour vous connecter à différents services AEM. Valeurs spécifiques à la version pour la variable `AEM_SCHEME` et `AEM_HOST` sont utilisés en fonction de la version sélectionnée dans Xcode, ce qui entraîne le client AEM personnalisé sans affichage à se connecter au service AEM approprié.

+++

+++ Exemple Android™

Cet exemple, basé sur la variable [Exemple AEM application Android™ sans affichage](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustre la manière dont AEM demandes d’API GraphQL peuvent être configurées pour se connecter à différents services AEM en fonction de variables de configuration spécifiques aux versions (ou versions).

Les applications Android™ (lorsqu’elles sont écrites en Java™) doivent utiliser la variable [AEM client sans affichage pour Java™](https://github.com/adobe/aem-headless-client-java) pour interagir avec AEM API GraphQL. Le client AEM sans affichage, fourni par AEM client sans affichage pour Java™, doit être initialisé avec l’hôte de service auquel il se connecte.

#### Créer un fichier de configuration

Les applications Android™ définissent des &quot;productFlavors&quot; utilisés pour créer des artefacts à des fins différentes.
Cet exemple montre comment deux versions de produit Android™ peuvent être définies, fournissant différents hôtes de service AEM (`AEM_HOST`) pour les valeurs de développement (`dev`) et de production (`prod`).

Dans l’application `build.gradle` fichier, un nouveau `flavorDimension` named `env` est créée.

Dans le `env` dimension, deux `productFlavors` sont définies : `dev` et `prod`. Chaque `productFlavor` uses `buildConfigField` pour définir des variables spécifiques à la version définissant le service AEM auquel se connecter.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Chargeur Android™

Initialisez la `AEMHeadlessClient` du créateur, fourni par AEM client headless pour Java™ avec la fonction `AEM_HOST` de la variable `buildConfigField` champ .

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

Lors de la création de l’application Android™ pour différents usages, spécifiez la variable `env` et la valeur d’hôte AEM correspondante est utilisée.

+++

## AEM URL des images

Les demandes d’image de l’application sans tête à AEM doivent être configurées pour interagir avec le service AEM approprié, comme décrit dans la section [tableau ci-dessus](#managing-aem-hosts).

Lors de l’AEM de GraphQL `... on ImageRef` fournit des champs `_authorUrl` et `_publishUrl` contenant des URL absolues pour les services AEM respectifs, il est souvent plus simple d’utiliser la variable `_path` champ et préfixe l’hôte de service AEM utilisé pour interroger AEM API GraphQL.

Utilisation `_path` Cela peut s’avérer particulièrement bénéfique si l’application sans interface utilisateur peut se connecter à l’auteur AEM ou à la publication AEM en fonction du contexte de déploiement.

Si l’application sans interface interagit exclusivement avec l’auteur ou la publication AEM, `_authorUrl` ou `_publishUrl` Les champs peuvent être utilisés pour simplifier la mise en oeuvre et les conseils des exemples ci-dessous peuvent être ignorés.

### Exemples

Vous trouverez ci-dessous des exemples de la manière dont les URL d’image peuvent préfixer la valeur d’hôte AEM configurée pour différentes structures d’application sans interface utilisateur. Les exemples supposent l’utilisation de requêtes GraphQL qui renvoient des références d’image à l’aide de la fonction `_path` champ .

Par exemple :

#### Requête persistante GraphQL

Cette requête GraphQL renvoie une référence d’image `_path`. Comme le montre le rapport [Réponse GraphQL](#examples-react-graphql-response) qui exclut un hôte.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
        }
      }
    }
  }
}
```

#### Réponse GraphQL

Cette réponse GraphQL renvoie le `_path` qui exclut un hôte.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
        }
      }
    }
  }
}
```

+++ Exemple React

Cet exemple, basé sur la variable [exemple AEM application React sans affichage](../../example-apps/react-app.md), illustre la manière dont les URL d’image peuvent être configurées pour se connecter aux services AEM corrects en fonction des variables d’environnement.

Cet exemple montre comment préfixer la référence de l’image `_path` avec un champ configurable `REACT_APP_AEM_HOST` Variable d’environnement React.

#### Fichier d’environnement React

Utilisation de React [fichiers d’environnement personnalisés](https://create-react-app.dev/docs/adding-custom-environment-variables/)ou `.env` fichiers, stockés à la racine du projet pour définir des valeurs spécifiques à la version. Par exemple, la variable `.env.development` contient les valeurs utilisées pour lors du développement, tandis que `.env.production` contient les valeurs utilisées pour les versions de production.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` des fichiers destinés à d’autres fins [peut être spécifié](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) par postfixage `.env` et un descripteur sémantique, tel que `.env.stage` ou `.env.production`. Différent `.env` peut être utilisé lors de l’exécution ou de la création de l’application React, en définissant la variable `REACT_APP_ENV` avant d’exécuter une `npm` .

Par exemple, une application React `package.json` peut contenir les éléments suivants : `scripts` config :

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### Composant React

Le composant React importe le `REACT_APP_AEM_HOST` Variable d’environnement et préfixe l’image `_path` pour fournir une URL d’image entièrement résolvable.

Même `REACT_APP_AEM_HOST` La variable d’environnement est utilisée pour initialiser le client AEM sans affichage utilisé par `useAdventureByPath(..)` crochet useEffect personnalisé utilisé pour récupérer les données GraphQL d’AEM. En utilisant la même variable pour construire la demande d’API GraphQL comme URL d’image, assurez-vous que l’application React interagit avec le même service d’AEM pour les deux cas d’utilisation.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._path }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ Exemple iOS™

Cet exemple, basé sur la variable [exemple AEM application iOS™ sans affichage](../../example-apps/ios-swiftui-app.md), illustre comment AEM URL d’image peuvent être configurées pour se connecter à différents hôtes AEM en fonction de [variables de configuration spécifiques à la version](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Configuration de création

Le fichier de configuration Xcode contient les détails de configuration par défaut.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Générateur d’URL d’image

Dans `Aem.swift`, l’implémentation client AEM personnalisée sans interface utilisateur, une fonction personnalisée `imageUrl(..)` emprunte le chemin d’accès à l’image, comme indiqué dans la variable `_path` dans la réponse GraphLQ et la préfixe avec l’hôte AEM. Cette fonction est ensuite appelée dans les vues iOS chaque fois qu’une image est rendue.

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image paths wit the AEM scheme/host
    func imageUrl(path: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(path)")!
    }
    ...
}
```

#### Vue iOS

La vue iOS et le préfixe de l’image `_path` pour fournir une URL d’image entièrement résolvable.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image path to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(path: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[De nouveaux fichiers de configuration de build peuvent être créés.](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) pour vous connecter à différents services AEM. Valeurs spécifiques à la version pour la variable `AEM_SCHEME` et `AEM_HOST` sont utilisés en fonction de la version sélectionnée dans XCode, ce qui entraîne l’interaction du client AEM sans affichage personnalisé avec le service AEM correct.

+++

+++ Exemple Android™

Cet exemple, basé sur la variable [Exemple AEM application Android™ sans affichage](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustre comment AEM URL d’image peuvent être configurées pour se connecter à différents services AEM en fonction de variables de configuration spécifiques à la version (ou aux versions).

#### Créer un fichier de configuration

Les applications Android™ définissent &quot;productFlavors&quot;, qui sont utilisées pour créer des artefacts à des fins différentes.
Cet exemple montre comment deux versions de produit Android™ peuvent être définies, fournissant différents hôtes de service AEM (`AEM_HOST`) pour les valeurs de développement (`dev`) et de production (`prod`).

Dans l’application `build.gradle` fichier, un nouveau `flavorDimension` named `env` est créée.

Dans le `env` dimension, deux `productFlavors` sont définies : `dev` et `prod`. Chaque `productFlavor` uses `buildConfigField` pour définir des variables spécifiques à la version définissant le service AEM auquel se connecter.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Chargement de l&#39;AEM image

Android™ utilise une `ImageGetter` pour récupérer et mettre en cache localement les données d’image d’AEM. Dans `prepareDrawableFor(..)` l’hôte de service AEM, défini dans la configuration de build principale, est utilisé pour préfixer le chemin d’accès à l’image créant une URL résolvable à AEM.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String path) {
        // Get the image data from the cache using the path as the key
        Drawable drawable = drawablesByPath.get(path);
        return drawable;
    }
}
```

#### Vue Android™

La vue Android™ récupère les données d’image au moyen de la fonction `RemoteImagesCache` en utilisant la variable `_path` de la réponse GraphQL.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImagePath()));
        ...
    }
...
}
```

Lors de la création de l’application Android™ pour différents usages, spécifiez la variable `env` et la valeur d’hôte AEM correspondante est utilisée.

+++