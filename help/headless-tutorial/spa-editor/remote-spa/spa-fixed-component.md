---
title: Ajout de composants fixes modifiables à un SPA distant
description: Découvrez comment ajouter des composants fixes modifiables à un SPA distant.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# Composants fixes modifiables

Les composants React modifiables peuvent être &quot;corrigés&quot; ou codés en dur dans les vues SPA. Cela permet aux développeurs de placer SPA composants compatibles avec l’éditeur dans les vues SPA et aux utilisateurs de créer le contenu des composants dans l’éditeur d’.

![Composants fixes](./assets/spa-fixed-component/intro.png)

Dans ce chapitre, nous remplaçons le titre de la vue d’accueil, &quot;Current Adventures&quot;, qui est un texte codé en dur dans `Home.js` avec un composant Titre fixe mais modifiable. Les composants fixes garantissent le placement du titre, mais permettent également la création du texte du titre et le changement en dehors du cycle de développement.

## Mise à jour de l’application WKND

Pour ajouter une __Fixe__ composant à la vue d’accueil :

+ Créez un composant Titre modifiable personnalisé et enregistrez-le dans le type de ressource Titre du projet.
+ Placez le composant Titre modifiable dans la vue d’accueil SPA

### Création d’un composant React Title modifiable

Dans la vue Accueil SPA, remplacez le texte codé en dur. `<h2>Current Adventures</h2>` avec un composant Titre modifiable personnalisé. Avant de pouvoir utiliser le composant Titre, nous devons :

1. Création d’un composant Titre React personnalisé
1. Décorez le composant Titre personnalisé à l’aide de méthodes issues de `@adobe/aem-react-editable-components` pour le rendre modifiable.
1. Enregistrez le composant Titre modifiable avec `MapTo` afin qu’il puisse être utilisé dans [composant de conteneur ultérieur](./spa-container-component.md).

Pour ce faire :

1. Ouvrez le projet SPA à distance à l’adresse `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` dans votre IDE
1. Créez un composant React à l’adresse `react-app/src/components/editable/core/Title.js`
1. Ajoutez le code suivant à `Title.js`.

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   Notez que ce composant React n’est pas encore modifiable à l’aide d’AEM SPA Editor. Ce composant de base sera rendu modifiable à l’étape suivante.

   Lisez les commentaires du code pour plus d’informations sur l’implémentation.

1. Créez un composant React à l’adresse `react-app/src/components/editable/EditableTitle.js`
1. Ajoutez le code suivant à `EditableTitle.js`.

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   Ceci `EditableTitle` Le composant React encapsule la propriété `Title` Composant React, encapsulation et décoration pour qu’il soit modifiable dans AEM SPA Editor.

### Utilisation du composant React EditableTitle

Maintenant que le composant React EditableTitle est enregistré dans et disponible pour utilisation dans l’application React, remplacez le texte du titre codé en dur sur la vue Accueil.

1. Modifier `react-app/src/components/Home.js`
1. Dans le `Home()` en bas, importez `EditableTitle` et remplacez le titre codé en dur par le nouveau `AEMTitle` component :

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

Le `Home.js` doit se présenter comme suit :

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## Création du composant Titre dans AEM

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND__
1. Appuyer __Accueil__ et sélectionnez __Modifier__ à partir de la barre d’actions supérieure
1. Sélectionner __Modifier__ dans le sélecteur de mode d’édition en haut à droite de l’éditeur de page ;
1. Passez la souris sur le texte de titre par défaut sous le logo WKND et au-dessus de la liste des aventures, jusqu’à ce que la composition de modification bleue s’affiche.
1. Appuyez sur pour afficher la barre d’actions du composant, puis appuyez sur la __clé à molette__  pour modifier

   ![Barre d’actions du composant Titre](./assets/spa-fixed-component/title-action-bar.png)

1. Créez le composant Titre :
   + Titre : __Les aventures de WKND__
   + Type/Taille : __H2__

      ![Boîte de dialogue du composant Titre](./assets/spa-fixed-component/title-dialog.png)

1. Appuyer __Terminé__ pour enregistrer
1. Prévisualisez vos modifications dans AEM Éditeur SPA
1. Actualisez l’application WKND s’exécutant localement sur [http://localhost:3000](http://localhost:3000) et voir les modifications du titre créé immédiatement répercutées.

   ![Composant du titre dans SPA](./assets/spa-fixed-component/title-final.png)

## Félicitations !

Vous avez ajouté un composant fixe et modifiable à l’application WKND ! Vous savez maintenant comment :

+ Création d’un composant fixe, mais modifiable, dans le SPA
+ Créez le composant fixe dans AEM
+ Voir le contenu créé dans la SPA distante

## Étapes suivantes

Les prochaines étapes sont les suivantes : [ajout d’un composant de conteneur ResponsiveGrid AEM](./spa-container-component.md) à la SPA qui permet à l’auteur d’ajouter et de modifier des composants au SPA !
