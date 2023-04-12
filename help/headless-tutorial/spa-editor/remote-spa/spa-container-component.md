---
title: Ajouter des composants de conteneur modifiables React à une SPA distante
description: Découvrez comment les créateurs et créatrices d’AEM peuvent ajouter des composants de conteneur modifiables à une SPA distante en procédant par glisser-déposer.
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
workflow-type: ht
source-wordcount: '1109'
ht-degree: 100%

---

# Composants de conteneur modifiables

Les [composants fixes](./spa-fixed-component.md) offrent une certaine flexibilité pour créer du contenu SPA, mais cette approche est rigide et nécessite que le développement définisse la composition exacte du contenu modifiable. Pour que les créateurs et créatrices puissent proposer des expériences exceptionnelles, l’éditeur de SPA prend en charge l’utilisation de composants de conteneur dans la SPA. Les composants de conteneur permettent aux créateurs et créatrices de glisser-déposer des composants autorisés dans le conteneur afin de les créer, tout comme ils procèdent dans la création traditionnelle d’AEM Sites.

![Composants de conteneur modifiables.](./assets/spa-container-component/intro.png)

Dans ce chapitre, nous ajoutons un conteneur modifiable à la vue d’accueil, ce qui permet aux créateurs et créatrices de composer et de mettre en page des expériences de contenu riches à l’aide des composants React modifiables directement dans la SPA.

## Mettre à jour l’application WKND

Pour ajouter un composant de conteneur à la vue d’accueil :

+ Importez le composant `ResponsiveGrid` du composant React modifiable d’AEM
+ Importez et enregistrez des composants React modifiables personnalisés (texte et image) à utiliser dans le composant ResponsiveGrid

### Utiliser le composant ResponsiveGrid.

Pour ajouter un domaine modifiable à la vue d’accueil :

1. Ouvrez et modifiez `react-app/src/components/Home.js`
1. Importez le composant `ResponsiveGrid` depuis `@adobe/aem-react-editable-components` et ajoutez-le au composant `Home`.
1. Définissez les attributs suivants sur le composant `<ResponsiveGrid...>`.
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Cette instruction indique au composant `ResponsiveGrid` de récupérer son contenu à partir de la ressource AEM :

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   L’attribut `itemPath` est mappé au nœud `responsivegrid` défini dans le modèle AEM `Remote SPA Page` et est automatiquement créé sur les nouvelles pages AEM conçues à partir du modèle AEM `Remote SPA Page`.

   Mettez à jour `Home.js` de sorte à ajouter le composant `<ResponsiveGrid...>`.

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

Le fichier `Home.js` doit se présenter comme suit :

![Home.js.](./assets/spa-container-component/home-js.png)

## Créer des composants modifiables

Vous pouvez pleinement tirer parti de l’expérience de création flexible fournie par les conteneurs dans l’éditeur de SPA. Nous avons déjà créé un composant Titre modifiable. Nous allons à présent en ajouter quelques autres pour permettre aux créateurs et créatrices d’utiliser des composants Texte et Image modifiables dans le nouveau composant ResponsiveGrid ajouté.

Les nouveaux composants React modifiables Texte et Image sont créés à l’aide du modèle de définition de composant modifiable exporté dans les [composants fixes modifiables](./spa-fixed-component.md).

### Composant Texte modifiable

1. Ouvrez le projet SPA dans votre IDE.
1. Créez un composant React sous `src/components/editable/core/Text.js`.
1. Ajoutez le code suivant à `Text.js`.

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

1. Créez un composant modifiable React sous `src/components/editable/EditableText.js`.
1. Ajoutez le code suivant à `EditableText.js`.

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

L’implémentation du composant Texte modifiable doit se présenter comme suit :

![Composant Texte modifiable.](./assets/spa-container-component/text-js.png)

### Composant d’image

1. Ouvrez le projet SPA dans votre IDE.
1. Créez un composant React sous `src/components/editable/core/Image.js`.
1. Ajoutez le code suivant à `Image.js`.

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

1. Créez un composant React modifiable sous `src/components/editable/EditableImage.js`.
1. Ajoutez le code suivant à `EditableImage.js`.

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


1. Créez un fichier SCSS `src/components/editable/EditableImage.scss` qui fournit des styles personnalisés à `EditableImage.scss`. Ces styles ciblent les classes CSS du composant React modifiable.
1. Ajoutez le SCSS suivant à `EditableImage.scss`.

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importez `EditableImage.scss` dans `EditableImage.js`.

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

L’implémentation du composant Image modifiable doit se présenter comme suit :

![Composant Image modifiable.](./assets/spa-container-component/image-js.png)


### Importer les composants modifiables

Les nouveaux composants React `EditableText` et `EditableImage` sont référencés dans la SPA et sont instanciés dynamiquement en fonction du code JSON renvoyé par AEM. Pour vous assurer que ces composants sont disponibles pour la SPA, créez les instructions d’importation correspondantes dans `Home.js`.

1. Ouvrez le projet SPA dans votre IDE.
1. Ouvrez le fichier `src/Home.js`.
1. Ajoutez des instructions d’importation pour `AEMText` et `AEMImage`.

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

Le résultat doit ressembler à ceci :

![Home.js.](./assets/spa-container-component/home-js-imports.png)

Si ces importations ne sont _pas_ ajoutées, les codes `EditableText` et `EditableImage` ne sont pas appelés par la SPA. Par conséquent, les composants ne sont pas mappés aux types de ressources fournis.

## Configuration du conteneur dans AEM

Les composants de conteneur AEM utilisent des stratégies pour dicter leurs composants autorisés. Il s’agit d’une configuration essentielle lors de l’utilisation de l’éditeur de SPA, car seuls les composants AEM ayant mappé des homologues de composant de SPA sont rendus par la SPA. Assurez-vous que seuls les composants pour lesquels nous avons fourni des implémentations de SPA sont autorisés :

+ `EditableTitle` mappé à `wknd-app/components/title`
+ `EditableText` mappé à `wknd-app/components/text`
+ `EditableImage` mappé à `wknd-app/components/image`.

Pour configurer le conteneur reponsivegrid du modèle de page de SPA distante :

1. Connectez-vous au service de création AEM.
1. Accédez à __Outils > Général > Modèles > Application WKND__.
1. Modifiez la __page de SPA distante__.

   ![Stratégies de grille réactive.](./assets/spa-container-component/templates-remote-spa-page.png)

1. Sélectionnez __Structure__ dans le sélecteur de mode en haut à droite.
1. Cliquez pour sélectionner le __conteneur de disposition__.
1. Cliquez sur l’icône __Stratégie__ dans la barre contextuelle.

   ![Stratégies de grille réactive.](./assets/spa-container-component/templates-policies-action.png)

1. Sur la droite, sous l’onglet __Composants autorisés__, développez __APPLICATION WKND - CONTENU__.
1. Assurez-vous que seuls les éléments suivants sont sélectionnés :
   + Image
   + Texte
   + Titre

   ![Page de SPA distante.](./assets/spa-container-component/templates-allowed-components.png)

1. Appuyez sur __Terminé__.

## Créer le conteneur dans AEM

Après la mise à jour de la SPA pour incorporer `<ResponsiveGrid...>` et les wrappers pour trois composants React modifiables (`EditableTitle`, `EditableText`, et `EditableImage`), et la mise à jour d’AEM avec une stratégie de modèle correspondante, nous pouvons commencer à créer du contenu dans le composant de conteneur.

1. Connectez-vous au service de création AEM.
1. Accédez à __Sites > Application WKND__.
1. Cliquez sur __Accueil__, puis sélectionnez __Modifier__ dans la barre d’actions du haut.
   + Un composant de texte « Hello World » s’affiche, car il a été automatiquement ajouté lors de la génération du projet à partir de l’archétype de projet AEM.
1. Sélectionnez __Modifier__ dans le sélecteur de mode en haut à droite de l’éditeur de page.
1. Recherchez le domaine modifiable du __conteneur de disposition__ sous le titre.
1. Ouvrez la __barre latérale de l’éditeur de page__, puis sélectionnez la __vue Composants__.
1. Faites glisser les composants suivants dans le __conteneur de disposition__ :
   + Image
   + Titre
1. Faites glisser les composants pour les réorganiser dans l’ordre suivant :
   1. Titre
   1. Image
   1. Texte
1. __Créez__ le composant de __titre__.
   1. Appuyez sur le composant de titre, puis sur l’icône __clé à molette__ pour __modifier__ le composant de titre.
   1. Ajoutez le texte suivant :
      + Titre : __L’été arrive, profitons-en au maximum.__
      + Type : __H1__
   1. Appuyez sur __Terminé__.
1. __Créez__ le composant d’__image__.
   1. Faites glisser une image à partir de la barre latérale (après avoir basculé sur le mode d’affichage Ressources) sur le composant d’image.
   1. Appuyez sur le composant d’image, puis sur l’icône __clé à molette__ pour modifier.
   1. Cochez la case __L’image est décorative__.
   1. Appuyez sur __Terminé__.
1. __Créez__ le composant de __texte__.
   1. Modifiez le composant de texte en appuyant dessus et en cliquant sur l’icône __clé à molette__.
   1. Ajoutez le texte suivant :
      + _Actuellement, vous pouvez obtenir 15 % de réduction sur toutes les Adventures d’une semaine et 20 % sur toutes celles de 2 semaines ou plus. Au passage en caisse, ajoutez le code de campagne SUMMERISCOMING pour obtenir vos remises._
   1. Appuyez sur __Terminé__.

1. Vos composants ont été créés, mais s’empilent verticalement.

   ![Composants créés.](./assets/spa-container-component/authored-components.png)

Utilisez le mode de disposition d’AEM pour ajuster la taille et la mise en page des composants.

1. Basculez sur le __mode de disposition__ à l’aide du sélecteur de mode situé en haut à droite.
1. __Redimensionnez__ les composants Image et Texte, de manière à ce qu’ils soient côte à côte ;
   + Le composant __Image__ doit avoir une __largeur de 8 colonnes__.
   + Le composant __Texte__ doit avoir une __largeur de 3 colonnes__.

   ![Composants de disposition.](./assets/spa-container-component/layout-components.png)

1. __Prévisualisez__ vos modifications dans l’éditeur de page d’AEM.
1. Actualisez l’application WKND s’exécutant localement sur [http://localhost:3000](http://localhost:3000) pour afficher les modifications.

   ![Composant de conteneur dans la SPA.](./assets/spa-container-component/localhost-final.png)


## Félicitations.

Vous avez ajouté un composant de conteneur qui autorise les composants modifiables à être ajoutés par les créateurs et créatrices sur l’application WKND. Vous savez maintenant comment :

+ Utiliser le composant `ResponsiveGrid` du composant React modifiable d’AEM dans la SPA
+ Créer et enregistrer des composants React modifiables (texte et image) à utiliser dans la SPA via le composant de conteneur
+ Configurer le modèle de page SPA distante pour autoriser les composants SPA
+ Ajouter des composants modifiables au composant de conteneur
+ Créer et disposer des composants dans l’éditeur de SPA.

## Étapes suivantes

L’étape suivante utilise cette même technique pour [ajouter un composant modifiable à un itinéraire Détails de l’Adventure](./spa-dynamic-routes.md) dans la SPA.
