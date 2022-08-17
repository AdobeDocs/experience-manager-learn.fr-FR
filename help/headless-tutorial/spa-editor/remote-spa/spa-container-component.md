---
title: Ajout de composants de conteneur modifiables à un SPA distant
description: Découvrez comment ajouter des composants de conteneur modifiables à un SPA distant qui permettent aux auteurs AEM de les faire glisser et de les déposer.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 2%

---

# Composants de conteneur modifiables

[Composants fixes](./spa-fixed-component.md) offre une certaine flexibilité pour créer SPA contenu, mais cette approche est rigide et nécessite que les développeurs définissent la composition exacte du contenu modifiable. Pour prendre en charge la création d’expériences exceptionnelles par les auteurs, SPA Editor prend en charge l’utilisation de composants de conteneur dans le SPA. Les composants de conteneur permettent aux auteurs de faire glisser des composants autorisés dans le conteneur et de les créer, comme ils le peuvent dans la création AEM Sites traditionnelle.

![Composants de conteneur modifiables](./assets/spa-container-component/intro.png)

Dans ce chapitre, nous ajoutons un conteneur modifiable à la vue d’accueil, ce qui permet aux auteurs de composer et de mettre en page des expériences de contenu enrichi à l’aide AEM composants principaux React directement dans le SPA.

## Mise à jour de l’application WKND

Pour ajouter un composant de conteneur à la vue d’accueil :

+ Importez le composant ResponsiveGrid du composant React Modifiable AEM
+ Importez et enregistrez AEM composants principaux React (texte et image) à utiliser dans le composant de conteneur.

### Importation dans le composant de conteneur ResponsiveGrid

Pour placer une zone modifiable dans la vue Accueil, nous devons :

1. Importez le composant ResponsiveGrid depuis `@adobe/aem-react-editable-components`
1. Enregistrez-le à l’aide de `withMappable` afin que les développeurs puissent le placer dans la SPA
1. Vous pouvez également vous inscrire à `MapTo` afin qu’il puisse être réutilisé dans d’autres composants de conteneur, imbriquant ainsi des conteneurs.

Pour ce faire :

1. Ouvrez le projet SPA dans votre IDE.
1. Créez un composant React à l’adresse `src/components/aem/AEMResponsiveGrid.js`
1. Ajoutez le code suivant à `AEMResponsiveGrid.js`

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

Le code est similaire `AEMTitle.js` that [importation du composant Titre des composants principaux de la portée AEM](./spa-fixed-component.md).


Le `AEMResponsiveGrid.js` doit se présenter comme suit :

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### Utilisation du composant SPA AEMResponsiveGrid

Maintenant que le composant ResponsiveGrid AEM est enregistré dans et disponible pour une utilisation dans le SPA, nous pouvons le placer dans la vue d’accueil.

1. Ouvrir et modifier `react-app/src/Home.js`
1. Importez la variable `AEMResponsiveGrid` et placez-le au-dessus du composant `<AEMTitle ...>` composant.
1. Définissez les attributs suivants sur la variable `<AEMResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Cette instruction indique à la fonction `AEMResponsiveGrid` pour récupérer son contenu de la ressource AEM :

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   Le `itemPath` est mappée à la variable `responsivegrid` noeud défini dans la variable `Remote SPA Page` AEM Modèle et est automatiquement créé sur les nouvelles pages AEM créées à partir de la `Remote SPA Page` AEM Modèle.

   Mettre à jour `Home.js` pour ajouter la variable `<AEMResponsiveGrid...>` composant.

   ```
   ...
   import AEMResponsiveGrid from './aem/AEMResponsiveGrid';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <AEMResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
               <Adventures />
           </div>
       );
   }
   ```

Le `Home.js` doit se présenter comme suit :

![Home.js](./assets/spa-container-component/home-js.png)

## Création de composants modifiables

Pour tirer pleinement parti des conteneurs d’expérience de création flexibles fournis dans SPA Editor. Nous avons déjà créé un composant Titre modifiable, mais nous allons en faire quelques autres qui permettent aux auteurs d’utiliser des composants principaux Texte et Image AEM WCM dans le nouveau composant de conteneur ajouté.

### Composant textuel

1. Ouvrez le projet SPA dans votre IDE.
1. Créez un composant React à l’adresse `src/components/aem/AEMText.js`
1. Ajoutez le code suivant à `AEMText.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

Le `AEMText.js` doit se présenter comme suit :

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### Composant d’image

1. Ouvrez le projet SPA dans votre IDE.
1. Créez un composant React à l’adresse `src/components/aem/AEMImage.js`
1. Ajoutez le code suivant à `AEMImage.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. Création d’un fichier SCSS `src/components/aem/AEMImage.scss` qui fournit des styles personnalisés pour le `AEMImage.scss`. Ces styles ciblent les classes CSS de notation BEM du composant principal React AEM.
1. Ajoutez le SCSS suivant à `AEMImage.scss`

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importer `AEMImage.scss` in `AEMImage.js`

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

Le `AEMImage.js` et `AEMImage.scss` doit ressembler à :

![AEMImage.js et AEMImage.scss](./assets/spa-container-component/aem-image-js-scss.png)

### Importation des composants modifiables

Le `AEMText` et `AEMImage` Les composants SPA sont référencés dans le SPA et sont instanciés dynamiquement en fonction du JSON renvoyé par l’entité. Pour vous assurer que ces composants sont disponibles pour la SPA, créez des instructions d’importation pour eux dans `Home.js`

1. Ouvrez le projet SPA dans votre IDE.
1. Ouvrez le fichier `src/Home.js`
1. Ajout d’instructions d’importation pour `AEMText` et `AEMImage`

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


Le résultat doit se présenter comme suit :

![Home.js](./assets/spa-container-component/home-js-imports.png)

Si ces importations sont _not_ ajouté, la variable `AEMText` et `AEMImage` n’est pas appelé par SPA. Par conséquent, les composants ne sont pas enregistrés par rapport aux types de ressources fournis.

## Configuration du conteneur dans AEM

AEM composants de conteneur utilisent des stratégies pour dicter leurs composants autorisés. Il s’agit d’une configuration essentielle lors de l’utilisation de SPA Editor, puisque seuls AEM composants principaux de la gestion du contenu web qui ont mappé des homologues de composants de base sont rendus par le . Assurez-vous que seuls les composants pour lesquels nous avons fourni SPA implémentations sont autorisés :

+ `AEMTitle` mappé à `wknd-app/components/title`
+ `AEMText` mappé à `wknd-app/components/text`
+ `AEMImage` mappé à `wknd-app/components/image`

Pour configurer le conteneur reponsivegrid du modèle Page de SPA distante :

1. Connexion à l’auteur AEM
1. Accédez à __Outils > Général > Modèles > Application WKND__
1. Modifier __Page SPA rapport__

   ![Stratégies de grille réactive](./assets/spa-container-component/templates-remote-spa-page.png)

1. Sélectionner __Structure__ dans le sélecteur de mode en haut à droite.
1. Appuyez sur pour sélectionner la variable __Conteneur de mises en page__
1. Appuyez sur le bouton __Stratégie__ dans la barre contextuelle

   ![Stratégies de grille réactive](./assets/spa-container-component/templates-policies-action.png)

1. Sur la droite, sous le __Composants autorisés__ onglet, développer __APPLICATION WKND - CONTENU__
1. Assurez-vous que seuls les éléments suivants sont sélectionnés :
   + Image
   + Texte
   + Titre

   ![Page de SPA distante](./assets/spa-container-component/templates-allowed-components.png)

1. Appuyez sur __Terminé__

## Création du conteneur dans AEM

Après la mise à jour de la SPA pour incorporer la variable `<AEMResponsiveGrid...>`, wrappers pour trois composants principaux React AEM (`AEMTitle`, `AEMText`, et `AEMImage`), et AEM est mis à jour avec une stratégie Modèle correspondante, nous pouvons commencer à créer du contenu dans le composant de conteneur.

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND__
1. Appuyer __Accueil__ et sélectionnez __Modifier__ à partir de la barre d’actions supérieure
   + Un composant Texte &quot;Hello World&quot; s’affiche, car il a été automatiquement ajouté lors de la génération du projet à partir de l’archétype de projet AEM.
1. Sélectionner __Modifier__ dans le sélecteur de mode en haut à droite de l’éditeur de page.
1. Recherchez la variable __Conteneur de mises en page__ zone modifiable sous le titre
1. Ouvrez le __Barre latérale de l’éditeur de page__, puis sélectionnez la variable __Vue Composants__
1. Faites glisser les composants suivants dans le __Conteneur de mises en page__
   + Image
   + Titre
1. Faites glisser les composants pour les réorganiser dans l’ordre suivant :
   1. Titre
   1. Image
   1. Texte
1. __Auteur__ la valeur __Titre__ component
   1. Appuyez sur le composant Titre , puis sur le __clé à molette__ icône à __edit__ le composant Titre ;
   1. Ajoutez le texte suivant :
      + Titre : __L&#39;été arrive, profitons au maximum !__
      + Type : __H1__
   1. Appuyez sur __Terminé__
1. __Auteur__ la valeur __Image__ component
   1. Faites glisser une image dans à partir de la barre latérale (après avoir accédé à la vue Ressources) sur le composant Image.
   1. Appuyez sur le composant Image, puis sur la __clé à molette__ icône à modifier
   1. Vérifiez les __L’image est décorative__ checkbox
   1. Appuyez sur __Terminé__
1. __Auteur__ la valeur __Texte__ component
   1. Modifiez le composant Texte en appuyant sur le composant Texte, puis en appuyant sur le __clé à molette__ icon
   1. Ajoutez le texte suivant :
      + _Actuellement, vous pouvez obtenir 15 % de réduction sur toutes les aventures d&#39;une semaine et 20 % de réduction sur toutes les aventures de 2 semaines ou plus ! Au passage en caisse, ajoutez le code de campagne SUMMERISCOMING pour obtenir vos remises !_
   1. Appuyez sur __Terminé__

1. Vos composants sont maintenant créés, mais empilés verticalement.

   ![Composants créés](./assets/spa-container-component/authored-components.png)

   Utilisez AEM mode Mise en page pour ajuster la taille et la mise en page des composants.

1. Basculer vers __Mode Mise en page__ à l’aide du sélecteur de mode en haut à droite.
1. __Redimensionner__ les composants Image et Texte, de sorte qu’ils soient côte à côte ;
   + __Image__ component doit être __8 colonnes larges__
   + __Texte__ component doit être __3 colonnes larges__

   ![Composants de mise en page](./assets/spa-container-component/layout-components.png)

1. __Aperçu__ vos modifications dans AEM éditeur de page
1. Actualisez l’application WKND s’exécutant localement sur [http://localhost:3000](http://localhost:3000) pour afficher les modifications créées.

   ![Composant de conteneur dans SPA](./assets/spa-container-component/localhost-final.png)


## Félicitations !

Vous avez ajouté un composant de conteneur qui permet aux auteurs d’ajouter des composants modifiables à l’application WKND ! Vous savez maintenant comment :

+ Utilisation du composant ResponsiveGrid du composant ReactModifiable AEM dans le SPA
+ Enregistrez AEM composants principaux React (texte et image) pour les utiliser dans le SPA via le composant de conteneur.
+ Configurez le modèle Page de SPA distante pour autoriser les composants principaux activés SPA
+ Ajout de composants modifiables au composant de conteneur
+ Composants de création et de mise en page dans SPA éditeur

## Étapes suivantes

L’étape suivante consiste à utiliser cette même technique pour [ajouter un composant modifiable à un itinéraire Détails de l’aventure](./spa-dynamic-routes.md) dans le SPA.
