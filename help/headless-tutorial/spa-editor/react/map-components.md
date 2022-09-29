---
title: Mappage des composants SPA aux composants AEM | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment mapper les composants React aux composants Adobe Experience Manager (AEM) avec le SDK JS d’AEM Editor. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme pour la création d’ traditionnelle. Vous apprendrez également à utiliser les composants principaux prêts à l’emploi AEM React.
sub-product: sites
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2256'
ht-degree: 2%

---

# Mappage des composants SPA aux composants AEM {#map-components}

Découvrez comment mapper les composants React aux composants Adobe Experience Manager (AEM) avec le SDK JS d’AEM Editor. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme pour la création d’ traditionnelle.

Ce chapitre aborde plus en détail l’API de modèle JSON AEM et la manière dont le contenu JSON exposé par un composant AEM peut être automatiquement injecté dans un composant React sous la forme de props.

## Objectif

1. Découvrez comment mapper AEM composants à SPA composants.
1. Inspect : la manière dont un composant React utilise les propriétés dynamiques transmises à partir d’AEM.
1. Découvrez comment utiliser des ressources prêtes à l’emploi [Composants principaux React AEM](https://github.com/adobe/aem-react-core-wcm-components-examples).

## Ce que vous allez créer

Ce chapitre examine la manière dont le `Text` Le composant SPA est mappé sur l’AEM `Text`composant. Les composants principaux React tels que `Image` Le composant SPA est utilisé dans le SPA et créé dans l’. Fonctionnalités prêtes à l’emploi du **Conteneur de mises en page** et **Éditeur de modèles** les stratégies peuvent également être utilisées pour créer une vue un peu plus variée sur le plan de l’aspect.

![Exemple de création finale de chapitre](./assets/map-components/final-page.png)

## Prérequis

Examinez les outils et les instructions requis pour configurer une [environnement de développement local](overview.md#local-dev-environment). Ce chapitre est la suite du chapitre [Intégration de la SPA](integrate-spa.md) , toutefois, pour suivre ce processus, vous avez besoin d’un projet AEM activé SPA.

## Approche de mappage

Le concept de base consiste à mapper un composant SPA à un composant AEM. AEM des composants, exécuter côté serveur, exporter du contenu dans le cadre de l’API de modèle JSON. Le contenu JSON est consommé par le SPA, en exécutant côté client dans le navigateur. Un mappage 1:1 entre les composants SPA et un composant AEM est créé.

![Présentation générale du mappage d’un composant AEM à un composant React](./assets/map-components/high-level-approach.png)

*Présentation générale du mappage d’un composant AEM à un composant React*

## Inspect du composant Texte

Le [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype) fournit une `Text` qui est mappé à l’AEM [Composant textuel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=fr). Il s’agit d’un exemple de **content** du composant, en ce qu’il effectue le rendu. *content* d’AEM.

Voyons comment fonctionne le composant.

### Inspect du modèle JSON

1. Avant de passer au code SPA, il est important de comprendre le modèle JSON fourni par AEM. Accédez au [Bibliothèque de composants principaux](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) et afficher la page du composant Texte. La bibliothèque des composants principaux fournit des exemples de tous les composants principaux AEM.
1. Sélectionnez la **JSON** pour l’un des exemples :

   ![Modèle JSON de texte](./assets/map-components/text-json.png)

   Vous devriez voir trois propriétés : `text`, `richText`, et `:type`.

   `:type` est une propriété réservée qui répertorie la variable `sling:resourceType` (ou chemin) du composant AEM. La valeur de `:type` est utilisé pour mapper le composant AEM au composant SPA.

   `text` et `richText` sont des propriétés supplémentaires qui sont exposées au composant SPA.

1. Afficher la sortie JSON à l’adresse [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Vous devriez pouvoir trouver une entrée similaire à :

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspect du composant SPA texte

1. Dans l’IDE de votre choix, ouvrez le projet AEM pour le SPA. Développez l’objet `ui.frontend` et ouvrez le fichier `Text.js` under `ui.frontend/src/components/Text/Text.js`.

1. La première zone que nous allons inspecter est la suivante : `class Text` ~line 40 :

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` est un composant React standard. Le composant utilise `this.props.richText` pour déterminer si le contenu à rendre sera du texte enrichi ou du texte brut. Le &quot;contenu&quot; réel utilisé provient de `this.props.text`.

   Pour éviter une attaque XSS potentielle, le texte enrichi est échappé via `DOMPurify` avant d’utiliser [dangereusementSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) pour effectuer le rendu du contenu. Rappeler la fonction `richText` et `text` propriétés du modèle JSON plus tôt dans l’exercice.

1. Jetez ensuite un coup d’oeil au `TextEditConfig` ~line 29 :

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Le code ci-dessus est chargé de déterminer quand effectuer le rendu de l’espace réservé dans l’environnement de création AEM. Si la variable `isEmpty` method renvoie **true** l’espace réservé est alors rendu.

1. Enfin, jetez un coup d’oeil au `MapTo` appelez à la ligne 62 :

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` est fourni par le SDK JS de l’éditeur d’AEM SPA (`@adobe/aem-react-editable-components`). Chemin d’accès `wknd-spa-react/components/text` représente la variable `sling:resourceType` du composant AEM. Ce chemin d’accès est mis en correspondance avec la variable `:type` exposé par le modèle JSON observé précédemment. `MapTo` prend soin d’analyser la réponse du modèle JSON et de transmettre les valeurs correctes en tant que `props` au composant SPA.

   Vous trouverez l’AEM `Text` définition de composant à l’emplacement `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Utilisation des composants principaux React

[AEM Composants WCM - Mise en oeuvre principale React](https://github.com/adobe/aem-react-core-wcm-components-base) et [AEM Composants WCM - Éditeur de Spa - Mise en oeuvre principale React](https://github.com/adobe/aem-react-core-wcm-components-spa). Il s’agit d’un ensemble de composants réutilisables de l’interface utilisateur qui correspondent aux composants AEM prêts à l’emploi. La plupart des projets peuvent réutiliser ces composants comme point de départ de leur propre mise en oeuvre.

1. Dans le code du projet, ouvrez le fichier . `import-components.js` at `ui.frontend/src/components`.
Ce fichier importe tous les composants SPA qui mappent les composants AEM. Étant donné la nature dynamique de l’implémentation de l’éditeur de SPA, nous devons référencer explicitement tous les composants SPA liés à des composants pouvant faire l’objet d’un auteur. Cela permet à un auteur d’AEM de choisir d’utiliser un composant où il le souhaite dans l’application.
1. Les instructions d’importation suivantes incluent SPA composants écrits dans le projet :

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Il existe plusieurs autres `imports` de `@adobe/aem-core-components-react-spa` et `@adobe/aem-core-components-react-base`. Ils importent les composants React Core et les rendent disponibles dans le projet en cours. Ils sont ensuite mappés à des composants AEM spécifiques au projet à l’aide de la variable `MapTo`, comme avec le `Text` exemple de composant précédent.

### Mettre à jour les stratégies AEM

Les stratégies sont une fonctionnalité des modèles d’AEM qui permet aux développeurs et aux utilisateurs expérimentés de contrôler précisément quels composants peuvent être utilisés. Les composants principaux React sont inclus dans le code SPA, mais doivent être activés via une stratégie avant de pouvoir être utilisés dans l’application.

1. Dans l’écran AEM Démarrer , accédez à **Outils** > **Modèles** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Sélectionnez et ouvrez le **Page SPA** modèle à modifier.

1. Sélectionnez la **Conteneur de mises en page** et cliquez sur it&#39;s **policy** pour modifier la stratégie :

   ![stratégie de conteneur de mises en page](assets/map-components/edit-spa-page-template.png)

1. Sous **Composants autorisés** > **WKND SPA React - Contenu** > vérifier **Image**, **Teaser**, et **Titre**.

   ![Composants mis à jour disponibles](assets/map-components/update-components-available.png)

   Sous **Composants par défaut** > **Ajouter un mappage** et sélectionnez la variable **Image : WKND SPA React - Contenu** component :

   ![Définition des composants par défaut](./assets/map-components/default-components.png)

   Saisissez un **type mime** de `image/*`.

   Cliquez sur **Terminé** pour enregistrer les mises à jour de stratégie.

1. Dans le **Conteneur de mises en page** cliquez sur **policy** pour la **Texte** composant.

   Création d’une stratégie nommée **Texte SPA WKND**. Sous **Modules externes** > **Formatage** > cochez toutes les cases pour activer des options de formatage supplémentaires :

   ![Activation du formatage de l’éditeur de texte enrichi](assets/map-components/enable-formatting-rte.png)

   Sous **Modules externes** > **Styles de paragraphe** > cochez la case pour **Activation des styles de paragraphe**:

   ![Activer les styles de paragraphe](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Cliquez sur **Terminé** pour enregistrer la mise à jour de la stratégie.

### Création de contenu

1. Accédez au **Page d’accueil** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Vous devriez maintenant pouvoir utiliser les composants supplémentaires. **Image**, **Teaser**, et **Titre** sur la page.

   ![Composants supplémentaires](assets/map-components/additional-components.png)

1. Vous devriez également pouvoir modifier la variable `Text` composant et ajouter des styles de paragraphe supplémentaires dans **plein écran** mode .

   ![Modification de texte enrichi en plein écran](assets/map-components/full-screen-rte.png)

1. Vous devriez également pouvoir faire glisser et déposer une image à partir de la fonction **Outil de recherche de ressources**:

   ![Faire glisser et déposer une image](assets/map-components/drag-drop-image.png)

1. Testez le **Titre** et **Teaser** composants.

1. Ajoutez vos propres images via [AEM Assets](http://localhost:4502/assets.html/content/dam) ou installer la base de code terminée pour la norme [Site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le [Site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) inclut de nombreuses images qui peuvent être réutilisées sur le SPA WKND. Le module peut être installé à l’aide de [AEM Gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect du conteneur de mises en page

Prise en charge de **Conteneur de mises en page** est automatiquement fourni par le SDK AEM SPA Editor. Le **Conteneur de mises en page**, comme indiqué par le nom, est un **container** composant. Les composants de conteneur sont des composants qui acceptent les structures JSON qui représentent *other* et instanciez-les de manière dynamique.

Examinons davantage le conteneur de mises en page.

1. Dans un navigateur, accédez à [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API de modèle JSON - Grille réactive](./assets/map-components/responsive-grid-modeljson.png)

   Le **Conteneur de mises en page** comporte un composant `sling:resourceType` de `wcm/foundation/components/responsivegrid` et est reconnu par l’éditeur de SPA à l’aide de la fonction `:type` , comme la propriété `Text` et `Image` composants.

   Les mêmes fonctionnalités que le redimensionnement d’un composant à l’aide de [Mode Mise en page](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sont disponibles avec SPA Editor.

2. Revenir à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Ajouter d’autres **Image** et essayez de les redimensionner à l’aide de la fonction **Disposition** option :

   ![Redimensionnement d’une image en mode Mise en page](./assets/map-components/responsive-grid-layout-change.gif)

3. Réouverture du modèle JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) et observez la `columnClassNames` dans le cadre du fichier JSON :

   ![Noms des classes de cloud](./assets/map-components/responsive-grid-classnames.png)

   Nom de la classe `aem-GridColumn--default--4` indique que le composant doit comporter 4 colonnes de large en fonction d’une grille de 12 colonnes. Plus d’informations sur [grille réactive se trouve ici](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Revenez à l’IDE et dans le `ui.apps` Il existe une bibliothèque côté client définie à l’adresse `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Ouvrez le fichier `less/grid.less`.

   Ce fichier détermine les points d’arrêt (`default`, `tablet`, et `phone`) utilisé par la variable **Conteneur de mises en page**. Ce fichier est conçu pour être personnalisé selon les spécifications du projet. Actuellement, les points d’arrêt sont définis sur `1200px` et `768px`.

5. Vous devriez être en mesure d’utiliser les fonctionnalités réactives et les stratégies de texte enrichi mises à jour de la variable `Text` pour créer une vue comme suit :

   ![Exemple de création finale de chapitre](assets/map-components/final-page.png)

## Félicitations ! {#congratulations}

Félicitations, vous avez appris à mapper SPA composants à AEM composants et vous avez utilisé les composants principaux React. Vous avez également la possibilité d’explorer les fonctionnalités réactives de la variable **Conteneur de mises en page**.

### Étapes suivantes {#next-steps}

[Navigation et routage](navigation-routing.md) - Découvrez comment plusieurs vues de la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de formulaires. La navigation dynamique est mise en oeuvre à l’aide des composants principaux React Router et React.

## (bonus) Conserver les configurations pour le contrôle de code source {#bonus-configs}

Dans de nombreux cas, en particulier au début d’un projet AEM, il est utile de conserver les configurations, comme les modèles et les stratégies de contenu associées, pour le contrôle de code source. Cela garantit que tous les développeurs travaillent sur le même ensemble de contenu et de configurations et peut garantir une cohérence supplémentaire entre les environnements. Une fois qu’un projet atteint un certain niveau de maturité, la pratique de gestion des modèles peut être transmise à un groupe spécial d’utilisateurs expérimentés.

Les étapes suivantes se dérouleront à l’aide de l’IDE Visual Studio Code et [Synchronisation des AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) mais peut utiliser n’importe quel outil et n’importe quel IDE que vous avez configuré pour **pull** ou **import** contenu d’une instance locale d’AEM.

1. Dans l’IDE Visual Studio Code, assurez-vous que vous disposez de **Synchronisation des AEM VSCode** installé via l’extension Marketplace :

   ![Synchronisation des AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Développez l’objet **ui.content** dans l’explorateur de projets et accédez à `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Clic droit** la valeur `templates` et sélectionnez **Importation depuis AEM serveur**:

   ![Modèle d’import VSCode](./assets/map-components/import-aem-servervscode.png)

4. Répétez les étapes pour importer du contenu, mais sélectionnez l’option **policies** dossier situé à l’emplacement `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect `filter.xml` fichier situé à l’emplacement `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   Le `filter.xml` est chargé d’identifier les chemins d’accès des noeuds installés avec le package. Remarquez la variable `mode="merge"` sur chacun des filtres qui indique que le contenu existant ne sera pas modifié, seul un nouveau contenu est ajouté. Étant donné que les auteurs de contenu peuvent mettre à jour ces chemins, il est important qu’un déploiement de code **not** remplacer le contenu. Voir [Documentation de FileVault](https://jackrabbit.apache.org/filevault/filter.html) pour plus d’informations sur l’utilisation des éléments de filtre.

   Comparer `ui.content/src/main/content/META-INF/vault/filter.xml` et `ui.apps/src/main/content/META-INF/vault/filter.xml` pour comprendre les différents noeuds gérés par chaque module.

## (bonus) Création d’un composant d’image personnalisé {#bonus-image}

Un composant Image SPA a déjà été fourni par les composants React Core. Cependant, si vous souhaitez une pratique supplémentaire, créez votre propre mise en oeuvre React qui mappe à l’AEM [Composant d’image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=fr). Le `Image` est un autre exemple d’un composant **content** composant.

### Inspect du JSON

Avant de passer au code SPA, examinez le modèle JSON fourni par AEM.

1. Accédez au [Exemples d’images dans la bibliothèque de composants principaux](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![JSON du composant principal d’image](./assets/map-components/image-json.png)

   Propriétés de `src`, `alt`, et `title` sont utilisés pour remplir les SPA `Image` composant.

   >[!NOTE]
   >
   > D’autres propriétés d’image sont exposées (`lazyEnabled`, `widths`) qui permettent à un développeur de créer un composant de chargement adaptatif et différé. Le composant créé dans ce tutoriel est simple et fonctionne comme suit : **not** utilisez ces propriétés avancées.

### Mise en oeuvre du composant Image

1. Créez ensuite un dossier nommé `Image` under `ui.frontend/src/components`.
1. Sous la `Image` créer un dossier nommé `Image.js`.

   ![Fichier Image.js](./assets/map-components/image-js-file.png)

1. Ajoutez ce qui suit : `import` instructions à `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. Ajoutez ensuite la variable `ImageEditConfig` pour déterminer quand afficher l’espace réservé dans AEM :

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   L’espace réservé affiche si la variable `src` n’est pas définie.

1. Implémentez ensuite le `Image` Classe :

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   Le code ci-dessus génère une `<img>` en fonction des props `src`, `alt`, et `title` transmis par le modèle JSON.

1. Ajoutez la variable `MapTo` code pour mapper le composant React au composant AEM :

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Notez la chaîne `wknd-spa-react/components/image` correspond à l’emplacement du composant AEM dans `ui.apps` at : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Créez un fichier nommé `Image.css` dans le même répertoire et ajoutez les éléments suivants :

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. Dans `Image.js` ajoutez une référence au fichier dans la partie supérieure sous l’onglet `import` instructions :

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Ouvrir le fichier `ui.frontend/src/components/import-components.js` et ajoutez une référence à la nouvelle `Image` component :

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. Dans `import-components.js` commentez l’image du composant principal React :

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   Cela garantit que notre composant Image personnalisé est utilisé à la place.

1. À partir de la racine du projet, déployez le code SPA vers AEM à l’aide de Maven :

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspect le SPA dans AEM. Les composants Image de la page doivent continuer à fonctionner. Inspect la sortie générée et vous devriez voir le balisage de notre composant Image personnalisé au lieu du composant principal React.

   *Balisage du composant Image personnalisé*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *Balisage d’image du composant principal React*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   Il s’agit d’une bonne introduction à l’extension et à l’implémentation de vos propres composants.
