---
title: Ajout de composants modifiables à des itinéraires dynamiques SPA distants
description: Découvrez comment ajouter des composants modifiables aux itinéraires dynamiques dans un SPA distant.
topic: Sans tête, SPA, développement
feature: Éditeur SPA, composants principaux, API, développement
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 1%

---


# Itinéraires dynamiques et composants modifiables

Dans ce chapitre, nous autorisons deux itinéraires dynamiques Détails de l’aventure à prendre en charge les composants modifiables ; __Bali Surf Camp__ et __Beervana à Portland__.

![Itinéraires dynamiques et composants modifiables](./assets/spa-dynamic-routes/intro.png)

L’itinéraire SPA détails de l’aventure est défini comme `/adventure:path` où `path` est le chemin d’accès à l’aventure WKND (fragment de contenu) pour afficher des détails sur .

## Mappage des URL SPA aux pages AEM

Dans les deux chapitres précédents, nous avons mappé le contenu du composant modifiable de la vue SPA accueil à la page racine de SPA distante correspondante dans l’AEM à `/content/wknd-app/us/en/`.

La définition du mappage pour les composants modifiables pour les itinéraires dynamiques SPA est similaire. Toutefois, nous devons établir un schéma de mappage 1:1 entre les instances de l’itinéraire et les pages AEM.

Dans ce tutoriel, nous allons prendre le nom du fragment de contenu WKND Adventure, qui est le dernier segment du chemin, et le mapper à un chemin simple sous `/content/wknd-app/us/en/adventure`.

| Route de SPA distante | AEM chemin de page |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /aventure:/content/dam/wknd/fr/aventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/fr/home/aventure/__bali-surf-camp__ |
| /aventure:/content/dam/wknd/fr/aventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/aventure/__beervana-in-portland__ |

Ainsi, en fonction de ce mappage, nous devons créer deux nouvelles pages AEM à l’adresse :

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mappage des SPA distantes

Le mappage des requêtes qui quittent la SPA distante est configuré via la configuration `setupProxy` effectuée dans [le Bootstrap SPA](./spa-bootstrap.md).

## Mappage de l’éditeur de SPA

Le mappage pour les requêtes SPA lorsque le SPA est ouvert via l’éditeur d’ est configuré via la configuration des mappages Sling effectuée dans [Configuration d’](./aem-configure.md).

## Création de pages de contenu dans AEM

Créez tout d’abord le segment de page `adventure` intermédiaire :

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND > us > en > Page d’accueil de l’application WKND__
   + Cette page d’AEM est mappée en tant que racine de l’SPA. C’est donc là que nous commençons à développer la structure de page d’AEM pour d’autres itinéraires de .
1. Appuappuyez sur __Créer__ et sélectionnez __Page__.
1. Sélectionnez le modèle __Page SPA distante__ et appuyez sur __Suivant__.
1. Remplissage des propriétés de page
   + __Titre__ : Adventure
   + __Nom__ : `adventure`
      + Cette valeur définit l’URL de la page d’AEM et doit donc correspondre au segment d’itinéraire SPA.
1. Appuyez sur __Terminé__

Créez ensuite les pages d’AEM qui correspondent à chacune des URL SPA nécessitant des zones modifiables.

1. Accédez à la nouvelle page __Adventure__ de l’administrateur du site.
1. Appuappuyez sur __Créer__ et sélectionnez __Page__.
1. Sélectionnez le modèle __Page SPA distante__ et appuyez sur __Suivant__.
1. Remplissage des propriétés de page
   + __Titre__ : Le camp de surf de Bali
   + __Nom__ : `bali-surf-camp`
      + Cette valeur définit l’URL de la page d’AEM et doit donc correspondre au dernier segment de l’itinéraire SPA.
1. Appuyez sur __Terminé__
1. Répétez les étapes 3 à 6 pour créer la page __Beervana dans Portland__, avec :
   + __Titre__ : Beervana à Portland
   + __Nom__ : `beervana-in-portland`
      + Cette valeur définit l’URL de la page d’AEM et doit donc correspondre au dernier segment de l’itinéraire SPA.

Ces deux pages AEM contiennent le contenu créé correspondant à leurs itinéraires SPA correspondants. Si d’autres itinéraires SPA nécessitent la création, de nouvelles pages AEM doivent être créées à leur URL de base sous la page racine de la page de  distante (`/content/wknd-app/us/en/home`) dans.

## Mise à jour de l’application WKND

Placons le composant `<AEMResponsiveGrid...>` créé dans le [dernier chapitre](./spa-container-component.md) dans notre composant `AdventureDetail` SPA, créant ainsi un conteneur modifiable.

### Placez le composant SPA AEMResponsiveGrid

Le placement de `<AEMResponsiveGrid...>` dans le composant `AdventureDetail` crée un conteneur modifiable dans cet itinéraire. L’astuce est que plusieurs itinéraires utilisent le composant `AdventureDetail` pour le rendu. Nous devons ajuster dynamiquement l’attribut `<AEMResponsiveGrid...>'s pagePath`. `pagePath` doit être dérivé pour pointer vers la page d’AEM correspondante, en fonction de l’aventure affichée par l’instance de l’itinéraire.

1. Ouvrez et modifiez `react-app/src/components/AdventureDetail.js`
1. Ajoutez la ligne suivante avant la `AdventureDetail(..)'s` seconde `return(..)` instruction qui dérive le nom de l’aventure du chemin du fragment de contenu.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. Importez le composant `AEMResponsiveGrid` et placez-le au-dessus du composant `<h2>Itinerary</h2>`.
1. Définissez les attributs suivants sur le composant `<AEMResponsiveGrid...>`
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Cela indique au composant `AEMResponsiveGrid` de récupérer son contenu de la ressource AEM :

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Mettez à jour `AdventureDetail.js` avec les lignes suivantes :

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = adventureData._path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

Le fichier `AdventureDetail.js` doit se présenter comme suit :

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Création du conteneur dans AEM

Avec la balise `<AEMResponsiveGrid...>` en place et sa balise `pagePath` dynamiquement définie en fonction du rendu de l’aventure, nous essayons de créer du contenu dans celle-ci.

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND > us > en__
1. ____ Modification de la  __page d’accueil de l’application__ WKND
   + Accédez à l’itinéraire __Bali Surf Camp__ dans le SPA pour le modifier.
1. Sélectionnez __Aperçu__ dans le sélecteur de mode en haut à droite.
1. Appuyez sur la carte __Bali Surf Camp__ dans le SPA pour accéder à son itinéraire.
1. Sélectionnez __Modifier__ dans le sélecteur de mode.
1. Recherchez la __zone modifiable Conteneur de mises en page__ juste au-dessus de la __Itinéraire__
1. Ouvrez la __barre latérale de l’éditeur de page__, puis sélectionnez la __vue Composants__.
1. Faites glisser certains des composants activés dans le __conteneur de mises en page__
   + Image
   + Texte
   + Titre

   Et créer du matériel marketing promotionnel. Cela pourrait ressembler à ceci :

   ![Création de détails d’aventure à Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. ____ Aperçu des modifications dans AEM éditeur de page
1. Actualisez l’application WKND s’exécutant localement sur [http://localhost:3000](http://localhost:3000), accédez à l’itinéraire __Bali Surf Camp__ pour afficher les modifications créées.

   ![Bali SPA distante](./assets/spa-dynamic-routes/remote-spa-final.png)

Lorsque vous accédez à un itinéraire de détails d’aventure qui ne comporte pas de page d’AEM mappée, il n’y a aucune fonctionnalité de création sur cette instance d’itinéraire. Pour activer la création sur ces pages, il vous suffit de créer une page AEM avec le nom correspondant sous la page __Adventure__ .

## Félicitations ! 

Félicitations ! Vous avez ajouté la capacité de création aux itinéraires dynamiques dans la SPA !

+ Ajout du composant ResponsiveGrid du composant React Modifiable AEM à un itinéraire dynamique.
+ Création de pages AEM pour la création de deux itinéraires spécifiques dans le SPA (Bali Surf Camp et Beervana à Portland)
+ Contenu créé sur l&#39;itinéraire dynamique Bali Surf Camp !

Vous avez maintenant terminé d’explorer les premières étapes de la manière dont AEM Éditeur SPA peut être utilisé pour ajouter des zones modifiables spécifiques à un  distant !


>[!NOTE]
>
>Restez à l&#39;écoute ! Ce tutoriel sera développé afin de présenter les bonnes pratiques et les recommandations de l’Adobe sur le déploiement de la solution SPA Editor vers AEM as a Cloud Service et les environnements de production.