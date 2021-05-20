---
title: Ajout de composants fixes modifiables à un SPA distant
description: Découvrez comment ajouter des composants fixes modifiables à un SPA distant.
topic: Sans tête, SPA, développement
feature: Éditeur SPA, composants principaux, API, développement
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---


# Composants fixes modifiables

Les composants React modifiables peuvent être &quot;corrigés&quot; ou codés en dur dans les vues SPA. Cela permet aux développeurs de placer SPA composants compatibles avec l’éditeur dans les vues SPA et aux utilisateurs de créer le contenu des composants dans l’éditeur d’.

![Composants fixes](./assets/spa-fixed-component/intro.png)

Dans ce chapitre, nous remplaçons le titre de la vue Accueil, &quot;Current Adventures&quot;, qui est un texte codé en dur dans `Home.js` par un composant Titre fixe mais modifiable. Les composants fixes garantissent le placement du titre, mais permettent également la création du texte du titre et le changement en dehors du cycle de développement.

## Mise à jour de l’application WKND

Pour ajouter un composant fixe à la vue Accueil :

+ Importez le composant Titre du composant principal React AEM et enregistrez-le dans le type de ressource du titre du projet.
+ Placez le composant Titre modifiable dans la vue d’accueil SPA

### Importation dans le composant Titre du composant principal React AEM

Dans la vue d’accueil SPA, remplacez le texte codé en dur `<h2>Current Adventures</h2>` par le composant Titre des composants principaux React AEM. Avant de pouvoir utiliser le composant Titre, nous devons :

1. Importez le composant Titre à partir de `@adobe/aem-core-components-react-base`
1. Enregistrez-le à l’aide de `withMappable` afin que les développeurs puissent le placer dans le SPA
1. Enregistrez-le également avec `MapTo` afin qu’il puisse être utilisé dans le composant de conteneur [ultérieur](./spa-container-component.md).

Pour ce faire :

1. Ouvrez le projet SPA distant à `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` dans votre IDE.
1. Créez un composant React à l’adresse `react-app/src/components/aem/AEMTitle.js`
1. Ajoutez le code suivant à `AEMTitle.js`.

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

Lisez les commentaires du code pour plus d’informations sur l’implémentation.

Le fichier `AEMTitle.js` doit se présenter comme suit :

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### Utilisation du composant React AEMTitle

Maintenant que le composant Titre du composant principal React d’AEM est enregistré dans et disponible pour utilisation dans l’application React, remplacez le texte du titre codé en dur dans la vue d’accueil.

1. Modifier `react-app/src/App.js`
1. dans la partie inférieure, remplacez le titre codé en dur par le nouveau composant `AEMTitle` :`Home()`

   ```
   <h2>Current Adventures</h2>
   ```

   par

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   Mettez à jour `Apps.js` avec le code suivant :

   ```
   ...
   import { AEMTitle } from './components/aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

Le fichier `Apps.js` doit se présenter comme suit :

![App.js](./assets/spa-fixed-component/app-js.png)

## Création du composant Titre dans AEM

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND__
1. Appuyez sur __Home__ et sélectionnez __Edit__ dans la barre d’actions supérieure.
1. Sélectionnez __Modifier__ dans le sélecteur de mode d’édition en haut à droite de l’éditeur de page.
1. Passez la souris sur le texte de titre par défaut sous le logo WKND et au-dessus de la liste des aventures, jusqu’à ce que la composition de modification bleue s’affiche.
1. Appuyez pour afficher la barre d’actions du composant, puis appuyez sur la __clé à molette__ pour modifier.

   ![Barre d’actions du composant Titre](./assets/spa-fixed-component/title-action-bar.png)

1. Créez le composant Titre :
   + Titre : __Aventures WKND__
   + Type/Taille : __H2__

      ![Boîte de dialogue du composant Titre](./assets/spa-fixed-component/title-dialog.png)

1. Appuyez sur __Terminé__ pour enregistrer.
1. Prévisualisez vos modifications dans AEM Éditeur SPA
1. Actualisez l’application WKND s’exécutant localement sur [http://localhost:3000](http://localhost:3000) et consultez les modifications de titre créées immédiatement répercutées.

   ![Composant du titre dans SPA](./assets/spa-fixed-component/title-final.png)

## Félicitations ! 

Vous avez ajouté un composant fixe et modifiable à l’application WKND ! Vous savez maintenant comment :

+ Importation et réutilisation d’un composant principal React AEM dans le SPA
+ Ajoutez un composant fixe mais modifiable à la SPA
+ Créez le composant fixe dans AEM
+ Voir le contenu créé dans la SPA distante

## Étapes suivantes

Les étapes suivantes consistent à [ajouter un composant de conteneur ResponsiveGrid AEM](./spa-container-component.md) à l’SPA qui permet à l’auteur d’ajouter des composants modifiables à l’SPA !
