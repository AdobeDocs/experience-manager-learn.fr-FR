---
title: Exploration des API GraphQL - Prise en main d’AEM sans affichage - GraphQL
description: Prise en main d’Adobe Experience Manager (AEM) et de GraphQL. Explorez AEM API GraphQL à l’aide de l’IDE GraphQL intégré. Découvrez comment AEM génère automatiquement un schéma GraphQL basé sur un modèle de fragment de contenu. Testez la création de requêtes de base à l’aide de la syntaxe GraphQL.
version: Cloud Service
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
source-git-commit: 410eb23534e083940bf716194576e099d22ca205
workflow-type: tm+mt
source-wordcount: '1532'
ht-degree: 7%

---

# Exploration des API GraphQL {#explore-graphql-apis}

L’API GraphQL d’AEM fournit un langage de requête puissant pour exposer les données de fragments de contenu aux applications en aval. Les modèles de fragment de contenu définissent le schéma de données utilisé par les fragments de contenu. Chaque fois qu’un modèle de fragment de contenu est créé ou mis à jour, le schéma est traduit et ajouté au &quot;graphique&quot; qui constitue l’API GraphQL.

Dans ce chapitre, nous allons explorer certaines requêtes GraphQL courantes pour rassembler du contenu à l’aide d’un IDE appelé [GraphiQL](https://github.com/graphql/graphiql). L’IDE GraphiQL vous permet de tester et d’affiner rapidement les requêtes et les données renvoyées. GraphiQL permet également d’accéder facilement à la documentation, ce qui facilite l’apprentissage et la compréhension des méthodes disponibles.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que les étapes décrites dans la section [Création de fragments de contenu](./author-content-fragments.md) ont été terminées.

## Objectifs {#objectives}

* Découvrez comment utiliser l’outil GraphiQL pour créer une requête à l’aide de la syntaxe GraphQL.
* Découvrez comment interroger une liste de fragments de contenu et un seul fragment de contenu.
* Découvrez comment filtrer et demander des attributs de données spécifiques.
* Découvrez comment joindre une requête de plusieurs modèles de fragments de contenu
* Découvrez comment Persist GraphQL query.

## Activation du point d’entrée GraphQL {#enable-graphql-endpoint}

Un point d’entrée GraphQL doit être configuré pour activer les requêtes d’API GraphQL pour les fragments de contenu.

1. Dans l’écran AEM Démarrer , accédez à **Outils** > **Général** > **GraphQL**.

   ![Accédez au point d’entrée GraphQL](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. Appuyer **Créer** dans le coin supérieur droit. Dans la boîte de dialogue, saisissez les valeurs suivantes :

   * Nom* : **Mon point de terminaison de projet**.
   * Utilisez le schéma GraphQL fourni par ... * : **Mon projet**

   ![Création d’un point d’entrée GraphQL](assets/explore-graphql-api/create-graphql-endpoint.png)

   Appuyer **Créer** pour enregistrer le point de terminaison .

   Les points d’entrée GraphQL créés à partir d’une configuration de projet activent uniquement les requêtes par rapport aux modèles appartenant à ce projet. Dans ce cas, les seules requêtes concernant la variable **Personne** et **Équipe** peuvent être utilisés.

   >[!NOTE]
   >
   > Un point de terminaison global peut également être créé pour activer les requêtes par rapport aux modèles sur plusieurs configurations. Cette méthode doit être utilisée avec précaution, car elle peut ouvrir l’environnement à d’autres vulnérabilités de sécurité et ajouter à la complexité globale de la gestion des AEM.

1. Vous devriez maintenant voir un point d’entrée GraphQL activé dans votre environnement.

   ![Points d’entrée graphql activés](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## Utilisation de l’IDE GraphiQL

Le [Outil GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) permet aux développeurs de créer et de tester des requêtes par rapport au contenu de l’environnement AEM actuel. L’outil GraphiQL permet également aux utilisateurs de **persist** ou enregistrer les requêtes à utiliser par les applications clientes dans un paramètre de production.

Ensuite, explorez la puissance de l’API GraphQL AEM à l’aide de l’IDE GraphiQL intégré.

1. Dans l’écran AEM Démarrer , accédez à **Outils** > **Général** > **Éditeur de requêtes GraphQL**.

   ![Accédez à l’IDE GraphiQL](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > Pour les versions plus anciennes d’AEM, l’IDE GraphiQL peut ne pas être intégré. Il peut être installé manuellement en procédant comme suit : [instructions](#install-graphiql).

1. Dans le coin supérieur droit, assurez-vous que la variable **Point d’entrée** est défini sur **Mon point de terminaison de projet**.

   ![Définition du point d’entrée GraphQL](assets/explore-graphql-api/set-my-project-endpoint.png)

Cela permettra d’étendre toutes les requêtes aux modèles créés dans la variable **Mon projet** projet.

### Requête sur une liste de fragments de contenu {#query-list-cf}

Une exigence courante est de rechercher plusieurs fragments de contenu.

1. Collez la requête suivante dans le panneau principal (en remplaçant la liste des commentaires) :

   ```graphql
   query allTeams {
     teamList {
       items {
         _path
         title
       }
     }
   } 
   ```

1. Appuyez sur la touche **Play** dans le menu supérieur pour exécuter la requête. Vous devriez voir les résultats des fragments de contenu du chapitre précédent :

   ![Résultats de la liste des personnes](assets/explore-graphql-api/all-teams-list.png)

1. Placez le curseur sous le `title` texte et saisie **Ctrl+Espace** pour déclencher des conseils sur le code. Ajouter `shortname` et `description` à la requête.

   ![Mettre à jour la requête avec le masquage du code](assets/explore-graphql-api/update-query-codehinting.png)

1. Exécutez à nouveau la requête en appuyant sur la touche **Play** et vous devriez voir que les résultats incluent les propriétés supplémentaires de `shortname` et `description`.

   ![résultats de nom court et de description](assets/explore-graphql-api/updated-query-shortname-description.png)

   Le `shortname` est une propriété simple et `description` est un champ de texte multiligne et l’API GraphQL nous permet de choisir divers formats pour les résultats, comme `html`, `markdown`, `json` ou `plaintext`.

### Requête pour les fragments imbriqués

Ensuite, essayez de récupérer les fragments imbriqués dans l’instance de requêtage, rappelez-vous que la fonction **Équipe** référence le modèle **Personne** modèle.

1. Mettez à jour la requête pour inclure le `teamMembers` . Rappelez-vous qu’il s’agit d’une **Référence de fragment** au modèle de personne. Les propriétés du modèle Personne peuvent être renvoyées :

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   Réponse JSON :

   ```json
   {
       "data": {
           "teamList": {
           "items": [
               {
               "_path": "/content/dam/my-project/en/team-alpha",
               "title": "Team Alpha",
               "shortName": "team-alpha",
               "description": {
                   "plaintext": "This is a description of Team Alpha!"
               },
               "teamMembers": [
                   {
                   "fullName": "John Doe",
                   "occupation": [
                       "Artist",
                       "Influencer"
                   ]
                   },
                   {
                   "fullName": "Alison Smith",
                   "occupation": [
                       "Photographer"
                   ]
                   }
                 ]
           }
           ]
           }
       }
   }
   ```

   La possibilité d’effectuer des requêtes sur des fragments imbriqués est une puissante de l’API GraphQL AEM. Dans cet exemple simple, l’imbrication n’a que deux niveaux de profondeur. Cependant, il est possible d’imbriquer encore plus les fragments. Par exemple, si une variable **Adresse** modèle associé à un **Personne** il serait possible de renvoyer des données des trois modèles dans une seule requête.

### Filtrage d’une liste de fragments de contenu {#filter-list-cf}

Examinons ensuite comment il est possible de filtrer les résultats en un sous-ensemble de fragments de contenu en fonction d’une valeur de propriété.

1. Saisissez la requête suivante dans l’interface utilisateur GraphiQL :

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{
         _path
         fullName
         occupation
       }
     }
   }  
   ```

   La requête ci-dessus effectue une recherche par rapport à tous les fragments de personne du système. Le filtre ajouté au début de la requête effectue une comparaison sur la variable `name` champ et chaîne de variable `$name`.

1. Dans le **Variables de requête** saisissez les informations suivantes :

   ```json
   {"name": "John Doe"}
   ```

1. Exécuter la requête, il est attendu que **Personnes** est renvoyée avec la valeur &quot;John Doe&quot;.

   ![Utilisation des variables de requête pour le filtrage](assets/explore-graphql-api/using-query-variables-filter.png)

   Il existe de nombreuses autres options pour filtrer et créer des requêtes complexes. Voir [Formation à l’utilisation de GraphQL avec AEM - Exemple de contenu et de requêtes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html?lang=fr).

1. Amélioration de la requête ci-dessus pour récupérer une image de profil

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{  
         _path
         fullName
         occupation
         profilePicture{
           ... on ImageRef{
             _path
             _authorUrl
             _publishUrl
             height
             width
   
           }
         }
       }
     }
   } 
   ```

   Le `profilePicture` est une référence de contenu et il doit s’agir d’une image. Il est donc intégré à `ImageRef` est utilisé. Cela nous permet de demander des données supplémentaires sur l’image à référencer, comme le `width` et `height`.

### Requête sur un seul fragment de contenu {#query-single-cf}

Il est également possible d’interroger directement un seul fragment de contenu. Le contenu d’AEM est stocké de manière hiérarchique et l’identifiant unique d’un fragment est basé sur le chemin du fragment.

1. Saisissez la requête suivante dans l’éditeur GraphiQL :

   ```graphql
   query personByPath($path: String!) {
       personByPath(_path: $path) {
           item {
           fullName
           occupation
           }
       }
   }
   ```

1. Saisissez les informations suivantes pour la variable **Variables de requête**:

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. Exécutez la requête et vérifiez que le seul résultat est renvoyé.

## Persistance des requêtes {#persist-queries}

Une fois qu’un développeur est satisfait de la requête et des données renvoyées, l’étape suivante consiste à stocker ou à conserver la requête à AEM. [Requêtes persistantes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html) est le mécanisme préféré pour exposer l’API GraphQL aux applications clientes. Une fois qu’une requête a été conservée, elle peut être demandée à l’aide d’une requête de GET et mise en cache aux couches Dispatcher et CDN. Les performances des requêtes persistantes sont bien meilleures. Outre les avantages de performances, les requêtes persistantes garantissent que les données supplémentaires ne sont pas exposées accidentellement aux applications clientes. Plus d’informations sur [Les requêtes personnalisées se trouvent ici](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html).

Ensuite, conservez deux requêtes simples ; elles seront utilisées dans le chapitre suivant.

1. Dans l’IDE GraphiQL, saisissez la requête suivante :

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   Vérifiez que la requête fonctionne.

1. Appuyez sur **Enregistrer sous** et saisissez `all-teams` comme la propriété **Nom de la requête**.

   La requête doit maintenant s’afficher sous **Requêtes persistantes** dans le rail de gauche.

   ![Requête persistante de toutes les équipes](assets/explore-graphql-api/all-teams-persisted-query.png)
1. Appuyez ensuite sur les points de suspension **...** en regard de la requête persistante et appuyez sur **Copier l’URL** pour copier le chemin d’accès dans le presse-papiers.

   ![Copier l’URL de requête persistante](assets/explore-graphql-api/copy-persistent-query-url.png)

1. Ouvrez un nouvel onglet et collez le chemin copié dans votre navigateur :

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   Il doit ressembler au chemin ci-dessus. Vous devriez voir les résultats JSON de la requête renvoyée.

   Ventilation de l’URL :

   | Nom | Description |
   | ---------|---------- |
   | `/graphql/execute.json` | Point d’entrée de requête persistant |
   | `/my-project` | Configuration du projet pour `/conf/my-project` |
   | `/all-teams` | Nom de la requête permanente |

1. Revenez à l’IDE GraphiQL et utilisez le bouton plus **+** pour pérenniser la requête NEW

   ```graphql
   query personByName($name: String!) {
     personList(
       filter: {
         fullName:{
           _expressions: [{
             value: $name
             _operator:EQUALS
           }]
         }
       }){
       items {
         _path
         fullName
         occupation
         biographyText {
           json
         }
         profilePicture {
           ... on ImageRef {
             _path
             _authorUrl
             _publishUrl
             width
             height
           }
         }
       }
     }
   }
   ```

1. Enregistrez la requête comme suit : **person-by-name**.
1. 2 requêtes persistantes doivent être enregistrées :

   ![Requêtes conservées finales](assets/explore-graphql-api/final-persisted-queries.png)


## Publier le point d’entrée GraphQL et les requêtes persistantes

Lors de la révision et de la vérification, publiez la variable `GraphQL Endpoint` &amp; `Persisted Queries`

1. Dans l’écran AEM Démarrer , accédez à **Outils** > **Général** > **GraphQL**.

1. Cochez la case en regard de **Mon point de terminaison de projet** et appuyez sur **Publier**

   ![Publier le point d’entrée GraphQL](assets/explore-graphql-api/publish-graphql-endpoint.png)

1. Dans l’écran AEM Démarrer , accédez à **Outils** > **Général** > **Éditeur de requêtes GraphQL**

1. Appuyez sur le bouton *toutes les équipes* dans le panneau Requêtes persistantes et appuyez sur **Publier**

   ![Publier les requêtes persistantes](assets/explore-graphql-api/publish-persisted-query.png)

1. Répétez l’étape ci-dessus pour `person-by-name` query

## Fichiers de solution {#solution-files}

Téléchargez le contenu, les modèles et les requêtes persistantes créés dans les trois derniers chapitres : [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip)

## Ressources supplémentaires

Pour obtenir de nombreux autres exemples de requêtes GraphQL, voir : [Formation à l’utilisation de GraphQL avec AEM - Exemple de contenu et de requêtes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## Félicitations ! {#congratulations}

Félicitations, vous venez de créer et d’exécuter plusieurs requêtes GraphQL !

## Étapes suivantes {#next-steps}

Dans le chapitre suivant, [Créer l’application React](./graphql-and-react-app.md), vous allez découvrir comment une application externe peut interroger AEM points de terminaison GraphQL et exploiter ces deux requêtes persistantes. Vous serez également initié à une gestion des erreurs de base.

## Installation de l’outil GraphiQL (facultatif) {#install-graphiql}

Pour certaines versions d’AEM l’outil IDE GraphiQL doit être installé manuellement. Suivez les instructions ci-dessous pour installer manuellement :

1. Accédez au **[Portail de distribution de logiciels](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Recherchez « GraphiQL » (veillez à inclure le **i** dans **GraphiQL**.
1. Télécharger la dernière version du **Package de contenu GraphiQL v.x.x.x**

   ![Téléchargement du package GraphiQL](assets/explore-graphql-api/software-distribution.png)

   Le fichier zip est un package AEM qui peut être installé directement.

1. Dans le menu **Accueil AEM**, accédez à **Outils** > **Déploiement** > **Packages**.
1. Cliquez sur **Télécharger le package** et sélectionnez le package téléchargé à l’étape précédente. Cliquez sur **Installer** pour installer le package.

   ![Installation du package GraphiQL](assets/explore-graphql-api/install-graphiql-package.png)
