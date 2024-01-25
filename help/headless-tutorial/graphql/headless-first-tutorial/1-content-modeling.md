---
title: Modéliser du contenu - Premier tutoriel AEM Headless
description: Découvrez comment exploiter les fragments de contenu, créer des modèles de fragment et utiliser des points d’entrée GraphQL dans AEM.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 6e5e3cb4-9a47-42af-86af-da33fd80cb47
duration: 225
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 100%

---

# Modéliser du contenu

Bienvenue dans le chapitre du tutoriel sur les fragments de contenu et les points d’entrée GraphQL dans Adobe Experience Manager (AEM). Nous aborderons l’exploitation de fragments de contenu, la création de modèles de fragment et l’utilisation de points d’entrée GraphQL dans AEM.

Les fragments de contenu offrent une approche structurée de la gestion du contenu sur tous les canaux, offrant ainsi flexibilité et réutilisation. L’activation des fragments de contenu dans AEM permet la création de contenu modulaire, ce qui améliore la cohérence et l’adaptabilité.

Tout d’abord, nous vous guiderons tout au long de l’activation des fragments de contenu dans AEM, en abordant les configurations et paramètres nécessaires à une intégration transparente.

Nous allons ensuite aborder la création de modèles de fragment, qui définissent la structure et les attributs. Découvrez comment concevoir des modèles conformes à vos exigences en matière de contenu et les gérer efficacement.

Ensuite, nous allons démontrer la création de fragments de contenu à partir des modèles, en fournissant des conseils détaillés concernant la création et la publication.

En outre, nous allons explorer la définition des points d’entrée GraphQL AEM. GraphQL récupère efficacement les données d’AEM et nous configurerons des points d’entrée pour exposer les données souhaitées. Les requêtes persistantes optimisent les performances et la mise en cache.

Tout au long du tutoriel, nous vous proposons des explications, des exemples de code et des conseils pratiques. À la fin de ce tutoriel, vous disposerez des compétences nécessaires pour activer les fragments de contenu, créer des modèles de fragment, générer des fragments et définir des points d’entrée GraphQL AEM et des requêtes persistantes. Commençons.

## Configuration tenant compte du contexte

1. Accédez à __Outils > Explorateur de configurations__ pour créer une configuration pour l’expérience découplée.

   ![Création d’un dossier.](./assets/1/create-configuration.png)

   Fournissez un __titre__ et un __nom__, et cochez __Requêtes persistantes GraphQL__ et __Modèles de fragment de contenu__.


## Modèles de fragment de contenu

1. Accédez à __Outils > Modèles de fragment de contenu__ et sélectionnez le dossier avec le nom de la configuration créée à l’étape 1.

   ![Dossier de modèle.](./assets/1/model-folder.png)

1. Dans le dossier, sélectionnez __Créer__ et donnez au modèle le nom __Teaser__. Ajoutez les types de données suivants au modèle __Teaser__.

   | Type de données | Nom | Requis | Options |
   |----------|------|----------|---------|
   | Référence de contenu | Ressource | Oui | Ajoutez une image par défaut si vous le souhaitez. Par exemple : /content/dam/wknd-headless/assets/AdobeStock_307513975.mp4. |
   | Une seule ligne de texte | Titre | Oui |
   | Texte monoligne | Pré-titre | Non |
   | Texte multiligne | Description | Non | Assurez-vous que le type par défaut est de texte enrichi. |
   | Énumération | Style | Oui | Effectuez le rendu sous forme de liste déroulante. Les options sont Héros -> Héros et En vedette -> En vedette. |

   ![Modèle Teaser.](./assets/1/teaser-model.png)

1. Dans le dossier, créez un deuxième modèle nommé __Offre__. Cliquez sur Créer et donnez au modèle le nom « Offre » et ajoutez les types de données suivants :

   | Type de données | Nom | Requis | Options |
   |----------|------|----------|---------|
   | Référence de contenu | Ressource | Oui | Ajoutez une image par défaut. Exemple : `/content/dam/wknd-headless/assets/AdobeStock_238607111.jpeg` |
   | Texte multiligne | Description | Non |  |
   | Texte multiligne | Article | Non |  |

   ![Modèle Offre.](./assets/1/offer-model.png)

1. Dans le dossier, créez un troisième modèle nommé __Liste des images__. Cliquez sur Créer et donnez au modèle le nom « Liste des images » et ajoutez les types de données suivants :

   | Type de données | Nom | Requis | Options |
   |----------|------|----------|---------|
   | Référence du fragment | Énumérer les éléments | Oui | Effectuer le rendu sous la forme de champ multiple. Le modèle de fragment de contenu autorisé est Offre. |

   ![Modèle Liste des images.](./assets/1/imagelist-model.png)

## Fragments de contenu

1. Accédez maintenant à Assets et créez un dossier pour le nouveau site. Cliquez sur créer et nommez le dossier.

   ![Ajout d’un dossier.](./assets/1/create-folder.png)

1. Une fois le dossier créé, sélectionnez-le et ouvrez ses __Propriétés__.
1. Dans l’onglet __Configurations cloud__ du dossier, sélectionnez la configuration [créée précédemment](#enable-content-fragments-and-graphql).

   ![Configuration cloud AEM Headless de dossier de ressources.](./assets/1/cloud-config.png)

   Cliquez dans le nouveau dossier et créez un teaser. Cliquez sur __Créer__ et __Fragment de contenu__ et sélectionnez le modèle __Teaser__. Nommez le modèle __Héros__ et cliquez sur __Créer__.

   | Nom | Remarques |
   |----------|------|
   | Ressource | Laissez comme valeur par défaut ou choisissez une autre ressource (vidéo ou image). |
   | Titre | `Explore. Discover. Live.` |
   | Pré-titre | `Join use for your next adventure.` |
   | Description | Laissez vide. |
   | Style | `Hero` |

   ![Fragment héros.](./assets/1/teaser-model.png)

## Points d’entrée GraphQL

1. Accédez à __Outils > GraphQL__.

   ![AEM GraphiQL.](./assets/1/endpoint-nav.png)

1. Cliquez sur __Créer__ et donnez un nom au nouveau point d’entrée, puis choisissez la configuration nouvellement créée.

   ![Point d’entrée GraphQL AEM Headless.](./assets/1/endpoint.png)

## Requêtes persistantes GraphQL

1. Testons le nouveau point d’entrée. Accédez à __Outils > Éditeur de requêtes GraphQL__ et sélectionnez notre point d’entrée dans la liste déroulante en haut à droite de la fenêtre.

1. Dans l’éditeur de requêtes, créez quelques requêtes différentes.


   ```graphql
   {
       teaserList {
           items {
           title
           }
       }
   }
   ```

   Vous devriez obtenir une liste contenant le fragment unique créé [plus haut](#create-content).

   Pour cet exercice, créez une requête complète utilisée par l’application AEM Headless. Créez une requête qui renvoie un seul teaser par chemin d’accès. Dans l’éditeur de requêtes, saisissez la requête suivante :

   ```graphql
   query TeaserByPath($path: String!) {
   component: teaserByPath(_path: $path) {
       item {
       __typename
       _path
       _metadata {
           stringMetadata {
           name
           value
           }
       }
       title
       preTitle
       style
       asset {
           ... on MultimediaRef {
           __typename
           _authorUrl
           _publishUrl
           format
           }
           ... on ImageRef {
           __typename
           _authorUrl
           _publishUrl
           mimeType
           width
           height
           }
       }
       description {
           html
           plaintext
       }
       }
   }
   }
   ```

   Dans l’entrée __Variables de requête__ en bas, saisissez ce qui suit :

   ```json
   {
       "path": "/content/dam/pure-headless/hero"
   }
   ```

   >[!NOTE]
   >
   > Vous devrez peut-être ajuster la variable de requête `path` en fonction des noms des dossiers et des fragments.


   Exécutez la requête pour recevoir les résultats du fragment de contenu créé précédemment.

1. Cliquez sur __Enregistrer__ pour conserver (enregistrer) la requête et donnez-lui le nom __teaser__. Cela nous permet de référencer la requête par nom dans l’application.

## Étapes suivantes

Félicitations. Vous avez correctement configuré AEM as a Cloud Service pour permettre la création de fragments de contenu et de points d’entrée GraphQL. Vous avez également créé un modèle de fragment de contenu et un fragment de contenu, défini un point d’entrée GraphQL et une requête persistante. Vous pouvez maintenant passer au chapitre suivant du tutoriel, où vous apprendrez à créer une application React AEM Headless qui consomme les fragments de contenu et le point d’entrée GraphQL que vous avez créés dans ce chapitre.

[Chapitre suivant : API AEM Headless et React](./2-aem-headless-apis-and-react.md)
