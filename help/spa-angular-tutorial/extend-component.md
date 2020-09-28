---
title: Étendre un composant | Prise en main de l’éditeur AEM d’application d’une seule page et Angular
description: Découvrez comment étendre un composant principal existant à utiliser avec l’éditeur d’applications monopages AEM. Comprendre comment ajouter des propriétés et du contenu à un composant existant est une technique puissante permettant d’étendre les fonctionnalités d’une mise en oeuvre AEM SPA Editor. Apprenez à utiliser le modèle de délégation pour étendre les modèles Sling et les fonctionnalités de Sling Resource Merger.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1984'
ht-degree: 2%

---


# Étendre un composant principal {#extend-component}

Découvrez comment étendre un composant principal existant à utiliser avec l’éditeur d’applications monopages AEM. Comprendre comment étendre un composant existant est une technique puissante permettant de personnaliser et d’étendre les fonctionnalités d’une mise en oeuvre AEM SPA Editor.

## Intention

1. Étendez un composant principal existant avec des propriétés et du contenu supplémentaires.
2. Comprendre le principe de base de l&#39;héritage des composants avec l&#39;utilisation de `sling:resourceSuperType`.
3. Découvrez comment tirer parti du modèle de [délégation](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) pour les modèles Sling pour réutiliser la logique et les fonctionnalités existantes.

## Ce que vous allez construire

Dans ce chapitre, un nouveau `Card` composant sera créé. Le `Card` composant étend le composant [principal](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/image.html) Image en ajoutant des champs de contenu supplémentaires, tels qu’un titre et un bouton d’appel à l’action, afin d’exécuter le rôle d’une bande annonce pour d’autres contenus dans l’application d’une seule page.

![Création finale du composant Carte](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> Dans une mise en oeuvre dans le monde réel, il peut être plus approprié d&#39;utiliser simplement le composant [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html) Teaser, puis d&#39;étendre le composant [principal](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/image.html) Image pour en faire un `Card` composant en fonction des besoins du projet. Il est toujours recommandé d’utiliser les composants [](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html) principaux directement lorsque cela est possible.

## Conditions préalables

Examiner les outils et les instructions nécessaires à la mise en place d&#39;un environnement [de développement](overview.md#local-dev-environment)local.

### Obtention du code

1. Téléchargez le point de départ de ce didacticiel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Déployez la base de code sur une instance AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility) , ajoutez le `classic` profil :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installez le package fini pour le site [de référence](https://github.com/adobe/aem-guides-wknd/releases/latest)WKND traditionnel. Les images fournies par le site [de référence](https://github.com/adobe/aem-guides-wknd/releases/latest) deWKND seront réutilisées sur l&#39;application WKND SPA. Le package peut être installé à l’aide d’ [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ou le retirer localement en passant à la branche `Angular/extend-component-solution`.

## Implémentation initiale de la carte Inspect

Un composant Carte initial a été fourni par le code de démarrage du chapitre. inspect est le point de départ de l’implémentation de la carte.

1. Dans l&#39;IDE de votre choix, ouvrez le `ui.apps` module.
2. Accédez au `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` fichier et vue le `.content.xml` fichier.

   ![Début de définition du composant de carte](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   La propriété `sling:resourceSuperType` indique `wknd-spa-angular/components/image` que le `Card` composant héritera de toutes les fonctionnalités du composant WKND SPA Image.

3. inspect le fichier `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Notez que le `sling:resourceSuperType` pointeur est `core/wcm/components/image/v2/image`. Cela indique que le composant WKND SPA Image hérite de toutes les fonctionnalités de l’image du composant principal.

   Connu également sous le nom de modèle [de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) proxy L&#39;héritage de ressources Sling est un modèle de conception puissant qui permet aux composants enfants d&#39;hériter de fonctionnalités et d&#39;étendre/remplacer le comportement si nécessaire. L’héritage Sling prend en charge plusieurs niveaux d’héritage, de sorte que le nouveau `Card` composant hérite finalement des fonctionnalités de l’image du composant principal.

   De nombreuses équipes de développement s&#39;efforcent d&#39;être D.R.Y. (ne vous répétez pas). L&#39;héritage de Sling rend cela possible avec AEM.

4. Sous le `card` dossier, ouvrez le fichier `_cq_dialog/.content.xml`.

   Ce fichier est la définition de la boîte de dialogue Composant pour le `Card` composant. Si vous utilisez l&#39;héritage Sling, il est possible d&#39;utiliser les fonctionnalités de la fusion [de ressources](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) Sling pour remplacer ou étendre des parties de la boîte de dialogue. Dans cet exemple, un nouvel onglet a été ajouté à la boîte de dialogue pour capturer des données supplémentaires d’un auteur afin de renseigner le composant de carte.

   Des propriétés telles que `sling:orderBefore` les propriétés permettent au développeur de choisir l’emplacement où insérer de nouveaux onglets ou champs de formulaire. Dans ce cas, l&#39; `Text` onglet est inséré avant l&#39; `asset` onglet. Pour utiliser pleinement la fusion de ressources Sling, il est important de connaître la structure de noeud de boîte de dialogue d’origine pour la boîte de dialogue [du composant](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)Image.

5. Sous le `card` dossier, ouvrez le fichier `_cq_editConfig.xml`. Ce fichier détermine le comportement de glisser-déposer dans l’interface utilisateur de création AEM. Lors de l’extension du composant Image, il est important que le type de ressource corresponde au composant lui-même. Vérifiez le `<parameters>` noeud :

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La plupart des composants ne nécessitent pas de balise `cq:editConfig`, les descendants d’image et d’enfant du composant Image sont des exceptions.

6. Dans le commutateur IDE vers le `ui.frontend` module, accédez à `ui.frontend/src/app/components/card`:

   ![Début de composant angulaire](assets/extend-component/angular-card-component-start.png)

7. inspect le fichier `card.component.ts`.

   Le composant a déjà été assemblé pour être mappé au composant AEM `Card` à l&#39;aide de la fonction standard `MapTo` .

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Examinez les trois `@Input` paramètres de la classe pour `src`, `alt`et `title`. Il s’agit des valeurs JSON attendues du composant AEM qui sera mappé au composant angulaire.

8. Open the file `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   Dans cet exemple, nous avons choisi de réutiliser le composant Image angulaire existant `app-image` en transmettant simplement les `@Input` paramètres de `card.component.ts`. Plus loin dans le didacticiel, d’autres propriétés seront ajoutées et affichées.

## Mettre à jour la stratégie de modèle

Avec cette première `Card` mise en oeuvre, passez en revue les fonctionnalités de l’éditeur d’applications monopages AEM. Pour afficher le composant initial, une mise à jour de la stratégie Modèle est nécessaire. `Card`

1. Déployez le code de démarrage sur une instance locale d’AEM, si ce n’est déjà le cas :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Accédez au modèle de page de l’application d’une seule page à l’adresse [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Mettez à jour la stratégie du Conteneur de mise en page pour ajouter le nouveau `Card` composant en tant que composant autorisé :

   ![Mettre à jour la stratégie de Conteneur de mise en page](assets/extend-component/card-component-allowed.png)

   Enregistrez les modifications apportées à la stratégie et observez le `Card` composant en tant que composant autorisé :

   ![Composant de carte en tant que composant autorisé](assets/extend-component/card-component-allowed-layout-container.png)

## Composant Carte initial de l’auteur

Créez ensuite le `Card` composant à l’aide de l’éditeur d’applications monopages AEM.

1. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. En `Edit` mode, ajoutez le `Card` composant au `Layout Container`:

   ![Insérer un nouveau composant](assets/extend-component/insert-custom-component.png)

3. Drag and drop an image from the Asset finder onto the `Card` component:

   ![ajouter une image](assets/extend-component/card-add-image.png)

4. Ouvrez la boîte de dialogue `Card` du composant et notez l’ajout d’un onglet **Texte** .
5. Saisissez les valeurs suivantes dans l’onglet **Texte** :

   ![Onglet Composant de texte](assets/extend-component/card-component-text.png)

   **Chemin** de la carte : choisissez une page sous la page d&#39;accueil de l&#39;application d&#39;une seule page.

   **Texte** CTA - &quot;En savoir plus&quot;

   **Titre** de la carte - laisser vide

   **Obtenir le titre de la page** liée : cochez la case pour indiquer la valeur true.

6. Mettez à jour l’onglet Métadonnées **du** fichier pour ajouter des valeurs pour le texte **de** remplacement et la **légende**.

   Actuellement, aucune modification supplémentaire n’apparaît après la mise à jour de la boîte de dialogue. Pour exposer les nouveaux champs au composant angulaire, nous devons mettre à jour le modèle Sling pour le `Card` composant.

7. Ouvrez un nouvel onglet et accédez à [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). inspect les noeuds de contenu sous `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` pour trouver le contenu du `Card` composant.

   ![Propriétés de composant CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Observez que les propriétés `cardPath``ctaText`, `titleFromPage` sont conservées par la boîte de dialogue.

## Mettre à jour le modèle Sling de carte

Pour exposer en fin de compte les valeurs de la boîte de dialogue du composant au composant angulaire, nous devons mettre à jour le modèle Sling qui renseigne le JSON pour le `Card` composant. Nous avons également l&#39;occasion de mettre en oeuvre deux logiques commerciales :

* Si `titleFromPage` la valeur est **true**, renvoie le titre de la page spécifié par `cardPath` sinon renvoie la valeur de `cardTitle` textfield.
* Renvoie la dernière date de modification de la page spécifiée par `cardPath`.

Revenez à l&#39;IDE de votre choix et ouvrez le `core` module.

1. Open the file `Card.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Observez que l’ `Card` interface s’étend actuellement `com.adobe.cq.wcm.core.components.models.Image` et hérite donc de toutes les méthodes de l’ `Image` interface. L’ `Image` interface étend déjà l’ `ComponentExporter` interface qui permet d’exporter le modèle Sling en tant que JSON et de le mapper par l’éditeur SPA. Par conséquent, nous n&#39;avons pas besoin d&#39;étendre explicitement `ComponentExporter` l&#39;interface comme nous l&#39;avons fait dans le chapitre [Composant](custom-component.md)personnalisé.

2. ajoutez les méthodes suivantes à l’interface :

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   Ces méthodes seront exposées via l’API du modèle JSON et transmises au composant Angular.

3. Ouvrez `CardImpl.java`. Il s&#39;agit de l&#39;implémentation de l&#39; `Card.java` interface. Cette implémentation a déjà été partiellement bloquée pour accélérer le tutoriel.  Notez l’utilisation des `@Model` annotations et `@Exporter` des annotations pour vous assurer que le modèle Sling peut être sérialisé en tant que JSON via Sling Model Exporter.

   `CardImpl.java` utilise également le modèle [Délégation pour les modèles](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Sling pour éviter de réécrire toute la logique du composant Image core.

4. Observez les lignes suivantes :

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   L’annotation ci-dessus instanciera un objet Image nommé `image` en fonction de l’ `sling:resourceSuperType` héritage du `Card` composant.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Il est alors possible d&#39;utiliser simplement l&#39; `image` objet pour mettre en oeuvre des méthodes définies par l&#39; `Image` interface, sans avoir à écrire la logique nous-mêmes. Cette technique est utilisée pour `getSrc()`, `getAlt()` et `getTitle()`.

5. Ensuite, implémentez la `initModel()` méthode pour lancer une variable privée `cardPage` en fonction de la valeur de `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Le `@PostConstruct initModel()` sera toujours appelé lorsque le modèle Sling est initialisé, c&#39;est donc une bonne occasion d&#39;initialiser des objets qui peuvent être utilisés par d&#39;autres méthodes du modèle. Il `pageManager` s’agit de l’un des nombreux objets [globaux adossés à](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) Java mis à la disposition des modèles Sling via l’ `@ScriptVariable` annotation. La méthode [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) prend un chemin et renvoie un objet [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) AEM ou null si le chemin ne pointe pas vers une page valide.

   Cette opération initialise la `cardPage` variable, qui sera utilisée par les autres nouvelles méthodes pour renvoyer des données sur la page liée sous-jacente.

6. Examinez les variables globales déjà mises en correspondance avec les propriétés JCR enregistrées dans la boîte de dialogue d’auteur. L’ `@ValueMapValue` annotation est utilisée pour effectuer automatiquement le mappage.

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   Ces variables seront utilisées pour implémenter les méthodes supplémentaires de l’ `Card.java` interface.

7. Implémentez les méthodes supplémentaires définies dans l’ `Card.java` interface :

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > Vous pouvez vue ici [le fichier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java)terminé CardImpl.java.

8. Ouvrez une fenêtre de terminal et déployez uniquement les mises à jour du `core` module à l&#39;aide du `autoInstallBundle` profil Maven du `core` répertoire.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility) , ajoutez le `classic` profil.

9. Vue de la réponse du modèle JSON à : [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) et recherchez `wknd-spa-angular/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-angular/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-angular/us/en/home/page-1.html",
       ":type": "wknd-spa-angular/components/card"
   }
   ```

   Notez que le modèle JSON est mis à jour avec des paires clé/valeur supplémentaires après la mise à jour des méthodes dans le modèle `CardImpl` Sling.

## Mettre à jour le composant angulaire

Maintenant que le modèle JSON est renseigné avec de nouvelles propriétés pour `ctaLinkURL`, `ctaText`, `cardTitle` `cardLastModified` et nous pouvons mettre à jour le composant Angular pour les afficher.

1. Revenez à l&#39;IDE et ouvrez le `ui.frontend` module. Si vous le souhaitez, vous pouvez début le serveur de développement webpack à partir d’une nouvelle fenêtre de terminal afin de visualiser les modifications en temps réel :

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Ouvrez `card.component.ts` à `ui.frontend/src/app/components/card/card.component.ts`. ajoutez les `@Input` annotations supplémentaires pour capturer le nouveau modèle :

   ```diff
   export class CardComponent implements OnInit {
   
        @Input() src: string;
        @Input() alt: string;
        @Input() title: string;
   +    @Input() cardTitle: string;
   +    @Input() cardLastModified: number;
   +    @Input() ctaLinkURL: string;
   +    @Input() ctaText: string;
   ```

3. ajoutez des méthodes pour vérifier si l&#39;appel à l&#39;action est prêt et pour renvoyer une chaîne de date/heure en fonction de l&#39; `cardLastModified` entrée :

   ```js
   export class CardComponent implements OnInit {
       ...
       get hasCTA(): boolean {
           return this.ctaLinkURL && this.ctaLinkURL.trim().length > 0 && this.ctaText && this.ctaText.trim().length > 0;
       }
   
       get lastModifiedDate(): string {
           const lastModifiedDate = this.cardLastModified ? new Date(this.cardLastModified) : null;
   
           if (lastModifiedDate) {
           return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

4. Ouvrez `card.component.html` et ajoutez l’annotation suivante pour afficher le titre, l’appel à l’action et la date de la dernière modification :

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
       <div class="card__content">
           <h2 class="card__title">
               {{cardTitle}}
               <span class="card__lastmod" *ngIf="lastModifiedDate">{{lastModifiedDate}}</span>
           </h2>
           <div class="card__action-container" *ngIf="hasCTA">
               <a [routerLink]="ctaLinkURL" class="card__action-link" [title]="ctaText">
                   {{ctaText}}
               </a>
           </div>
       </div>
   </div>
   ```

   Des règles Sass ont déjà été ajoutées à `card.component.scss` pour mettre en forme le titre, l’appel à l’action et la date de la dernière modification.

   >[!NOTE]
   >
   > Vous pouvez vue ici [le code du composant de carte](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card)angulaire terminé.

5. Déployez toutes les modifications apportées à l’AEM à partir de la racine du projet à l’aide de Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) pour afficher le composant mis à jour :

   ![Composant de carte mis à jour dans AEM](assets/extend-component/updated-card-in-aem.png)

7. Vous devez être en mesure de recréer le contenu existant pour créer une page semblable à ce qui suit :

   ![Création finale du composant Carte](assets/extend-component/final-authoring-card.png)

## Félicitations ! {#congratulations}

Félicitations, vous avez appris à étendre un composant AEM à l’aide de la section et comment les modèles et boîtes de dialogue Sling fonctionnent avec le modèle JSON.

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ou le retirer localement en passant à la branche `Angular/extend-component-solution`.
