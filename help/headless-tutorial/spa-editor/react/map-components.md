---
title: Mappage des composants SPA aux composants AEM | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment mapper les composants React aux composants Adobe Experience Manager (AEM) avec le SDK JS d’AEM Editor. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme pour la création d’ traditionnelle. Vous apprendrez également à utiliser les composants principaux prêts à l’emploi AEM React.
sub-product: sites
feature: Éditeur de SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2267'
ht-degree: 2%

---


# Mappage des composants SPA aux composants AEM {#map-components}

Découvrez comment mapper les composants React aux composants Adobe Experience Manager (AEM) avec le SDK JS d’AEM Editor. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme pour la création d’ traditionnelle.

Ce chapitre aborde plus en détail l’API de modèle JSON AEM et la manière dont le contenu JSON exposé par un composant AEM peut être automatiquement injecté dans un composant React sous la forme de props.

## Objectif

1. Découvrez comment mapper AEM composants à SPA composants.
1. Inspect : la manière dont un composant React utilise les propriétés dynamiques transmises à partir d’AEM.
1. Découvrez comment utiliser les [React AEM Core Components](https://github.com/adobe/aem-react-core-wcm-components-examples) prêts à l’emploi.

## Ce que vous allez créer

Ce chapitre examine la façon dont le composant `Text` SPA fourni est mappé au composant `Text`AEM. Les composants principaux React tels que le composant `Image` SPA seront utilisés dans le SPA et créés dans l’. Les fonctionnalités prêtes à l’emploi des stratégies **Conteneur de mises en page** et **Éditeur de modèles** seront également utilisées pour créer une vue un peu plus variée en apparence.

![Exemple de création finale de chapitre](./assets/map-components/final-page.png)

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment). Ce chapitre est la suite du chapitre [Intégrer le chapitre SPA](integrate-spa.md), mais pour suivre, tout ce dont vous avez besoin est un projet d’SPA activé par AEM.

## Approche de mappage

Le concept de base consiste à mapper un composant SPA à un composant AEM. AEM des composants, exécuter côté serveur, exporter du contenu dans le cadre de l’API de modèle JSON. Le contenu JSON est consommé par le SPA, en exécutant côté client dans le navigateur. Un mappage 1:1 entre les composants SPA et un composant AEM est créé.

![Présentation générale du mappage d’un composant AEM à un composant React](./assets/map-components/high-level-approach.png)

*Présentation générale du mappage d’un composant AEM à un composant React*

## Inspect du composant Texte

L’ [archétype de projet AEM](https://github.com/adobe/aem-project-archetype) fournit un composant `Text` mappé au composant [Texte AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Il s’agit d’un exemple de composant **content**, dans la mesure où il effectue le rendu de *content* à partir d’AEM.

Voyons comment fonctionne le composant.

### Inspect du modèle JSON

1. Avant de passer au code SPA, il est important de comprendre le modèle JSON fourni par AEM. Accédez à la [bibliothèque de composants principaux](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) et affichez la page du composant Texte. La bibliothèque des composants principaux fournit des exemples de tous les composants principaux AEM.
1. Sélectionnez l’onglet **JSON** pour l’un des exemples :

   ![Modèle JSON de texte](./assets/map-components/text-json.png)

   Vous devriez voir trois propriétés : `text`, `richText` et `:type`.

   `:type` est une propriété réservée qui répertorie le  `sling:resourceType` (ou chemin) du composant AEM. La valeur `:type` est utilisée pour mapper le composant AEM au composant SPA.

   `text` et  `richText` sont des propriétés supplémentaires qui seront exposées au composant SPA.

1. Affichez la sortie JSON à l’adresse [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Vous devriez pouvoir trouver une entrée similaire à :

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

1. Dans l’IDE de votre choix, ouvrez le projet AEM pour le SPA. Développez le module `ui.frontend` et ouvrez le fichier `Text.js` sous `ui.frontend/src/components/Text/Text.js`.

1. La première zone que nous examinerons est la `class Text` à la ligne 40 ~:

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

   Pour éviter toute attaque XSS potentielle, le texte enrichi est échappé via `DOMPurify` avant d’utiliser [dangereusementSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) pour le rendu du contenu. Rappelez les propriétés `richText` et `text` du modèle JSON plus tôt dans l’exercice.

1. Examinez ensuite la `TextEditConfig` à la ligne 29 :

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Le code ci-dessus est chargé de déterminer quand effectuer le rendu de l’espace réservé dans l’environnement de création AEM. Si la méthode `isEmpty` renvoie **true**, l’espace réservé est rendu.

1. Enfin, jetez un coup d&#39;oeil à l&#39;appel `MapTo` à la ligne 62 ~line :

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` est fourni par le SDK JS de l’éditeur d’AEM (`@adobe/aem-react-editable-components`). Le chemin `wknd-spa-react/components/text` représente la `sling:resourceType` du composant AEM. Ce chemin est mis en correspondance avec la balise `:type` exposée par le modèle JSON observé précédemment. `MapTo` se charge d’analyser la réponse du modèle JSON et de transmettre les valeurs correctes  `props` au composant SPA.

   Vous trouverez la définition du composant `Text` AEM à l’adresse `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Utilisation des composants principaux React

[AEM Composants WCM - ](https://github.com/adobe/aem-react-core-wcm-components-base) Mise en oeuvre principale de React et  [AEM Composants WCM - Éditeur de Spa - Mise en oeuvre principale de React](https://github.com/adobe/aem-react-core-wcm-components-spa). Il s’agit d’un ensemble de composants réutilisables de l’interface utilisateur qui correspondent aux composants AEM prêts à l’emploi. La plupart des projets peuvent réutiliser ces composants comme point de départ de leur propre mise en oeuvre.

1. Dans le code du projet, ouvrez le fichier `import-components.js` à l’adresse `ui.frontend/src/components`.
Ce fichier importe tous les composants SPA qui mappent les composants AEM. Étant donné la nature dynamique de l’implémentation de l’éditeur de SPA, nous devons référencer explicitement tous les composants SPA liés à des composants pouvant faire l’objet d’un auteur. Cela permet à un auteur d’AEM de choisir d’utiliser un composant où il le souhaite dans l’application.
1. Les instructions d’importation suivantes incluent SPA composants écrits dans le projet :

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Il existe plusieurs autres `imports` de `@adobe/aem-core-components-react-spa` et `@adobe/aem-core-components-react-base`. Ils importent les composants React Core et les rendent disponibles dans le projet en cours. Ils sont ensuite mappés à des composants AEM spécifiques au projet à l’aide de `MapTo`, comme avec l’exemple de composant `Text` précédent.

### Mettre à jour les stratégies AEM

Les stratégies sont une fonctionnalité des modèles d’AEM qui permet aux développeurs et aux utilisateurs expérimentés de contrôler précisément quels composants peuvent être utilisés. Les composants principaux React sont inclus dans le code SPA, mais doivent être activés via une stratégie avant de pouvoir être utilisés dans l’application.

1. Dans l’écran AEM Démarrer, accédez à **Outils** > **Modèles** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Sélectionnez le modèle **SPA Page** et ouvrez-le pour le modifier.

1. Sélectionnez le **Conteneur de mises en page** et cliquez sur l’icône **policy** pour modifier la stratégie :

   ![stratégie de conteneur de mises en page](assets/map-components/edit-spa-page-template.png)

1. Sous **Composants autorisés** > **WKND SPA React - Contenu** > cochez **Image**, **Teaser** et **Titre**.

   ![Composants mis à jour disponibles](assets/map-components/update-components-available.png)

   Sous **Composants par défaut** > **Ajouter un mappage** et sélectionnez le composant **Image - WKND SPA React - Contenu** :

   ![Définition des composants par défaut](./assets/map-components/default-components.png)

   Saisissez un **type MIME** de `image/*`.

   Cliquez sur **Terminé** pour enregistrer les mises à jour de stratégie.

1. Dans **Conteneur de mises en page**, cliquez sur l’icône **policy** pour le composant **Texte**.

   Créez une nouvelle stratégie nommée **Texte SPA WKND**. Sous **Plugins** > **Formatage** > cochez toutes les cases pour activer des options de formatage supplémentaires :

   ![Activation du formatage de l’éditeur de texte enrichi](assets/map-components/enable-formatting-rte.png)

   Sous **Plugins** > **Styles de paragraphe** > cochez la case **Activer les styles de paragraphe** :

   ![Activer les styles de paragraphe](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Cliquez sur **Terminé** pour enregistrer la mise à jour de la stratégie.

### Création de contenu

1. Accédez à la **page d’accueil** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Vous devez maintenant pouvoir utiliser les composants supplémentaires **Image**, **Teaser** et **Titre** sur la page.

   ![Composants supplémentaires](assets/map-components/additional-components.png)

1. Vous devez également pouvoir modifier le composant `Text` et ajouter des styles de paragraphe supplémentaires en mode **plein écran**.

   ![Modification de texte enrichi en plein écran](assets/map-components/full-screen-rte.png)

1. Vous devez également pouvoir faire glisser et déposer une image à partir de l’**outil de recherche de ressources** :

   ![Faire glisser et déposer une image](assets/map-components/drag-drop-image.png)

1. Teaser avec les composants **Titre** et **Teaser**.

1. Ajoutez vos propres images via [AEM Assets](http://localhost:4502/assets.html/content/dam) ou installez la base de code finalisée pour le [site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) standard. Le [site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) comprend de nombreuses images qui peuvent être réutilisées sur le SPA WKND. Le package peut être installé à l’aide de [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect du conteneur de mises en page

La prise en charge du **conteneur de mises en page** est automatiquement fournie par le SDK de l’éditeur SPA d’AEM. Le **conteneur de mises en page**, comme indiqué par le nom, est un composant **conteneur**. Les composants de conteneur sont des composants qui acceptent les structures JSON qui représentent *d’autres composants* et les instancient dynamiquement.

Examinons davantage le conteneur de mises en page.

1. Dans un navigateur, accédez à [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API de modèle JSON - Grille réactive](./assets/map-components/responsive-grid-modeljson.png)

   Le composant **Conteneur de mises en page** a une valeur `sling:resourceType` de `wcm/foundation/components/responsivegrid` et est reconnu par l’éditeur de SPA à l’aide de la propriété `:type`, tout comme les composants `Text` et `Image`.

   Les mêmes fonctionnalités de redimensionnement d’un composant à l’aide du [mode Mise en page](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sont disponibles avec l’éditeur de SPA.

2. Revenez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Ajoutez d’autres composants **Image** et essayez de les redimensionner à l’aide de l’option **Mise en page** :

   ![Redimensionnement d’une image en mode Mise en page](./assets/map-components/responsive-grid-layout-change.gif)

3. Ouvrez à nouveau le modèle JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) et observez la balise `columnClassNames` dans le cadre du fichier JSON :

   ![Noms des classes de cloud](./assets/map-components/responsive-grid-classnames.png)

   Le nom de classe `aem-GridColumn--default--4` indique que le composant doit comporter 4 colonnes larges basées sur une grille de 12 colonnes. Vous trouverez plus d’informations sur la [grille réactive ici](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Revenez à l’IDE et dans le module `ui.apps`, il existe une bibliothèque côté client définie à l’adresse `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Ouvrez le fichier `less/grid.less`.

   Ce fichier détermine les points d’arrêt (`default`, `tablet` et `phone`) utilisés par le **conteneur de mises en page**. Ce fichier est conçu pour être personnalisé selon les spécifications du projet. Actuellement, les points d’arrêt sont définis sur `1200px` et `768px`.

5. Vous devez pouvoir utiliser les fonctionnalités réactives et les stratégies de texte enrichi mises à jour du composant `Text` pour créer une vue telle que celle-ci :

   ![Exemple de création finale de chapitre](assets/map-components/final-page.png)

## Félicitations ! {#congratulations}

Félicitations, vous avez appris à mapper SPA composants à AEM composants et vous avez utilisé les composants principaux React. Vous avez également eu la possibilité d’explorer les fonctionnalités réactives du **conteneur de mises en page**.

### Étapes suivantes {#next-steps}

[Navigation et routage](navigation-routing.md)  : découvrez comment plusieurs vues dans la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de ressources. La navigation dynamique est mise en oeuvre à l’aide des composants principaux React Router et React.

## (bonus) Conserver les configurations pour le contrôle de code source {#bonus-configs}

Dans de nombreux cas, en particulier au début d’un projet AEM, il est utile de conserver les configurations, comme les modèles et les stratégies de contenu associées, pour le contrôle de code source. Cela garantit que tous les développeurs travaillent sur le même ensemble de contenu et de configurations et peut garantir une cohérence supplémentaire entre les environnements. Une fois qu’un projet atteint un certain niveau de maturité, la pratique de gestion des modèles peut être transmise à un groupe spécial d’utilisateurs expérimentés.

Les étapes suivantes se dérouleront à l’aide de l’IDE Visual Studio Code et de [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) mais peuvent utiliser n’importe quel outil et tout IDE que vous avez configuré pour **extraire** ou **importer** du contenu d’une instance locale d’AEM.

1. Dans l’IDE Visual Studio Code, assurez-vous que **VSCode AEM Sync** est installé via l’extension Marketplace :

   ![Synchronisation des AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Développez le module **ui.content** dans l’explorateur de projets et accédez à `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Cliquez avec le bouton droit de la souris** sur le  `templates` dossier et sélectionnez  **Importer depuis AEM serveur** :

   ![Modèle d’import VSCode](./assets/map-components/import-aem-servervscode.png)

4. Répétez les étapes pour importer du contenu, mais sélectionnez le dossier **policies** situé à l’emplacement `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect le fichier `filter.xml` situé à l’emplacement `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Le fichier `filter.xml` est chargé d’identifier les chemins d’accès des noeuds qui seront installés avec le package. Notez la balise `mode="merge"` de chacun des filtres qui indique que le contenu existant ne sera pas modifié, seul le nouveau contenu est ajouté. Comme les auteurs de contenu peuvent mettre à jour ces chemins, il est important qu’un déploiement de code remplace **et non** le contenu. Pour plus d’informations sur l’utilisation des éléments de filtre, voir la [documentation FileVault](https://jackrabbit.apache.org/filevault/filter.html) .

   Comparez `ui.content/src/main/content/META-INF/vault/filter.xml` et `ui.apps/src/main/content/META-INF/vault/filter.xml` pour comprendre les différents noeuds gérés par chaque module.

## (bonus) Création d’un composant d’image personnalisé {#bonus-image}

Un composant Image SPA a déjà été fourni par les composants React Core. Cependant, si vous souhaitez une pratique supplémentaire, créez votre propre mise en oeuvre React qui mappe au [composant Image ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) AEM. Le composant `Image` est un autre exemple de composant **content**.

### Inspect du JSON

Avant de passer au code SPA, examinez le modèle JSON fourni par AEM.

1. Accédez aux [Exemples d’images dans la bibliothèque de composants principaux](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON du composant principal d’image](./assets/map-components/image-json.png)

   Les propriétés `src`, `alt` et `title` seront utilisées pour remplir le composant `Image` SPA.

   >[!NOTE]
   >
   > D’autres propriétés d’image sont exposées (`lazyEnabled`, `widths`) qui permettent au développeur de créer un composant de chargement adaptatif et différé. Le composant créé dans ce tutoriel sera simple et **n’utilisera pas** ces propriétés avancées.

### Mise en oeuvre du composant Image

1. Créez ensuite un dossier nommé `Image` sous `ui.frontend/src/components`.
1. Sous le dossier `Image` , créez un fichier nommé `Image.js`.

   ![Fichier Image.js](./assets/map-components/image-js-file.png)

1. Ajoutez les instructions `import` suivantes à `Image.js` :

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. Ajoutez ensuite la balise `ImageEditConfig` pour déterminer à quel moment afficher l’espace réservé dans AEM :

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   L’espace réservé indique si la propriété `src` n’est pas définie.

1. Implémentez ensuite la classe `Image` :

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

   Le code ci-dessus génère un `<img>` basé sur les props `src`, `alt` et `title` transmis par le modèle JSON.

1. Ajoutez le code `MapTo` pour mapper le composant React au composant AEM :

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Notez que la chaîne `wknd-spa-react/components/image` correspond à l’emplacement du composant AEM dans `ui.apps` à l’emplacement suivant : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Créez un nouveau fichier nommé `Image.css` dans le même répertoire et ajoutez les éléments suivants :

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. Dans `Image.js`, ajoutez une référence au fichier dans la partie supérieure sous les instructions `import` :

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Ouvrez le fichier `ui.frontend/src/components/import-components.js` et ajoutez une référence au nouveau composant `Image` :

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. Dans `import-components.js`, commentez l’image du composant principal React :

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

