---
title: Requêtes GraphQL persistantes - Concepts avancés d’AEM sans affichage - GraphQL
description: Dans ce chapitre des concepts avancés d’Adobe Experience Manager (AEM) sans affichage, découvrez comment créer et mettre à jour des requêtes GraphQL persistantes avec des paramètres. Découvrez comment transférer des paramètres de contrôle du cache dans des requêtes persistantes.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 1%

---

# Requêtes GraphQL persistantes

Les requêtes persistantes sont des requêtes stockées sur le serveur Adobe Experience Manager (AEM). Les clients peuvent envoyer une requête de GET HTTP avec le nom de la requête pour l’exécuter. L’avantage de cette approche est la mise en cache. Bien que les requêtes GraphQL côté client puissent également être exécutées à l’aide de requêtes de POST HTTP, qui ne peuvent pas être mises en cache, les requêtes persistantes peuvent être mises en cache par des caches HTTP ou un réseau de diffusion de contenu, ce qui améliore les performances. Les requêtes persistantes vous permettent de simplifier vos requêtes et d’améliorer la sécurité, car vos requêtes sont encapsulées sur le serveur et l’administrateur AEM en a le contrôle total. Il s’agit de **bonne pratique et fortement recommandée** pour utiliser des requêtes persistantes lors de l’utilisation de l’API GraphQL AEM.

Dans le chapitre précédent, vous avez exploré certaines requêtes GraphQL avancées afin de collecter des données pour l’application WKND. Dans ce chapitre, vous conservez les requêtes à AEM et apprenez à utiliser le contrôle du cache sur les requêtes persistantes.

## Prérequis {#prerequisites}

Ce document fait partie d’un tutoriel en plusieurs parties. Assurez-vous que la variable [chapitre précédent](explore-graphql-api.md) a été terminé avant de poursuivre ce chapitre.

## Objectifs {#objectives}

Dans ce chapitre, découvrez comment :

* Persistance des requêtes GraphQL avec des paramètres
* Utilisation des paramètres de contrôle du cache avec des requêtes persistantes

## Réviser _Requêtes persistantes GraphQL_ paramètre de configuration

Examinons que _Requêtes persistantes GraphQL_ sont activés pour le projet de site WKND dans votre instance AEM.

1. Accédez à **Outils** > **Général** > **Explorateur de configuration**.

1. Sélectionner **WKND partagé**, puis sélectionnez **Propriétés** dans la barre de navigation supérieure pour ouvrir les propriétés de configuration. Sur la page Propriétés de configuration, vous devriez voir que la variable **Requêtes persistantes GraphQL** l’autorisation est activée.

   ![Propriétés de configuration](assets/graphql-persisted-queries/configuration-properties.png)

## Persistance des requêtes GraphQL à l’aide de l’outil GraphiQL Explorer de création

Dans cette section, conservons la requête GraphQL qui est ensuite utilisée dans l’application cliente pour récupérer et générer les données du fragment de contenu Adventure.

1. Saisissez la requête suivante dans l’explorateur GraphiQL :

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   Vérifiez que la requête fonctionne avant de l’enregistrer.

1. Appuyez ensuite sur Enregistrer sous et saisissez `adventure-details-by-slug` comme nom de la requête.

   ![Persist GraphQL Query](assets/graphql-persisted-queries/persist-graphql-query.png)

## Exécution de la requête persistante avec des variables en encodant des caractères spéciaux

Comprenons comment les requêtes persistantes avec des variables sont exécutées par l’application côté client en encodant les caractères spéciaux.

Pour exécuter une requête persistante, l’application cliente effectue une requête GET en utilisant la syntaxe suivante :

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

Pour exécuter une requête persistante _avec une variable_, la syntaxe ci-dessus change en :

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

Les caractères spéciaux tels que les points-virgules (;), le signe égal (=), les barres obliques (/) et l’espace doivent être convertis pour utiliser le codage UTF-8 correspondant.

En exécutant la fonction `getAllAdventureDetailsBySlug` à partir du terminal de ligne de commande, nous passons en revue ces concepts en action.

1. Ouvrez l’explorateur GraphiQL et cliquez sur le bouton **ellipses** (...) en regard de la requête persistante `getAllAdventureDetailsBySlug`, puis cliquez sur **Copier l’URL**. Collez l’URL copiée dans un bloc de texte, comme suit :

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. Ajouter `yosemite-backpacking` comme valeur de variable

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. Codez les points-virgules (;) et le signe égal (=) les caractères spéciaux

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. Ouvrez un terminal de ligne de commande et utilisez [Curl](https://curl.se/) exécution de la requête

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    Si vous exécutez la requête ci-dessus sur l’environnement de création AEM, vous devez envoyer les informations d’identification. Voir [Jeton d’accès au développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) pour en démontrer l’existence et [Appel de l’API AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) pour plus d’informations sur la documentation.

En outre, passez en revue [Exécution d’une requête persistante](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [Utilisation de variables de requête](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables), et [Encodage de l’URL de requête à utiliser par une application](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) pour découvrir l’exécution de requêtes persistantes par les applications clientes.

## Mise à jour des paramètres de contrôle du cache dans les requêtes persistantes {#cache-control-all-adventures}

L’API GraphQL AEM vous permet de mettre à jour les paramètres de contrôle du cache par défaut vers vos requêtes afin d’améliorer les performances. Les valeurs par défaut de contrôle du cache sont les suivantes :

* 60 secondes est la durée de vie par défaut (maxage=60) du client (par exemple, un navigateur).

* 7 200 secondes est la durée de vie par défaut (s-maxage=7 200) pour Dispatcher et CDN ; également appelés caches partagés

Utilisez la variable `adventures-all` pour mettre à jour les paramètres de contrôle du cache. La réponse de la requête est volumineuse et utile pour contrôler ses `age` dans le cache. Cette requête persistante est utilisée ultérieurement pour mettre à jour la variable [application cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. Ouvrez l’explorateur GraphiQL et cliquez sur le bouton **ellipses** (...) en regard de la requête persistante, puis cliquez sur **En-têtes** pour ouvrir **Configuration du cache** modale.

   ![Option Persist GraphQL Header](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. Dans le **Configuration du cache** modale, mettez à jour le `max-age` valeur d’en-tête à `600 `secondes (10 minutes), puis cliquez sur **Enregistrer**

   ![Persistance de la configuration du cache GraphQL](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


Réviser [Mise en cache de vos requêtes persistantes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) pour plus d’informations sur les paramètres de contrôle du cache par défaut.


## Félicitations !

Félicitations ! Vous avez maintenant appris à conserver les requêtes GraphQL avec des paramètres, à mettre à jour les requêtes persistantes et à utiliser des paramètres de contrôle du cache avec des requêtes persistantes.

## Étapes suivantes

Dans le [chapitre suivant](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md), vous implémenterez les requêtes de requêtes persistantes dans l’application WKND.
