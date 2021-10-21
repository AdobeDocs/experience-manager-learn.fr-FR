---
title: Application Android - Exemple AEM sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Une application Android est fournie. Elle indique comment interroger le contenu à l’aide des API GraphQL d’AEM. Le client Apollo Android est utilisé pour générer les requêtes GraphQL et mapper les données aux objets Swift pour alimenter l’application. SwiftUI est utilisé pour effectuer le rendu d’une vue de liste et de détails simple du contenu.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 0ab14016c27d3b91252f3cbf5f97550d89d4a0c9
workflow-type: tm+mt
source-wordcount: '722'
ht-degree: 3%

---


# Application Android SwiftUI

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Une application Android est fournie. Elle indique comment interroger le contenu à l’aide des API GraphQL d’AEM. Le [AEM client sans affichage pour Java](https://github.com/adobe/aem-headless-client-java) est utilisé pour exécuter les requêtes GraphQL et mapper les données aux objets Java pour alimenter l’application.

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

* [Android Studio](https://developer.android.com/studio)
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

1. Launch [Android Studio](https://developer.android.com/studio) et ouvrez le dossier `android-app`
1. Modification du fichier `config.properties` at `app/src/main/assets/config.properties` et mettre à jour `contentApi.endpoint` pour correspondre à votre environnement AEM cible :

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Téléchargement d’une [Appareil virtuel Android](https://developer.android.com/studio/run/managing-avds) (API min 28)
1. Créez et déployez l’application à l’aide de l’émulateur Android.


### Connexion à des environnements AEM

`10.0.2.2` est un [alias spécial](https://developer.android.com/studio/run/emulator-networking) pour localhost lors de l’utilisation de l’émulateur. Donc `10.0.2.2:4502` équivaut à `localhost:4502`. Si vous vous connectez à un environnement de publication AEM (recommandé), aucune autorisation n’est requise et `contentAPi.user` et `contentApi.password` peut être laissé vide.

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

Below is a brief summary of the important files and code used to power the application. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### Fetching Content

Le [AEM client sans affichage pour Java](https://github.com/adobe/aem-headless-client-java) est utilisé par l’application pour exécuter la requête GraphQL sur AEM et charger le contenu aventure dans l’application.

`AdventuresLoader.java` is the file that fetches and loads the initial list of Adventures on the home screen of the application. Il utilise [Requêtes persistantes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) qui [préconditionné](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) avec le site de référence WKND. Le point de terminaison est `/wknd/adventures-all`. `AEMHeadlessClientBuilder` instantiates a new instance based on the api endpoint set in `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` est le fichier qui récupère et charge le contenu Adventure pour chacune des vues de détail. Une fois de plus, `AEMHeadlessClient` est utilisé pour exécuter la requête. A regular graphQL query is executed based on the path to the Adventure content fragment. The query is maintained in a separate file named [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` est un POJO qui est initialisé avec les données JSON de la requête GraphQL .

`RemoteImagesCache.java` est une classe d’utilitaire qui permet de préparer des images distantes dans un cache afin qu’elles puissent être utilisées avec les éléments de l’interface utilisateur Android. Le contenu Adventure référence des images dans AEM Assets via une URL et cette classe est utilisée pour afficher ce contenu.

### Vues

`AdventureListFragment.java` : lorsqu’il est appelé déclenche la variable `AdventuresLoader` et affiche les aventures renvoyées dans une liste.

`AdventureDetailFragment.java` - initialize `AdventureLoader` et affiche les détails d’une seule aventure.

## Ressources supplémentaires

* [Prise en main d’AEM sans affichage - Tutoriel GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [AEM client sans affichage pour Java](https://github.com/adobe/aem-headless-client-java)

