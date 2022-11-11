---
title: Ajout de composants modifiables à des itinéraires dynamiques SPA distants
description: Découvrez comment ajouter des composants modifiables aux itinéraires dynamiques dans un SPA distant.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 1%

---

# Itinéraires dynamiques et composants modifiables

Dans ce chapitre, nous autorisons deux itinéraires dynamiques Détails de l’aventure à prendre en charge les composants modifiables ; __Le camp de surf de Bali__ et __Beervana à Portland__.

![Itinéraires dynamiques et composants modifiables](./assets/spa-dynamic-routes/intro.png)

L’itinéraire de l’SPA Détails de l’aventure est défini comme `/adventure/:slug` where `slug` est une propriété d’identifiant unique sur le fragment de contenu aventure.

## Mappage des URL SPA aux pages AEM

Dans les deux chapitres précédents, nous avons mappé le contenu du composant modifiable de la vue SPA accueil à la page racine de SPA distante correspondante dans l’AEM à l’emplacement `/content/wknd-app/us/en/`.

La définition du mappage pour les composants modifiables pour les itinéraires dynamiques SPA est similaire. Toutefois, nous devons établir un schéma de mappage 1:1 entre les instances de l’itinéraire et les pages AEM.

Dans ce tutoriel, nous prenons le nom du fragment de contenu WKND Adventure, qui est le dernier segment du chemin, et nous le mappons à un chemin simple sous `/content/wknd-app/us/en/adventure`.

| Route de SPA distante | AEM chemin de page |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /aventure/__bali-surf-camp__ | /content/wknd-app/us/en/home/aventure/__bali-surf-camp__ |
| /aventure/__beervana-portland__ | /content/wknd-app/us/en/home/aventure/__beervana-in-portland__ |

Ainsi, en fonction de ce mappage, nous devons créer deux nouvelles pages AEM à l’adresse :

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mappage des SPA distantes

Le mappage des requêtes quittant la SPA distante est configuré via le `setupProxy` configuration effectuée dans [Bootstrap de la SPA](./spa-bootstrap.md).

## Mappage de l’éditeur de SPA

Le mappage pour les requêtes SPA lorsque le SPA est ouvert via l’éditeur d’ est configuré via la configuration des mappages Sling effectuée dans [Configuration d’AEM](./aem-configure.md).

## Création de pages de contenu dans AEM

Créez tout d’abord l’intermédiaire `adventure` Segment de page :

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND > us > en > Page d’accueil de l’application WKND__
   + Cette page d’AEM est mappée en tant que racine de l’SPA. C’est donc là que nous commençons à développer la structure de page d’AEM pour d’autres itinéraires de .
1. Appuyer __Créer__ et sélectionnez __Page__
1. Sélectionnez la __Page de SPA distante__ modèle, puis appuyez sur __Suivant__
1. Remplissage des propriétés de page
   + __Titre__: Adventure
   + __Nom__ : `adventure`
      + Cette valeur définit l’URL de la page d’AEM et doit donc correspondre au segment d’itinéraire SPA.
1. Appuyez sur __Terminé__

Créez ensuite les pages d’AEM qui correspondent à chacune des URL SPA nécessitant des zones modifiables.

1. Accédez au __Adventure__ dans Administration de sites
1. Appuyer __Créer__ et sélectionnez __Page__
1. Sélectionnez la __Page de SPA distante__ modèle, puis appuyez sur __Suivant__
1. Remplissage des propriétés de page
   + __Titre__: Le camp de surf de Bali
   + __Nom__ : `bali-surf-camp`
      + Cette valeur définit l’URL de la page d’AEM et doit donc correspondre au dernier segment de l’itinéraire SPA.
1. Appuyez sur __Terminé__
1. Répétez les étapes 3 à 6 pour créer le __Beervana à Portland__ avec :
   + __Titre__: Beervana à Portland
   + __Nom__ : `beervana-in-portland`
      + Cette valeur définit l’URL de la page d’AEM et doit donc correspondre au dernier segment de l’itinéraire SPA.

Ces deux pages AEM contiennent le contenu correspondant pour leurs itinéraires SPA correspondants. Si d’autres itinéraires SPA nécessitent la création, de nouvelles pages AEM doivent être créées à leur URL de base sous la page racine de la page de  distante (`/content/wknd-app/us/en/home`) dans AEM.

## Mise à jour de l’application WKND

Placons le `<ResponsiveGrid...>` du composant créé dans [dernier chapitre](./spa-container-component.md), dans notre `AdventureDetail` SPA, création d’un conteneur modifiable.

### Placez le composant SPA ResponsiveGrid

Placement de la variable `<ResponsiveGrid...>` dans le `AdventureDetail` crée un conteneur modifiable dans cet itinéraire. L’astuce est que plusieurs itinéraires utilisent la variable `AdventureDetail` pour effectuer le rendu, nous devons ajuster dynamiquement la variable  `<ResponsiveGrid...>'s pagePath` attribut. Le `pagePath` doit être dérivé pour pointer vers la page d’AEM correspondante, en fonction de l’aventure affichée par l’instance de l’itinéraire.

1. Ouvrir et modifier `react-app-/src/components/AdventureDetail.js`
1. Importez la variable `ResponsiveGrid` et placez-le au-dessus du composant `<h2>Itinerary</h2>` composant.
1. Définissez les attributs suivants sur la variable `<ResponsiveGrid...>` composant. Notez que `pagePath` ajoute l’attribut `slug` qui correspond à la page aventure selon le mappage défini ci-dessus.
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   Cette instruction indique à la fonction `ResponsiveGrid` pour récupérer son contenu de la ressource AEM :

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`


Mettre à jour `AdventureDetail.js` avec les lignes suivantes :

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

Le `AdventureDetail.js` doit se présenter comme suit :

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Création du conteneur dans AEM

Avec le `<ResponsiveGrid...>` en place et `pagePath` défini dynamiquement en fonction de l’aventure en cours de rendu, nous essayons de créer du contenu dans celui-ci.

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND > us > en__
1. __Modifier__ la valeur __Page d’accueil de l’application WKND__ page
   + Accédez au __Le camp de surf de Bali__ itinéraire dans le SPA pour le modifier
1. Sélectionner __Aperçu__ à partir du sélecteur de mode en haut à droite
1. Appuyez sur le __Le camp de surf de Bali__ de la SPA pour accéder à son itinéraire ;
1. Sélectionner __Modifier__ à partir du sélecteur de mode
1. Recherchez la variable __Conteneur de mises en page__ zone modifiable juste au-dessus de __Itinéraire__
1. Ouvrez le __Barre latérale de l’éditeur de page__, puis sélectionnez la variable __Vue Composants__
1. Faites glisser certains des composants activés dans le __Conteneur de mises en page__
   + Image
   + Texte
   + Titre

   Et créer du matériel marketing promotionnel. Cela pourrait ressembler à ceci :

   ![Création de détails d’aventure à Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Aperçu__ vos modifications dans AEM éditeur de page
1. Actualisez l’application WKND s’exécutant localement sur [http://localhost:3000](http://localhost:3000), accédez au __Le camp de surf de Bali__ itinéraire pour afficher les modifications créées.

   ![Bali SPA distante](./assets/spa-dynamic-routes/remote-spa-final.png)

Lorsque vous accédez à un itinéraire de détails d’aventure qui ne comporte pas de page d’AEM mappée, il n’existe aucune fonctionnalité de création sur cette instance d’itinéraire. Pour activer la création sur ces pages, il vous suffit d’effectuer une page d’AEM avec le nom correspondant sous __Adventure__ page !

## Félicitations !

Félicitations ! Vous avez ajouté la capacité de création aux itinéraires dynamiques dans la SPA !

+ Ajout du composant ResponsiveGrid du composant React Modifiable AEM à un itinéraire dynamique.
+ Création de pages AEM pour la création de deux itinéraires spécifiques dans le SPA (Bali Surf Camp et Beervana à Portland)
+ Contenu créé sur l&#39;itinéraire dynamique Bali Surf Camp !

Vous avez maintenant terminé d’explorer les premières étapes de la manière dont AEM Éditeur SPA peut être utilisé pour ajouter des zones modifiables spécifiques à un  distant !
