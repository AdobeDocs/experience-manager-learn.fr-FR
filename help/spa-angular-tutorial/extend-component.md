---
title: Étendre un composant | Prise en main de l’éditeur et de l’Angular SPA d’AEM
description: Découvrez comment étendre un composant principal existant à utiliser avec l'AEM SPA Editor. Comprendre comment ajouter des propriétés et du contenu à un composant existant est une technique puissante pour étendre les fonctionnalités d'une mise en oeuvre AEM SPA Editor. Apprenez à utiliser le modèle de délégation pour étendre les modèles Sling et les fonctionnalités de Sling Resource Merger.
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1991'
ht-degree: 4%

---


# Étendre un composant principal {#extend-component}

Découvrez comment étendre un composant principal existant à utiliser avec l&#39;AEM SPA Editor. Comprendre comment étendre un composant existant est une technique puissante pour personnaliser et développer les fonctionnalités d&#39;une mise en oeuvre AEM SPA Editor.

## Intention

1. Étendez un composant principal existant avec des propriétés et du contenu supplémentaires.
2. Comprendre le principe de base de l&#39;héritage des composants à l&#39;aide de `sling:resourceSuperType`.
3. Découvrez comment tirer parti de [Modèle de délégation](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) pour les modèles Sling afin de réutiliser la logique et les fonctionnalités existantes.

## Ce que vous allez créer

Dans ce chapitre, un nouveau composant `Card` sera créé. Le composant `Card` étend le composant [Image Core Component](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/image.html) en ajoutant des champs de contenu supplémentaires, tels qu&#39;un titre et un bouton d&#39;appel à l&#39;action, afin d&#39;exécuter le rôle d&#39;un bande-annonce pour d&#39;autres contenus dans le SPA.

![Création finale du composant Carte](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> Dans une mise en oeuvre dans le monde réel, il peut être plus approprié d&#39;utiliser simplement le [composant Teaser](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/teaser.html) puis d&#39;étendre le [composant Image Core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html) pour créer un composant `Card` en fonction des besoins du projet. Il est toujours recommandé d&#39;utiliser [les composants de base](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html) directement lorsque cela est possible.

## Conditions préalables

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

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

   Si vous utilisez [AEM 6.x](overview.md#compatibility), ajoutez le profil `classic` :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installez le package fini pour le site de référence traditionnel [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Les images fournies par [le site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) seront réutilisées sur le SPA WKND. Le package peut être installé à l’aide de [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ou vérifier le code localement en passant à la branche `Angular/extend-component-solution`.

## Implémentation initiale de la carte Inspect

Un composant Carte initial a été fourni par le code de démarrage du chapitre. Inspect est le point de départ de l’implémentation de la carte.

1. Dans l&#39;IDE de votre choix, ouvrez le module `ui.apps`.
2. Accédez à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` et vue le fichier `.content.xml`.

   ![Début de définition du composant de carte](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   La propriété `sling:resourceSuperType` pointe vers `wknd-spa-angular/components/image`, ce qui indique que le composant `Card` héritera de toutes les fonctionnalités du composant WKND SPA Image.

3. Inspectez le fichier `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml` :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Notez que le `sling:resourceSuperType` pointe vers `core/wcm/components/image/v2/image`. Cela indique que le composant WKND SPA Image hérite de toutes les fonctionnalités de l’image du composant principal.

   Connu également sous le nom de modèle de proxy [l&#39;héritage de ressources Sling est un modèle de conception puissant qui permet aux composants enfants d&#39;hériter de fonctionnalités et d&#39;étendre/remplacer le comportement si nécessaire. ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) L’héritage Sling prend en charge plusieurs niveaux d’héritage, de sorte que le nouveau composant `Card` hérite finalement des fonctionnalités de l’image du composant principal.

   De nombreuses équipes de développement s&#39;efforcent d&#39;être D.R.Y. (ne vous répétez pas). L&#39;héritage de Sling rend cela possible avec AEM.

4. Sous le dossier `card`, ouvrez le fichier `_cq_dialog/.content.xml`.

   Ce fichier est la définition de la boîte de dialogue Composant pour le composant `Card`. Si vous utilisez l&#39;héritage Sling, il est possible d&#39;utiliser les fonctionnalités de la [fusion de ressources Sling](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) pour remplacer ou étendre des parties de la boîte de dialogue. Dans cet exemple, un nouvel onglet a été ajouté à la boîte de dialogue pour capturer des données supplémentaires d’un auteur afin de renseigner le composant de carte.

   Des propriétés telles que `sling:orderBefore` permettent aux développeurs de choisir l’emplacement où insérer de nouveaux onglets ou champs de formulaire. Dans ce cas, l&#39;onglet `Text` sera inséré avant l&#39;onglet `asset`. Pour utiliser pleinement la fusion de ressources Sling, il est important de connaître la structure de noeud de boîte de dialogue d’origine pour la boîte de dialogue [Composant d’image](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Sous le dossier `card`, ouvrez le fichier `_cq_editConfig.xml`. Ce fichier détermine le comportement de glisser-déposer dans l’interface utilisateur de création AEM. Lors de l’extension du composant Image, il est important que le type de ressource corresponde au composant lui-même. Examinez le noeud `<parameters>` :

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La plupart des composants ne nécessitent pas de balise `cq:editConfig`, les descendants d’image et d’enfant du composant Image sont des exceptions.

6. Dans le commutateur IDE vers le module `ui.frontend`, accédez à `ui.frontend/src/app/components/card` :

   ![Début de composants Angular](assets/extend-component/angular-card-component-start.png)

7. Inspectez le fichier `card.component.ts`.

   Le composant a déjà été coupé pour être mappé au composant `Card` AEM en utilisant la fonction standard `MapTo`.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Examinez les trois paramètres `@Input` de la classe pour `src`, `alt` et `title`. Il s’agit des valeurs JSON attendues du composant AEM qui seront mises en correspondance avec le composant Angular.

8. Ouvrez le fichier `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   Dans cet exemple, nous avons choisi de réutiliser le composant Image d&#39;Angular existant `app-image` en transmettant simplement les paramètres `@Input` de `card.component.ts`. Plus loin dans le didacticiel, d’autres propriétés seront ajoutées et affichées.

## Mettre à jour la stratégie de modèle

Avec cette mise en oeuvre `Card` initiale, passez en revue les fonctionnalités de l’AEM SPA Editor. Pour afficher le composant `Card` initial, une mise à jour de la stratégie Modèle est nécessaire.

1. Déployez le code de démarrage sur une instance locale d’AEM, si ce n’est déjà le cas :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Accédez au modèle de page SPA à l’adresse [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Mettez à jour la stratégie du Conteneur de mise en page pour ajouter le nouveau composant `Card` en tant que composant autorisé :

   ![Mettre à jour la stratégie de Conteneur de mise en page](assets/extend-component/card-component-allowed.png)

   Enregistrez les modifications apportées à la stratégie et observez le composant `Card` en tant que composant autorisé :

   ![Composant de carte en tant que composant autorisé](assets/extend-component/card-component-allowed-layout-container.png)

## Composant Carte initial de l’auteur

Ensuite, créez le composant `Card` à l’aide de l’AEM SPA Editor.

1. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. En mode `Edit`, ajoutez le composant `Card` au `Layout Container` :

   ![Insérer un nouveau composant](assets/extend-component/insert-custom-component.png)

3. Faites glisser une image de l’outil de recherche de ressources vers le composant `Card` :

   ![Ajouter une image](assets/extend-component/card-add-image.png)

4. Ouvrez la boîte de dialogue du composant `Card` et notez l&#39;ajout d&#39;un onglet **Texte**.
5. Saisissez les valeurs suivantes dans l’onglet **Texte** :

   ![Onglet Composant de texte](assets/extend-component/card-component-text.png)

   **Chemin**  de la carte : choisissez une page sous la page d&#39;accueil SPA.

   **Texte**  CTA - &quot;En savoir plus&quot;

   **Titre**  de la carte - laisser vide

   **Obtenir le titre de la page**  liée : cochez la case pour indiquer la valeur true.

6. Mettez à jour l’onglet **Métadonnées des ressources** pour ajouter des valeurs pour **Texte de remplacement** et **Légende**.

   Actuellement, aucune modification supplémentaire n’apparaît après la mise à jour de la boîte de dialogue. Pour exposer les nouveaux champs au composant d&#39;Angular, nous devons mettre à jour le modèle Sling pour le composant `Card`.

7. Ouvrez un nouvel onglet et accédez à [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect les noeuds de contenu sous `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` pour trouver le contenu du composant `Card`.

   ![Propriétés de composant CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Observez que les propriétés `cardPath`, `ctaText`, `titleFromPage` sont conservées par la boîte de dialogue.

## Mettre à jour le modèle Sling de carte

Pour exposer en fin de compte les valeurs de la boîte de dialogue du composant au composant d’Angular, nous devons mettre à jour le modèle Sling qui renseigne le JSON pour le composant `Card`. Nous avons également l&#39;occasion de mettre en oeuvre deux logiques commerciales :

* Si `titleFromPage` devient **true**, renvoyez le titre de la page spécifiée par `cardPath` sinon renvoyez la valeur de `cardTitle` textfield.
* Renvoie la dernière date de modification de la page spécifiée par `cardPath`.

Revenez à l&#39;IDE de votre choix et ouvrez le module `core`.

1. Ouvrez le fichier `Card.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Observez que l&#39;interface `Card` étend actuellement `com.adobe.cq.wcm.core.components.models.Image` et hérite donc de toutes les méthodes de l&#39;interface `Image`. L&#39;interface `Image` étend déjà l&#39;interface `ComponentExporter` qui permet au modèle Sling d&#39;être exporté en tant que JSON et mappé par l&#39;éditeur de SPA. Par conséquent, nous n&#39;avons pas besoin d&#39;étendre explicitement l&#39;interface `ComponentExporter` comme nous l&#39;avons fait dans le chapitre [Composant personnalisé](custom-component.md).

2. Ajoutez les méthodes suivantes à l’interface :

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

3. Ouvrez `CardImpl.java`. Il s&#39;agit de l&#39;implémentation de l&#39;interface `Card.java`. Cette implémentation a déjà été partiellement bloquée pour accélérer le tutoriel.  Notez l’utilisation des annotations `@Model` et `@Exporter` pour vous assurer que le modèle Sling peut être sérialisé en tant que JSON via l’Exportateur de modèle Sling.

   `CardImpl.java` utilise également le modèle  [Délégation pour Sling ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Modelsafin d’éviter de réécrire toute la logique du composant principal Image.

4. Observez les lignes suivantes :

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   L&#39;annotation ci-dessus instanciera un objet Image nommé `image` en fonction de l&#39;héritage `sling:resourceSuperType` du composant `Card`.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Il est alors possible d&#39;utiliser simplement l&#39;objet `image` pour implémenter des méthodes définies par l&#39;interface `Image`, sans avoir à écrire la logique nous-mêmes. Cette technique est utilisée pour `getSrc()`, `getAlt()` et `getTitle()`.

5. Ensuite, implémentez la méthode `initModel()` pour lancer une variable privée `cardPage` en fonction de la valeur de `cardPath`.

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Le `@PostConstruct initModel()` est toujours appelé lorsque le modèle Sling est initialisé, c&#39;est donc une bonne occasion d&#39;initialiser des objets qui peuvent être utilisés par d&#39;autres méthodes du modèle. `pageManager` est l’un des nombreux [objets globaux adossés à Java](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) mis à la disposition des modèles Sling par l’intermédiaire de l’annotation `@ScriptVariable`. La méthode [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) prend un chemin et renvoie un objet AEM [Page](https://docs.adobe.com/content/help/fr/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) ou null si le chemin ne pointe pas vers une page valide.

   Cela initialisera la variable `cardPage`, qui sera utilisée par les autres nouvelles méthodes pour renvoyer des données sur la page liée sous-jacente.

6. Examinez les variables globales déjà mises en correspondance avec les propriétés JCR enregistrées dans la boîte de dialogue d’auteur. L&#39;annotation `@ValueMapValue` est utilisée pour effectuer automatiquement le mappage.

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

   Ces variables seront utilisées pour implémenter les méthodes supplémentaires de l&#39;interface `Card.java`.

7. Implémentez les méthodes supplémentaires définies dans l&#39;interface `Card.java` :

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
   > Vous pouvez vue le fichier [fini CardImpl.java ici](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. Ouvrez une fenêtre de terminal et déployez uniquement les mises à jour du module `core` à l&#39;aide du profil Maven `autoInstallBundle` du répertoire `core`.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility), ajoutez le profil `classic`.

9. Vue de la réponse du modèle JSON à : [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) et recherchez `wknd-spa-angular/components/card` :

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

   Notez que le modèle JSON est mis à jour avec des paires clé/valeur supplémentaires après la mise à jour des méthodes dans le modèle Sling `CardImpl`.

## Mettre à jour le composant Angular

Maintenant que le modèle JSON est renseigné avec de nouvelles propriétés pour `ctaLinkURL`, `ctaText`, `cardTitle` et `cardLastModified`, nous pouvons mettre à jour le composant Angular pour les afficher.

1. Revenez à l&#39;IDE et ouvrez le module `ui.frontend`. Si vous le souhaitez, vous pouvez début le serveur de développement webpack à partir d’une nouvelle fenêtre de terminal afin de visualiser les modifications en temps réel :

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Ouvrez `card.component.ts` à `ui.frontend/src/app/components/card/card.component.ts`. Ajoutez les annotations `@Input` supplémentaires pour capturer le nouveau modèle :

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

3. Ajoutez des méthodes pour vérifier si l&#39;appel à l&#39;action est prêt et pour renvoyer une chaîne date/heure basée sur l&#39;entrée `cardLastModified` :

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

4. Ouvrez `card.component.html` et ajoutez l&#39;annotation suivante pour afficher le titre, l&#39;appel à l&#39;action et la date de la dernière modification :

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

   Des règles Sass ont déjà été ajoutées à `card.component.scss` pour mettre en forme le titre, l&#39;appel à l&#39;action et la date de la dernière modification.

   >[!NOTE]
   >
   > Vous pouvez vue le [code du composant de carte d&#39;Angular terminé ici](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Déployez toutes les modifications apportées à l’AEM à partir de la racine du projet à l’aide de Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) pour afficher le composant mis à jour :

   ![Composant de carte mis à jour dans AEM](assets/extend-component/updated-card-in-aem.png)

7. Vous devez être en mesure de recréer le contenu existant pour créer une page semblable à ce qui suit :

   ![Création finale du composant Carte](assets/extend-component/final-authoring-card.png)

## Félicitations! {#congratulations}

Félicitations, vous avez appris à étendre un composant AEM à l’aide de la section et comment les modèles et boîtes de dialogue Sling fonctionnent avec le modèle JSON.

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ou vérifier le code localement en passant à la branche `Angular/extend-component-solution`.
