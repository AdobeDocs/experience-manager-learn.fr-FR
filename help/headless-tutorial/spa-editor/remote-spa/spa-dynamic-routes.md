---
title: Ajouter des composants modifiables à des itinéraires dynamiques de SPA distante
description: Découvrez comment ajouter des composants modifiables aux itinéraires dynamiques dans une SPA distante.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 247
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 100%

---

# Itinéraires dynamiques et composants modifiables

Dans ce chapitre, nous autorisons deux itinéraires dynamiques de détails de l’Adventure à prendre en charge les composants modifiables : __Bali Surf Camp__ et __Beervana in Portland__.

![Itinéraires dynamiques et composants modifiables.](./assets/spa-dynamic-routes/intro.png)

L’itinéraire de la SPA des détails de l’Adventure est défini sur `/adventure/:slug` où `slug` est une propriété d’identifiant unique sur le fragment de contenu d’Adventure.

## Mapper les URL de la SPA aux pages AEM

Dans les deux chapitres précédents, nous avons mappé le contenu de composant modifiable de l’affichage d’accueil de la SPA à la page racine de la SPA distante correspondante dans AEM sous `/content/wknd-app/us/en/`.

La définition du mappage pour les composants modifiables pour les itinéraires dynamiques de la SPA est similaire. Toutefois, nous devons établir un schéma de mappage 1:1 entre les instances de l’itinéraire et les pages AEM.

Dans ce tutoriel, nous prenons le nom du fragment de contenu WKND Adventure, qui est le dernier segment du chemin d’accès, et nous le mappons à un chemin d’accès simple sous `/content/wknd-app/us/en/adventure`.

| Itinéraire de SPA distante | Chemin d’accès à la page AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure /__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

Ainsi, en fonction de ce mappage, nous devons créer deux nouvelles pages AEM dans :

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mappage de SPA distante

Le mappage des requêtes quittant la SPA distante est configuré via la configuration `setupProxy` effectuée dans [Amorcer la SPA](./spa-bootstrap.md).

## Mappage de l’éditeur de SPA

Le mappage pour les requêtes de SPA lorsque la SPA est ouverte via l’éditeur de SPA d’AEM est configuré via la configuration des mappages Sling effectuée dans [Configuration d’AEM](./aem-configure.md).

## Créer des pages de contenu dans AEM

Créez tout d’abord le segment de page `adventure` intermédiaire :

1. Connectez-vous au service de création AEM.
1. Accédez à __Sites > WKND App > us > en > WKND App Home Page__.
   + Cette page d’AEM étant mappée en tant que racine de la SPA, c’est là que nous commençons à créer la structure de page d’AEM pour d’autres itinéraires de SPA.
1. Appuyez sur __Créer__ puis sélectionnez __Page__.
1. Sélectionnez le modèle __Page de SPA distante__, puis appuyez sur __Suivant__.
1. Remplissez les propriétés de page
   + __Titre__ : Adventure
   + __Nom__ : `adventure`
      + Cette valeur définit l’URL de la page d’AEM et doit donc correspondre au segment d’itinéraire de SPA.
1. Appuyez sur __Terminé__.

Créez ensuite les pages d’AEM qui correspondent à chacune des URL de SPA nécessitant des domaines modifiables.

1. Accédez à la nouvelle page __Adventure__ dans l’administration du site.
1. Sélectionnez __Créer__ puis sélectionnez __Page__.
1. Sélectionnez le modèle __Page de SPA distante__, puis appuyez sur __Suivant__.
1. Remplissez les propriétés de page
   + __Titre__ : Bali Surf Camp
   + __Nom__ : `bali-surf-camp`
      + Cette valeur définit l’URL de la page AEM et doit donc correspondre au dernier segment de l’itinéraire de SPA.
1. Appuyez sur __Terminé__.
1. Répétez les étapes 3 à 6 pour créer la page __Beervana in Portland__ avec :
   + __Titre__ : Beervana in Portland
   + __Nom__ : `beervana-in-portland`
      + Cette valeur définit l’URL de la page AEM et doit donc correspondre au dernier segment de l’itinéraire de SPA.

Ces deux pages AEM contiennent le contenu respectif créé pour leurs itinéraires de SPA correspondants. Si d’autres itinéraires de SPA nécessitent de la création, de nouvelles pages AEM doivent être créées à leur URL de SPA sous la page racine de la page de SPA distante (`/content/wknd-app/us/en/home`) dans AEM.

## Mettre à jour l’application WKND

Plaçons le composant `<ResponsiveGrid...>`, créé au [chapitre précédent](./spa-container-component.md), dans notre composant de SPA `AdventureDetail` afin d’obtenir un conteneur modifiable.

### Placez le composant de SPA ResponsiveGrid

Si vous placez `<ResponsiveGrid...>` dans le composant `AdventureDetail`, vous créez un conteneur modifiable dans cet itinéraire. Comme plusieurs itinéraires utilisent le composant `AdventureDetail` pour effectuer le rendu, nous devons ajuster dynamiquement l’attribut `<ResponsiveGrid...>'s pagePath`. L’attribut `pagePath` doit être dérivé pour pointer vers la page AEM correspondante, en fonction de l’Adventure que l’instance de l’itinéraire affiche.

1. Ouvrez et modifiez `react-app-/src/components/AdventureDetail.js`.
1. Importez le composant `ResponsiveGrid` et placez-le au-dessus du composant `<h2>Itinerary</h2>`.
1. Définissez les attributs suivants sur le composant `<ResponsiveGrid...>`. Notez que l’attribut `pagePath` ajoute l’attribut `slug` actuel qui correspond à la page d’Adventure selon le mappage défini ci-dessus.
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   Cette instruction indique au composant `ResponsiveGrid` de récupérer son contenu à partir de la ressource d’AEM :

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

Mettez à jour `AdventureDetail.js` avec les lignes suivantes :

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

Le fichier `AdventureDetail.js` doit se présenter comme suit :

![AdventureDetail.js.](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Créer le conteneur dans AEM

Avec le `<ResponsiveGrid...>` en place et son `pagePath` défini dynamiquement en fonction de l’Adventure en cours de rendu, nous essayons de créer du contenu dans celui-ci.

1. Connectez-vous au service de création AEM.
1. Accédez à __Sites > Application WKND > us > en__.
1. __Modifiez__ la page __Page d’accueil de l’application WKND__.
   + Accédez à l’itinéraire de __Bali Surf Camp__ dans la SPA pour le modifier.
1. Sélectionnez __Aperçu__ à partir du sélecteur de mode en haut à droite.
1. Appuyez sur la carte du __Bali Surf Camp__ dans la SPA pour accéder à son itinéraire.
1. Sélectionnez __Modifier__ à partir du sélecteur de mode.
1. Recherchez le domaine modifiable du __conteneur de disposition__ juste au-dessus de l’__Itinerary__.
1. Ouvrez la __Barre latérale de l’éditeur de page__, puis sélectionnez la __vue Components__.
1. Faites glisser certains des composants activés dans le __conteneur de disposition__.
   + Image
   + Texte
   + Titre

   Créez ensuite des ressources marketing promotionnelles. Elles pourraient ressembler à ceci :

   ![Création de détails sur l’Adventure à Bali.](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Prévisualiser__ vos modifications dans l’éditeur de page d’AEM
1. Actualisez l’application WKND s’exécutant localement sur [http://localhost:3000](http://localhost:3000), accédez à l’itinéraire de __Bali Surf Camp__ pour afficher les modifications créées.

   ![SPA distante Bali.](./assets/spa-dynamic-routes/remote-spa-final.png)

Lorsque vous accédez à un itinéraire de détails d’Adventure qui ne comporte pas de page AEM mappée, il n’existe aucune fonctionnalité de création sur cette instance d’itinéraire. Pour activer la création sur ces pages, il vous suffit de faire une page AEM avec le nom correspondant sous la page d’__Adventure__.

## Félicitations.

Félicitations. Vous avez ajouté la capacité de création aux itinéraires dynamiques dans la SPA.

+ Vous avez ajouté le composant ResponsiveGrid du composant React Modifiable d’AEM à un itinéraire dynamique.
+ Vous avez créé des pages AEM pour permettre la création de deux itinéraires spécifiques dans la SPA (Bali Surf Camp et Beervana in Portland).
+ Vous avez créé du contenu sur l’itinéraire dynamique de Bali Surf Camp.

Vous avez maintenant terminé d’explorer les premières étapes sur la manière dont l’éditeur de SPA d’AEM peut être utilisé pour ajouter des domaines modifiables spécifiques à une SPA distante.
