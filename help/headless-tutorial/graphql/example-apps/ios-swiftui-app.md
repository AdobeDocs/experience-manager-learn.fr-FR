---
title: Application iOS SwiftUI - Exemple AEM sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Une application iOS est fournie. Elle indique comment interroger le contenu à l’aide des API GraphQL d’AEM. Le client Apollo iOS est utilisé pour générer les requêtes GraphQL et mapper les données aux objets Swift pour alimenter l’application. SwiftUI est utilisé pour effectuer le rendu d’une vue de liste et de détails simple du contenu.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 2%

---


# Application iOS SwiftUI

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application iOS explique comment interroger le contenu à l’aide des API GraphQL d’AEM. Le client Apollo iOS est utilisé pour générer les requêtes GraphQL et mapper les données aux objets Swift pour alimenter l’application. SwiftUI est utilisé pour effectuer le rendu d’une vue de liste et de détails simple du contenu.

>[!VIDEO](https://video.tv.adobe.com/v/338042/?quality=12&learn=on)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

* [Xcode 9.3+](https://developer.apple.com/xcode/)
* [Git](https://git-scm.com/)

## Conditions AEM

L’application est conçue pour se connecter à une AEM **Publier** environnement avec la dernière version d’ [Site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) installé.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=fr)

Nous vous recommandons [déploiement du site de référence WKND dans un environnement Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Une configuration locale à l’aide de [le SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) ou [Fichier jar QuickStart AEM 6.5](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) peut également être utilisé.

## Mode d’emploi

1. Cloner le `aem-guides-wknd-graphql` référentiel :

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) et ouvrez le dossier `ios-swiftui-app`
1. Modification du fichier `Config.xcconfig` fichier et mise à jour `AEM_HOST` pour correspondre à votre environnement de publication AEM cible

   ```plain
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   // GraphQL Endpoint
   AEM_GRAPHQL_ENDPOINT = /content/cq:graphql/wknd/endpoint.json
   ```

1. Création de l’application à l’aide de Xcode et déploiement de l’application sur le simulateur iOS
1. Une liste des aventures du site de référence WKND doit s’afficher sur l’application.

## Le code

Vous trouverez ci-dessous un bref résumé des fichiers et du code importants utilisés pour alimenter l’application. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app).

### Apollo iOS

Le [Apollo iOS](https://www.apollographql.com/docs/ios/) client est utilisé par l’application pour exécuter la requête GraphQL sur AEM. Le fonctionnaire [Tutoriel Apollo](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/) contient beaucoup plus de détails sur l’installation et l’utilisation.

`schema.json` est un fichier qui représente le schéma GraphQL d’un environnement AEM avec le site de référence WKND installé. `schema.json` a été téléchargé à partir d’AEM et ajouté au projet. Le client Apollo inspecte tous les fichiers avec l’extension . `.graphql` dans le cadre d’une phase de création personnalisée. Le client Apollo utilise ensuite la variable `schema.json` fichier et tout `.graphql` requêtes pour générer automatiquement le fichier `API.swift`.

L’application dispose ainsi d’un modèle fortement typé pour exécuter la requête et le ou les modèles représentant les résultats.

![Phase de génération personnalisée Xcode](assets/ios-swiftui-app/xcode-build-phase-apollo.png)

`AdventureList.graphql` contient la requête utilisée pour interroger les aventures :

```
query AdventureList
{
  adventureList {
    items {
      _path
      adventureTitle
      adventurePrice
      adventureActivity
      adventureDescription {
        plaintext
        markdown
      }
      adventureDifficulty
      adventureTripLength
      adventurePrimaryImage {
        ...on ImageRef {
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

`Network.swift` construit le `ApolloClient`. Le `endpointURL` utilisé est construit en lisant les valeurs de la variable `Config.xcconfig` fichier . Si vous souhaitez vous connecter à une AEM **Auteur** et nécessaire pour ajouter des en-têtes supplémentaires pour l’authentification, vous souhaitez modifier la variable `ApolloClient` ici.

```swift
// Network.swift
private(set) lazy var apollo: ApolloClient = {
        // The cache is necessary to set up the store, which we're going to hand to the provider
        let cache = InMemoryNormalizedCache()
        let store = ApolloStore(cache: cache)
  
        let client = URLSessionClient()
        let provider = DefaultInterceptorProvider(client: client, shouldInvalidateClientOnDeinit: true, store: store)
        let url = Connection.baseURL // from Configx.xcconfig 

        // no additional headers, public instances by default require no additional authentication
        let requestChainTransport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)

        return ApolloClient(networkTransport: requestChainTransport,store: store)
    }()
}
```

### Données aventure

L&#39;application est conçue pour afficher une liste d&#39;aventures, puis une vue détaillée de chaque aventure.

`AdventuresDataModel.swift` est une classe qui inclut une fonction `fetchAdventures()`. Cette fonction utilise la fonction `ApolloClient` pour exécuter la requête. Pour une requête réussie, le tableau de résultats est de type `AdventureListQuery.Data.AdventureList.Item`, généré automatiquement par la variable `API.swift` fichier .

```swift
func fetchAdventures() {
        Network.shared.apollo
            //AdventureListQuery() generated based on AdventureList.graphql file
           .fetch(query: AdventureListQuery()) { [weak self] result in
           
             guard let self = self else {
               return
             }
                   
             switch result {
             case .success(let graphQLResult):
                print("Success AdventureListQuery() from: \(graphQLResult.source)")

                if let adventureDataItems =  graphQLResult.data?.adventureList.items {
                    // map graphQL items to an array of Adventure objects
                    self.adventures = adventureDataItems.compactMap { Adventure(adventureData: $0!) }
                }
                ...
             }
           }
}
```

Il est possible d’utiliser `AdventureListQuery.Data.AdventureList.Item` directement pour alimenter l’application. Cependant, il est très possible que certaines données soient incomplètes et que certaines propriétés soient donc nulles.

`Adventure.swift` est un modèle personnalisé qui agit comme un wrapper du modèle généré par Apollo. `Adventure` est initialisé avec `AdventureListQuery.Data.AdventureList.Item`. A `typealias` est utilisé pour raccourcir le code afin qu’il soit plus lisible :

```
// use typealias
typealias AdventureData = AdventureListQuery.Data.AdventureList.Item
```

Le `Adventure` struct est initialisé avec une `AdventureData` objet :

```swift
struct Adventure: Identifiable {
    let id: String
    let adventureTitle: String
    let adventurePrice: String
    let adventureDescription: String
    let adventureActivity: String
    let adventurePrimaryImageUrl: String
    
    // initialize with AdventureData object aka AdventureListQuery.Data.AdventureList.Item
    init(adventureData: AdventureData) {
        // use path as unique idenitifer, otherwise
        self.id = adventureData._path ?? UUID().uuidString
        self.adventureTitle = adventureData.adventureTitle ?? "Untitled"
        self.adventurePrice = adventureData.adventurePrice ?? "Free"
        self.adventureActivity = adventureData.adventureActivity ?? ""
        ...
```

Cela nous permet ensuite de fournir des valeurs par défaut et d’effectuer des vérifications supplémentaires pour les données incomplètes. Nous pouvons alors utiliser la variable `Adventure` modélisez en toute sécurité pour alimenter divers éléments de l’interface utilisateur et n’avez pas à vérifier constamment les valeurs nulles.

Dans AEM, les éléments de contenu sont identifiés de manière unique par `_path`. Dans `Adventure.swift` nous renseignons le `id` avec la valeur de `_path`. Cela permet `Adventure` pour mettre en oeuvre la variable `Identifiable` et facilite l’itération sur un tableau ou une liste.

### Vues

SwiftUI est utilisé pour les différentes vues de l’application. Un bon tutoriel pour [création de listes et navigation](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation) est disponible sur le site de développement Apple. Le code de cette application en est vaguement dérivé.

`WKNDAdventuresApp.swift` est l’entrée de l’application. Elle comprend `AdventureListView` et le `.onAppear` est utilisé pour récupérer les données d’aventure.

`AdventureListView.swift` - crée une `NavigationView` et une liste d&#39;aventures renseignées par `AdventureRowView`. Navigation vers une `AdventureDetailView` est configuré ici.

`AdventureRowView` - affiche la Principale image de l’aventure et le Titre de l’aventure dans une ligne.

`AdventureDetailView` - affiche un détail complet de l&#39;aventure individuelle, avec titre, description, prix, type d&#39;activité et Principale image.

Lorsque l’interface de ligne de commande Apollo s’exécute et se génère à nouveau `API.swift` l’aperçu s’arrête alors. Pour utiliser la fonction d’aperçu automatique, vous devez mettre à jour la variable **Interface de ligne de commande d’Apollo** Phase de création et vérifier pour exécuter le script **Pour les versions d’installation uniquement**.

![Vérifier uniquement les versions d’installation](assets/ios-swiftui-app/update-build-phases.png)

### Images distantes

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) et [SDWEbImage](https://github.com/SDWebImage/SDWebImage) sont utilisées pour charger les images distantes d’AEM qui renseignent l’image Principale Adventure dans les vues Ligne et Détail.

Le [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) est une vue SwiftUI native qui peut également être utilisée. `AsyncImage` n’est pris en charge que pour iOS 15.0+.

## Ressources supplémentaires

* [Prise en main d’AEM sans affichage - Tutoriel GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=fr)
* [Tutoriel sur les listes et la navigation dans SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
* [Tutoriel client Apollo iOS](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/)

