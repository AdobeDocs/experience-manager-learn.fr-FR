---
title: Personnalisation de la couche de données du client Adobe avec les composants AEM
description: Découvrez comment personnaliser la couche de données du client Adobe avec le contenu des composants d'AEM personnalisés. Découvrez comment utiliser les API fournies par AEM Core Components pour étendre et personnaliser la couche de données.
feature: core-component
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
translation-type: tm+mt
source-git-commit: 46936876de355de9923f7a755aa6915a13cca354
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 4%

---


# Personnaliser la couche de données du client Adobe avec les composants AEM {#customize-data-layer}

Découvrez comment personnaliser la couche de données du client Adobe avec le contenu des composants d&#39;AEM personnalisés. Découvrez comment utiliser les API fournies par [AEM Core Components pour étendre](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) et personnaliser la couche de données.

## Ce que vous allez créer

![Signature de la couche de données](assets/adobe-client-data-layer/byline-data-layer-html.png)

Dans ce didacticiel, vous allez explorer diverses options pour étendre la couche de données du client Adobe en mettant à jour le composant [Signature ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html) WKND. Il s’agit d’un composant personnalisé et les leçons apprises dans ce didacticiel peuvent être appliquées à d’autres composants personnalisés.

### Objectifs {#objective}

1. Injecter des données de composant dans la couche de données en étendant un modèle Sling et un composant HTL
1. Utiliser les utilitaires de couche de données du composant principal pour réduire les efforts
1. Utiliser les attributs de données du composant principal pour se connecter aux événements de couche de données existants

## Conditions préalables {#prerequisites}

Un **environnement de développement local** est nécessaire pour compléter ce tutoriel. Les captures d’écran et la vidéo sont capturées à l’aide de l’AEM en tant que SDK Cloud Service s’exécutant sur un macOS. Sauf indication contraire, les commandes et le code sont indépendants du système d&#39;exploitation local.

**Vous découvrez AEM as a Cloud Service ?** Consultez le guide  [suivant pour configurer un environnement de développement local à l’aide de l’AEM en tant que SDK](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) Cloud Service.

**Nouveau à AEM 6.5 ?** Consultez le guide  [suivant pour la configuration d&#39;un environnement](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de développement local.

## Téléchargement et déploiement du site de référence WKND {#set-up-wknd-site}

Ce didacticiel étend le composant Signature du site de référence WKND. Cloner et installer la base de code WKND sur votre environnement local.

1. Début d’une instance locale de démarrage rapide **author** de l’AEM s’exécutant à [http://localhost:4502](http://localhost:4502).
1. Ouvrez une fenêtre de terminal et cloner la base de code WKND à l’aide de Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Déployez la base de code WKND sur une instance locale d’AEM :

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 et le dernier Service Pack, ajoutez le profil `classic` à la commande Maven :
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Ouvrez une nouvelle fenêtre de navigateur et connectez-vous à AEM. Ouvrez une page **Magazine** comme suit : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Composant signature sur la page](assets/adobe-client-data-layer/byline-component-onpage.png)

   Vous devriez voir un exemple du composant Signature qui a été ajouté à la page dans le cadre d’un fragment d’expérience. Vous pouvez vue au fragment d’expérience à l’adresse [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html).
1. Ouvrez vos outils de développement et saisissez la commande suivante dans la **console** :

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect la réponse pour afficher l’état actuel de la couche de données sur un site AEM. Vous devriez voir des informations sur la page et les composants individuels.

   ![Réponse de la couche de données d&#39;Adobe](assets/data-layer-state-response.png)

   Observez que le composant Signature n’est pas répertorié dans la couche de données.

## Mettre à jour le modèle Sling de signature {#sling-model}

Pour injecter des données sur le composant dans la couche de données, nous devons d’abord mettre à jour le modèle Sling du composant. Ensuite, mettez à jour l’interface Java de la signature et l’implémentation du modèle Sling pour ajouter une nouvelle méthode `getData()`. Cette méthode contiendra les propriétés que nous voulons injecter dans la couche de données.

1. Dans l&#39;IDE de votre choix, ouvrez le projet `aem-guides-wknd`. Accédez au module `core`.
1. Ouvrez le fichier `Byline.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Interface Java de signature](assets/adobe-client-data-layer/byline-java-interface.png)

1. Ajoutez une nouvelle méthode à l’interface :

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. Ouvrez le fichier `BylineImpl.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

   Il s’agit de l’implémentation de l’interface `Byline` et elle est implémentée en tant que modèle Sling.

1. Ajoutez les instructions d&#39;importation suivantes au début du fichier :

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Les API `fasterxml.jackson` seront utilisées pour sérialiser les données à exposer en tant que JSON. Le `ComponentUtils` des composants principaux AEM sera utilisé pour vérifier si la couche de données est activée.

1. Ajoutez la méthode `getData()` non implémentée sur `BylineImple.java` :

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   Dans la méthode ci-dessus, un nouveau `HashMap` est utilisé pour capturer les propriétés que nous voulons exposer en tant que JSON. Notez que des méthodes existantes telles que `getName()` et `getOccupations()` sont utilisées. `@type` représente le type de ressource unique du composant, ce qui permet à un client d&#39;identifier facilement des événements et/ou des déclencheurs en fonction du type de composant.

   `ObjectMapper` est utilisé pour sérialiser les propriétés et renvoyer une chaîne JSON. Cette chaîne JSON peut alors être injectée dans la couche de données.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `core` en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Mettre à jour la signature HTL {#htl}

Ensuite, mettez à jour `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL (HTML Template Language) est le modèle utilisé pour générer le code HTML du composant.

Un attribut de données spécial `data-cmp-data-layer` sur chaque composant AEM est utilisé pour exposer sa couche de données.  Le code JavaScript fourni par AEM Core Components recherche cet attribut de données, dont la valeur sera renseignée avec la chaîne JSON renvoyée par la méthode `getData()` du modèle Sling de signature, et injecte les valeurs dans la couche Données du client de l’Adobe.

1. Dans l&#39;IDE, ouvrez le projet `aem-guides-wknd`. Accédez au module `ui.apps`.
1. Ouvrez le fichier `byline.html` dans `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Signature HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. Mettez à jour `byline.html` pour inclure l&#39;attribut `data-cmp-data-layer` :

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   La valeur de `data-cmp-data-layer` a été définie sur `"${byline.data}"` où `byline` correspond au modèle Sling mis à jour précédemment. `.data` est la notation standard pour l’appel d’une méthode Java Getter dans HTL de l’exercice  `getData()` précédent.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `ui.apps` en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page avec un composant Signature : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Ouvrez les outils de développement et examinez la source HTML de la page pour le composant Signature :

   ![Signature de la couche de données](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Vous devez voir que `data-cmp-data-layer` a été renseigné avec la chaîne JSON du modèle Sling.

1. Ouvrez les outils de développement du navigateur et saisissez la commande suivante dans la **console** :

   ```js
   window.adobeDataLayer.getState();
   ```

1. Accédez sous la réponse sous `component` pour trouver l&#39;instance du composant `byline` ajoutée à la couche de données :

   ![Partie de signature de la couche de données](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Vous devriez voir une entrée comme suit :

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Observez que les propriétés exposées sont les mêmes que celles ajoutées dans le `HashMap` dans le modèle Sling.

## Ajouter un Événement de clic {#click-event}

La couche de données du client Adobe est pilotée par événement et l&#39;un des événements les plus courants pour déclencher une action est le événement `cmp:click`. Les composants AEM Core facilitent l’enregistrement de votre composant à l’aide de l’élément de données : `data-cmp-clickable`.

Les éléments cliquables sont généralement un bouton CTA ou un lien de navigation. Malheureusement, le composant Byline n&#39;en a pas, mais nous l&#39;enregistrerons de toute façon car cela peut être courant pour d&#39;autres composants personnalisés.

1. Ouvrez le module `ui.apps` dans votre IDE.
1. Ouvrez le fichier `byline.html` dans `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Mettez à jour `byline.html` pour inclure l&#39;attribut `data-cmp-clickable` dans l&#39;élément **name** de la signature :

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Ouvrez un nouveau terminal. Créez et déployez uniquement le module `ui.apps` en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page en ajoutant le composant Signature : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Pour tester notre événement, nous allons ajouter manuellement du code JavaScript à l&#39;aide de la console de développement. Voir [Utilisation de la couche de données du client Adobe avec les composants principaux AEM](data-layer-overview.md) pour une vidéo sur la façon de procéder.

1. Ouvrez les outils de développement du navigateur et saisissez la méthode suivante dans la **console** :

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Cette méthode simple doit gérer le clic sur le nom du composant Byline.

1. Saisissez la méthode suivante dans la **Console** :

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   La méthode ci-dessus pousse un écouteur de événement sur la couche de données pour écouter le événement `cmp:click` et appelle le `bylineClickHandler`.

   >[!CAUTION]
   >
   > Il sera important de **ne pas** actualiser le navigateur tout au long de cet exercice, sinon le code JavaScript de la console sera perdu.

1. Dans le navigateur, avec la **console** ouverte, cliquez sur le nom de l’auteur dans le composant Signature :

   ![Composant de signature cliqué](assets/adobe-client-data-layer/byline-component-clicked.png)

   Vous devriez voir le message de la console `Byline Clicked!` et le nom de la signature.

   Le événement `cmp:click` est le plus facile à relier. Pour les composants plus complexes et pour suivre d’autres comportements, il est possible d’ajouter du code javascript personnalisé pour ajouter et enregistrer de nouveaux événements. Un excellent exemple est le composant de carrousel, qui déclenche un événement `cmp:show` chaque fois qu&#39;une diapositive est basculée. Voir le code source [pour plus de détails](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## Utiliser l&#39;utilitaire DataLayerBuilder {#data-layer-builder}

Lorsque le modèle Sling était [mis à jour](#sling-model) plus tôt dans le chapitre, nous avons choisi de créer la chaîne JSON en utilisant `HashMap` et en définissant manuellement chacune des propriétés. Cette méthode fonctionne bien pour les petits composants ponctuels, mais pour les composants qui étendent les composants principaux de l&#39;AEM, cela peut entraîner beaucoup de code supplémentaire.

Il existe une classe d&#39;utilitaires `DataLayerBuilder` pour effectuer la majeure partie de la charge lourde. Cela permet aux implémentations d’étendre uniquement les propriétés qu’elles souhaitent. Mettons à jour le modèle Sling pour utiliser le `DataLayerBuilder`.

1. Revenez à l&#39;IDE et accédez au module `core`.
1. Ouvrez le fichier `Byline.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Modifiez la méthode `getData()` pour renvoyer un type de `ComponentData`

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` est un objet fourni par AEM Core Components. Il génère une chaîne JSON, tout comme dans l’exemple précédent, mais effectue également beaucoup de travail supplémentaire.

1. Ouvrez le fichier `BylineImpl.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Ajoutez les instructions d&#39;importation suivantes :

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Remplacez la méthode `getData()` par la méthode suivante :

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   Le composant Signature réutilise des parties du composant Image Core pour afficher une image représentant l’auteur. Dans le fragment de code ci-dessus, [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) est utilisé pour étendre la couche de données du composant `Image`. Ceci préremplit l’objet JSON avec toutes les données relatives à l’image utilisée. Il exécute également certaines des fonctions de routine telles que la définition de `@type` et de l&#39;identifiant unique du composant. Notez que la méthode est vraiment petite !

   La seule propriété a étendu le `withTitle` qui est remplacé par la valeur `getName()`.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `core` en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Revenez à l&#39;IDE et ouvrez le fichier `byline.html` sous `ui.apps`
1. Mettez à jour le fichier HTL pour utiliser `byline.data.json` pour renseigner l&#39;attribut `data-cmp-data-layer` :

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Souvenez-vous que nous renvoyons maintenant un objet de type `ComponentData`. Cet objet comprend une méthode getter `getJson()` qui est utilisée pour remplir l&#39;attribut `data-cmp-data-layer`.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `ui.apps` en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page en ajoutant le composant Signature : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Ouvrez les outils de développement du navigateur et saisissez la commande suivante dans la **console** :

   ```js
   window.adobeDataLayer.getState();
   ```

1. Accédez sous la réponse sous `component` pour trouver l&#39;instance du composant `byline` :

   ![Signature de la couche de données mise à jour](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   Vous devriez voir une entrée comme suit :

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   Observez qu&#39;il existe désormais un objet `image` dans l&#39;entrée de composant `byline`. Cette section contient beaucoup plus d’informations sur la ressource dans la gestion des actifs numériques. Notez également que les attributs `@type` et l&#39;identifiant unique (dans ce cas `byline-136073cfcb`) ont été automatiquement renseignés, ainsi que les attributs `repo:modifyDate` qui indiquent la date de modification du composant.

## Exemples supplémentaires {#additional-examples}

1. Un autre exemple d&#39;extension de la couche de données peut être affiché en examinant le composant `ImageList` dans la base de code WKND :
   * `ImageList.java` - Interface Java dans le  `core` module.
   * `ImageListImpl.java` - Modèle Sling dans le  `core` module.
   * `image-list.html` - Modèle HTML dans le  `ui.apps` module.

   >[!NOTE]
   >
   > Il est un peu plus difficile d’inclure des propriétés personnalisées telles que `occupation` lors de l’utilisation de [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Cependant, si vous étendez un composant principal qui comprend une image ou une page, l’utilitaire économise beaucoup de temps.

   >[!NOTE]
   >
   > Si vous créez une couche de données avancée pour les objets réutilisés tout au long d’une implémentation, il est recommandé d’extraire les éléments de couche de données dans leurs propres objets Java spécifiques à la couche de données. Par exemple, les composants de base du commerce ont ajouté des interfaces pour `ProductData` et `CategoryData` car elles peuvent être utilisées sur de nombreux composants dans une implémentation du commerce. Consultez [le code dans le repo aem-cif-core-components](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) pour plus de détails.

## Félicitations! {#congratulations}

Vous venez d&#39;explorer quelques façons d&#39;étendre et de personnaliser la couche de données du client Adobe avec les composants AEM !

## Ressources supplémentaires {#additional-resources}

* [Documentation de la couche de données du client Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Intégration de la couche de données aux composants principaux](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Utilisation de la couche de données du client Adobe et de la documentation des composants principaux](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/data-layer/overview.html)
