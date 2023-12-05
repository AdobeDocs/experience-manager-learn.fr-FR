---
title: Créer un composant de météo personnalisé | Prendre en main l’éditeur de SPA d’AEM et de React
description: Découvrez comment créer un composant de météo personnalisé à utiliser avec l’éditeur de SPA d’AEM. Découvrez comment développer des boîtes de dialogue de création et des modèles Sling pour étendre le modèle JSON et renseigner un composant personnalisé. Les éléments utilisés sont l’API Open Weather et les composants React Open Weather.
feature: SPA Editor
version: Cloud Service
jira: KT-5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 82466e0e-b573-440d-b806-920f3585b638
duration: 472
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1105'
ht-degree: 100%

---

# Créer un composant de météo personnalisé {#custom-component}

Découvrez comment créer un composant de météo personnalisé à utiliser avec l’éditeur de SPA d’AEM. Découvrez comment développer des boîtes de dialogue de création et des modèles Sling pour étendre le modèle JSON et renseigner un composant personnalisé. Les éléments utilisés sont l’[API Open Weather](https://openweathermap.org) et le composant [React Open Weather](https://www.npmjs.com/package/react-open-weather).

## Objectif

1. Comprendre le rôle des modèles Sling dans la manipulation de l’API de modèle JSON fournie par AEM.
2. Découvrez comment créer des boîtes de dialogue de composant AEM.
3. Découvrez comment créer un composant AEM **personnalisé** compatible avec le framework de l’éditeur de SPA.

## Ce que vous allez créer

Un composant de météo simple est créé. Ce composant peut être ajouté à la SPA par les personnes créant le contenu. À l’aide d’une boîte de dialogue AEM, les auteurs et autrices peuvent définir l’emplacement de la météo à afficher.  L’implémentation de ce composant illustre les étapes nécessaires à la création d’un nouveau composant AEM compatible avec le framework de l’éditeur de SPA d’AEM.

![Configuration du composant Open Weather.](assets/custom-component/enter-dialog.png)

## Prérequis

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment). Ce chapitre est la suite du chapitre [Navigation et routage](navigation-routing.md), toutefois, vous n’avez besoin pour le suivre que d’un projet AEM compatible avec les SPA déployé sur une instance AEM locale.

### Clé API Open Weather

Une clé API [Open Weather](https://openweathermap.org/) est nécessaire pour suivre le tutoriel. [L’inscription est gratuite](https://home.openweathermap.org/users/sign_up), mais avec un nombre limité d’appels API.

## Définir le composant AEM

Un composant AEM est défini comme un nœud et des propriétés. Dans le projet, ces nœuds et propriétés sont représentés sous la forme de fichiers XML dans le module `ui.apps`. Créez ensuite le composant AEM dans le module `ui.apps`.

>[!NOTE]
>
> Un bref rappel [des principes de base des composants AEM peut s’avérer utile](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Dans l’IDE de votre choix, ouvrez le dossier `ui.apps`.
2. Accédez à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` et créez un dossier nommé `open-weather`.
3. Créez un nouveau fichier nommé `.content.xml` sous le dossier `open-weather`. Remplissez le `open-weather/.content.xml` avec les éléments suivants :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Définition d’un composant personnalisé de création.](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` : identifie que ce nœud est un composant AEM.

   `jcr:title` est la valeur affichée pour les auteurs et autrices de contenu et le `componentGroup` détermine le regroupement des composants dans l’interface utilisateur de création.

4. Sous le dossier `custom-component`, créez un autre dossier nommé `_cq_dialog`.
5. Sous le dossier `_cq_dialog`, créez un fichier nommé `.content.xml` et renseignez-le avec les éléments suivants :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Open Weather"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <label
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The label to display for the component"
                                                   fieldLabel="Label"
                                                   name="./label"/>
                                               <lat
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The latitude of the location."
                                                   fieldLabel="Latitude"
                                                   step="any"
                                                   name="./lat" />
                                               <lon
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The longitude of the location."
                                                   fieldLabel="Longitude"
                                                   step="any"
                                                   name="./lon"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![Définition d’un composant personnalisé.](assets/custom-component/dialog-custom-component-defintion.png)

   Le fichier XML ci-dessus génère une boîte de dialogue très simple pour le `Weather Component`. La partie essentielle du fichier est constituée des nœuds `<label>`, `<lat>` et `<lon>` internes. Cette boîte de dialogue contient deux `numberfield` et un `textfield` qui permettent à l’utilisateur ou à l’utilisatrice de configurer la météo à afficher.

   Un modèle Sling est ensuite créé pour exposer la valeur des propriétés `label`,`lat` et `long` via le modèle JSON.

   >[!NOTE]
   >
   > Vous pouvez afficher de nombreux autres [exemples de boîtes de dialogue en affichant les définitions des composants principaux](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Vous pouvez également afficher des champs de formulaire supplémentaires, tels que `select`, `textarea`, `pathfield`, disponibles sous `/libs/granite/ui/components/coral/foundation/form` dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Avec un composant AEM traditionnel, un script [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=fr) est généralement requis. Comme la SPA effectue le rendu du composant, aucun script HTL n’est nécessaire.

## Créer le modèle Sling

Les modèles Sling sont des objets POJO (Plain Old Java Object) Java pilotés par des annotations qui facilitent le mappage des données du JCR aux variables Java. Les [modèles Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=fr#sling-models) fonctionnent en règle générale pour encapsuler une logique commerciale côté serveur complexe pour les composants AEM.

Dans le contexte de l’éditeur de SPA, les modèles Sling exposent le contenu d’un composant par le biais du modèle JSON grâce à une fonctionnalité utilisant l’[exporteur de modèles Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=fr).

1. Dans l’IDE de votre choix, ouvrez le module `core` dans `aem-guides-wknd-spa.react/core`.
1. Créez un fichier nommé `OpenWeatherModel.java` dans `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Remplissez `OpenWeatherModel.java` avec les éléments suivants :

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.export.json.ComponentExporter;
   
   // Sling Models intended to be used with SPA Editor must extend ComponentExporter interface
   public interface OpenWeatherModel extends ComponentExporter {
       public String getLabel();
       public double getLat();
       public double getLon();
   }
   ```

   Il s’agit de l’interface Java de notre composant. Pour que notre modèle Sling soit compatible avec le framework d’éditeur de SPA, il doit étendre la classe `ComponentExporter`.

1. Créez un dossier nommé `impl` sous `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Créez un fichier nommé `OpenWeatherModelImpl.java` sous `impl` et renseignez les éléments suivants :

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import com.adobe.aem.guides.wkndspa.react.core.models.OpenWeatherModel;
   
   // Sling Model annotation
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { OpenWeatherModel.class, ComponentExporter.class }, 
       resourceType = OpenWeatherModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter( //Exporter annotation that serializes the modoel as JSON
       name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
       extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class OpenWeatherModelImpl implements OpenWeatherModel {
   
       @ValueMapValue
       private String label; //maps variable to jcr property named "label" persisted by Dialog
   
       @ValueMapValue
       private double lat; //maps variable to jcr property named "lat"
   
       @ValueMapValue
       private double lon; //maps variable to jcr property named "lon"
   
       // points to AEM component definition in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/open-weather";
   
       // public getter method to expose value of private variable `label`
       // adds additional logic to default the label to "(Default)" if not set.
       @Override
       public String getLabel() {
           return StringUtils.isNotBlank(label) ? label : "(Default)";
       }
   
       // public getter method to expose value of private variable `lat`
       @Override
       public double getLat() {
           return lat;
       }
   
       // public getter method to expose value of private variable `lon`
       @Override
       public double getLon() {
           return lon;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/open-weather`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return OpenWeatherModelImpl.RESOURCE_TYPE;
       }
   } 
   ```

   La variable statique `RESOURCE_TYPE` doit pointer vers le chemin d’accès dans le `ui.apps` du composant. Le `getExportedType()` est utilisé pour mapper les propriétés JSON au composant SPA via `MapTo`. `@ValueMapValue` est une annotation qui lit la propriété jcr enregistrée par la boîte de dialogue.

## Mettre à jour la SPA

Ensuite, mettez à jour le code React afin d’inclure le [Composant React Open Weather](https://www.npmjs.com/package/react-open-weather) et faites en sorte qu’il soit mappé au composant AEM créé lors des étapes précédentes.

1. Installez le composant React Open Weather en tant que dépendance **npm** :

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. Créez un nouveau dossier nommé `OpenWeather` dans `ui.frontend/src/components/OpenWeather`.
1. Ajoutez un fichier nommé `OpenWeather.js` et renseignez-le avec les éléments suivants :

   ```js
   import React from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   import ReactWeather, { useOpenWeather } from 'react-open-weather';
   
   // Open weather API Key
   // For simplicity it is hard coded in the file, ideally this is extracted in to an environment variable
   const API_KEY = 'YOUR_API_KEY';
   
   // Logic to render placeholder or component
   const OpenWeatherEditConfig = {
   
       emptyLabel: 'Weather',
       isEmpty: function(props) {
           return !props || !props.lat || !props.lon || !props.label;
       }
   };
   
   // Wrapper function that includes react-open-weather component
   function ReactWeatherWrapper(props) {
       const { data, isLoading, errorMessage } = useOpenWeather({
           key: API_KEY,
           lat: props.lat, // passed in from AEM JSON
           lon: props.lon, // passed in from AEM JSON
           lang: 'en',
           unit: 'imperial', // values are (metric, standard, imperial)
       });
   
       return (
           <div className="cmp-open-weather">
               <ReactWeather
                   isLoading={isLoading}
                   errorMessage={errorMessage}
                   data={data}
                   lang="en"
                   locationLabel={props.label} // passed in from AEM JSON
                   unitsLabels={{ temperature: 'F', windSpeed: 'mph' }}
                   showForecast={false}
                 />
           </div>
       );
   }
   
   export default function OpenWeather(props) {
   
           // render nothing if component not configured
           if (OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. Mettez à jour `import-components.js` dans `ui.frontend/src/components/import-components.js` pour inclure la composant `OpenWeather` :

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. Déployez toutes les mises à jour sur un environnement AEM local à partir de la racine du répertoire du projet, en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Mettre à jour la stratégie des modèles

Ensuite, accédez à AEM pour vérifier les mises à jour, et autorisez le composant `OpenWeather` à être ajouté à la SPA.

1. Vérifiez l’enregistrement du nouveau modèle Sling en accédant à [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   Vous devriez voir les deux lignes ci-dessus qui indiquent que le `OpenWeatherModelImpl` est associé avec le composant `wknd-spa-react/components/open-weather` et qu’il est enregistré via l’exporteur de modèle Sling.

1. Accédez au modèle de page SPA à l’adresse [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Mettez à jour la stratégie du conteneur de disposition pour ajouter le nouveau composant `Open Weather` en tant que composant autorisé :

   ![Mise à jour de la stratégie du conteneur de disposition.](assets/custom-component/custom-component-allowed.png)

   Enregistrez les modifications apportées à la stratégie et observez le composant `Open Weather` en tant que composant autorisé :

   ![Composant personnalisé en tant que composant autorisé.](assets/custom-component/custom-component-allowed-layout-container.png)

## Créer le composant Open Weather

Ensuite, créez le composant `Open Weather` à l’aide de l’éditeur de SPA d’AEM.

1. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
1. Dans le mode `Edit`, ajoutez le composant `Open Weather` au `Layout Container` :

   ![Insertion d’un nouveau composant.](assets/custom-component/insert-custom-component.png)

1. Ouvrez la boîte de dialogue du composant et saisissez un **Libellé**, une **Latitude**, et une **Longitude**. Par exemple **San Diego**, **32,7157°**, et **- 117,1611°**. Les nombres de l’hémisphère occidental et de l’hémisphère sud sont représentés sous forme de nombres négatifs avec l’API Open Weather.

   ![Configuration du composant Open Weather.](assets/custom-component/enter-dialog.png)

   Il s’agit de la boîte de dialogue qui a été créée à partir du fichier XML plus tôt dans le chapitre.

1. Enregistrez les modifications. Observez que la météo pour **San Diego** s’affiche maintenant :

   ![Composant de météo mis à jour.](assets/custom-component/weather-updated.png)

1. Affichez le modèle JSON en accédant à [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Recherchez `wknd-spa-react/components/open-weather` :

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   Les valeurs JSON sont générées par le modèle Sling. Ces valeurs JSON sont transmises au composant React sous la forme de propriétés.

## Félicitations. {#congratulations}

Félicitations, vous avez appris à créer un composant AEM personnalisé à utiliser avec l’éditeur de SPA. Vous avez également appris comment les boîtes de dialogue, les propriétés JCR et les modèles Sling interagissent pour générer le modèle JSON.

### Étapes suivantes {#next-steps}

[Étendre un composant principal](extend-component.md) - Découvrez comment étendre un composant AEM principal existant à utiliser avec l’editeur de SPA d’AEM. Comprendre comment ajouter des propriétés et du contenu à un composant existant est une technique puissante pour étendre les fonctionnalités d’une implémentation d’éditeur de SPA AEM.
