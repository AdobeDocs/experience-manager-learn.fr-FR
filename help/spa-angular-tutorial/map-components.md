---
title: Mappage des composants d’une application d’une seule page aux composants AEM | Prise en main de l’éditeur AEM d’application d’une seule page et Angular
description: Découvrez comment mapper des composants angulaires aux composants Adobe Experience Manager (AEM) avec le SDK JS AEM SPA Editor. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques des composants d’une application d’une seule page dans AEM éditeur d’applications d’une seule page, à l’instar de la création AEM traditionnelle.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2387'
ht-degree: 1%

---


# Mappage des composants d’une application d’une seule page aux composants AEM {#map-components}

Découvrez comment mapper des composants angulaires aux composants Adobe Experience Manager (AEM) avec le SDK JS AEM SPA Editor. Le mappage de composants permet aux utilisateurs d’effectuer des mises à jour dynamiques des composants d’une application d’une seule page dans AEM éditeur d’applications d’une seule page, à l’instar de la création AEM traditionnelle.

Ce chapitre approfondit l’API de modèle JSON AEM et explique comment le contenu JSON exposé par un composant AEM peut être automatiquement injecté dans un composant Angular en tant que props.

## Intention

1. Découvrez comment mapper des composants AEM aux composants SPA.
2. Comprenez la différence entre les composants **Conteneur** et les composants **Contenu** .
3. Créez un nouveau composant angulaire qui mappe à un composant AEM existant.

## Ce que vous allez construire

Ce chapitre examine comment le composant `Text` SPA fourni est mappé au `Text`composant AEM. Un nouveau composant `Image` d’application d’une seule page sera créé et pourra être utilisé dans l’application d’une seule page et créé dans AEM. Les fonctionnalités prêtes à l’emploi des stratégies Conteneur **de** mise en page et Editeur **de** modèle seront également utilisées pour créer une vue un peu plus variée en apparence.

![Exemple de chapitre de création finale](./assets/map-components/final-page.png)

## Conditions préalables

Examiner les outils et les instructions nécessaires pour la mise en place d&#39;un environnement [de développement](overview.md#local-dev-environment)local.

### Obtention du code

1. Téléchargez le point de départ de ce didacticiel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Déployez la base de code sur une instance AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility) , ajoutez le `classic` profil :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ou le retirer localement en passant à la branche `Angular/map-components-solution`.

## Approche de mappage

Le concept de base est de mapper un composant SPA à un composant AEM. aem composants, exécuter côté serveur, exporter du contenu dans le cadre de l’API du modèle JSON. Le contenu JSON est consommé par l’application d’une seule page, exécutant côté client dans le navigateur. Un mappage 1:1 entre les composants SPA et un composant AEM est créé.

![Présentation générale du mappage d&#39;un composant AEM à un composant angulaire](./assets/map-components/high-level-approach.png)

*Présentation générale du mappage d&#39;un composant AEM à un composant angulaire*

## inspect du composant de texte

L&#39;archétype [de projet](https://github.com/adobe/aem-project-archetype) AEM fournit un `Text` composant mappé au composant [](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/text.html)Texte AEM. Il s’agit d’un exemple de composant de **contenu** , en ce sens qu’il rend le *contenu* à partir d’AEM.

Voyons comment fonctionne le composant.

### inspect, modèle JSON

1. Avant de passer au code d’application d’une seule page, il est important de comprendre le modèle JSON fourni par AEM. Accédez à la bibliothèque [de composants](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) principaux et vue la page du composant Texte. La bibliothèque de composants principaux fournit des exemples de tous les composants principaux AEM.
2. Sélectionnez l’onglet **JSON** pour l’un des exemples suivants :

   ![Modèle JSON de texte](./assets/map-components/text-json.png)

   Trois propriétés doivent s’afficher : `text`, `richText`et `:type`.

   `:type` est une propriété réservée qui liste le `sling:resourceType` (ou chemin) du composant AEM. La valeur de `:type` est utilisée pour mapper le composant AEM au composant SPA.

   `text` et `richText` sont des propriétés supplémentaires qui seront exposées au composant SPA.

### inspect du composant Texte

1. Ouvrez un nouveau terminal et accédez au `ui.frontend` dossier à l’intérieur du projet. Exécutez `npm install` puis `npm start` pour début du serveur **de développement** webpack :

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   Le `ui.frontend` module est actuellement configuré pour utiliser le modèle [JSON](./integrate-spa.md#mock-json)simulé.

2. Une nouvelle fenêtre de navigateur s’ouvre sur [http://localhost:4200/content/wknd-spa-angular/us/en/home.html.](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![Serveur Webpack dev avec du contenu factice](assets/map-components/initial-start.png)

3. Dans l&#39;IDE de votre choix, ouvrez l&#39;AEM Project for the WKND SPA. Développez le `ui.frontend` module et ouvrez le fichier **text.component.ts** sous `ui.frontend/src/app/components/text/text.component.ts`:

   ![Code source du composant angulaire Text.js](assets/map-components/vscode-ide-text-js.png)

4. La première zone à inspecter est la `class TextComponent` à ~ligne 35 :

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

   [Le décorateur @Input()](https://angular.io/api/core/Input) est utilisé pour déclarer les champs dont les valeurs sont définies via l’objet JSON mappé, révisé précédemment.

   `@HostBinding('innerHtml') get content()` est une méthode qui expose le contenu de texte créé à partir de la valeur de `this.text`. Dans le cas où le contenu est en texte enrichi (déterminé par l&#39; `this.richText` indicateur), la sécurité intégrée d&#39;Angular est ignorée. Le [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) d&#39;Angular est utilisé pour &quot;gommer&quot; le code HTML brut et empêcher les vulnérabilités de script intersite. La méthode est liée à la `innerHtml` propriété à l’aide du décorateur [@HostBinding](https://angular.io/api/core/HostBinding) .

5. Examinez ensuite la `TextEditConfig` question à la ligne 24 :

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   Le code ci-dessus est chargé de déterminer quand rendre l’espace réservé dans l’environnement d’auteur AEM. Si la `isEmpty` méthode renvoie **true** , l’espace réservé est rendu.

6. Enfin, jetez un coup d&#39;oeil à l&#39; `MapTo` appel à la ligne 53 :

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** est fourni par l’AEM SPA Editor JS SDK (`@adobe/cq-angular-editable-components`). Le chemin `wknd-spa-angular/components/text` représente le `sling:resourceType` composant AEM. Ce chemin est mis en correspondance avec le `:type` exposé par le modèle JSON observé précédemment. **MapTo** analyse la réponse du modèle JSON et transmet les valeurs correctes aux variables `@Input()` du composant SPA.

   Vous trouverez la définition du `Text` composant AEM à l’adresse `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Testez en modifiant le fichier **en.model.json** à `ui.frontend/src/mocks/json/en.model.json`.

   À ~line 62, mettez à jour la première `Text` valeur pour utiliser une **`H1`** balise et **`u`** :

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Revenez au navigateur pour voir les effets servis par le serveur **de développement** webpack:

   ![Modèle de texte mis à jour](assets/map-components/updated-text-model.png)

   Essayez de faire basculer la `richText` propriété entre **true** / **false** pour voir la logique de rendu en action.

8. inspect **text.component.html** sur `ui.frontend/src/app/components/text/text.component.html`.

   Ce fichier est vide, car le contenu entier du composant sera défini par la `innerHTML` propriété.

9. inspect à **app.module.ts** sur `ui.frontend/src/app/app.module.ts`.

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

   Le **TextComponent** n’est pas explicitement inclus, mais dynamiquement via **AEMResponsiveGridComponent** fourni par le SDK JS de l’éditeur d’applications monopages AEM. Par conséquent, doit être répertorié dans le tableau **entryComponents** de [app.module.ts](https://angular.io/guide/entry-components) .

## Création du composant Image

Créez ensuite un composant `Image` angulaire mappé au composant [AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/image.html)Image. Le `Image` composant est un autre exemple de composant de **contenu** .

### inspect et JSON

Avant de passer au code de l’application d’une seule page, inspectez le modèle JSON fourni par AEM.

1. Accédez aux exemples [d’images de la bibliothèque](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)de composants principaux.

   ![JSON du composant principal d’image](./assets/map-components/image-json.png)

   Les propriétés de `src`, `alt`et `title` seront utilisées pour renseigner le `Image` composant SPA.

   >[!NOTE]
   >
   > D’autres propriétés Image exposées (`lazyEnabled`, `widths`) permettent au développeur de créer un composant de chargement adaptatif et différé. Le composant généré dans ce didacticiel sera simple et **n’utilisera pas** ces propriétés avancées.

2. Revenez à votre IDE et ouvrez le `en.model.json` à `ui.frontend/src/mocks/json/en.model.json`. Comme il s&#39;agit d&#39;un composant net-new pour notre projet, nous devons nous moquer de l&#39;image JSON.

   À ~line 70, ajoutez une entrée JSON pour le `image` modèle (n&#39;oubliez pas la virgule de fin `,` après la seconde `text_386303036`) et mettez à jour le `:itemsOrder` tableau.

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

   Le projet comprend un exemple d&#39;image `/mock-content/adobestock-140634652.jpeg` qui sera utilisé avec le serveur **de développement** webpack.

   Vous pouvez vue le fichier complet [en.model.json ici](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. ajoutez une photo d’un stock à afficher par le composant.

   Créez un nouveau dossier nommé **images** sous `ui.frontend/src/mocks`. Téléchargez [adobpet-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) et importez-le dans le dossier **images** que vous venez de créer. N’hésitez pas à utiliser votre propre image, si vous le souhaitez.

### Mise en oeuvre du composant Image

1. Arrêtez le serveur **de développement** webpacksi vous avez démarré.
2. Créez un composant Image en exécutant la commande `ng generate component` d’interface de ligne de commande angulaire à partir du `ui.frontend` dossier :

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

   `ImageEditConfig` est la configuration permettant de déterminer si l’espace réservé d’auteur doit être rendu dans AEM, selon si la `src` propriété est renseignée.

   `@Input()` des `src`, `alt`et `title` sont les propriétés mises en correspondance à partir de l’API JSON.

   `hasImage()` est une méthode qui détermine si l’image doit être rendue.

   `MapTo` mappe le composant SPA au composant AEM situé à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. Ouvrez **image.component.html** et mettez-le à jour comme suit :

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Le rendu de l’ `<img>` élément s’ `hasImage` effectue sur la valeur **true**.

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
   > La `:host-context` règle est **essentielle** pour que l’espace réservé de l’éditeur d’applications monopages AEM fonctionne correctement. Tous les composants d’une application d’une seule page destinés à être créés dans l’éditeur de page AEM auront besoin au minimum de cette règle.

6. Ouvrez `app.module.ts` et ajoutez le `ImageComponent` à la `entryComponents` baie :

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Comme le `TextComponent`, le `ImageComponent` est chargé dynamiquement et doit être inclus dans la `entryComponents` baie.

7. Début le serveur **de développement** webpack pour afficher le `ImageComponent` rendu.

   ```shell
   $ npm run start:mock
   ```

   ![Image ajoutée à la maquette](assets/map-components/image-added-mock.png)

   *Image ajoutée à l’application d’une seule page*

   >[!NOTE]
   >
   > **Défi**: Implémentez une nouvelle méthode pour afficher la valeur de `title` sous forme de légende sous l’image.

## Mettre à jour les stratégies dans AEM

Le `ImageComponent` composant est visible uniquement sur le serveur **de développement** webpack. Ensuite, déployez l’application d’une seule page afin d’AEM et de mettre à jour les stratégies de modèle.

1. Arrêtez le serveur **de développement** webpack **et à partir de la** racinedu projet, déployez les modifications sur AEM en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Dans l’écran Début de l’AEM, accédez à **[!UICONTROL Outils]** > **[!UICONTROL Modèles]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Sélectionnez et modifiez la page **** d’application d’une seule page :

   ![Modifier le modèle de page de l’application d’une seule page](assets/map-components/edit-spa-page-template.png)

3. Sélectionnez le Conteneur **de** mise en page et cliquez sur son icône de **stratégie** pour modifier la stratégie :

   ![Stratégie de Conteneur de mise en page](./assets/map-components/layout-container-policy.png)

4. Sous Composants **** autorisés > **WKND SPA Angular - Content** > vérifiez le composant **Image** :

   ![Composant d’image sélectionné](assets/map-components/check-image-component.png)

   Sous Composants **** par défaut > Mappage **** d’Ajoute et sélectionnez le composant **Image - Angulaire WKND SPA - Contenu** :

   ![Définition des composants par défaut](assets/map-components/default-components.png)

   Entrez un type **** mime de `image/*`.

   Cliquez sur **Terminé** pour enregistrer les mises à jour de stratégie.

5. Dans le Conteneur **de** mise en page, cliquez sur l’icône de **stratégie** du composant **Texte** :

   ![Icône de stratégie de composant de texte](./assets/map-components/edit-text-policy.png)

   Créez une nouvelle stratégie nommée **WKND SPA Text**. Sous **Plugins** > **Formatage** > cochez toutes les cases pour activer d’autres options de formatage :

   ![Activer la mise en forme RTE](assets/map-components/enable-formatting-rte.png)

   Sous **Plugins** > Styles **de** paragraphe > cochez la case **Activer les styles** de paragraphe :

   ![Activer les styles de paragraphe](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Cliquez sur **Terminé** pour enregistrer la mise à jour de la stratégie.

6. Accédez à la **page d&#39;accueil** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   Vous devez également pouvoir modifier le `Text` composant et ajouter des styles de paragraphe supplémentaires en mode **plein écran** .

   ![Modification du texte enrichi en plein écran](assets/map-components/full-screen-rte.png)

7. Vous devez également pouvoir faire glisser et déposer une image à partir de l’outil de recherche **de** ressources :

   ![Glisser-déposer d’image](./assets/map-components/drag-drop-image.gif)

8. ajoutez vos propres images par [AEM Assets](http://localhost:4502/assets.html/content/dam) ou installez la base de code finalisée pour le site [](https://github.com/adobe/aem-guides-wknd/releases/latest)de référence WKND standard. Le site [de référence](https://github.com/adobe/aem-guides-wknd/releases/latest) WKND comprend de nombreuses images qui peuvent être réutilisées sur l&#39;application d&#39;une seule page. Le package peut être installé à l’aide d’ [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## inspect le Conteneur de mise en page

Le Conteneur **de** mise en page est automatiquement pris en charge par le SDK AEM SPA Editor. Le Conteneur **de** mise en page, tel qu’indiqué par son nom, est un composant de **conteneur** . Les composants de conteneur sont des composants qui acceptent les structures JSON qui représentent *d’autres* composants et les instancient dynamiquement.

Examinons le Conteneur de mise en page plus loin.

1. Dans l&#39;IDE, ouvrez **réactif-grid.component.ts** à `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   Le `AEMResponsiveGridComponent` logiciel est mis en oeuvre dans le cadre du SDK AEM SPA Editor et est inclus dans le projet via `import-components`.

2. Dans un navigateur, accédez à [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![API du modèle JSON - Grille réactive](./assets/map-components/responsive-grid-modeljson.png)

   Le composant Conteneur **de** mise en page `sling:resourceType` possède une valeur `wcm/foundation/components/responsivegrid` de `:type` et est reconnu par l’éditeur d’applications monopages à l’aide de la propriété `Text` , tout comme les `Image` et composants.

   Les mêmes fonctionnalités de redimensionnement d’un composant à l’aide du mode [de](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) mise en page sont disponibles avec l’éditeur d’applications monopages.

3. Revenez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). ajoutez d’autres composants **d’image** et essayez de les redimensionner à l’aide de l’option **Disposition** :

   ![Redimensionner l’image en mode Mise en page](./assets/map-components/responsive-grid-layout-change.gif)

4. rouvrez le modèle JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) et observez-le `columnClassNames` dans le cadre du fichier JSON :

   ![Noms de classe de cloud](./assets/map-components/responsive-grid-classnames.png)

   Le nom de classe `aem-GridColumn--default--4` indique que le composant doit avoir une largeur de 4 colonnes sur la base d&#39;une grille de 12 colonnes. Vous trouverez plus de détails sur la grille [réactive ici](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Revenez à l&#39;IDE et dans le `ui.apps` module il y a une bibliothèque côté client définie à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. Open the file `less/grid.less`.

   Ce fichier détermine les points d’arrêt (`default`, `tablet`et `phone`) utilisés par le Conteneur **** de mise en page. Ce fichier est destiné à être personnalisé selon les spécifications du projet. Actuellement, les points d’arrêt sont définis sur `1200px` et `650px`.

6. Vous devez pouvoir utiliser les fonctionnalités réactives et les stratégies de texte enrichi mises à jour du `Text` composant pour créer une vue du type suivant :

   ![Exemple de chapitre de création finale](assets/map-components/final-page.png)

## Félicitations ! {#congratulations}

Félicitations, vous avez appris à mapper des composants SPA à des composants AEM et vous avez mis en oeuvre un nouveau `Image` composant. Vous avez également la possibilité d’explorer les fonctionnalités réactives du Conteneur **de** mise en page.

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ou le retirer localement en passant à la branche `Angular/map-components-solution`.

### Étapes suivantes {#next-steps}

[Navigation et Routage](navigation-routing.md) : découvrez comment plusieurs vues de l’application d’une seule page peuvent être prises en charge en mappant des pages AEM avec le SDK de l’éditeur d’une seule page. La navigation dynamique est mise en oeuvre à l&#39;aide du routeur angulaire et ajoutée à un composant d&#39;en-tête existant.

## Bonus - Configuration persistante du contrôle de code source {#bonus}

Dans de nombreux cas, en particulier au début d&#39;un projet AEM, il est important de conserver les configurations, comme les modèles et les stratégies de contenu connexes, pour contrôler la source. Ceci garantit que tous les développeurs travaillent sur le même ensemble de contenu et de configurations et peut garantir une cohérence supplémentaire entre les environnements. Une fois qu&#39;un projet atteint un certain niveau de maturité, la pratique de gestion des modèles peut être transmise à un groupe spécial d&#39;utilisateurs de la puissance.

Les prochaines étapes se dérouleront à l&#39;aide de l&#39;IDE du code Visual Studio et de la synchronisation [](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) VSCode AEM, mais elles peuvent être effectuées à l&#39;aide de n&#39;importe quel outil et de tout IDE que vous avez configuré pour **extraire** ou **importer** du contenu d&#39;une instance locale d&#39;AEM.

1. Dans l’IDE du code Visual Studio, assurez-vous que **VSCode AEM Sync** est installé via l’extension Marketplace :

   ![Synchronisation des AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Développez le module **ui.content** dans l&#39;explorateur de projets et accédez à `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Cliquez avec le bouton droit de la souris sur** le `templates` dossier et sélectionnez **Importer à partir du serveur** AEM :

   ![Modèle d&#39;import VSCode](assets/map-components/import-aem-servervscode.png)

4. Répétez les étapes pour importer du contenu, mais sélectionnez le dossier **policies** situé dans `/conf/wknd-spa-angular/settings/wcm/templates/policies`.

5. inspect le `filter.xml` fichier situé dans `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Le `filter.xml` fichier est chargé d&#39;identifier les chemins d&#39;accès des noeuds qui seront installés avec le package. Notez que `mode="merge"` sur chacun des filtres indique que le contenu existant ne sera pas modifié, seul le nouveau contenu sera ajouté. Les auteurs de contenu pouvant mettre à jour ces chemins, il est important qu’un déploiement de code **ne remplace pas** le contenu. Pour plus d&#39;informations sur l&#39;utilisation des éléments de filtre, consultez la documentation [](https://jackrabbit.apache.org/filevault/filter.html) FileVault.

   Comparer `ui.content/src/main/content/META-INF/vault/filter.xml` et `ui.apps/src/main/content/META-INF/vault/filter.xml` comprendre les différents noeuds gérés par chaque module.
