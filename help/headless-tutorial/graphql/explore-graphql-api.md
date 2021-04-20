---
title: Explorez les API GraphQL - Prise en main sans AEM en-tête - GraphQL
description: Commencez avec Adobe Experience Manager (AEM) et GraphQL. Explorez AEM API GraphQL à l'aide de l'IDE intégré GrapiQL. Découvrez comment AEM génère automatiquement un schéma GraphQL basé sur un modèle de fragment de contenu. Testez la construction de requêtes de base en utilisant la syntaxe GraphQL.
sub-product: ressources
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 1%

---


# Explorez les API GraphQL {#explore-graphql-apis}

L’API GraphQL de AEM fournit un langage de requête puissant pour exposer les données de fragments de contenu aux applications en aval. Les modèles de fragments de contenu définissent le schéma de données utilisé par les fragments de contenu. Chaque fois qu’un modèle de fragment de contenu est créé ou mis à jour, le schéma est traduit et ajouté au &quot;graphique&quot; qui constitue l’API GraphQL.

Dans ce chapitre, nous allons explorer certaines requêtes GraphQL courantes pour rassembler du contenu à l&#39;aide d&#39;un IDE intitulé [GraphiQL](https://github.com/graphql/graphiql). L&#39;IDE GraphiQL vous permet de tester et d&#39;affiner rapidement les requêtes et données renvoyées. GraphiQL permet également d&#39;accéder facilement à la documentation, ce qui permet d&#39;apprendre et de comprendre les méthodes disponibles.

## Conditions préalables {#prerequisites}

Il s’agit d’un didacticiel en plusieurs parties et on suppose que les étapes décrites dans la section [Création de fragments de contenu](./author-content-fragments.md) ont été terminées.

## Objectifs {#objectives}

* Découvrez comment utiliser l&#39;outil GraphiQL pour construire une requête à l&#39;aide de la syntaxe GraphQL.
* Découvrez comment requête d’une liste de fragments de contenu et d’un seul fragment de contenu.
* Découvrez comment filtrer et demander des attributs de données spécifiques.
* Découvrez comment requête d’une variante d’un fragment de contenu.
* Découvrez comment rejoindre une requête de plusieurs modèles de fragments de contenu

## Installer l&#39;outil GraphiQL {#install-graphiql}

L&#39;IDE GraphiQL est un outil de développement qui n&#39;est nécessaire que sur les environnements de niveau inférieur comme un développement ou une instance locale. Par conséquent, il n&#39;est pas inclus dans le projet AEM, mais il s&#39;agit d&#39;un package distinct qui peut être installé sur une base ad hoc.

1. Accédez au **[Portail de distribution de logiciels](https://experience.adobe.com/#/downloads/content/software-distribution/fr-FR/aemcloud.html)** > **AEM en tant que Cloud Service**.
1. Recherchez &quot;GraphiQL&quot; (veillez à inclure **i** dans **GraphiQL**.
1. Téléchargez le dernier **package de contenu GraphiQL v.x.x.x**

   ![Téléchargement du package GraphiQL](assets/explore-graphql-api/software-distribution.png)

   Le fichier zip est un package AEM qui peut être installé directement.

1. Dans le menu **AEM Début**, accédez à **Outils** > **Déploiement** > **Packages**.
1. Cliquez sur **Télécharger le package** et choisissez le package téléchargé à l’étape précédente. Cliquez sur **Installer** pour installer le package.

   ![Installation du package GraphiQL](assets/explore-graphql-api/install-graphiql-package.png)

## Requête d’une liste de fragments de contenu {#query-list-cf}

Une exigence commune sera de requête pour plusieurs fragments de contenu.

1. Accédez à l’IDE GraphiQL à l’adresse [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html).
1. Collez la requête suivante dans le panneau de gauche (sous la liste des commentaires) :

   ```graphql
   {
     contributorList {
       items {
           _path
         }
     }
   }
   ```

1. Appuyez sur le bouton **Lecture** dans le menu supérieur pour exécuter la requête. Vous devriez voir les résultats des fragments de contenu des contributeurs du chapitre précédent :

   ![Résultats de la Liste des contributeurs](assets/explore-graphql-api/contributorlist-results.png)

1. Positionnez le curseur sous le texte `_path` et entrez **CTRL+Space** pour déclencher des conseils de code. Ajoutez `fullName` et `occupation` à la requête.

   ![Mettre à jour la Requête avec la sélection de code](assets/explore-graphql-api/update-query-codehinting.png)

1. Exécutez de nouveau la requête en appuyant sur le bouton **Lecture** et vous devriez voir les résultats inclure les propriétés supplémentaires de `fullName` et `occupation`.

   ![Nom complet et résultats de la profession](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` et  `occupation` sont des propriétés simples. Rappelez-vous du chapitre [Définir des modèles de fragments de contenu](./content-fragment-models.md) que `fullName` et `occupation` sont les valeurs utilisées lors de la définition du **Nom de propriété** des champs respectifs.

1. `pictureReference` et  `biographyText` représentent des champs plus complexes. Mettez à jour la requête avec ce qui suit pour renvoyer des données sur les champs `pictureReference` et `biographyText`.

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biographyText {
           html
         }
         pictureReference {
           ... on ImageRef {
               _path
               width
               height
               }
           }
       }
     }
   }
   ```

   `biographyText` est un champ de texte multiligne et l&#39;API GraphQL nous permet de choisir divers formats pour les résultats, par exemple  `html`,  `markdown`ou  `json`  `plaintext`.

   `pictureReference` est une référence de contenu et est censée être une image. Par conséquent, un  `ImageRef` objet intégré est utilisé. Cela nous permet de demander des données supplémentaires sur l&#39;image en tant que référence, comme `width` et `height`.

1. Ensuite, essayez de demander une liste de **Aventures**. Exécutez la requête suivante :

   ```graphql
   {
     adventureList {
       items {
         adventureTitle
         adventureType
         adventurePrimaryImage {
           ...on ImageRef {
             _path
             mimeType
           }
         }
       }
     }
   }
   ```

   Vous devriez voir une liste de **Aventures** renvoyée. N&#39;hésitez pas à expérimenter en ajoutant des champs supplémentaires à la requête.

## Filtrer une Liste de fragments de contenu {#filter-list-cf}

Examinons ensuite comment il est possible de filtrer les résultats dans un sous-ensemble de fragments de contenu en fonction d’une valeur de propriété.

1. Saisissez la requête suivante dans l’interface utilisateur GraphiQL :

   ```graphql
   {
   contributorList(filter: {
     occupation: {
       _expressions: {
         value: "Photographer"
         }
       }
     }) {
       items {
         _path
         fullName
         occupation
       }
     }
   }
   ```

   La requête ci-dessus effectue une recherche sur tous les contributeurs du système. Le filtre ajouté au début de la requête effectue une comparaison sur le champ `occupation` et la chaîne &quot;**Photographe**&quot;.

1. Exécutez la requête, il est prévu qu&#39;un seul **contributeur** soit renvoyé.
1. Entrez la requête suivante pour requête une liste de **Aventures** où `adventureActivity` est **non** égale à **&quot;Surfing&quot;** :

   ```graphql
   {
     adventureList(filter: {
       adventureActivity: {
           _expressions: {
               _operator: EQUALS_NOT
               value: "Surfing"
           }
       }
   }) {
       items {
       _path
       adventureTitle
       adventureActivity
       }
     }
   }
   ```

1. Exécutez la requête et vérifiez les résultats. Notez qu’aucun des résultats n’inclut un `adventureType` égal à **&quot;Surfing&quot;**.

Il existe de nombreuses autres options de filtrage et de création de requêtes complexes, ci-dessus ne sont que quelques exemples.

## Requête d’un fragment de contenu unique {#query-single-cf}

Il est également possible de requête directement d’un seul fragment de contenu. Le contenu de l’AEM est stocké de manière hiérarchique et l’identifiant unique d’un fragment est basé sur le chemin d’accès du fragment. Si l’objectif est de renvoyer des données sur un fragment unique, il est préférable d’utiliser directement le chemin d’accès et la requête du modèle. L’utilisation de cette syntaxe signifie que la complexité de la requête sera très faible et que le résultat sera plus rapide.

1. Saisissez la requête suivante dans l’éditeur GraphiQL :

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

1. Exécutez la requête et observez que le résultat unique du fragment **Stacey Roswells** est renvoyé.

   Lors de l’exercice précédent, vous avez utilisé un filtre pour restreindre une liste de résultats. Vous pouvez utiliser une syntaxe similaire pour filtrer par chemin d’accès, mais la syntaxe ci-dessus est préférable pour des raisons de performances.

1. Rappelez-vous dans le chapitre [Création de fragments de contenu](./author-content-fragments.md) qu’une variante **Résumé** a été créée pour **Stacey Roswells**. Mettez à jour la requête pour renvoyer la variation **Résumé** :

   ```graphql
   {
   contributorByPath
   (
       _path: "/content/dam/wknd/en/contributors/stacey-roswells"
       variation: "summary"
   ) {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

   Bien que la variation ait été nommée **Résumé**, les variations sont conservées en minuscules et, par conséquent, `summary` est utilisé.

1. Exécutez la requête et observez que le champ `biography` contient un résultat beaucoup plus court `html`.

## Requête pour plusieurs modèles de fragments de contenu {#query-multiple-models}

Il est également possible de combiner des requêtes distinctes en une seule requête. Cela s’avère utile pour réduire au minimum le nombre de requêtes HTTP nécessaires à l’alimentation de l’application. Par exemple, la vue *Accueil* d&#39;une application peut afficher du contenu basé sur **deux** différents modèles de fragment de contenu. Plutôt que d&#39;exécuter **deux requêtes distinctes**, nous pouvons combiner les requêtes en une seule requête.

1. Saisissez la requête suivante dans l’éditeur GraphiQL :

   ```graphql
   {
     adventureList {
       items {
         _path
         adventureTitle
       }
     }
     contributorList {
       items {
         _path
         fullName
       }
     }
   }
   ```

1. Exécutez la requête et observez que le jeu de résultats contient les données de **Adventures** et **Contributeurs** :

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
          "adventureTitle": "Bali Surf Camp"
        },
        {
          "_path": "/content/dam/wknd/en/adventures/beervana-portland/beervana-in-portland",
          "adventureTitle": "Beervana in Portland"
        },
        ...
      ]
    },
    "contributorList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/contributors/jacob-wester",
          "fullName": "Jacob Wester"
        },
        {
          "_path": "/content/dam/wknd/en/contributors/stacey-roswells",
          "fullName": "Stacey Roswells"
        }
      ]
    }
  }
}
```

## Ressources supplémentaires

Pour d&#39;autres exemples de requêtes GraphQL, voir : [Apprendre à utiliser GraphQL avec AEM - Exemple de contenu et de Requêtes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## Félicitations! {#congratulations}

Félicitations, vous venez de créer et d&#39;exécuter plusieurs requêtes GraphQL !

## Étapes suivantes {#next-steps}

Dans le chapitre suivant, [Recherche d&#39;AEM à partir d&#39;une application React](./graphql-and-external-app.md), vous allez découvrir comment une application externe peut requête AEM points de terminaison GraphQL. Application externe modifiant l’exemple d’application WKND GraphQL React afin d’ajouter des requêtes GraphQL de filtrage, ce qui permet à l’utilisateur de l’application de filtrer les aventures par activité. Vous serez également initié à la gestion des erreurs de base.
