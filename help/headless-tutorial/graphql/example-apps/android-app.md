---
title: Application Android - Exemple AEM sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application Android explique comment interroger du contenu à l’aide des API GraphQL d’AEM.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 0204d9aaf7b79b0745adbe749f44245716203b88
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 5%

---

# Application Android

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application Android explique comment interroger du contenu à l’aide des API GraphQL d’AEM. Le [AEM client sans affichage pour Java](https://github.com/adobe/aem-headless-client-java) est utilisé pour exécuter les requêtes GraphQL et mapper les données aux objets Java pour alimenter l’application.

![Application Java Android avec AEM sans affichage](./assets/android-java-app/android-app.png)


Afficher la variable [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## Configuration requise AEM

L’application Android fonctionne avec les options de déploiement AEM suivantes. Tous les déploiements nécessitent la variable [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) à installer.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=fr)
+ Configuration locale à l’aide de [le SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr?lang=en#install-local-aem-instances)

L’application Android est conçue pour se connecter à un __Publication AEM__ , toutefois, il peut source du contenu à partir de l’auteur AEM si l’authentification est fournie dans la configuration de l’application Android.

## Utilisation

1. Cloner le `adobe/aem-guides-wknd-graphql` référentiel :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) et ouvrez le dossier `android-app`
1. Modification du fichier `config.properties` at `app/src/main/assets/config.properties` et mettre à jour `contentApi.endpoint` pour correspondre à votre environnement AEM cible :

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __Authentification de base__

   Le `contentApi.user` et `contentApi.password` authentifiez un utilisateur AEM local ayant accès au contenu WKND GraphQL.

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Téléchargement d’une [Appareil virtuel Android](https://developer.android.com/studio/run/managing-avds) (API minimale 28).
1. Créez et déployez l’application à l’aide de l’émulateur Android.


### Connexion à des environnements AEM

`10.0.2.2` est un [IP d’alias spécial](https://developer.android.com/studio/run/emulator-networking) pour localhost lors de l’utilisation de l’émulateur en cours de création `10.0.2.2:4502` équivaut à `localhost:4502`. Si vous vous connectez à un environnement de publication AEM (recommandé), aucune autorisation n’est requise et `contentAPi.user` et `contentApi.password` peut être laissé vide.

Connexion à un environnement de création AEM [authorization](https://github.com/adobe/aem-headless-client-java#using-authorization) est obligatoire. Par défaut, l’application est configurée pour utiliser une authentification de base avec un nom d’utilisateur et un mot de passe de `admin:admin`. Le [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) permet d’utiliser des [authentification par jeton](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Pour utiliser l’authentification par jeton, mettez à jour le créateur de client dans `AdventureLoader.java` et `AdventuresLoader.java`:

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## Le code

Vous trouverez ci-dessous un bref résumé des fichiers et du code importants utilisés pour alimenter l’application. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

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

## Exécuter la requête persistante GraphQL

AEM requêtes persistantes sont exécutées sur une GET HTTP et, par conséquent, la variable [AEM client sans affichage pour Java](https://github.com/adobe/aem-headless-client-java) est utilisé pour exécuter les requêtes GraphQL persistantes sur AEM et charger le contenu aventure dans l’application.

Chaque requête conservée possède une classe &quot;loader&quot; correspondante, qui appelle de manière asynchrone le point de terminaison HTTP AEM et renvoie les données d’aventure à l’aide de la classe personnalisée définie. [modèle de données](#data-models).

+ `loader/AdventuresLoader.java`

   Récupère la liste des aventures sur l’écran d’accueil de l’application à l’aide de la fonction `wknd-shared/adventures-all` requête persistante.

+ `loader/AdventureLoader.java`

   Récupère une seule aventure en la sélectionnant via le `slug` , à l’aide du paramètre `wknd-shared/adventure-by-slug` requête persistante.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### Modèles de données de réponse GraphQL{#data-models}

`Adventure.java` est un POJO Java initialisé avec les données JSON de la requête GraphQL et modélise une aventure à utiliser dans les vues de l’application Android.

### Vues

L’application Android utilise deux vues pour présenter les données d’aventure dans l’expérience mobile.

+ `AdventureListFragment.java`

   Appelle le `AdventuresLoader` et affiche les aventures renvoyées dans une liste.

+ `AdventureDetailFragment.java`

   Appelle le `AdventureLoader` en utilisant la variable `slug` param transmis via la sélection aventure sur le `AdventureListFragment` et affiche les détails d’une seule aventure.

### Images distantes

`loader/RemoteImagesCache.java` est une classe d’utilitaire qui permet de préparer des images distantes dans un cache afin qu’elles puissent être utilisées avec les éléments de l’interface utilisateur Android. Le contenu aventure référence des images dans AEM Assets via une URL et cette classe est utilisée pour afficher ce contenu.

## Ressources supplémentaires

+ [Prise en main d’AEM sans affichage - Tutoriel GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [AEM client sans affichage pour Java](https://github.com/adobe/aem-headless-client-java)
