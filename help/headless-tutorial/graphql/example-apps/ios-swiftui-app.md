---
title: Application iOS - Exemple AEM Headless
description: Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application iOS explique comment interroger du contenu à l’aide des API GraphQL d’AEM en utilisant les requêtes persistantes.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: 3e4960bf2d243e33fb9f36fd3fbb45f57260229a
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 98%

---

# Application iOS

Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application iOS explique comment interroger du contenu à l’aide des API GraphQL d’AEM en utilisant les requêtes persistantes.

![Application SwiftUI iOS avec AEM Headless.](./assets/ios-swiftui-app/ios-app.png)

Afficher le [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [Xcode](https://developer.apple.com/xcode/) (nécessite macOS)
+ [Git](https://git-scm.com/)

## Configuration requise d’AEM

L’application iOS fonctionne avec les options de déploiement AEM suivantes. Tous les déploiements nécessitent que la version [v3.0.0 ou supérieure du site WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) soit installée.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=fr)
+ Configuration locale à l’aide du [SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr)

L’application iOS est conçue pour se connecter à un environnement de __publication AEM__. Cependant, elle peut récupérer du contenu depuis l’instance de création AEM, si l’authentification est fournie dans la configuration de l’application iOS.

## Utilisation

1. Clonez le référentiel `adobe/aem-guides-wknd-graphql` :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Lancez [Xcode](https://developer.apple.com/xcode/) et ouvrez le dossier `ios-app`.
1. Modifiez le fichier `Config.xcconfig` et mettez à jour `AEM_SCHEME` et `AEM_HOST` pour correspondre à votre service de publication AEM cible.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Si vous vous connectez au service de création AEM, ajoutez `AEM_AUTH_TYPE` et les propriétés d’authentification nécessaires à `Config.xcconfig`.

   __Authentification de base__

   Les éléments `AEM_USERNAME` et `AEM_PASSWORD` authentifient une personne utilisatrice locale d’AEM ayant accès au contenu GraphQL de WKND.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Authentification par jeton__

   Le `AEM_TOKEN` est un [jeton d’accès](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=fr) qui s’authentifie auprès d’un utilisateur ou d’une utilisatrice d’AEM ayant accès au contenu GraphQL de WKND.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Créer l’application à l’aide de Xcode et la déployer sur un simulateur iOS
1. Une liste des Adventures du site WKND s’affiche dans l’application. Si vous en sélectionnez une, vous ouvrez les détails de l’Adventure. Dans l’affichage en liste des Adventures, balayez du haut de l’écran vers le bas pour actualiser les données d’AEM.

## Le code

Vous trouverez ci-dessous un résumé de la création de l’application iOS, de sa connexion à AEM Headless pour récupérer du contenu à l’aide de requêtes persistantes GraphQL et de la manière dont ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Requêtes persistantes

En suivant les bonnes pratiques d’AEM Headless, l’application iOS utilise les requêtes persistantes GraphQL d’AEM pour interroger les données d’Adventure. L’application utilise deux requêtes persistantes :

+ La requête persistante `wknd/adventures-all`, qui renvoie toutes les Adventures dans AEM avec un ensemble abrégé de propriétés. Cette requête persistante génère la liste des Adventures de la vue initiale.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ La requête persistante `wknd/adventure-by-slug`, qui renvoie une seule Adventure par `slug` (propriété personnalisée qui identifie de manière unique une Adventure) avec un ensemble complet de propriétés. Cette requête persistante alimente les vues détaillées de l’Adventure.

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Exécuter la requête persistante GraphQL

Les requêtes persistantes d’AEM sont exécutées via HTTP GET. Par conséquent, les bibliothèques GraphQL courantes qui utilisent HTTP POST, comme Apollo, ne peuvent pas être utilisées. Créez plutôt une classe personnalisée qui exécute les requêtes HTTP GET persistantes vers AEM.

`AEM/Aem.swift` instancie la classe `Aem` utilisée pour toutes les interactions avec AEM Headless. Le modèle est le suivant :

1. Chaque requête persistante a une fonction publique correspondante (par exemple, `getAdventures(..)` ou `getAdventureBySlug(..)`) que les vues de l’application iOS appellent pour obtenir des données de l’Adventure.
1. La fonction publique appelle une fonction privée `makeRequest(..)` qui appelle une requête HTTP GET asynchrone vers AEM Headless et renvoie les données JSON.
1. Chaque fonction publique décode ensuite les données JSON et effectue toutes les vérifications ou transformations requises, avant de renvoyer les données de l’adventure à la vue.

   + Les données JSON GraphQL d’AEM sont décodées à l’aide des structures/classes définies dans `AEM/Models.swift`, qui correspondent aux objets JSON renvoyés par AEM Headless.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }
    
    ...

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

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### Modèles de données de réponse GraphQL

iOS préfère mapper les objets JSON aux modèles de données avec un type.

L’élément `src/AEM/Models.swift` définit les structures et classes Swift [décodables](https://developer.apple.com/documentation/swift/decodable) qui correspondent aux réponses JSON d’AEM renvoyées par les réponses JSON d’AEM.

### Vues

SwiftUI est utilisé pour les différentes vues de l’application. Apple fournit un tutoriel de prise en main permettant de [créer des listes et une navigation avec SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  Entrée de l’application. Inclut `AdventureListView` dont le gestionnaire d’événements `.onAppear` est utilisé pour récupérer toutes les données des Adventures via `aem.getAdventures()`. L’objet `aem` partagé est initialisé ici et exposé à d’autres vues sous la forme d’un [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Affiche une liste d’Adventures (en fonction des données de `aem.getAdventures()`) et affiche un élément de liste pour chaque Adventure à l’aide de `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  Affiche chaque élément de la liste des Adventures (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Affiche les détails d’une Adventure, notamment le titre, la description, le Price, le type d’Activity et l’image principale. Cette vue interroge AEM pour obtenir les détails complets de l’Adventure à l’aide de `aem.getAdventureBySlug(slug: slug)`, où le paramètre `slug` est transmis en fonction de la ligne de liste sélectionnée.

### Images distantes

Les images référencées par les fragments de contenu des Adventures sont diffusées par AEM. Cette application iOS utilise le champ de chemin d’accès `_dynamicUrl` dans la réponse GraphQL et ajoute un préfixe à `AEM_SCHEME` et `AEM_HOST` pour créer une URL complète. En cas de développement par rapport au SDK AEM, `_dynamicUrl` renvoie null, de sorte que pour le développement, la version de secours de l’image soit `_path` champ .

Si vous vous connectez à des ressources protégées sur AEM qui nécessitent une autorisation, les informations d’identification doivent également être ajoutées aux requêtes d’images.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) et [SDWebImage](https://github.com/SDWebImage/SDWebImage) sont utilisés pour charger les images distantes d’AEM qui renseignent l’image de l’Adventure sur les vues `AdventureListItemView` et `AdventureDetailView`.

La classe `aem` (dans `AEM/Aem.swift`) facilite l’utilisation des images AEM de deux manières :

1. `aem.imageUrl(path: String)` est utilisé dans les vues pour ajouter le schéma et l’hôte d’AEM au chemin d’accès de l’image, créant ainsi une URL complète.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. L’élément `convenience init(..)` dans `Aem` définit les en-têtes d’autorisation HTTP sur la requête HTTP d’image, en fonction de la configuration des applications iOS.

   + Si l’__authentification de base__ est configurée, elle est jointe à toutes les requêtes d’images.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + Si l’__authentification par jeton__ est configurée, elle est jointe à toutes les requêtes d’images.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + Si __aucune authentification__ n’est configurée, aucune authentification n’est jointe aux requêtes d’images.

Une approche similaire peut être utilisée avec l’élément [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) natif sur SwiftUI. `AsyncImage` est pris en charge sur iOS 15.0+.

## Ressources supplémentaires

+ [Prise en main d’AEM Headless - Tutoriel GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=fr)
+ [Tutoriel sur les listes et la navigation dans SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
