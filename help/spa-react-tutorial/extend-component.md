---
title: Étendre un composant | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment étendre un composant principal existant à utiliser avec l’éditeur SPA d’AEM. Comprendre comment ajouter des propriétés et du contenu à un composant existant est une technique puissante pour étendre les fonctionnalités d’une mise en oeuvre d’AEM SPA éditeur. Découvrez comment utiliser le modèle de délégation pour étendre les modèles Sling et les fonctionnalités de Sling Resource Merger.
sub-product: sites
feature: Éditeur SPA, composants principaux
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1974'
ht-degree: 4%

---


# Étendre un composant principal {#extend-component}

Découvrez comment étendre un composant principal existant à utiliser avec l’éditeur SPA d’AEM. Comprendre comment étendre un composant existant est une technique puissante pour personnaliser et étendre les fonctionnalités d’une implémentation d’AEM SPA éditeur.

## Intention

1. Étendez un composant principal existant avec des propriétés et du contenu supplémentaires.
2. Comprenez les principes de base de l’héritage des composants à l’aide de `sling:resourceSuperType`.
3. Découvrez comment tirer parti de [Modèle de délégation](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) pour les modèles Sling afin de réutiliser la logique et les fonctionnalités existantes.

## Ce que vous allez créer

Dans ce chapitre, un nouveau composant `Card` sera créé. Le composant `Card` étend le [composant principal de l’image](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/image.html) en ajoutant des champs de contenu supplémentaires, tels qu’un titre et un bouton Appel à l’action, afin d’effectuer le rôle d’un teaser pour d’autres contenus dans le SPA.

![Création finale du composant Carte](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> Dans une mise en oeuvre réelle, il peut être plus approprié d’utiliser simplement le [composant Teaser](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/components/teaser.html) puis d’étendre le [composant principal de l’image](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html) pour créer un composant `Card` en fonction des exigences du projet. Il est toujours recommandé d’utiliser les [composants principaux](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html) directement lorsque cela est possible.

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Obtention du code

1. Téléchargez le point de départ de ce tutoriel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/extend-component-start
   ```

2. Déployez la base de code sur une instance d’AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility), ajoutez le profil `classic` :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installez le package terminé pour le [site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) traditionnel. Les images fournies par [le site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) seront réutilisées sur le SPA WKND. Le package peut être installé à l’aide de [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) ou extraire le code localement en passant à la branche `React/extend-component-solution`.

## Mise en oeuvre initiale de la carte Inspect

Un composant Carte initial a été fourni par le code de démarrage du chapitre. Inspect est le point de départ de la mise en oeuvre de la carte.

1. Dans l’IDE de votre choix, ouvrez le module `ui.apps`.
2. Accédez à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/card` et consultez le fichier `.content.xml`.

   ![Démarrage de AEM définition du composant Carte](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   La propriété `sling:resourceSuperType` pointe vers `wknd-spa-react/components/image`, indiquant que le composant `Card` héritera de toutes les fonctionnalités du composant Image WKND SPA.

3. Inspectez le fichier `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image/.content.xml` :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Notez que le `sling:resourceSuperType` pointe vers `core/wcm/components/image/v2/image`. Cela indique que le composant d’image WKND SPA hérite de toutes les fonctionnalités de l’image du composant principal.

   Connu également sous le nom de [modèle de proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) l’héritage de ressources Sling est un modèle de conception puissant qui permet aux composants enfants d’hériter de fonctionnalités et d’étendre/remplacer le comportement si nécessaire. L’héritage Sling prend en charge plusieurs niveaux d’héritage. Par conséquent, le nouveau composant `Card` hérite des fonctionnalités de l’image du composant principal.

   De nombreuses équipes de développement s&#39;efforcent d&#39;être DE (ne vous répétez pas). L’héritage Sling rend cela possible avec AEM.

4. Sous le dossier `card` , ouvrez le fichier `_cq_dialog/.content.xml`.

   Ce fichier est la définition de la boîte de dialogue Composant pour le composant `Card`. Si vous utilisez l’héritage Sling, il est possible d’utiliser les fonctionnalités de [Sling Resource Merger](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) pour remplacer ou étendre des parties de la boîte de dialogue. Dans cet exemple, un nouvel onglet a été ajouté à la boîte de dialogue pour capturer des données supplémentaires d’un auteur afin de renseigner le composant Carte.

   Les propriétés telles que `sling:orderBefore` permettent aux développeurs de choisir où insérer de nouveaux onglets ou champs de formulaire. Dans ce cas, l&#39;onglet `Text` sera inséré avant l&#39;onglet `asset`. Pour utiliser pleinement Sling Resource Merger, il est important de connaître la structure de noeud de boîte de dialogue d’origine pour la [boîte de dialogue du composant Image](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Sous le dossier `card` , ouvrez le fichier `_cq_editConfig.xml`. Ce fichier détermine le comportement de glisser-déposer dans l’interface utilisateur de création d’AEM. Lors de l’extension du composant Image, il est important que le type de ressource corresponde au composant lui-même. Vérifiez le noeud `<parameters>` :

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La plupart des composants ne nécessitent pas de balise `cq:editConfig`, les descendants d’images et enfants du composant Image sont des exceptions.

6. Dans l’IDE, basculez vers le module `ui.frontend`, en accédant à `ui.frontend/src/components/Card` :

   ![Démarrage du composant React](assets/extend-component/react-card-component-start.png)

7. Inspectez le fichier `Card.js`.

   Le composant a déjà été bouché pour être mappé au composant `Card` AEM à l’aide de la fonction `MapTo` standard.

   ```js
   MapTo('wknd-spa-react/components/card')(Card, CardEditConfig);
   ```

8. Inspect de la méthode `get imageContent()` :

   ```js
    get imageContent() {
       return (
           <div className="Card__image">
               <Image {...this.props} />
           </div>)
   }
   ```

   Dans cet exemple, nous avons choisi de réutiliser le composant React Image `Image` existant en transmettant simplement la balise `this.props` du composant `Card`. Plus loin dans le tutoriel, la méthode `get bodyContent()` sera implémentée pour afficher un titre, une date et un bouton d’appel à l’action.

## Mise à jour de la stratégie de modèle

Avec cette mise en oeuvre `Card` initiale, passez en revue les fonctionnalités de l’éditeur SPA d’AEM. Pour afficher le composant `Card` initial, une mise à jour de la stratégie Modèle est nécessaire.

1. Déployez le code de démarrage sur une instance locale d’AEM, le cas échéant :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Accédez au modèle de page SPA à l’adresse [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. Mettez à jour la stratégie du conteneur de mises en page pour ajouter le nouveau composant `Card` en tant que composant autorisé :

   ![Mettre à jour la stratégie de conteneur de mises en page](assets/extend-component/card-component-allowed.png)

   Enregistrez les modifications apportées à la stratégie et observez le composant `Card` comme composant autorisé :

   ![Composant Carte en tant que composant autorisé](assets/extend-component/card-component-allowed-layout-container.png)

## Composant Carte initial de création

Créez ensuite le composant `Card` à l’aide de l’éditeur SPA d’AEM.

1. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
2. En mode `Edit` , ajoutez le composant `Card` à `Layout Container` :

   ![Insérer un nouveau composant](assets/extend-component/insert-card-component.png)

3. Faites glisser et déposez une image de l’outil de recherche de ressources sur le composant `Card` :

   ![Ajouter une image](assets/extend-component/card-add-image.png)

4. Ouvrez la boîte de dialogue du composant `Card` et notez l’ajout d’un onglet **Texte** .
5. Saisissez les valeurs suivantes dans l&#39;onglet **Texte** :

   ![Onglet Composant Texte](assets/extend-component/card-component-text.png)

   **Chemin d’accès aux cartes**  : choisissez une page sous la page d’accueil SPA.

   **Texte CTA**  - &quot;En savoir plus&quot;

   **Titre de carte**  - laissez le champ vide

   **Obtenir le titre de la page**  liée : cochez la case pour indiquer la valeur true.

6. Mettez à jour l’onglet **Métadonnées de ressource** pour ajouter des valeurs pour **Texte de remplacement** et **Légende**.

   Actuellement, aucune modification supplémentaire n’apparaît après la mise à jour de la boîte de dialogue. Pour exposer les nouveaux champs au composant React, nous devons mettre à jour le modèle Sling du composant `Card`.

7. Ouvrez un nouvel onglet et accédez à [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-react/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect les noeuds de contenu sous `/content/wknd-spa-react/us/en/home/jcr:content/root/responsivegrid` pour trouver le contenu du composant `Card`.

   ![Propriétés du composant CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Notez que les propriétés `cardPath`, `ctaText`, `titleFromPage` sont conservées par la boîte de dialogue.

## Mettre à jour le modèle Sling de carte

Pour exposer finalement les valeurs de la boîte de dialogue du composant au composant React, nous devons mettre à jour le modèle Sling qui renseigne le fichier JSON pour le composant `Card`. Nous avons également la possibilité de mettre en oeuvre deux logiques commerciales :

* Si `titleFromPage` devient **true**, renvoie le titre de la page spécifié par `cardPath` sinon renvoie la valeur de `cardTitle` textfield.
* Renvoie la date de dernière modification de la page spécifiée par `cardPath`.

Revenez à l’IDE de votre choix et ouvrez le module `core`.

1. Ouvrez le fichier `Card.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/Card.java`.

   Notez que l’interface `Card` étend actuellement `com.adobe.cq.wcm.core.components.models.Image` et hérite donc de toutes les méthodes de l’interface `Image`. L’interface `Image` étend déjà l’interface `ComponentExporter` qui permet d’exporter le modèle Sling au format JSON et de le mapper par l’éditeur de SPA. Par conséquent, nous n’avons pas besoin d’étendre explicitement l’interface `ComponentExporter` comme nous l’avons fait dans le [chapitre Composant personnalisé](custom-component.md).

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

   Ces méthodes seront exposées via l’API de modèle JSON et transmises au composant React.

3. Ouvrez `CardImpl.java`. Il s’agit de l’implémentation de l’interface `Card.java`. Cette implémentation a déjà été partiellement bloquée pour accélérer le tutoriel.  Notez l’utilisation des annotations `@Model` et `@Exporter` pour vous assurer que le modèle Sling peut être sérialisé en tant que JSON via l’exportateur de modèle Sling.

   `CardImpl.java` utilise également le modèle  [Délégation pour les ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) modèles Sling afin d’éviter de réécrire toute la logique à partir du composant principal Image .

4. Observez les lignes suivantes :

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   L’annotation ci-dessus instancie un objet Image nommé `image` en fonction de l’héritage `sling:resourceSuperType` du composant `Card`.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Il est alors possible d’utiliser simplement l’objet `image` pour implémenter des méthodes définies par l’interface `Image`, sans avoir à écrire la logique nous-mêmes. Cette technique est utilisée pour `getSrc()`, `getAlt()` et `getTitle()`.

5. Implémentez ensuite la méthode `initModel()` pour lancer une variable privée `cardPage` en fonction de la valeur de `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   `@PostConstruct initModel()` sera toujours appelé lorsque le modèle Sling est initialisé. C’est donc une bonne occasion d’initialiser des objets qui peuvent être utilisés par d’autres méthodes du modèle. `pageManager` est l’un des nombreux [objets globaux pris en charge par Java](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) mis à la disposition des modèles Sling via l’annotation `@ScriptVariable`. La méthode [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) prend un chemin et renvoie un objet [Page](https://docs.adobe.com/content/help/fr/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) AEM ou null si le chemin ne pointe pas vers une page valide.

   Cela initialisera la variable `cardPage`, qui sera utilisée par les autres nouvelles méthodes pour renvoyer les données sur la page liée sous-jacente.

6. Passez en revue les variables globales déjà mappées aux propriétés JCR enregistrées dans la boîte de dialogue de création. L’annotation `@ValueMapValue` est utilisée pour effectuer automatiquement le mappage.

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

   Ces variables seront utilisées pour implémenter les méthodes supplémentaires de l’interface `Card.java`.

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
   > Vous pouvez afficher le fichier [terminé CardImpl.java ici](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CardImpl.java).

8. Ouvrez une fenêtre de terminal et déployez uniquement les mises à jour du module `core` à l’aide du profil Maven `autoInstallBundle` du répertoire `core`.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility), ajoutez le profil `classic`.

9. Affichez la réponse du modèle JSON à l’adresse : [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) et recherchez `wknd-spa-react/components/card` :

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-react/us/en/home/page-1.html",
       ":type": "wknd-spa-react/components/card"
   }
   ```

   Notez que le modèle JSON est mis à jour avec des paires clé/valeur supplémentaires après la mise à jour des méthodes dans le modèle Sling `CardImpl`.

## Mettre à jour le composant React

Maintenant que le modèle JSON est renseigné avec de nouvelles propriétés pour `ctaLinkURL`, `ctaText`, `cardTitle` et `cardLastModified`, nous pouvons mettre à jour le composant React pour les afficher.

1. Revenez à l’IDE et ouvrez le module `ui.frontend`. Vous pouvez éventuellement démarrer le serveur de développement webpack à partir d’une nouvelle fenêtre de terminal pour afficher les modifications en temps réel :

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Ouvrez `Card.js` à `ui.frontend/src/components/Card/Card.js`.
3. Ajoutez la méthode `get ctaButton()` pour effectuer le rendu de l’appel à l’action :

   ```js
   import {Link} from "react-router-dom";
   ...
   
   export default class Card extends Component {
   
       get ctaButton() {
           if(this.props && this.props.ctaLinkURL && this.props.ctaText) {
               return (
                   <div className="Card__action-container">
                       <Link to={this.props.ctaLinkURL} title={this.props.title}
                           className="Card__action-link">
                           {this.props.ctaText}
                       </Link>
                   </div>
               );
           }
   
           return null;
       }
       ...
   }
   ```

4. Ajoutez une méthode pour `get lastModifiedDisplayDate()` pour transformer `this.props.cardLastModified` en chaîne localisée représentant la date.

   ```js
   export default class Card extends Component {
       ...
       get lastModifiedDisplayDate() {
           const lastModifiedDate = this.props.cardLastModified ? new Date(this.props.cardLastModified) : null;
   
           if (lastModifiedDate) {
               return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

5. Mettez à jour `get bodyContent()` pour afficher `this.props.cardTitle` et utilisez les méthodes créées lors des étapes précédentes :

   ```js
   export default class Card extends Component {
       ...
       get bodyContent() {
          return (<div class="Card__content">
                       <h2 class="Card__title"> {this.props.cardTitle}
                           <span class="Card__lastmod">
                               {this.lastModifiedDisplayDate}
                           </span>
                       </h2>
                       {this.ctaButton}
               </div>);
       }
       ...
   }
   ```

6. Des règles Sass ont déjà été ajoutées à `Card.scss` pour appliquer un style au titre, à l’appel à l’action et à la date de dernière modification. Insérez ces styles en ajoutant la ligne suivante à `Card.js` en haut du fichier :

   ```diff
     import {MapTo} from '@adobe/aem-react-editable-components';
   
   + require('./Card.scss');
   
     export const CardEditConfig = {
   ```

   >[!NOTE]
   >
   > Vous pouvez afficher le [code du composant de carte React terminé ici](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/ui.frontend/src/components/Card/Card.js).

7. Déployez toutes les modifications apportées à AEM à partir de la racine du projet à l’aide de Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

8. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html) pour afficher le composant mis à jour :

   ![Mise à jour du composant Carte dans AEM](assets/extend-component/updated-card-in-aem.png)

9. Vous devriez être en mesure de recréer le contenu existant pour créer une page semblable à ce qui suit :

   ![Création finale du composant Carte](assets/extend-component/final-authoring-card.png)

## Félicitations !  {#congratulations}

Félicitations, vous avez appris à étendre un composant AEM à l’aide de et du fonctionnement des modèles et boîtes de dialogue Sling avec le modèle JSON.

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) ou extraire le code localement en passant à la branche `React/extend-component-solution`.
