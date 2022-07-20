---
title: Application iOS - AEM exemple sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application iOS explique comment interroger du contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: cd7cb89f407f5e0c465544593563534472daf928
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 4%

---

# application iOS

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application iOS explique comment interroger du contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes.

![Application iOS SwiftUI avec AEM sans affichage](./assets/ios-swiftui-app/ios-app.png)

Afficher la variable [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [Xcode 9.3+](https://developer.apple.com/xcode/) (nécessite macOS)
+ [Git](https://git-scm.com/)

## Configuration requise AEM

L’application iOS fonctionne avec les options de déploiement AEM suivantes. Tous les déploiements nécessitent la variable [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) à installer.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=fr)
+ Configuration locale à l’aide de [le SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr?lang=en#install-local-aem-instances)

L’application iOS est conçue pour se connecter à une __Publication AEM__ , mais il peut toutefois sources du contenu à partir de l’auteur AEM si l’authentification est fournie dans la configuration de l’application iOS.

## Utilisation

1. Cloner le `adobe/aem-guides-wknd-graphql` référentiel :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) et ouvrez le dossier `ios-app`
1. Modification du fichier `Config.xcconfig` fichier et mise à jour `AEM_SCHEME` et `AEM_HOST` pour correspondre à votre service AEM Publish cible.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   Si vous vous connectez à l’auteur AEM, ajoutez le `AEM_AUTH_TYPE` et la prise en charge des propriétés d’authentification pour la `Config.xcconfig`.

   __Authentification de base__

   Le `AEM_USERNAME` et `AEM_PASSWORD` authentifiez un utilisateur AEM local ayant accès au contenu WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Authentification des jetons__

   Le `AEM_TOKEN` est un [jeton d’accès](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) qui s’authentifie auprès d’un utilisateur AEM ayant accès au contenu GraphQL WKND.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Création de l’application à l’aide de Xcode et déploiement de l’application sur le simulateur iOS
1. Une liste des aventures du site WKND doit s’afficher sur l’application. Sélectionner une aventure ouvre les détails de l&#39;aventure. Dans la vue Liste des aventures, extrayez pour actualiser les données d’AEM.

## Le code

Vous trouverez ci-dessous un résumé de la création de l’application iOS, de sa connexion à AEM sans affichage pour récupérer du contenu à l’aide de requêtes persistantes GraphQL et de la manière dont ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Requêtes persistantes

En suivant AEM bonnes pratiques sans affichage, l’application iOS utilise AEM requêtes persistantes GraphQL pour interroger les données d’aventure. L’application utilise deux requêtes persistantes :

+ `wknd/adventures-all` requête persistante, qui renvoie toutes les aventures d’AEM avec un jeu abrégé de propriétés. Cette requête persistante entraîne la liste des aventures de la vue initiale.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` requête persistante, qui renvoie une seule aventure par `slug` (propriété personnalisée qui identifie de manière unique une aventure) avec un ensemble complet de propriétés. Cette requête persistante alimente les vues détaillées de l’aventure.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

AEM requêtes persistantes sont exécutées sur des GET HTTP. Par conséquent, les bibliothèques GraphQL courantes qui utilisent un POST HTTP tel qu’Apollo ne peuvent pas être utilisées. Créez plutôt une classe personnalisée qui exécute les requêtes HTTP de GET persistantes à AEM.

`AEM/Aem.swift` instancie la variable `Aem` classe utilisée pour toutes les interactions avec AEM sans affichage. Le modèle est le suivant :

1. Chaque requête conservée a un func public correspondant (ex. `getAdventures(..)` ou `getAdventureBySlug(..)`) les vues de l’application iOS sont invoquées pour obtenir des données d’aventure.
1. Le fonds public appelle un fonds privé. `makeRequest(..)` qui appelle une demande de GET HTTP asynchrone à AEM sans affichage et renvoie les données JSON.
1. Chaque fonction publique décode ensuite les données JSON et effectue toutes les vérifications ou transformations requises, avant de renvoyer les données Adventure à la vue.

+ AEM les données JSON GraphQL sont décodées à l’aide des structs/classes définis dans `AEM/Models.swift`, qui correspondait aux objets JSON renvoyait mon AEM sans affichage.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
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

iOS préfère mapper les objets JSON aux modèles de données tapés.

Le `src/AEM/Models.swift` définit la variable [décodable](https://developer.apple.com/documentation/swift/decodable) Swift crée des structures et des classes qui correspondent aux réponses JSON AEM renvoyées par AEM réponses JSON.

### Vues

SwiftUI est utilisé pour les différentes vues de l’application. Apple fournit un tutoriel de prise en main pour [création de listes et navigation avec SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   L’entrée de l’application et inclut `AdventureListView` who `.onAppear` Le gestionnaire d’événements est utilisé pour récupérer toutes les données d’aventure via `aem.getAdventures()`. Le partage `aem` est initialisé ici et exposé à d’autres vues sous la forme d’une [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   Affiche une liste d’aventures (en fonction des données de `aem.getAdventures()`) et affiche un élément de liste pour chaque aventure à l’aide du `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   Affiche chaque élément de la liste des aventures (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

   Affiche les détails d’une aventure, notamment le titre, la description, le prix, le type d’activité et la Principale image. Cette vue interroge AEM détails complets de l’aventure à l’aide de `aem.getAdventureBySlug(slug: slug)`, où la variable `slug` est transmis en fonction de la ligne de liste sélectionnée.

### Images distantes

Les images référencées par des fragments de contenu d’aventure sont diffusées par AEM. Cette application iOS utilise le chemin d’accès `_path` dans la réponse GraphQL et préfixe la variable `AEM_SCHEME` et `AEM_HOST` pour créer une URL complète.

Si vous vous connectez à des ressources protégées sur AEM nécessitant une autorisation, les informations d’identification doivent également être ajoutées aux demandes d’image.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) et [SDWebImage](https://github.com/SDWebImage/SDWebImage) sont utilisées pour charger les images distantes d’AEM qui renseignent l’image Adventure sur la `AdventureListItemView` et `AdventureDetailView` vues.

Le `aem` (dans `AEM/Aem.swift`) facilite l’utilisation d’AEM images de deux manières :

1. `aem.imageUrl(path: String)` est utilisé dans les vues pour ajouter le schéma d’AEM et l’hôte au chemin d’accès de l’image, créant ainsi une URL complète.

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. Le `convenience init(..)` in `Aem` définissez les en-têtes d’autorisation HTTP sur la demande d’image HTTP, en fonction de la configuration des applications iOS.

   + If __authentification de base__ est configuré, puis une authentification de base est jointe à toutes les demandes d’image.

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

   + If __authentification par jeton__ est configuré, puis l’authentification par jeton est jointe à toutes les demandes d’image.

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

   + If __pas d’authentification__ est configuré, puis aucune authentification n’est jointe aux demandes d’image.



Une approche similaire peut être utilisée avec SwiftUI-native [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` est pris en charge sur iOS 15.0+.

## Ressources supplémentaires

+ [Prise en main d’AEM sans affichage - Tutoriel GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Tutoriel sur les listes et la navigation dans SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
