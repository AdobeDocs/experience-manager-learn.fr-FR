---
title: Gérer les hôtes AEM pour AEM GraphQL
description: Découvrez comment configurer les hôtes AEM dans l’application AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 714
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 100%

---

# Gérer les hôtes AEM

Lorsque vous déployez une application AEM Headless, prêtez attention à la construction des URL AEM afin de vous assurer que vous utilisez l’hôte ou le domaine AEM adapté. Les principaux types d’URL et de requêtes à connaître sont les suivants :

+ Requêtes HTTP aux __[API GraphQL d’AEM](#aem-graphql-api-requests)__
+ __[URL d’image](#aem-image-urls)__ : pour les ressources d’image référencées dans les fragments de contenu et diffusées par AEM

En règle générale, une application AEM Headless interagit avec un seul service AEM pour les requêtes d’image et d’API GraphQL. Le service AEM change en fonction du déploiement de l’application AEM Headless :

| Type de déploiement AEM Headless | Environnement AEM | Service AEM |
|-------------------------------|:---------------------:|:----------------:|
| Production | Production | Publication |
| Aperçu de la création | Production | Prévisualisation |
| Développement | Développement | Publication |

Pour gérer les permutations de type déploiement, chaque déploiement d’application est créé à l’aide d’une configuration spécifiant le service AEM auquel se connecter. L’hôte ou le domaine du service AEM configuré est ensuite utilisé pour créer les URL de l’API GraphQL d’AEM et les URL d’image. Pour déterminer l’approche appropriée pour la gestion des configurations qui dépendent de la création, reportez-vous à la documentation du framework de l’application AEM Headless (par exemple, React, iOS, Android™, etc.), car l’approche varie selon le framework.

| Type de client | [Application monopage (SPA)](../spa.md) | [Composant Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Serveur à serveur](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuration des hôtes AEM | ✔ | ✔ | ✔ | ✔ |

Vous trouverez ci-dessous des exemples d’approches possibles pour la création d’URL pour l’[API GraphQL d’AEM](#aem-graphql-api-requests) et les [requêtes d’images](#aem-image-requests), pour plusieurs plateformes et frameworks découplés populaires.

## Requêtes à l’API GraphQL d’AEM

Les requêtes HTTP GET de l’application découplée à l’API GraphQL d’AEM doivent être configurées pour interagir avec le service AEM adapté, comme décrit dans le [tableau ci-dessus](#managing-aem-hosts).

Lors de l’utilisation des [SDK d’AEM Headless](../../how-to/aem-headless-sdk.md) (disponibles pour JavaScript basé sur un navigateur, JavaScript basé sur un serveur et Java™), un hôte AEM peut initialiser l’objet client d’AEM Headless avec le service AEM auquel se connecter.

Lors du développement d’un client AEM Headless personnalisé, assurez-vous que l’hôte du service AEM est paramétrable en fonction des paramètres de création.

### Exemples

Vous trouverez ci-dessous des exemples de la manière dont les requêtes d’API GraphQL d’AEM peuvent rendre la valeur d’hôte AEM configurable pour différents frameworks d’application découplée.

+++ Exemple pour React

Cet exemple, basé librement sur l’[application React AEM Headless](../../example-apps/react-app.md), illustre la manière dont les requêtes d’API GraphQL d’AEM peuvent être configurées pour se connecter à différents services AEM en fonction des variables d’environnement.

Les applications React doivent utiliser le [client AEM Headless pour JavaScript](../../how-to/aem-headless-sdk.md) pour interagir avec les API GraphQL d’AEM. Le client AEM Headless, fourni par le client AEM Headless pour JavaScript, doit être initialisé avec l’hôte du service AEM auquel il se connecte.

#### Fichier d’environnement React

React utilise des [fichiers d’environnement personnalisés](https://create-react-app.dev/docs/adding-custom-environment-variables/), ou fichiers `.env`, stockés à la racine du projet pour définir des valeurs spécifiques à la création. Par exemple, le fichier `.env.development` contient les valeurs utilisées lors du développement, tandis que `.env.production` contient les valeurs utilisées pour les créations de production.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

Les fichiers `.env` destinés à d’autres fins [peuvent être spécifiés](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) en postfixant `.env` et un descripteur sémantique, tel que `.env.stage` ou `.env.production`. Vous pouvez utilisez des fichiers `.env` différents lors de l’exécution ou de la création de l’application React, en définissant l’élément `REACT_APP_ENV` avant d’exécuter une commande `npm`.

Par exemple, un `package.json` d’application React peut contenir la configuration `scripts` suivante :

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

#### Client AEM Headless

Le [client AEM Headless pour JavaScript](../../how-to/aem-headless-sdk.md) contient un client AEM Headless qui effectue des requêtes HTTP aux API GraphQL d’AEM. Le client AEM Headless doit être initialisé avec l’hôte AEM avec lequel il interagit, à l’aide de la valeur du fichier `.env` actif.

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

#### Hook React useEffect(..)

Les hooks React useEffect personnalisés appellent le client AEM Headless, initialisé avec l’hôte AEM, au nom du composant React qui effectue le rendu de la vue.

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

Le hook useEffect personnalisé `useAdventureByPath` est importé et utilisé pour obtenir les données à l’aide du client AEM Headless. Il effectue le rendu du contenu à la personne utilisatrice finale.

+ « src/components/AdventureDetail.js »

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ Exemple iOS™

Cet exemple, basé sur l’[exemple d’application iOS™ AEM Headless](../../example-apps/ios-swiftui-app.md), illustre la manière dont les requêtes d’API GraphQL d’AEM peuvent être configurées pour se connecter à différents hôtes AEM en fonction de [variables de configuration spécifiques à la création](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Les applications iOS™ nécessitent un client AEM Headless personnalisé pour interagir avec les API GraphQL d’AEM. Le client AEM Headless doit être écrit de sorte que l’hôte de service AEM soit configurable.

#### Configuration de version

Le fichier de configuration XCode contient les détails de configuration par défaut.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Initialiser le client AEM Headless personnalisé

L’[exemple d’application AEM Headless iOS](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) utilise un client AEM Headless personnalisé et initialisé avec les valeurs de configuration pour `AEM_SCHEME` et `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

Le client AEM Headless personnalisé (`api/Aem.swift`) contient une méthode `makeRequest(..)` qui préfixe les requêtes d’API AEM GraphQL avec `scheme` et `host` configurés pour AEM.

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

Vous pouvez [créer de nouveaux fichiers de configuration de version](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) pour vous connecter à différents services AEM. Les valeurs spécifiques à la version pour `AEM_SCHEME` et `AEM_HOST` sont utilisées en fonction de la version sélectionnée dans XCode. Ainsi, le client AEM Headless personnalisé se connecte au service AEM approprié.

+++

+++ Exemple Android™

Cet exemple, basé sur l’[exemple d’application AEM Headless Android™](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustre la manière dont les requêtes d’API AEM GraphQL peuvent être configurées pour se connecter à différents services AEM en fonction de variables de configuration spécifiques à la version (ou aux versions).

Les applications Android™ (lorsqu’elles sont écrites en Java™) doivent utiliser la variable [Client AEM Headless pour Java™](https://github.com/adobe/aem-headless-client-java) pour interagir avec les API GraphQL d’AEM. Le client AEM Headless, fourni par le client AEM Headless pour Java™, doit être initialisé avec l’hôte de service AEM auquel il se connecte.

#### Créer un fichier de configuration

Les applications Android™ définissent des « productFlavors » utilisés pour créer des artefacts à diverses fins.
Cet exemple montre comment deux versions de produit Android™ peuvent être définies, fournissant différents hôtes de service AEM (`AEM_HOST`) pour les valeurs de développement (`dev`) et de production (`prod`).

Dans le fichier `build.gradle` de l’application, un nouvel élément `flavorDimension` nommé `env` est créé.

Dans la dimension `env`, deux `productFlavors` sont définis : `dev` et `prod`. Chaque `productFlavor` utilise `buildConfigField` pour définir des variables spécifiques à la version définissant le service AEM auquel se connecter.

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

Initialisez le créateur `AEMHeadlessClient` fourni par le client AEM Headless pour Java™ avec la valeur `AEM_HOST` du champ `buildConfigField`.

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

Lors de la création de l’application Android™ pour différentes utilisations, spécifiez la version `env` pour que la valeur d’hôte AEM correspondante soit utilisée.

+++

## URL des images AEM

Les requêtes d’images de l’application découplée à AEM doivent être configurées pour interagir avec le service AEM approprié, comme décrit dans le [tableau ci-dessus](#managing-aem-hosts).

Adobe recommande d’utiliser des [images optimisées](../../how-to/images.md) mises à disposition par le biais du champ `_dynamicUrl` dans les API GraphQL d’AEM. Le champ `_dynamicUrl` renvoie une URL sans hôte qui peut être précédée de l’hôte de service AEM utilisé pour interroger les API GraphQL d’AEM. Pour le champ `_dynamicUrl` dans GraphQL, la réponse ressemble à ce qui suit :

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Exemples

Vous trouverez ci-dessous des exemples de la manière dont les URL d’image peuvent ajouter un préfixe à la valeur d’hôte AEM configurée pour différentes structures d’application découplées. Les exemples supposent l’utilisation de requêtes GraphQL qui renvoient des références d’image à l’aide du champ `_dynamicUrl`.

Par exemple :

#### Requête persistante GraphQL

Cette requête GraphQL renvoie la variable `_dynamicUrl` de la référence de l’image. C’est ce que montre la [réponse GraphQL](#examples-react-graphql-response) qui exclut un hôte.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### Réponse GraphQL

Cette réponse GraphQL renvoie la variable `_dynamicUrl` de la référence d’image qui exclut un hôte.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ Exemple pour React

Cet exemple, basé sur l’[exemple d’application AEM Headless React](../../example-apps/react-app.md), illustre la manière dont les URL d’image peuvent être configurées pour se connecter aux services AEM appropriés en fonction des variables d’environnement.

Cet exemple montre comment ajouter un préfixe au champ `_dynamicUrl` de référence de l’image avec une variable d’environnement React `REACT_APP_AEM_HOST` configurable.

#### Fichier d’environnement React

React utilise des [fichiers d’environnement personnalisés](https://create-react-app.dev/docs/adding-custom-environment-variables/), ou fichiers `.env`, stockés à la racine du projet pour définir des valeurs spécifiques à la création. Par exemple, le fichier `.env.development` contient les valeurs utilisées lors du développement, tandis que `.env.production` contient les valeurs utilisées pour les créations de production.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

Les fichiers `.env` destinés à d’autres fins [peuvent être spécifiés](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) en ajoutant un suffixe `.env` et un descripteur sémantique, tel que `.env.stage` ou `.env.production`. Un fichier `.env` différent peut être utilisé lors de l’exécution ou de la création de l’application React, en définissant la variable `REACT_APP_ENV` avant d’exécuter une commande `npm`.

Par exemple, la variable `package.json` de l’application React peut contenir la configuration `scripts` suivante :

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

Le composant React importe la variable d’environnement `REACT_APP_AEM_HOST` et ajoute un préfixe à la valeur `_dynamicUrl` de l’image pour fournir une URL d’image entièrement résolvable.

La même variable d’environnement `REACT_APP_AEM_HOST` est utilisée pour initialiser le client AEM Headless utilisé par le hook useEffect `useAdventureByPath(..)` personnalisé, lui-même utilisé pour récupérer les données GraphQL à partir d’AEM. En utilisant la même variable pour construire la requête d’API GraphQL comme URL d’image, assurez-vous que l’application React interagit avec le même service AEM pour les deux cas d’utilisation.

+ « src/components/AdventureDetail.js »

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ Exemple iOS™

Cet exemple, basé sur l’[exemple d’application AEM Headless iOS™](../../example-apps/ios-swiftui-app.md), illustre comment les URL des images AEM peuvent être configurées pour se connecter à différents hôtes AEM en fonction de [variables de configuration spécifiques à la version](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Configuration de version

Le fichier de configuration XCode contient les détails de configuration par défaut.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Générateur d’URL d’image

Dans `Aem.swift`, l’implémentation du client AEM découplé personnalisée, qui est une fonction personnalisée `imageUrl(..)`, emprunte le chemin d’accès à l’image, comme indiqué dans le champ `_dynamicUrl` dans la réponse GraphQL, et y ajoute l’hôte AEM comme préfixe. Cette fonction est ensuite appelée dans les vues iOS chaque fois qu’une image est rendue.

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
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### Vue iOS

La vue iOS ajoute un préfixe à la valeur de l’image `_dynamicUrl` pour fournir une URL d’image entièrement résolvable.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

Vous pouvez [créer de nouveaux fichiers de configuration de version](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) pour vous connecter à différents services AEM. Les valeurs spécifiques à la version de `AEM_SCHEME` et `AEM_HOST` sont utilisées en fonction de la version sélectionnée dans XCode, ce qui entraîne l’interaction du client AEM Headless personnalisé avec le service AEM correct.

+++

+++ Exemple Android™

Cet exemple, basé sur l’[exemple d’application AEM Android™ Headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustre comment les URL des images AEM peuvent être configurées pour se connecter à différents services AEM en fonction de variables de configuration spécifiques à la version (ou aux versions).

#### Créer un fichier de configuration

Les applications Android™ définissent des « productFlavors » qui sont utilisés pour créer des artefacts à des fins différentes.
Cet exemple montre comment deux versions de produit Android™ peuvent être définies, fournissant différents hôtes de service AEM (`AEM_HOST`) pour les valeurs de développement (`dev`) et de production (`prod`).

Dans le fichier `build.gradle` de l’application, un nouvel élément `flavorDimension` nommé `env` est créé.

Dans la dimension `env`, deux `productFlavors` sont définis : `dev` et `prod`. Chaque `productFlavor` utilise `buildConfigField` pour définir des variables spécifiques à la version, définissant le service AEM auquel se connecter.

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

#### Charger l’image AEM

Android™ utilise `ImageGetter` pour récupérer et mettre en cache localement les données d’image d’AEM. Dans `prepareDrawableFor(..)`, l’hôte de service AEM, défini dans la configuration de version principale, est utilisé pour préfixer le chemin d’accès à l’image, créant une URL résolvable dans AEM.

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
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Vue Android™

La vue Android™ récupère les données d’image au moyen de `RemoteImagesCache` en utilisant la valeur `_dynamicUrl` de la réponse GraphQL.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

Lors de la création de l’application Android™ pour différentes utilisations, spécifiez la version `env` pour que la valeur d’hôte AEM correspondante soit utilisée.

+++
