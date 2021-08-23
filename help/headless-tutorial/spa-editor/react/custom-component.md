---
title: Création d’un composant météorologique personnalisé | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment créer un composant météorologique personnalisé à utiliser avec l’éditeur SPA d’AEM. Découvrez comment développer des boîtes de dialogue de création et des modèles Sling pour étendre le modèle JSON afin de renseigner un composant personnalisé. Les composants Open Weather API et React Open Weather sont utilisés.
sub-product: sites
feature: Éditeur de SPA
doc-type: tutorial
topics: development
version: cloud-service
kt: 5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1228'
ht-degree: 3%

---


# Création d’un composant météorologique personnalisé {#custom-component}

Découvrez comment créer un composant météorologique personnalisé à utiliser avec l’éditeur SPA d’AEM. Découvrez comment développer des boîtes de dialogue de création et des modèles Sling pour étendre le modèle JSON afin de renseigner un composant personnalisé. Les [API de météo ouverte](https://openweathermap.org) et [composant de météo ouverte React](https://www.npmjs.com/package/react-open-weather) sont utilisés.

## Objectif

1. Comprendre le rôle des modèles Sling dans la manipulation de l’API de modèle JSON fournie par AEM.
2. Découvrez comment créer des boîtes de dialogue de composant AEM.
3. Découvrez comment créer un composant d’AEM **personnalisé** qui sera compatible avec la structure de l’éditeur SPA.

## Ce que vous allez créer

Un composant météorologique simple sera construit. Ce composant pourra être ajouté à la SPA par les auteurs de contenu. À l’aide d’une boîte de dialogue AEM, les auteurs peuvent définir l’emplacement du temps à afficher.  L’implémentation de ce composant illustre les étapes nécessaires à la création d’un composant AEM net compatible avec la structure AEM Éditeur.

![Configuration du composant de météo ouverte](assets/custom-component/enter-dialog.png)

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment). Ce chapitre est la suite du chapitre [Navigation et routage](navigation-routing.md). Toutefois, pour suivre, vous avez besoin d’un projet AEM activé SPA déployé sur une instance d’ locale.

### Clé API météorologique ouverte

Une clé API [Open Weather](https://openweathermap.org/) est nécessaire pour suivre le tutoriel. [L’inscription est ](https://home.openweathermap.org/users/sign_up) gratuite pour un nombre limité d’appels API.

## Définition du composant AEM

Un composant AEM est défini comme un noeud et des propriétés. Dans le projet, ces noeuds et propriétés sont représentés sous forme de fichiers XML dans le module `ui.apps`. Créez ensuite le composant AEM dans le module `ui.apps`.

>[!NOTE]
>
> Une actualisation rapide des [bases des composants d’AEM peut s’avérer utile](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Dans l’IDE de votre choix, ouvrez le dossier `ui.apps`.
2. Accédez à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` et créez un dossier nommé `open-weather`.
3. Créez un fichier nommé `.content.xml` sous le dossier `open-weather`. Renseignez `open-weather/.content.xml` avec les éléments suivants :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Créer une définition de composant personnalisée](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifie que ce noeud sera un composant AEM.

   `jcr:title` est la valeur qui s’affichera pour les auteurs de contenu et qui  `componentGroup` détermine le regroupement des composants dans l’interface utilisateur de création.

4. Sous le dossier `custom-component` , créez un autre dossier nommé `_cq_dialog`.
5. Sous le dossier `_cq_dialog` , créez un fichier nommé `.content.xml` et renseignez-le avec ce qui suit :

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

   ![Définition d’un composant personnalisé](assets/custom-component/dialog-custom-component-defintion.png)

   Le fichier XML ci-dessus génère une boîte de dialogue très simple pour `Weather Component`. La partie essentielle du fichier est les noeuds `<label>`, `<lat>` et `<lon>` internes. Cette boîte de dialogue contiendra deux `numberfield`et une `textfield` qui permettront à un utilisateur de configurer la météo à afficher.

   Un modèle Sling sera créé en regard de pour exposer la valeur des propriétés `label`,`lat` et `long` via le modèle JSON.

   >[!NOTE]
   >
   > Vous pouvez afficher beaucoup plus [d’exemples de boîtes de dialogue en affichant les définitions des composants principaux](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Vous pouvez également afficher d’autres champs de formulaire, tels que `select`, `textarea`, `pathfield`, disponibles sous `/libs/granite/ui/components/coral/foundation/form` dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Avec un composant d’AEM traditionnel, un script [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=fr) est généralement requis. Comme le SPA effectue le rendu du composant, aucun script HTL n’est nécessaire.

## Création d’un modèle Sling

Les modèles Sling sont des objets POJO (Plain Old Java Object) Java pilotés par les annotations qui facilitent le mappage des données du JCR aux variables Java. [Sling ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models) Fonctionne de manière modélisée afin d’encapsuler une logique métier côté serveur complexe pour les composants AEM.

Dans le contexte de l’éditeur de SPA, les modèles Sling exposent le contenu d’un composant par le biais du modèle JSON par le biais d’une fonctionnalité utilisant l’[exportateur de modèle Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html).

1. Dans l’IDE de votre choix, ouvrez le module `core` à l’adresse `aem-guides-wknd-spa.react/core`.
1. Créez un fichier nommé à `OpenWeatherModel.java` à `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
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

   Il s’agit de l’interface Java de notre composant. Pour que notre modèle Sling soit compatible avec la structure d’éditeur SPA, il doit étendre la classe `ComponentExporter`.

1. Créez un dossier nommé `impl` sous `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Créez un fichier nommé `OpenWeatherModelImpl.java` sous `impl` et renseignez les éléments suivants :

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

   La variable statique `RESOURCE_TYPE` doit pointer vers le chemin d’accès dans `ui.apps` du composant. `getExportedType()` est utilisé pour mapper les propriétés JSON au composant SPA via `MapTo`. `@ValueMapValue` est une annotation qui lit la propriété jcr enregistrée par la boîte de dialogue.

## Mettre à jour le SPA

Ensuite, mettez à jour le code React afin d’inclure le [composant React Open Weather](https://www.npmjs.com/package/react-open-weather) et faites-le correspondre au composant AEM créé lors des étapes précédentes.

1. Installez le composant React Open Weather en tant que dépendance **npm** :

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. Créez un dossier nommé `OpenWeather` à l’adresse `ui.frontend/src/components/OpenWeather`.
1. Ajoutez un fichier nommé `OpenWeather.js` et renseignez-le avec ce qui suit :

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
           if(OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. Mettez à jour `import-components.js` à `ui.frontend/src/components/import-components.js` pour inclure le composant `OpenWeather` :

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. Déployez toutes les mises à jour sur un environnement d’AEM local à partir de la racine du répertoire du projet, en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Mise à jour de la stratégie de modèle

Ensuite, accédez à AEM pour vérifier les mises à jour et autoriser l’ajout du composant `OpenWeather` au SPA.

1. Vérifiez l’enregistrement du nouveau modèle Sling en accédant à [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   Vous devriez voir les deux lignes ci-dessus qui indiquent que `OpenWeatherModelImpl` est associé au composant `wknd-spa-react/components/open-weather` et qu’il est enregistré via l’exportateur de modèle Sling.

1. Accédez au modèle de page SPA à l’adresse [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Mettez à jour la stratégie du conteneur de mises en page pour ajouter le nouveau `Open Weather` en tant que composant autorisé :

   ![Mettre à jour la stratégie de conteneur de mises en page](assets/custom-component/custom-component-allowed.png)

   Enregistrez les modifications apportées à la stratégie et observez `Open Weather` comme composant autorisé :

   ![Composant personnalisé en tant que composant autorisé](assets/custom-component/custom-component-allowed-layout-container.png)

## Création du composant de météo ouverte

Créez ensuite le composant `Open Weather` à l’aide de l’éditeur SPA d’AEM.

1. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
1. En mode `Edit`, ajoutez `Open Weather` à la balise `Layout Container` :

   ![Insérer un nouveau composant](assets/custom-component/insert-custom-component.png)

1. Ouvrez la boîte de dialogue du composant et saisissez un **Libellé**, **Latitude** et **Longitude**. Par exemple **San Diego**, **32.7157** et **-117.1611**. Les nombres de l’hémisphère occidental et de l’hémisphère sud sont représentés sous forme de nombres négatifs avec l’API de météo ouverte

   ![Configuration du composant de météo ouverte](assets/custom-component/enter-dialog.png)

   Il s’agit de la boîte de dialogue qui a été créée à partir du fichier XML plus tôt dans le chapitre.

1. Enregistrez les modifications. Notez que la météo de **San Diego** s’affiche désormais :

   ![Composant météorologique mis à jour](assets/custom-component/weather-updated.png)

1. Affichez le modèle JSON en accédant à [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Recherchez `wknd-spa-react/components/open-weather`:

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   Les valeurs JSON sont générées par le modèle Sling. Ces valeurs JSON sont transmises au composant React sous la forme de props.

## Félicitations ! {#congratulations}

Félicitations, vous avez appris à créer un composant d’AEM personnalisé à utiliser avec l’éditeur SPA. Vous avez également appris comment les boîtes de dialogue, les propriétés JCR et les modèles Sling interagissent pour générer le modèle JSON.

### Étapes suivantes {#next-steps}

[Étendre un composant principal](extend-component.md)  : découvrez comment étendre un composant principal AEM existant à utiliser avec l’AEM Éditeur. Comprendre comment ajouter des propriétés et du contenu à un composant existant est une technique puissante pour étendre les fonctionnalités d’une mise en oeuvre d’AEM SPA éditeur.
