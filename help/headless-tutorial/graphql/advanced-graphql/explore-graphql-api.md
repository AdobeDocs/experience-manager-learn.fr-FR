---
title: Explorez l’API GraphQL AEM - Concepts avancés d’AEM sans affichage - GraphQL
description: Envoyez des requêtes GraphQL à l’aide de l’IDE GraphiQL. Découvrez les requêtes avancées à l’aide de filtres, de variables et de directives. Requête pour les références de fragment et de contenu, y compris les références à partir de champs de texte multiligne.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 0%

---

# Exploration de l’API GraphQL AEM

L’API GraphQL d’AEM vous permet d’exposer les données de fragments de contenu aux applications en aval. Dans la section précédente [tutoriel GraphQL en plusieurs étapes](../multi-step/explore-graphql-api.md), vous avez exploré l’environnement de développement intégré (IDE) GraphiQL dans lequel vous avez testé et affiné certaines requêtes GraphQL courantes. Dans ce chapitre, vous allez utiliser l’IDE GraphiQL pour explorer des requêtes plus avancées afin de rassembler les données des fragments de contenu que vous avez créés dans le chapitre précédent.

## Prérequis {#prerequisites}

Ce document fait partie d’un tutoriel en plusieurs parties. Assurez-vous que les chapitres précédents ont été terminés avant de poursuivre ce chapitre.

L’IDE GraphiQL doit être installé avant de terminer ce chapitre. Suivez les instructions d’installation de la section précédente. [tutoriel GraphQL en plusieurs étapes](../multi-step/explore-graphql-api.md) pour plus d’informations.

## Objectifs {#objectives}

Dans ce chapitre, vous apprendrez à :

* Filtrage d’une liste de fragments de contenu avec des références à l’aide de variables de requête
* Filtrage du contenu dans une référence à un fragment
* Requête pour le contenu intégré et les références aux fragments à partir d’un champ de texte multiligne
* Requête utilisant des directives
* Requête pour le type de contenu Objet JSON

## Filtrage d’une liste de fragments de contenu à l’aide de variables de requête

Dans la section précédente [tutoriel GraphQL en plusieurs étapes](../multi-step/explore-graphql-api.md), vous avez appris à filtrer une liste de fragments de contenu. Vous allez développer ces connaissances et les filtrer à l’aide de variables.

Dans la plupart des cas, lorsque vous développez des applications clientes, vous devez filtrer les fragments de contenu en fonction d’arguments dynamiques. L’API GraphQL AEM vous permet de transmettre ces arguments en tant que variables dans une requête afin d’éviter la construction de chaînes du côté client au moment de l’exécution. Pour plus d’informations sur les variables GraphQL, voir [Documentation GraphQL](https://graphql.org/learn/queries/#variables).

Pour cet exemple, interrogez tous les instructeurs ayant une compétence particulière.

1. Dans l’IDE GraphiQL, collez la requête suivante dans le panneau de gauche :

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
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
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   Le `listPersonBySkill` La requête ci-dessus accepte une variable (`skillFilter`) qui est obligatoire `String`. Cette requête effectue une recherche par rapport à tous les fragments de contenu de personne et les filtre en fonction de la variable `skills` et la chaîne transmise `skillFilter`.

   Notez que `listPersonBySkill` inclut la variable `contactInfo` , qui est une référence de fragment au modèle Contact Info défini dans les chapitres précédents. Le modèle Coordonnées contient `phone` et `email` champs. Vous devez inclure au moins un de ces champs dans la requête pour qu’elle s’exécute correctement.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. Ensuite, définissons `skillFilter` et obtenir tous les instructeurs qui maîtrisent le ski. Collez la chaîne JSON suivante dans le panneau Variables de requête de l’IDE GraphiQL :

   ```json
   {
   	    "skillFilter": "Skiing"
   }
   ```

1. Exécutez la requête. Le résultat doit ressembler à ce qui suit :

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer.\nBorn in Baltimore, Maryland, Stacey is the youngest of six children. Her father was a lieutenant colonel in the US Navy and her mother was a modern dance instructor. Her family moved frequently with her father’s duty assignments, and she took her first pictures when he was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

## Filtrage du contenu dans une référence à un fragment

L’API GraphQL AEM vous permet d’interroger des fragments de contenu imbriqués. Dans le chapitre précédent, vous avez ajouté trois nouvelles références à un fragment de contenu aventure : `location`, `instructorTeam`, et `administrator`. Désormais, filtrons toutes les aventures pour tout administrateur portant un nom particulier.

>[!CAUTION]
>
>Un seul modèle doit être autorisé comme référence pour que cette requête s’exécute correctement.

1. Dans l’IDE GraphiQL, collez la requête suivante dans le panneau de gauche :

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         adventureTitle
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. Ensuite, collez la chaîne JSON suivante dans le panneau Variables de requête :

   ```json
   {
   	    "name": "Jacob Wester"
   }
   ```

   Le `getAdventureAdministratorDetailsByAdministratorName` requête filtre toutes les aventures pour n’importe quelle `administrator` de `fullName` &quot;Jacob Wester&quot;, renvoyant des informations provenant de deux fragments de contenu imbriqués : L&#39;aventure et l&#39;instructeur.

1. Exécutez la requête. Le résultat doit ressembler à ce qui suit :

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "adventureTitle": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for 3 years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## Requête pour les références intégrées à partir d’un champ de texte multiligne {#query-rte-reference}

L’API GraphQL AEM vous permet de rechercher du contenu et des références à des fragments dans des champs de texte multiligne. Dans le chapitre précédent, vous avez ajouté les deux types de référence dans la variable **Description** champ du fragment de contenu de l’équipe Yosemite. Maintenant, récupérons ces références.

1. Dans l’IDE GraphiQL, collez la requête suivante dans le panneau de gauche :

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   Le `getTeamByAdventurePath` la requête filtre toutes les aventures par chemin et renvoie des données pour la variable `instructorTeam` référence de fragment d’une aventure spécifique.

   `_references` est un champ généré par le système utilisé pour afficher les références, y compris celles insérées dans des champs de texte multiligne.

   Le `getTeamByAdventurePath` requête récupère plusieurs références. Tout d’abord, il utilise le `ImageRef` pour récupérer l’objet `_path` et `__typename` des images insérées en tant que références de contenu dans le champ de texte multiligne. Ensuite, il utilise `LocationModel` pour récupérer les données du fragment de contenu d’emplacement inséré dans le même champ.

   Notez que la requête inclut également la variable `_metadata` champ . Vous pouvez ainsi récupérer le nom du fragment de contenu d’équipe et l’afficher ultérieurement dans l’application WKND.

1. Ensuite, collez la chaîne JSON suivante dans le panneau Variables de requête pour obtenir l’aventure Yosemite de décompression :

   ```json
   {
   	    "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Exécutez la requête. Le résultat doit ressembler à ce qui suit :

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   Notez que la variable `_references` affiche à la fois l’image du logo et le fragment de contenu Yosemite Valley Log qui a été inséré dans la **Description** champ .


## Requête utilisant des directives

Parfois, lorsque vous développez des applications clientes, vous devez modifier de manière conditionnelle la structure de vos requêtes. Dans ce cas, l’API GraphQL AEM vous permet d’utiliser les directives GraphQL afin de modifier le comportement de vos requêtes en fonction des critères fournis. Pour plus d’informations sur les directives GraphQL, voir [Documentation GraphQL](https://graphql.org/learn/queries/#directives).

Dans le [section précédente](#query-rte-reference), vous avez appris à rechercher des références intégrées dans des champs de texte multiligne. Notez que le contenu a été récupéré à partir du `description` dans le champ `plaintext` format. Ensuite, étendons cette requête et utilisons une directive pour récupérer de manière conditionnelle. `description` dans le `json` format également.

1. Dans l’IDE GraphiQL, collez la requête suivante dans le panneau de gauche :

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   La requête ci-dessus accepte une variable supplémentaire (`includeJson`) qui est obligatoire `Boolean`, également appelée directive de la requête. Une directive peut être utilisée pour inclure de manière conditionnelle des données provenant de la variable `description` dans le champ `json` format basé sur la valeur booléenne transmise `includeJson`.

1. Ensuite, collez la chaîne JSON suivante dans le panneau Variables de requête :

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Exécutez la requête. Vous devriez obtenir le même résultat que dans la section précédente sur [Requête pour des références intégrées dans des champs de texte multiligne](#query-rte-reference).

1. Mettez à jour le `includeJson` de `true` et exécuter à nouveau la requête. Le résultat doit ressembler à ce qui suit :

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Requête pour le type de contenu Objet JSON

N’oubliez pas que dans le chapitre précédent sur la création de fragments de contenu, vous avez ajouté un objet JSON à la variable **La météo par saison** champ . Maintenant, récupérons ces données dans le fragment de contenu de l’emplacement.

1. Dans l’IDE GraphiQL, collez la requête suivante dans le panneau de gauche :

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
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
     }
   }
   ```

1. Ensuite, collez la chaîne JSON suivante dans le panneau Variables de requête :

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Exécutez la requête. Le résultat doit ressembler à ce qui suit :

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California’s Sierra Nevada mountains. It’s famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   Notez que la variable `weatherBySeason` contient l’objet JSON ajouté au chapitre précédent.

## Requête pour tout contenu à la fois

Jusqu’à présent, plusieurs requêtes ont été exécutées pour illustrer les fonctionnalités de l’API GraphQL AEM. Les mêmes données peuvent être récupérées avec une seule requête :

```graphql
query getAllAdventureDetails($fragmentPath: String!) {
  adventureByPath(_path: $fragmentPath){
    item {
      _path
      adventureTitle
      adventureActivity
      adventureType
      adventurePrice
      adventureTripLength
      adventureGroupSize
      adventureDifficulty
      adventurePrice
      adventurePrimaryImage{
        ...on ImageRef{
          _path
          mimeType
          width
          height
        }
      }
      adventureDescription {
        html
        json
      }
      adventureItinerary {
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
        contactInfo{
          phone
          email
        }
        locationImage{
          ...on ImageRef{
            _path
          }
        }
        weatherBySeason
        address{
            streetAddress
            city
            state
            zipCode
            country
        }
      }
      instructorTeam {
        _metadata{
            stringMetadata{
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
            profilePicture{
                ...on ImageRef {
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
        ...on ImageRef {
            _path
        mimeType
        }
        ...on LocationModel {
            _path
                __typename
        }
    }
  }
}

# in Query Variables
{
  "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
}
```

## Requêtes supplémentaires pour l’application WKND

Les requêtes suivantes sont répertoriées pour récupérer toutes les données nécessaires dans l’application WKND. Ces requêtes ne présentent aucun nouveau concept et ne sont fournies qu’en tant que référence pour vous aider à créer votre mise en oeuvre.

1. **Obtenir des membres de l’équipe pour une aventure spécifique**:

   ```graphql
   query getTeamMembersByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath ) {
       item {
         instructorTeam {
           teamMembers{
             fullName
             contactInfo{
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
             biography{
               plaintext
             }
           }
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **Chemin d’accès à l’emplacement pour une aventure spécifique**

   ```graphql
   query getLocationPathByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath){
       item {
         location{
           _path  
         } 
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **Obtention de l’emplacement de l’équipe en fonction de son chemin**

   ```graphql
   query getTeamLocationByLocationPath ($fragmentPath: String!){
     locationByPath (_path: $fragmentPath) {
       item {
         name
         description{
           json
         }
         contactInfo{
           phone
           email
         }
           address{
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge"
   }
   ```

## Félicitations !

Félicitations ! Vous avez maintenant testé des requêtes avancées afin de collecter les données des fragments de contenu que vous avez créés dans le chapitre précédent.

## Étapes suivantes

Dans le [chapitre suivant](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), vous apprendrez comment conserver les requêtes GraphQL et pourquoi il est recommandé d’utiliser les requêtes persistantes dans vos applications.