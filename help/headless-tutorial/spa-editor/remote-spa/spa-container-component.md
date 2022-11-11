---
title: Ajout de composants de conteneur React modifiables à une SPA distante
description: Découvrez comment ajouter des composants de conteneur modifiables à un SPA distant qui permettent aux auteurs AEM de les faire glisser et de les déposer.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 2%

---

# Composants de conteneur modifiables

[Composants fixes](./spa-fixed-component.md) offre une certaine flexibilité pour créer SPA contenu, mais cette approche est rigide et nécessite que les développeurs définissent la composition exacte du contenu modifiable. Pour prendre en charge la création d’expériences exceptionnelles par les auteurs, SPA Editor prend en charge l’utilisation de composants de conteneur dans le SPA. Les composants de conteneur permettent aux auteurs de faire glisser des composants autorisés dans le conteneur et de les créer, comme ils le peuvent dans la création AEM Sites traditionnelle.

![Composants de conteneur modifiables](./assets/spa-container-component/intro.png)

Dans ce chapitre, nous ajoutons un conteneur modifiable à la vue d’accueil, ce qui permet aux auteurs de composer et de mettre en page des expériences de contenu enrichi à l’aide des composants React modifiables directement dans la SPA.

## Mise à jour de l’application WKND

Pour ajouter un composant de conteneur à la vue d’accueil :

+ Importez le composant React Modifiable AEM `ResponsiveGrid` component
+ Importez et enregistrez des composants React modifiables personnalisés (texte et image) à utiliser dans le composant ResponsiveGrid .

### Utilisation du composant ResponsiveGrid

Pour ajouter une zone modifiable à la vue Accueil :

1. Ouvrir et modifier `react-app/src/components/Home.js`
1. Importez la variable `ResponsiveGrid` du composant `@adobe/aem-react-editable-components` et ajoutez-le à la variable `Home` composant.
1. Définissez les attributs suivants sur la variable `<ResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Cette instruction indique à la fonction `ResponsiveGrid` pour récupérer son contenu de la ressource AEM :

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   Le `itemPath` est mappée à la variable `responsivegrid` noeud défini dans la variable `Remote SPA Page` AEM Modèle et est automatiquement créé sur les nouvelles pages AEM créées à partir de la `Remote SPA Page` AEM Modèle.

   Mettre à jour `Home.js` pour ajouter la variable `<ResponsiveGrid...>` composant.

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
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

Pour tirer pleinement parti des conteneurs d’expérience de création flexibles fournis dans SPA Editor. Nous avons déjà créé un composant Titre modifiable, mais nous allons en faire quelques autres qui permettent aux auteurs d’utiliser des composants Texte et Image modifiables dans le nouveau composant ResponsiveGrid ajouté.

Les nouveaux composants Texte et React d’image modifiables sont créés à l’aide du modèle de définition de composant modifiable exporté dans [composants fixes modifiables](./spa-fixed-component.md).

### Composant de texte modifiable

1. Ouvrez le projet SPA dans votre IDE.
1. Créez un composant React à l’adresse `src/components/editable/core/Text.js`
1. Ajoutez le code suivant à `Text.js`

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. Créez un composant React modifiable à l’adresse `src/components/editable/EditableText.js`
1. Ajoutez le code suivant à `EditableText.js`

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

L’implémentation du composant Texte modifiable doit se présenter comme suit :

![Composant de texte modifiable](./assets/spa-container-component/text-js.png)

### Composant d’image

1. Ouvrez le projet SPA dans votre IDE.
1. Créez un composant React à l’adresse `src/components/editable/core/Image.js`
1. Ajoutez le code suivant à `Image.js`

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. Créez un composant React modifiable à l’adresse `src/components/editable/EditableImage.js`
1. Ajoutez le code suivant à `EditableImage.js`

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. Création d’un fichier SCSS `src/components/editable/EditableImage.scss` qui fournit des styles personnalisés pour le `EditableImage.scss`. Ces styles ciblent les classes CSS du composant React modifiables.
1. Ajoutez le SCSS suivant à `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importer `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

L’implémentation du composant Image modifiable doit se présenter comme suit :

![Composant d’image modifiable](./assets/spa-container-component/image-js.png)


### Importation des composants modifiables

Le `EditableText` et `EditableImage` Les composants React sont référencés dans le SPA et sont instanciés dynamiquement en fonction du code JSON renvoyé par AEM. Pour vous assurer que ces composants sont disponibles pour la SPA, créez des instructions d’importation pour eux dans `Home.js`

1. Ouvrez le projet SPA dans votre IDE.
1. Ouvrez le fichier `src/Home.js`
1. Ajout d’instructions d’importation pour `AEMText` et `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

Le résultat doit se présenter comme suit :

![Home.js](./assets/spa-container-component/home-js-imports.png)

Si ces importations sont _not_ ajouté, la variable `EditableText` et `EditableImage` n’est pas appelé par SPA. Par conséquent, les composants ne sont pas mappés aux types de ressources fournis.

## Configuration du conteneur dans AEM

AEM composants de conteneur utilisent des stratégies pour dicter leurs composants autorisés. Il s’agit d’une configuration essentielle lors de l’utilisation de SPA Editor, car seuls AEM composants qui ont mappé des homologues de composant de l’autre sont rendus par le . Assurez-vous que seuls les composants pour lesquels nous avons fourni SPA implémentations sont autorisés :

+ `EditableTitle` mappé à `wknd-app/components/title`
+ `EditableText` mappé à `wknd-app/components/text`
+ `EditableImage` mappé à `wknd-app/components/image`

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

Après la mise à jour de la SPA pour incorporer la variable `<ResponsiveGrid...>`, wrappers pour trois composants React modifiables (`EditableTitle`, `EditableText`, et `EditableImage`), et AEM est mis à jour avec une stratégie Modèle correspondante, nous pouvons commencer à créer du contenu dans le composant de conteneur.

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

+ Utilisez le composant AEM React Modifiable de `ResponsiveGrid` du SPA
+ Créez et enregistrez des composants React modifiables (texte et image) à utiliser dans le SPA via le composant de conteneur.
+ Configurez le modèle Page de SPA distante pour autoriser les composants SPA
+ Ajout de composants modifiables au composant de conteneur
+ Composants de création et de mise en page dans SPA éditeur

## Étapes suivantes

L’étape suivante utilise cette même technique pour [ajouter un composant modifiable à un itinéraire Détails de l’aventure](./spa-dynamic-routes.md) dans le SPA.
