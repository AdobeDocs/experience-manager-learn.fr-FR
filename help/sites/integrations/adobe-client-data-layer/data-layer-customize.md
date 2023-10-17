---
title: Personnaliser la couche de données client Adobe avec les composants AEM
description: Apprenez à personnaliser la couche de données client Adobe avec le contenu des composants AEM personnalisés. Découvrez comment utiliser les API fournies par les composants principaux d’AEM pour étendre et personnaliser la couche de données.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: 99b3ecf7823ff9a116c47c88abc901f8878bbd7a
workflow-type: ht
source-wordcount: '2008'
ht-degree: 100%

---

# Personnaliser la couche de données client Adobe avec les composants AEM {#customize-data-layer}

Apprenez à personnaliser la couche de données client Adobe avec le contenu des composants AEM personnalisés. Découvrez comment utiliser les API fournies par [les composants principaux d’AEM pour étendre](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html?lang=fr) et personnaliser la couche de données.

## Ce que vous allez créer

![Couche de données de signature.](assets/adobe-client-data-layer/byline-data-layer-html.png)

Dans ce tutoriel, explorons différentes options pour étendre la couche de données client d’Adobe en mettant à jour le [composant de signature](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=fr) WKND. Le composant de _signature_ est un **composant personnalisé**. Les leçons apprises dans ce tutoriel peuvent être appliquées à d’autres composants personnalisés.

### Objectifs {#objective}

1. Injectez des données de composant dans la couche de données en étendant un modèle Sling et un composant HTL.
1. Utiliser les utilitaires de couche de données des composants principaux pour réduire les efforts
1. Utiliser des attributs de données des composants principaux pour se connecter aux événements de couche de données existants

## Conditions préalables {#prerequisites}

Pour suivre ce tutoriel, un **environnement de développement local** est nécessaire. Les captures d’écran et les vidéos sont réalisées à l’aide du SDK AEM as a Cloud Service s’exécutant sur macOS. Sauf indication contraire, les commandes et le code sont indépendants du système d’exploitation local.

**Vous découvrez AEM as a Cloud Service ?** Consultez le [guide suivant pour configurer un environnement de développement local à l’aide du SDK d’AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr).

**Vous découvrez AEM 6.5 ?** Consultez le [guide suivant pour configurer un environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr).

## Télécharger et déployer le site de référence WKND {#set-up-wknd-site}

Ce tutoriel étend le composant de signature sur le site de référence WKND. Clonez et installez la base de code WKND dans votre environnement local.

1. Démarrez une instance locale de démarrage rapide de **création** d’AEM exécutée à l’adresse [http://localhost:4502](http://localhost:4502).
1. Ouvrez une fenêtre de terminal et clonez la base de code WKND à l’aide de Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Déployez la base de code WKND sur une instance locale d’AEM :

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Pour AEM 6.5 et le dernier pack de services, ajoutez le profil `classic` à la commande Maven :
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Ouvrez une nouvelle fenêtre de navigateur et connectez-vous à AEM. Ouvrez une page de **Magazine**, comme par exemple : [http://localhost:4502/content/wknd/fr/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Composant de signature sur la page.](assets/adobe-client-data-layer/byline-component-onpage.png)

   Vous devriez voir un exemple du composant de signature qui a été ajouté à la page dans le cadre d’un fragment d’expérience. Vous pouvez afficher le fragment d’expérience à l’adresse [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/fr/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/fr/contributors/stacey-roswells/byline.html).
1. Ouvrez vos outils de développement et saisissez la commande suivante dans la **Console** :

   ```js
   window.adobeDataLayer.getState();
   ```

   Pour afficher l’état actuel de la couche de données sur un site AEM, examinez la réponse. Vous devriez voir des informations sur la page et les composants individuels.

   ![Réponse de la couche de données Adobe.](assets/data-layer-state-response.png)

   Notez que le composant de signature n’est pas répertorié dans la couche de données.

## Mettre à jour le modèle Sling de signature {#sling-model}

Pour injecter des données relatives au composant dans la couche de données, commençons par mettre à jour le modèle Sling du composant. Ensuite, mettez à jour l’interface Java™ de la signature et la mise en œuvre du modèle Sling afin d’obtenir une nouvelle méthode `getData()`. Cette méthode contient les propriétés à injecter dans la couche de données.

1. Ouvrez le projet `aem-guides-wknd` dans l’IDE de votre choix. Accédez au module `core`.
1. Ouvrez le fichier `Byline.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Interface Java de signature.](assets/adobe-client-data-layer/byline-java-interface.png)

1. Ajoutez la méthode ci-dessous à l’interface :

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

1. Ouvrez le fichier `BylineImpl.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`. Il s’agit de la mise en œuvre de l’interface `Byline`. Elle est implémentée en tant que modèle Sling.

1. Ajoutez les instructions d’import suivantes au début du fichier :

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Les API `fasterxml.jackson` sont utilisées pour sérialiser les données à exposer au format JSON. Les `ComponentUtils` des composants principaux d’AEM sont utilisés pour vérifier si la couche de données est activée.

1. Ajoutez la méthode non implémentée `getData()` à `BylineImple.java` :

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

   Dans la méthode ci-dessus, une nouvelle `HashMap` est utilisée pour capturer les propriétés à exposer au format JSON. Notez que les méthodes existantes comme `getName()` et `getOccupations()` sont utilisées. Le `@type` représente le type de ressource unique du composant. Il permet à un client ou une cliente d’identifier facilement des événements et/ou des déclencheurs en fonction du type de composant.

   Le `ObjectMapper` est utilisé pour sérialiser les propriétés et renvoyer une chaîne JSON. Cette chaîne JSON peut ensuite être injectée dans la couche de données.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `core` avec vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Mettre à jour le HTL de la signature {#htl}

Mettez ensuite à jour le [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=fr) de la `Byline`. HTL (HTML Template Language) est le modèle utilisé pour effectuer le rendu du HTML du composant.

Un attribut de données spécial `data-cmp-data-layer` sur chaque composant AEM est utilisé pour exposer sa couche de données. Le code JavaScript fourni par les composants principaux d’AEM recherche cet attribut de données. La valeur de cet attribut de données est renseignée avec la chaîne JSON renvoyée par la méthode `getData()` du modèle Sling de signature et injectée dans la couche de données client Adobe.

1. Ouvrez le projet `aem-guides-wknd` dans l’IDE. Accédez au module `ui.apps`.
1. Ouvrez le fichier `byline.html` dans `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![HTML de signature.](assets/adobe-client-data-layer/byline-html-template.png)

1. Mettez à jour `byline.html` pour inclure l’attribut `data-cmp-data-layer` :

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   La valeur de `data-cmp-data-layer` a été définie sur `"${byline.data}"` où `byline` est le modèle Sling mis à jour précédemment. `.data` est la notation standard pour appeler une méthode Java™ Getter en HTL de la méthode `getData()` mise en œuvre dans l’exercice précédent.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `ui.apps` avec vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page avec un composant de signature : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Ouvrez les outils de développement et examinez la source HTML de la page pour le composant de signature :

   ![Couche de données de signature.](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Vous devriez constater que la `data-cmp-data-layer` a été renseignée avec la chaîne JSON du modèle Sling.

1. Ouvrez les outils de développement du navigateur et saisissez la commande suivante dans la **console** :

   ```js
   window.adobeDataLayer.getState();
   ```

1. Naviguez sous la réponse sous `component` pour voir que l’instance du composant `byline` a été ajoutée à la couche de données :

   ![Partie de signature de la couche de données.](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Une entrée doit s’afficher comme suit :

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Notez que les propriétés exposées sont les mêmes que celles ajoutées dans la `HashMap` du modèle Sling.

## Ajouter un événement de clic {#click-event}

La couche de données client Adobe est pilotée par des événements et l’un des événements les plus courants pour déclencher une action est l’événement `cmp:click`. Les composants principaux AEM facilitent l’enregistrement de votre composant à l’aide de l’élément de données : `data-cmp-clickable`.

Les éléments cliquables sont généralement un bouton CTA ou un lien de navigation. Malheureusement, le composant « Signature » n’en a pas, mais nous allons tout de même l’enregistrer, car cela peut être courant pour d’autres composants personnalisés.

1. Ouvrez le module `ui.apps` dans votre IDE.
1. Ouvrez le fichier `byline.html` dans `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Mettez à jour `byline.html` pour inclure l’attribut `data-cmp-clickable` dans l’élément **nom** du composant Signature :

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Ouvrez un nouveau terminal. Créez et déployez uniquement le module `ui.apps` en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page en ajoutant le composant Signature : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Pour tester notre événement, nous allons ajouter manuellement du code JavaScript à l’aide de la Developer Console. Consultez [Utilisation de la couche de données client Adobe avec les composants principaux AEM](data-layer-overview.md) pour une vidéo sur la façon de procéder.

1. Ouvrez les outils de développement du navigateur et saisissez la commande suivante dans la **console** :

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Cette méthode simple doit gérer le clic sur le nom du composant Signature.

1. Saisissez la méthode suivante dans la **console** :

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   La méthode ci-dessus envoie un écouteur d’événement sur la couche de données pour écouter l’événement `cmp:click` et appelle le `bylineClickHandler`.

   >[!CAUTION]
   >
   > Il est important de **ne pas** actualiser le navigateur tout au long de cet exercice, car le code JavaScript de la console risque de se perdre.

1. Dans le navigateur, avec la **console** ouverte, cliquez sur le nom de l’auteur ou de l’autrice dans le composant Signature :

   ![Composant Signature cliqué.](assets/adobe-client-data-layer/byline-component-clicked.png)

   Le message `Byline Clicked!` devrait s’afficher dans la console ainsi que le nom de la Signature.

   L’événement `cmp:click` est le plus facile à relier. Pour les composants plus complexes et pour suivre d’autres comportements, il est possible d’ajouter du code JavaScript personnalisé pour ajouter et enregistrer de nouveaux événements. Le composant du carrousel est un excellent exemple, qui déclenche un événement `cmp:show` chaque fois qu’une diapositive est activée ou désactivée. Consultez le [code source pour plus d’informations](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js).

## Utiliser l’utilitaire DataLayerBuilder {#data-layer-builder}

Lorsque le modèle Sling a été [mis à jour](#sling-model) plus tôt dans le chapitre, nous avons choisi de créer la chaîne JSON en utilisant une `HashMap` et de définir manuellement chaque propriété. Cette méthode fonctionne bien pour les petits composants ponctuels, mais pour les composants qui étendent les composants principaux d’AEM, cela peut entraîner beaucoup de code supplémentaire.

Une classe d’utilitaires, `DataLayerBuilder`, existe pour effectuer la plus grande partie du travail. Cela permet aux implémentations d’étendre uniquement les propriétés qu’elles souhaitent. Mettons à jour le modèle Sling pour utiliser le `DataLayerBuilder`.

1. Revenez à l’IDE et accédez au module `core`.
1. Ouvrez le fichier `Byline.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Modifiez la méthode `getData()` pour renvoyer un type de `ComponentData`.

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

   `ComponentData` est un objet fourni par les composants principaux d’AEM. Il génère une chaîne JSON, tout comme dans l’exemple précédent, mais exécute également beaucoup de travail supplémentaire.

1. Ouvrez le fichier `BylineImpl.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Ajoutez les instructions d’import suivantes :

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Remplacez la méthode `getData()` par la suivante :

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

   Le composant Signature réutilise des parties du composant principal Image pour afficher une image représentant l’auteur ou l’autrice. Dans l’extrait de code ci-dessus, le [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) est utilisé pour étendre la couche de données du composant `Image`. Cela préremplit l’objet JSON avec toutes les données sur l’image utilisée. Il remplit également certaines fonctions de routine, comme la définition du `@type` et l’identifiant unique du composant. Notez que la méthode est petite.

   La seule propriété étend le `withTitle` qui est remplacé par la valeur de `getName()`.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `core` en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Revenez à l’IDE et ouvrez le fichier `byline.html` sous `ui.apps`.
1. Mettez à jour le HTL pour utiliser `byline.data.json` afin de renseigner l’attribut `data-cmp-data-layer` :

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Rappelez-vous que nous renvoyons maintenant un objet de type `ComponentData`. Cet objet comprend une méthode Getter `getJson()` et est utilisé pour renseigner l’attribut `data-cmp-data-layer`.

1. Ouvrez une fenêtre de terminal. Créez et déployez uniquement le module `ui.apps` avec vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Revenez au navigateur et rouvrez la page avec le composant de signature ajouté : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Ouvrez les outils de développement du navigateur et saisissez la commande suivante dans la **console** :

   ```js
   window.adobeDataLayer.getState();
   ```

1. Naviguez sous la réponse sous `component` pour trouver l’instance du composant `byline` :

   ![Couche de données de signature mise à jour.](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   Une entrée doit s’afficher comme suit :

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

   Observez qu’il existe maintenant un objet `image` dans l’entrée du composant `byline`. Il y a beaucoup plus d’informations sur la ressource dans la gestion des ressources numériques. Notez également que le `@type` et l’ID unique (dans ce cas, `byline-136073cfcb`) ont été automatiquement renseignés et l’élément `repo:modifyDate` qui indique quand le composant a été modifié.

## Exemples supplémentaires {#additional-examples}

1. Vous pouvez également consulter un autre exemple d’extension de la couche de données en examinant le composant `ImageList` dans la base de code WKND :
   * `ImageList.java` : interface Java dans le module `core`.
   * `ImageListImpl.java` : modèle Sling dans le module `core`.
   * `image-list.html` : modèle HTL dans le module `ui.apps`.

   >[!NOTE]
   >
   > Il est un peu plus difficile d’inclure des propriétés personnalisées comme `occupation` lors de l’utilisation du [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Cependant, si vous étendez un composant principal qui comprend une image ou une page, l’utilitaire vous fait gagner beaucoup de temps.

   >[!NOTE]
   >
   > Si vous créez une couche de données avancée pour les objets réutilisés tout au long d’une implémentation, il est recommandé d’extraire les éléments de couche de données dans leurs propres objets Java™ spécifiques à la couche de données. Par exemple, les composants principaux de Commerce ont ajouté des interfaces pour `ProductData` et `CategoryData`, car ils peuvent être utilisés sur de nombreux composants dans une implémentation de Commerce. Consultez le [code dans le référentiel aem-cif-core-components](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) pour plus d’informations.

## Félicitations. {#congratulations}

Vous venez d’explorer quelques façons d’étendre et de personnaliser la couche de données client Adobe avec les composants AEM.

## Ressources supplémentaires {#additional-resources}

* [Documentation sur la couche de données client Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Intégrer la couche de données aux composants principaux](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Documentation sur l’utilisation de la couche de données de la clientèle Adobe et des composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=fr)
