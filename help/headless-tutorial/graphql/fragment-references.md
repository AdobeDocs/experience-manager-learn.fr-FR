---
title: Modélisation avancée des données à l’aide des références de fragments - Prise en main AEM sans en-tête - GraphQL
description: Commencez avec Adobe Experience Manager (AEM) et GraphQL. Découvrez comment utiliser la fonction de référence aux fragments pour la modélisation avancée des données et pour créer une relation entre deux fragments de contenu différents. Découvrez comment modifier une requête GraphQL pour inclure un champ à partir d’un modèle référencé.
sub-product: ressources
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
feature: Fragments de contenu, API GraphQL
topic: Sans tête, Gestion de contenu
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 2%

---


# Modélisation avancée des données à l’aide des références de fragments

Il est possible de référencer un fragment de contenu à partir d’un autre fragment de contenu. Cela permet à un utilisateur de créer des modèles de données complexes avec des relations entre fragments.

Dans ce chapitre, vous allez mettre à jour le modèle Aventure pour inclure une référence au modèle Contributeur à l&#39;aide du champ **Référence du fragment**. Vous apprendrez également à modifier une requête GraphQL pour inclure des champs à partir d’un modèle référencé.

## Conditions préalables

Il s&#39;agit d&#39;un didacticiel en plusieurs parties et on suppose que les étapes décrites dans les parties précédentes ont été terminées.

## Objectifs

Dans ce chapitre, nous apprendrons à :

* Mettre à jour un modèle de fragment de contenu pour utiliser le champ Référence du fragment
* Création d’une requête GraphQL qui renvoie des champs à partir d’un modèle référencé

## Ajouter une référence à un fragment {#add-fragment-reference}

Mettez à jour le modèle de fragment de contenu d&#39;aventure pour ajouter une référence au modèle de contributeur.

1. Ouvrez un nouveau navigateur et accédez à AEM.
1. Dans le menu **AEM Début**, accédez à **Outils** > **Ressources** > **Modèles de fragments de contenu** > **Site WKND**.
1. Ouvrez le modèle de fragment de contenu **Adventure**.

   ![Ouvrir le modèle de fragment de contenu d&#39;aventure](assets/fragment-references/adventure-content-fragment-edit.png)

1. Sous **Data Types**, faites glisser un champ **Fragment Reference** dans le panneau principal.

   ![Champ Ajouter la référence au fragment](assets/fragment-references/add-fragment-reference-field.png)

1. Mettez à jour les **propriétés** de ce champ avec ce qui suit :

   * Afficher comme - `fragmentreference`
   * Étiquette de champ - **Contributeur d&#39;aventure**
   * Nom de la propriété - `adventureContributor`
   * Type de modèle : sélectionnez le modèle **Contributeur**
   * Chemin racine - `/content/dam/wknd`

   ![Propriétés de référence au fragment](assets/fragment-references/fragment-reference-properties.png)

   Le nom de propriété `adventureContributor` peut désormais être utilisé pour référencer un fragment de contenu contributeur.

1. Enregistrez les modifications apportées au modèle.

## Affecter un contributeur à une aventure

Maintenant que le modèle Fragment de contenu d&#39;aventure a été mis à jour, nous pouvons modifier un fragment existant et référencer un Contributeur. Notez que la modification du modèle Fragment de contenu *affecte* tout fragment de contenu existant créé à partir de celui-ci.

1. Accédez à **Assets** > **Files** > **WKND Site** > **English** > **Aventures** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Dossier Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Cliquez dans le fragment de contenu **Bali Surf Camp** pour ouvrir l’éditeur de fragments de contenu.
1. Mettez à jour le champ **Contributeur d&#39;aventure** et sélectionnez un contributeur en cliquant sur l&#39;icône du dossier.

   ![Sélectionner Stacey Roswells comme contributeur](assets/fragment-references/stacey-roswell-contributor.png)

   *Sélectionner un chemin d’accès à un fragment Contributeur*

   ![chemin d&#39;accès renseigné au contributeur](assets/fragment-references/populated-path.png)

   Notez que seuls les fragments créés à l&#39;aide du modèle **Contributeur** peuvent être sélectionnés.

1. Enregistrez les modifications apportées au fragment.

1. Répétez les étapes ci-dessus pour affecter un contributeur à des aventures telles que [Yosemite Backpackers](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) et [Colorado Rock Climbing](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)

## Requête d’un fragment de contenu imbriqué avec GraphiQL

Ensuite, effectuez une requête pour une aventure et ajoutez les propriétés imbriquées du modèle Contributeur référencé. Nous utiliserons l&#39;outil GraphiQL pour vérifier rapidement la syntaxe de la requête.

1. Accédez à l&#39;outil GraphiQL dans AEM : [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. Saisissez la requête suivante :

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   La requête ci-dessus est pour une seule Aventure par son chemin. La propriété `adventureContributor` fait référence au modèle Contributeur et nous pouvons ensuite demander des propriétés au fragment de contenu imbriqué.

1. Exécutez la requête et vous devez obtenir le résultat suivant :

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. Testez d’autres requêtes telles que `adventureList` et ajoutez des propriétés pour le fragment de contenu référencé sous `adventureContributor`.

## Mettre à jour l&#39;application Réagir pour afficher le contenu du collaborateur

Ensuite, mettez à jour les requêtes utilisées par la demande Réagir pour inclure le nouveau cotisant et afficher des informations sur le cotisant dans le cadre de la vue de détails sur l&#39;avance.

1. Ouvrez l’application WKND GraphQL React dans votre IDE.

1. Ouvrez le fichier `src/components/AdventureDetail.js`.

   ![Composant Détail d&#39;aventure IDE](assets/fragment-references/adventure-detail-ide.png)

1. Recherchez la fonction `adventureDetailQuery(_path)`. La fonction `adventureDetailQuery(..)` encapsule simplement une requête GraphQL de filtrage, qui utilise la syntaxe AEM `<modelName>ByPath` pour requête un fragment de contenu unique identifié par son chemin JCR.

1. Mettez à jour la requête pour inclure des informations sur le Contributeur référencé :

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
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
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   Avec cette mise à jour, d&#39;autres propriétés concernant `adventureContributor`, `fullName`, `occupation` et `pictureReference` seront incluses dans la requête.

1. Inspect le composant `Contributor` incorporé dans le fichier `AdventureDetail.js` à `function Contributor(...)`. Ce composant rendra le nom, la profession et l&#39;image du contributeur si les propriétés existent.

   Le composant `Contributor` est référencé dans la méthode `AdventureDetail(...)` `return` :

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. Enregistrez les modifications dans le fichier.
1. Début de l’application Réagir, si elle n’est pas déjà en cours d’exécution :

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Accédez à [http://localhost:3000](http://localhost:3000/) et cliquez sur une aventure qui comporte un contributeur référencé. Vous devriez maintenant voir les informations du Contributeur répertoriées sous l&#39;**Itinéraire** :

   ![Contributeur Ajouté dans l’application](assets/fragment-references/contributor-added-detail.png)

## Félicitations!{#congratulations}

Félicitations ! Vous avez mis à jour un modèle de fragment de contenu existant afin de référencer un fragment de contenu imbriqué à l’aide du champ **Référence du fragment**. Vous avez également appris à modifier une requête GraphQL pour inclure des champs à partir d’un modèle référencé.

## Étapes suivantes {#next-steps}

Dans le chapitre suivant, [Déploiement de production à l’aide d’un environnement de publication AEM](./production-deployment.md), découvrez les services d’auteur et de publication AEM et le modèle de déploiement recommandé pour les applications sans en-tête. Vous allez mettre à jour une application existante afin d’utiliser des variables d’environnement pour modifier dynamiquement un point de terminaison GraphQL en fonction de l’environnement de cible. Vous apprendrez également comment configurer correctement les AEM pour le partage de ressources entre Origines (CORS).
