---
title: Personnalisation de la couche de données client Adobe avec des composants AEM
description: Découvrez comment personnaliser la couche de données client Adobe avec du contenu provenant de composants d’AEM personnalisés. Découvrez comment utiliser les API fournies par AEM Core Components pour étendre et personnaliser la couche de données.
feature: Couche de données client Adobe, composant principal
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
topic: Intégrations
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2034'
ht-degree: 4%

---


# Personnaliser la couche de données client Adobe avec les composants AEM {#customize-data-layer}

Découvrez comment personnaliser la couche de données client Adobe avec du contenu provenant de composants d’AEM personnalisés. Découvrez comment utiliser les API fournies par [AEM les composants principaux pour étendre](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) et personnaliser la couche de données.

## Ce que vous allez créer

![Couche de données signature](assets/adobe-client-data-layer/byline-data-layer-html.png)

Dans ce tutoriel, vous allez découvrir différentes options pour étendre la couche de données client Adobe en mettant à jour le [composant de ligne ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html) WKND. Il s’agit d’un composant personnalisé. Les leçons apprises dans ce tutoriel peuvent être appliquées à d’autres composants personnalisés.

### Objectifs {#objective}

1. Injectez des données de composant dans la couche de données en étendant un modèle Sling et un composant HTL.
1. Utilisation des utilitaires de couche de données des composants principaux pour réduire les efforts
1. Utilisation des attributs de données des composants principaux pour se connecter aux événements de couche de données existants

## Prérequis {#prerequisites}

Un **environnement de développement local** est nécessaire pour terminer ce tutoriel. Les captures d’écran et la vidéo sont capturées à l’aide de AEM as a Cloud Service SDK s’exécutant sur macOS. Les commandes et le code sont indépendants du système d’exploitation local, sauf indication contraire.

**Vous découvrez AEM as a Cloud Service ?** Consultez le guide  [suivant pour configurer un environnement de développement local à l’aide du SDK AEM as a Cloud Service](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).

**Vous découvrez AEM 6.5 ?** Consultez le guide  [suivant pour configurer un environnement de développement local](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Téléchargez et déployez le site de référence WKND {#set-up-wknd-site}

Ce tutoriel étend le composant signature sur le site de référence WKND. Cloner et installer la base de code WKND dans votre environnement local.

1. Démarrez une instance locale de démarrage rapide **author** de l’AEM s’exécutant à l’adresse [http://localhost:4502](http://localhost:4502).
1. Ouvrez une fenêtre de terminal et clonez la base de code WKND à l’aide de Git :

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

   ![Composant de signature sur la page](assets/adobe-client-data-layer/byline-component-onpage.png)

   Vous devriez voir un exemple du composant Byline qui a été ajouté à la page dans le cadre d’un fragment d’expérience. Vous pouvez afficher le fragment d’expérience à l’adresse [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Ouvrez vos outils de développement et saisissez la commande suivante dans la **console** :

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect la réponse pour afficher l’état actuel de la couche de données sur un site AEM. Vous devriez voir des informations sur la page et les composants individuels.

   ![Réponse de la couche de données d’Adobe](assets/data-layer-state-response.png)

   Notez que le composant Byline n’est pas répertorié dans la couche de données.

## Mettre à jour le modèle Sling de signature {#sling-model}

Pour injecter des données sur le composant dans la couche de données, nous devons d’abord mettre à jour le modèle Sling du composant. Ensuite, mettez à jour l’interface Java de Byline et la mise en oeuvre du modèle Sling pour ajouter une nouvelle méthode `getData()`. Cette méthode contient les propriétés que nous voulons injecter dans la couche de données.

1. Dans l’IDE de votre choix, ouvrez le projet `aem-guides-wknd`. Accédez au module `core` .
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

1. Ajoutez les instructions d’importation suivantes au début du fichier :

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Les API `fasterxml.jackson` seront utilisées pour sérialiser les données que nous voulons exposer au format JSON. La balise `ComponentUtils` des composants principaux d’AEM sera utilisée pour vérifier si la couche de données est activée.

1. Ajoutez la méthode `getData()` non implémentée à `BylineImple.java` :

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

   Dans la méthode ci-dessus, un nouveau `HashMap` est utilisé pour capturer les propriétés que nous voulons exposer au format JSON. Notez que des méthodes existantes telles que `getName()` et `getOccupations()` sont utilisées. `@type` représente le type de ressource unique du composant, ce qui permet à un client d’identifier facilement des événements et/ou des déclencheurs en fonction du type de composant.

   `ObjectMapper` est utilisé pour sérialiser les propriétés et renvoyer une chaîne JSON. Cette chaîne JSON peut ensuite être injectée dans la couche de données.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `core` à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Mise à jour de la signature HTL {#htl}

Mettez ensuite à jour `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL (HTML Template Language) est le modèle utilisé pour effectuer le rendu HTML du composant.

Un attribut de données spécial `data-cmp-data-layer` sur chaque composant AEM est utilisé pour exposer sa couche de données.  JavaScript fourni par AEM Core Components recherche cet attribut de données, dont la valeur sera renseignée avec la chaîne JSON renvoyée par la méthode `getData()` du modèle Sling de signature, et injecte les valeurs dans la couche de données client d’Adobe.

1. Dans l’IDE, ouvrez le projet `aem-guides-wknd`. Accédez au module `ui.apps` .
1. Ouvrez le fichier `byline.html` dans `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Byline HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. Mettez à jour `byline.html` pour inclure l’attribut `data-cmp-data-layer` :

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   La valeur de `data-cmp-data-layer` a été définie sur `"${byline.data}"` où `byline` est le modèle Sling mis à jour précédemment. `.data` est la notation standard pour l’appel d’une méthode Java Getter en HTL  `getData()` implémentée dans l’exercice précédent.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `ui.apps` à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page avec un composant de signature : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Ouvrez les outils de développement et examinez la source HTML de la page pour le composant Byline :

   ![Couche de données signature](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Vous devriez constater que `data-cmp-data-layer` a été renseigné avec la chaîne JSON du modèle Sling.

1. Ouvrez les outils de développement du navigateur et saisissez la commande suivante dans la **console** :

   ```js
   window.adobeDataLayer.getState();
   ```

1. Accédez sous la réponse sous `component` pour trouver l’instance du composant `byline` ajouté à la couche de données :

   ![Partie de signature de la couche de données](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Une entrée doit s’afficher comme suit :

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Notez que les propriétés exposées sont les mêmes que celles ajoutées dans la balise `HashMap` du modèle Sling.

## Ajouter un événement de clic {#click-event}

La couche de données client Adobe est pilotée par un événement et l’un des événements les plus courants pour déclencher une action est l’événement `cmp:click` . Les composants principaux AEM facilitent l’enregistrement de votre composant à l’aide de l’élément de données : `data-cmp-clickable`.

Les éléments cliquables sont généralement un bouton CTA ou un lien de navigation. Malheureusement, le composant Byline n’en comporte pas, mais nous l’enregistrerons de toute façon, car cela peut être courant pour d’autres composants personnalisés.

1. Ouvrez le module `ui.apps` dans votre IDE.
1. Ouvrez le fichier `byline.html` dans `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Mettez à jour `byline.html` pour inclure l’attribut `data-cmp-clickable` sur l’élément **name** de la signature :

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Ouvrez un nouveau terminal. Créez et déployez uniquement le module `ui.apps` à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page en ajoutant le composant Byline : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Pour tester notre événement, nous allons ajouter manuellement du code JavaScript à l’aide de la console de développement. Voir [Utilisation de la couche de données client Adobe avec les composants principaux AEM](data-layer-overview.md) pour une vidéo sur la manière de procéder.

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

1. Saisissez la méthode suivante dans la **console** :

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   La méthode ci-dessus envoie un écouteur d’événement sur la couche de données pour écouter l’événement `cmp:click` et appelle la balise `bylineClickHandler`.

   >[!CAUTION]
   >
   > Il sera important de **ne pas** actualiser le navigateur tout au long de cet exercice, sinon le code JavaScript de la console sera perdu.

1. Dans le navigateur, avec la **console** ouverte, cliquez sur le nom de l’auteur dans le composant de signature :

   ![Composant en forme de signature cliqué](assets/adobe-client-data-layer/byline-component-clicked.png)

   Le message de la console doit s’afficher `Byline Clicked!` et le nom de la signature.

   L’événement `cmp:click` est le plus facile à relier. Pour les composants plus complexes et pour effectuer le suivi d’autres comportements, il est possible d’ajouter du code JavaScript personnalisé pour ajouter et enregistrer de nouveaux événements. Le composant du carrousel est un excellent exemple. Il déclenche un événement `cmp:show` chaque fois qu’une diapositive est activée ou désactivée. Voir le [code source pour plus de détails](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## Utilisation de l’utilitaire DataLayerBuilder {#data-layer-builder}

Lorsque le modèle Sling était [mis à jour](#sling-model) plus tôt dans le chapitre, nous avons choisi de créer la chaîne JSON à l’aide d’une balise `HashMap` et de définir manuellement chacune des propriétés. Cette méthode fonctionne bien pour les petits composants ponctuels, mais pour les composants qui étendent les composants principaux d’AEM, cela peut entraîner beaucoup de code supplémentaire.

Une classe utilitaire `DataLayerBuilder` existe pour effectuer la plus grande partie du transport lourd. Cela permet aux implémentations d’étendre uniquement les propriétés qu’elles souhaitent. Mettons à jour le modèle Sling pour utiliser le `DataLayerBuilder`.

1. Revenez à l’IDE et accédez au module `core`.
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

   `ComponentData` est un objet fourni par AEM Core Components. Elle génère une chaîne JSON, tout comme dans l’exemple précédent, mais exécute également beaucoup de travail supplémentaire.

1. Ouvrez le fichier `BylineImpl.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Ajoutez les instructions d’importation suivantes :

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Remplacez la méthode `getData()` par ce qui suit :

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

   Le composant Byline réutilise des parties du composant principal Image pour afficher une image représentant l’auteur. Dans le fragment de code ci-dessus, [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) est utilisé pour étendre la couche de données du composant `Image`. Cela préremplit l’objet JSON avec toutes les données sur l’image utilisée. Il exécute également certaines des fonctions de routine, comme la définition de `@type` et l’identifiant unique du composant. Remarquez que la méthode est vraiment petite !

   La seule propriété a étendu le `withTitle` qui est remplacé par la valeur `getName()`.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `core` à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Revenez à l’IDE et ouvrez le fichier `byline.html` sous `ui.apps`
1. Mettez à jour le HTL pour utiliser `byline.data.json` pour renseigner l’attribut `data-cmp-data-layer` :

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Souvenez-vous que nous renvoyons maintenant un objet de type `ComponentData`. Cet objet comprend une méthode getter `getJson()` utilisée pour renseigner l’attribut `data-cmp-data-layer`.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `ui.apps` à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page en ajoutant le composant Byline : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Ouvrez les outils de développement du navigateur et saisissez la commande suivante dans la **console** :

   ```js
   window.adobeDataLayer.getState();
   ```

1. Accédez sous la réponse sous `component` pour trouver l’instance du composant `byline` :

   ![Couche de données signature mise à jour](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   Une entrée doit s’afficher comme suit :

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

   Notez qu’il existe désormais un objet `image` dans l’entrée de composant `byline`. Il contient beaucoup plus d’informations sur la ressource dans la gestion des ressources numériques. Notez également que `@type` et l’identifiant unique (dans ce cas `byline-136073cfcb`) ont été automatiquement renseignés, ainsi que la valeur `repo:modifyDate` qui indique quand le composant a été modifié.

## Exemples supplémentaires {#additional-examples}

1. Vous pouvez également consulter un autre exemple d’extension de la couche de données en examinant le composant `ImageList` dans la base de code WKND :
   * `ImageList.java` - Interface Java du  `core` module.
   * `ImageListImpl.java` - Modèle Sling dans le  `core` module .
   * `image-list.html` - modèle HTL dans le  `ui.apps` module .

   >[!NOTE]
   >
   > Il est un peu plus difficile d’inclure des propriétés personnalisées telles que `occupation` lors de l’utilisation de [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Cependant, si vous étendez un composant principal qui comprend une image ou une page, l’utilitaire vous fait gagner beaucoup de temps.

   >[!NOTE]
   >
   > Si vous créez une couche de données avancée pour les objets réutilisés tout au long d’une mise en oeuvre, il est recommandé d’extraire les éléments de couche de données dans leurs propres objets Java spécifiques à la couche de données. Par exemple, les composants principaux de Commerce ont ajouté des interfaces pour `ProductData` et `CategoryData`, car ils peuvent être utilisés sur de nombreux composants dans une implémentation de Commerce. Pour plus d’informations, consultez [le code dans le référentiel aem-cif-core-components](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer).

## Félicitations !  {#congratulations}

Vous venez d’explorer quelques façons d’étendre et de personnaliser la couche de données client Adobe avec les composants AEM !

## Ressources supplémentaires {#additional-resources}

* [Documentation sur la couche de données client Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Intégration de la couche de données aux composants principaux](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Utilisation de la couche de données client Adobe et de la documentation des composants principaux](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/data-layer/overview.html)
