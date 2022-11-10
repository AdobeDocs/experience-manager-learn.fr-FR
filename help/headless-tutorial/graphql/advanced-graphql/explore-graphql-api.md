---
title: Explorez l’API GraphQL AEM - Concepts avancés d’AEM sans affichage - GraphQL
description: Envoyez des requêtes GraphQL à l’aide de l’IDE GraphiQL. Découvrez les requêtes avancées à l’aide de filtres, de variables et de directives. Requête pour les références de fragment et de contenu, y compris les références à partir de champs de texte multiligne.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 0%

---

# Exploration de l’API GraphQL AEM

L’API GraphQL d’AEM vous permet d’exposer les données de fragments de contenu aux applications en aval. Dans le tutoriel de base [tutoriel GraphQL en plusieurs étapes](../multi-step/explore-graphql-api.md), vous avez utilisé l’Explorateur GraphiQL pour tester et affiner les requêtes GraphQL.

Dans ce chapitre, vous utilisez l’Explorateur GraphiQL pour définir des requêtes plus avancées afin de rassembler les données des fragments de contenu que vous avez créés dans la variable [chapitre précédent](../advanced-graphql/author-content-fragments.md).

## Prérequis {#prerequisites}

Ce document fait partie d’un tutoriel en plusieurs parties. Assurez-vous que les chapitres précédents ont été terminés avant de poursuivre ce chapitre.

## Objectifs {#objectives}

Dans ce chapitre, vous apprendrez à :

* Filtrage d’une liste de fragments de contenu avec des références à l’aide de variables de requête
* Filtrage du contenu dans une référence à un fragment
* Requête pour le contenu intégré et les références aux fragments à partir d’un champ de texte multiligne
* Requête utilisant des directives
* Requête pour le type de contenu Objet JSON

## Utilisation de l’explorateur GraphiQL


Le [Explorateur GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) Cet outil permet aux développeurs de créer et de tester des requêtes par rapport au contenu de l’environnement AEM actuel. L’outil GraphiQL permet également aux utilisateurs de **persister ou enregistrer** requêtes à utiliser par les applications clientes dans un paramètre de production.

Ensuite, explorez la puissance de l’API GraphQL AEM à l’aide de l’explorateur GraphiQL intégré.

1. Dans l’écran AEM Démarrer, accédez à **Outils** > **Général** > **Éditeur de requêtes GraphQL**.

   ![Accédez à l’IDE GraphiQL](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>Dans, certaines versions d’AEM (6.X.X) l’outil d’explorateur GraphiQL (ou IDE GraphiQL) doivent être installés manuellement, suivez la section [instruction d’ici](../multi-step/explore-graphql-api.md#install-the-graphiql-tool-optional).

1. Dans le coin supérieur droit, assurez-vous que le point de fin est défini sur **Point de terminaison partagé WKND**. Changement de la variable _Point d’entrée_ cette valeur de liste déroulante affiche la valeur existante _Requêtes persistantes_ dans le coin supérieur gauche.

   ![Définition du point d’entrée GraphQL](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

Cela permettra d’étendre toutes les requêtes aux modèles créés dans la variable **WKND partagé** projet.


## Filtrage d’une liste de fragments de contenu à l’aide de variables de requête

Dans la section précédente [tutoriel GraphQL en plusieurs étapes](../multi-step/explore-graphql-api.md), vous avez défini et utilisé des requêtes persistantes de base pour obtenir des données de fragments de contenu. Ici, vous développez ces connaissances et filtrez les données de fragments de contenu en transmettant des variables aux requêtes persistantes.

Lors du développement d’applications clientes, vous devez généralement filtrer les fragments de contenu en fonction d’arguments dynamiques. L’API GraphQL AEM vous permet de transmettre ces arguments en tant que variables dans une requête afin d’éviter la construction de chaînes du côté client au moment de l’exécution. Pour plus d’informations sur les variables GraphQL, voir [Documentation GraphQL](https://graphql.org/learn/queries/#variables).

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

   Le `listPersonBySkill` inclut la variable `contactInfo` , qui est une référence de fragment au modèle Contact Info défini dans les chapitres précédents. Le modèle Coordonnées contient `phone` et `email` champs. Au moins un de ces champs de la requête doit être présent pour qu’elle s’exécute correctement.

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
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
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

Appuyez sur la touche **Play** dans le menu supérieur pour exécuter la requête. Vous devriez voir les résultats des fragments de contenu du chapitre précédent :

![Personne par résultats de compétences](assets/explore-graphql-api/person-by-skill.png)

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
         title
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
             "title": "Yosemite Backpacking",
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
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
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

   La requête inclut également la variable `_metadata` champ . Vous pouvez ainsi récupérer le nom du fragment de contenu d’équipe et l’afficher ultérieurement dans l’application WKND.

1. Ensuite, collez la chaîne JSON suivante dans le panneau Variables de requête pour obtenir l’aventure Yosemite de décompression :

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
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
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   Le `_references` affiche à la fois l’image du logo et le fragment de contenu Yosemite Valley Log qui a été inséré dans la **Description** champ .


## Requête utilisant des directives

Parfois, lorsque vous développez des applications clientes, vous devez modifier de manière conditionnelle la structure de vos requêtes. Dans ce cas, l’API GraphQL AEM vous permet d’utiliser les directives GraphQL afin de modifier le comportement de vos requêtes en fonction des critères fournis. Pour plus d’informations sur les directives GraphQL, voir [Documentation GraphQL](https://graphql.org/learn/queries/#directives).

Dans le [section précédente](#query-rte-reference), vous avez appris à rechercher des références intégrées dans des champs de texte multiligne. Le contenu a été récupéré à partir du `description` dans le champ `plaintext` format. Ensuite, étendons cette requête et utilisons une directive pour récupérer de manière conditionnelle. `description` dans le `json` format également.

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
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
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
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
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
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
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
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
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
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
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
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
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

   Le `weatherBySeason` contient l’objet JSON ajouté au chapitre précédent.

## Requête pour tout contenu à la fois

Jusqu’à présent, plusieurs requêtes ont été exécutées pour illustrer les fonctionnalités de l’API GraphQL AEM.

Les mêmes données ont pu être récupérées avec une seule requête et cette requête est ensuite utilisée dans l’application cliente pour récupérer des informations supplémentaires telles que l’emplacement, le nom de l’équipe, les membres de l’équipe d’une aventure :

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


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## Félicitations !

Félicitations ! Vous avez maintenant testé des requêtes avancées afin de collecter les données des fragments de contenu que vous avez créés dans le chapitre précédent.

## Étapes suivantes

Dans le [chapitre suivant](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), vous apprendrez comment conserver les requêtes GraphQL et pourquoi il est recommandé d’utiliser les requêtes persistantes dans vos applications.
