---
title: Mappage des composants SPA aux composants AEM | Prise en main de l’éditeur et de l’Angular de SPA d’AEM
description: Découvrez comment mapper les composants Angular aux composants Adobe Experience Manager (AEM) avec le SDK JS d’AEM Editor. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme pour la création d’ traditionnelle.
sub-product: sites
feature: Éditeur de SPA
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2390'
ht-degree: 2%

---


# Mappage des composants SPA aux composants AEM {#map-components}

Découvrez comment mapper les composants Angular aux composants Adobe Experience Manager (AEM) avec le SDK JS d’AEM Editor. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques sur SPA composants dans AEM Éditeur de ressources, comme pour la création d’ traditionnelle.

Ce chapitre approfondit l’API de modèle JSON AEM et la manière dont le contenu JSON exposé par un composant AEM peut être automatiquement injecté dans un composant d’Angular en tant que props.

## Intention

1. Découvrez comment mapper AEM composants à SPA composants.
2. Comprendre la différence entre les composants **Container** et **Content**.
3. Créez un composant Angular qui mappe à un composant AEM existant.

## Ce que vous allez créer

Ce chapitre examine la façon dont le composant `Text` SPA fourni est mappé au composant `Text`AEM. Un nouveau composant `Image` SPA sera créé, qui peut être utilisé dans le SPA et créé dans l’. Les fonctionnalités prêtes à l’emploi des stratégies **Conteneur de mises en page** et **Éditeur de modèles** seront également utilisées pour créer une vue un peu plus variée en apparence.

![Exemple de création finale de chapitre](./assets/map-components/final-page.png)

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Obtention du code

1. Téléchargez le point de départ de ce tutoriel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Déployez la base de code sur une instance d’AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility), ajoutez le profil `classic` :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ou extraire le code localement en passant à la branche `Angular/map-components-solution`.

## Approche de mappage

Le concept de base consiste à mapper un composant SPA à un composant AEM. AEM des composants, exécuter côté serveur, exporter du contenu dans le cadre de l’API de modèle JSON. Le contenu JSON est consommé par le SPA, en exécutant côté client dans le navigateur. Un mappage 1:1 entre les composants SPA et un composant AEM est créé.

![Présentation générale du mappage d’un composant AEM à un composant Angular](./assets/map-components/high-level-approach.png)

*Présentation générale du mappage d’un composant AEM à un composant Angular*

## Inspect du composant Texte

L’ [archétype de projet AEM](https://github.com/adobe/aem-project-archetype) fournit un composant `Text` mappé au composant [Texte AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/text.html). Il s’agit d’un exemple de composant **content**, dans la mesure où il effectue le rendu de *content* à partir d’AEM.

Voyons comment fonctionne le composant.

### Inspect du modèle JSON

1. Avant de passer au code SPA, il est important de comprendre le modèle JSON fourni par AEM. Accédez à la [bibliothèque de composants principaux](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) et affichez la page du composant Texte. La bibliothèque des composants principaux fournit des exemples de tous les composants principaux AEM.
2. Sélectionnez l’onglet **JSON** pour l’un des exemples :

   ![Modèle JSON de texte](./assets/map-components/text-json.png)

   Vous devriez voir trois propriétés : `text`, `richText` et `:type`.

   `:type` est une propriété réservée qui répertorie le  `sling:resourceType` (ou chemin) du composant AEM. La valeur `:type` est utilisée pour mapper le composant AEM au composant SPA.

   `text` et  `richText` sont des propriétés supplémentaires qui seront exposées au composant SPA.

### Inspect du composant Texte

1. Ouvrez un nouveau terminal et accédez au dossier `ui.frontend` dans le projet. Exécutez `npm install`, puis `npm start` pour démarrer le **serveur de développement webpack** :

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   Le module `ui.frontend` est actuellement configuré pour utiliser le [modèle JSON de simulation](./integrate-spa.md#mock-json).

2. Une nouvelle fenêtre de navigateur devrait s’ouvrir sur [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![Serveur de développement Webpack avec contenu factice](assets/map-components/initial-start.png)

3. Dans l’IDE de votre choix, ouvrez le projet AEM pour le SPA WKND. Développez le module `ui.frontend` et ouvrez le fichier **text.component.ts** sous `ui.frontend/src/app/components/text/text.component.ts` :

   ![Code source du composant d’Angular Text.js](assets/map-components/vscode-ide-text-js.png)

4. La première zone à inspecter est la `class TextComponent` à la ligne 35 :

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input()](https://angular.io/api/core/Input) le décorateur est utilisé pour déclarer les champs dont les valeurs sont définies via l’objet JSON mappé, examiné précédemment.

   `@HostBinding('innerHtml') get content()` est une méthode qui expose le contenu de texte créé à partir de la valeur de  `this.text`. Si le contenu est en texte enrichi (déterminé par l’indicateur `this.richText`), la sécurité intégrée de l’Angular est ignorée. L’Angular [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) est utilisé pour &quot;frotter&quot; le code HTML brut et empêcher les vulnérabilités de script intersite. La méthode est liée à la propriété `innerHtml` à l’aide du décorateur [@HostBinding](https://angular.io/api/core/HostBinding).

5. Examinez ensuite `TextEditConfig` à la ligne 24 :

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   Le code ci-dessus est chargé de déterminer quand effectuer le rendu de l’espace réservé dans l’environnement de création AEM. Si la méthode `isEmpty` renvoie **true**, l’espace réservé est rendu.

6. Enfin, jetez un coup d&#39;oeil à l&#39;appel `MapTo` à la ligne 53 ~line :

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **** MapTo est fourni par le SDK JS de l’éditeur SPA AEM (`@adobe/cq-angular-editable-components`). Le chemin `wknd-spa-angular/components/text` représente la `sling:resourceType` du composant AEM. Ce chemin est mis en correspondance avec la balise `:type` exposée par le modèle JSON observé précédemment. **** MapTo analyse la réponse du modèle JSON et transmet les valeurs correctes aux  `@Input()` variables du composant SPA.

   Vous trouverez la définition du composant `Text` AEM à l’adresse `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Testez en modifiant le fichier **en.model.json** à `ui.frontend/src/mocks/json/en.model.json`.

   À la ligne ~62, mettez à jour la première valeur `Text` pour utiliser des balises **`H1`** et **`u`** :

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Revenez au navigateur pour voir les effets servis par le **serveur de développement webpack** :

   ![Modèle de texte mis à jour](assets/map-components/updated-text-model.png)

   Essayez de faire basculer la propriété `richText` entre **true** / **false** pour voir la logique de rendu en action.

8. Inspect **text.component.html** à `ui.frontend/src/app/components/text/text.component.html`.

   Ce fichier est vide, car l’intégralité du contenu du composant est définie par la propriété `innerHTML`.

9. Inspect de **app.module.ts** à `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   **TextComponent** n’est pas explicitement inclus, mais de manière dynamique via **AEMResponsiveGridComponent** fourni par le SDK JS de l’SPA éditeur d’AEM. Par conséquent, doit être répertorié dans le tableau **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components).

## Création du composant d’image

Créez ensuite un composant d’Angular `Image` mappé à l’AEM [Composant d’image](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/image.html). Le composant `Image` est un autre exemple de composant **content**.

### Inspect du JSON

Avant de passer au code SPA, examinez le modèle JSON fourni par AEM.

1. Accédez aux [Exemples d’images dans la bibliothèque de composants principaux](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON du composant principal d’image](./assets/map-components/image-json.png)

   Les propriétés `src`, `alt` et `title` seront utilisées pour remplir le composant `Image` SPA.

   >[!NOTE]
   >
   > D’autres propriétés d’image sont exposées (`lazyEnabled`, `widths`) qui permettent au développeur de créer un composant de chargement adaptatif et différé. Le composant créé dans ce tutoriel sera simple et **n’utilisera pas** ces propriétés avancées.

2. Revenez à votre IDE et ouvrez le `en.model.json` à l’adresse `ui.frontend/src/mocks/json/en.model.json`. Puisqu’il s’agit d’un nouveau composant pour notre projet, nous devons nous moquer de l’image JSON.

   À la ligne 70, ajoutez une entrée JSON pour le modèle `image` (n’oubliez pas la virgule de fin `,` après la seconde `text_386303036`) et mettez à jour le tableau `:itemsOrder`.

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   Le projet comprend un exemple d’image à `/mock-content/adobestock-140634652.jpeg` qui sera utilisé avec le **serveur de développement webpack**.

   Vous pouvez afficher le [en.model.json complet ici](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. Ajoutez une photo de stock à afficher par le composant.

   Créez un dossier nommé **images** sous `ui.frontend/src/mocks`. Téléchargez [adobbeef-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) et placez-le dans le dossier **images** nouvellement créé. N’hésitez pas à utiliser votre propre image, si vous le souhaitez.

### Mise en oeuvre du composant Image

1. Arrêtez le **serveur de développement webpack** s’il est démarré.
2. Créez un composant Image en exécutant la commande d’Angular CLI `ng generate component` depuis le dossier `ui.frontend` :

   ```shell
   $ ng generate component components/image
   ```

3. Dans l’IDE, ouvrez **image.component.ts** à l’adresse `ui.frontend/src/app/components/image/image.component.ts` et mettez à jour comme suit :

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` est la configuration permettant de déterminer si l’espace réservé de création doit être rendu dans AEM, selon si la  `src` propriété est renseignée.

   `@Input()` de  `src`,  `alt` et  `title` sont les propriétés mappées à partir de l’API JSON.

   `hasImage()` est une méthode qui détermine si l’image doit être rendue.

   `MapTo` mappe le composant SPA au composant AEM situé à l’adresse  `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. Ouvrez **image.component.html** et mettez-le à jour comme suit :

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Cela rendra l’élément `<img>` si `hasImage` renvoie **true**.

5. Ouvrez **image.component.scss** et mettez-le à jour comme suit :

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > La règle `:host-context` est **critique** pour que l’espace réservé de l’éditeur SPA d’AEM fonctionne correctement. Tous les composants SPA destinés à être créés dans l’éditeur de page AEM auront besoin au minimum de cette règle.

6. Ouvrez `app.module.ts` et ajoutez `ImageComponent` au tableau `entryComponents` :

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Comme la balise `TextComponent`, la balise `ImageComponent` est chargée dynamiquement et doit être incluse dans le tableau `entryComponents`.

7. Démarrez le **serveur de développement webpack** pour afficher le rendu `ImageComponent`.

   ```shell
   $ npm run start:mock
   ```

   ![Image ajoutée à la maquette](assets/map-components/image-added-mock.png)

   *Image ajoutée à la SPA*

   >[!NOTE]
   >
   > **Défi bonus** : Implémentez une nouvelle méthode pour afficher la valeur de  `title` sous forme de légende sous l’image.

## Mettre à jour les stratégies dans AEM

Le composant `ImageComponent` n’est visible que dans le **serveur de développement webpack**. Ensuite, déployez le SPA mis à jour pour AEM et mettez à jour les stratégies de modèle.

1. Arrêtez le **serveur de développement webpack** et, à partir de la **racine** du projet, déployez les modifications sur AEM à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Dans l’écran AEM Démarrer, accédez à **[!UICONTROL Outils]** > **[!UICONTROL Modèles]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Sélectionnez et modifiez la **SPA Page** :

   ![Modifier SPA modèle de page](assets/map-components/edit-spa-page-template.png)

3. Sélectionnez le **Conteneur de mises en page** et cliquez sur l’icône **policy** pour modifier la stratégie :

   ![Stratégie de conteneur de mises en page](./assets/map-components/layout-container-policy.png)

4. Sous **Composants autorisés** > **Angular SPA WKND - Contenu** > vérifiez le composant **Image** :

   ![Composant d’image sélectionné](assets/map-components/check-image-component.png)

   Sous **Composants par défaut** > **Ajouter un mappage** et sélectionnez le composant **Image - Angular SPA WKND - Contenu** :

   ![Définition des composants par défaut](assets/map-components/default-components.png)

   Saisissez un **type MIME** de `image/*`.

   Cliquez sur **Terminé** pour enregistrer les mises à jour de stratégie.

5. Dans **Conteneur de mises en page**, cliquez sur l’icône **policy** pour le composant **Texte** :

   ![Icône de stratégie de composant Texte](./assets/map-components/edit-text-policy.png)

   Créez une nouvelle stratégie nommée **Texte SPA WKND**. Sous **Plugins** > **Formatage** > cochez toutes les cases pour activer des options de formatage supplémentaires :

   ![Activation du formatage de l’éditeur de texte enrichi](assets/map-components/enable-formatting-rte.png)

   Sous **Plugins** > **Styles de paragraphe** > cochez la case **Activer les styles de paragraphe** :

   ![Activer les styles de paragraphe](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Cliquez sur **Terminé** pour enregistrer la mise à jour de la stratégie.

6. Accédez à la **page d’accueil** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   Vous devez également pouvoir modifier le composant `Text` et ajouter des styles de paragraphe supplémentaires en mode **plein écran**.

   ![Modification de texte enrichi en plein écran](assets/map-components/full-screen-rte.png)

7. Vous devez également pouvoir faire glisser et déposer une image à partir de l’**outil de recherche de ressources** :

   ![Faire glisser et déposer une image](./assets/map-components/drag-drop-image.gif)

8. Ajoutez vos propres images via [AEM Assets](http://localhost:4502/assets.html/content/dam) ou installez la base de code finalisée pour le [site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) standard. Le [site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) comprend de nombreuses images qui peuvent être réutilisées sur le SPA WKND. Le package peut être installé à l’aide de [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect du conteneur de mises en page

La prise en charge du **conteneur de mises en page** est automatiquement fournie par le SDK de l’éditeur SPA d’AEM. Le **conteneur de mises en page**, comme indiqué par le nom, est un composant **conteneur**. Les composants de conteneur sont des composants qui acceptent les structures JSON qui représentent *d’autres composants* et les instancient dynamiquement.

Examinons davantage le conteneur de mises en page.

1. Dans l’IDE, ouvrez **responsive-grid.component.ts** à `ui.frontend/src/app/components/responsive-grid` :

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   `AEMResponsiveGridComponent` est implémenté dans le cadre du SDK de l’éditeur d’AEM SPA et est inclus dans le projet via `import-components`.

2. Dans un navigateur, accédez à [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![API de modèle JSON - Grille réactive](./assets/map-components/responsive-grid-modeljson.png)

   Le composant **Conteneur de mises en page** a une valeur `sling:resourceType` de `wcm/foundation/components/responsivegrid` et est reconnu par l’éditeur de SPA à l’aide de la propriété `:type`, tout comme les composants `Text` et `Image`.

   Les mêmes fonctionnalités de redimensionnement d’un composant à l’aide du [mode Mise en page](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sont disponibles avec l’éditeur de SPA.

3. Revenez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Ajoutez d’autres composants **Image** et essayez de les redimensionner à l’aide de l’option **Mise en page** :

   ![Redimensionnement d’une image en mode Mise en page](./assets/map-components/responsive-grid-layout-change.gif)

4. Ouvrez à nouveau le modèle JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) et observez la balise `columnClassNames` dans le cadre du fichier JSON :

   ![Noms des classes de cloud](./assets/map-components/responsive-grid-classnames.png)

   Le nom de classe `aem-GridColumn--default--4` indique que le composant doit comporter 4 colonnes larges basées sur une grille de 12 colonnes. Vous trouverez plus d’informations sur la [grille réactive ici](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Revenez à l’IDE et dans le module `ui.apps`, il existe une bibliothèque côté client définie à l’adresse `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. Ouvrez le fichier `less/grid.less`.

   Ce fichier détermine les points d’arrêt (`default`, `tablet` et `phone`) utilisés par le **conteneur de mises en page**. Ce fichier est conçu pour être personnalisé selon les spécifications du projet. Actuellement, les points d’arrêt sont définis sur `1200px` et `650px`.

6. Vous devez pouvoir utiliser les fonctionnalités réactives et les stratégies de texte enrichi mises à jour du composant `Text` pour créer une vue telle que celle-ci :

   ![Exemple de création finale de chapitre](assets/map-components/final-page.png)

## Félicitations !  {#congratulations}

Félicitations, vous avez appris à mapper SPA composants à AEM composants et vous avez mis en oeuvre un nouveau composant `Image`. Vous avez également eu la possibilité d’explorer les fonctionnalités réactives du **conteneur de mises en page**.

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ou extraire le code localement en passant à la branche `Angular/map-components-solution`.

### Étapes suivantes {#next-steps}

[Navigation et routage](navigation-routing.md)  : découvrez comment plusieurs vues dans la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de ressources. La navigation dynamique est mise en oeuvre à l’aide du routeur d’Angular et ajoutée à un composant d’en-tête existant.

## bonus - Persistance des configurations pour le contrôle de code source {#bonus}

Dans de nombreux cas, en particulier au début d’un projet AEM, il est utile de conserver les configurations, comme les modèles et les stratégies de contenu associées, pour le contrôle de code source. Cela garantit que tous les développeurs travaillent sur le même ensemble de contenu et de configurations et peut garantir une cohérence supplémentaire entre les environnements. Une fois qu’un projet atteint un certain niveau de maturité, la pratique de gestion des modèles peut être transmise à un groupe spécial d’utilisateurs expérimentés.

Les étapes suivantes se dérouleront à l’aide de l’IDE Visual Studio Code et de [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) mais peuvent utiliser n’importe quel outil et tout IDE que vous avez configuré pour **extraire** ou **importer** du contenu d’une instance locale d’AEM.

1. Dans l’IDE Visual Studio Code, assurez-vous que **VSCode AEM Sync** est installé via l’extension Marketplace :

   ![Synchronisation des AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Développez le module **ui.content** dans l’explorateur de projets et accédez à `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Cliquez avec le bouton droit de la souris** sur le  `templates` dossier et sélectionnez  **Importer depuis AEM serveur** :

   ![Modèle d’import VSCode](assets/map-components/import-aem-servervscode.png)

4. Répétez les étapes pour importer du contenu, mais sélectionnez le dossier **policies** situé à l’emplacement `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspect le fichier `filter.xml` situé à l’emplacement `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   Le fichier `filter.xml` est chargé d’identifier les chemins d’accès des noeuds qui seront installés avec le package. Notez la balise `mode="merge"` de chacun des filtres qui indique que le contenu existant ne sera pas modifié, seul le nouveau contenu est ajouté. Comme les auteurs de contenu peuvent mettre à jour ces chemins, il est important qu’un déploiement de code remplace **et non** le contenu. Pour plus d’informations sur l’utilisation des éléments de filtre, voir la [documentation FileVault](https://jackrabbit.apache.org/filevault/filter.html) .

   Comparez `ui.content/src/main/content/META-INF/vault/filter.xml` et `ui.apps/src/main/content/META-INF/vault/filter.xml` pour comprendre les différents noeuds gérés par chaque module.
