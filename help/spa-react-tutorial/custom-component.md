---
title: Créer un composant personnalisé | Prise en main de l’éditeur AEM d’application d’une seule page et réaction
description: Découvrez comment créer un composant personnalisé à utiliser avec AEM SPA Editor. Découvrez comment développer des boîtes de dialogue d’auteur et des modèles Sling pour étendre le modèle JSON afin de renseigner un composant personnalisé.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5878
thumbnail: 5878-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '1486'
ht-degree: 3%

---


# Créer un composant personnalisé {#custom-component}

Découvrez comment créer un composant personnalisé à utiliser avec AEM SPA Editor. Découvrez comment développer des boîtes de dialogue d’auteur et des modèles Sling pour étendre le modèle JSON afin de renseigner un composant personnalisé.

## Intention

1. Comprendre le rôle des modèles Sling dans la manipulation de l’API de modèle JSON fournie par AEM.
2. Découvrez comment créer de nouvelles boîtes de dialogue de composants AEM.
3. Découvrez comment créer un composant AEM **personnalisé** compatible avec la structure de l’éditeur d’applications monopages.

## Ce que vous allez construire

Les chapitres précédents ont porté sur l&#39;élaboration de composants d&#39;application d&#39;une seule page et sur leur mise en correspondance avec les composants *existants* AEM principaux composants. Ce chapitre porte sur la création et l’extension de *nouveaux* composants AEM et la manipulation du modèle JSON fourni par AEM.

Un simple `Custom Component` exemple illustre les étapes nécessaires pour créer un composant AEM net-new.

![Message affiché dans toutes les zones](assets/custom-component/message-displayed.png)

## Conditions préalables

Examiner les outils et les instructions nécessaires à la mise en place d&#39;un environnement [de développement](overview.md#local-dev-environment)local.

### Obtention du code

1. Téléchargez le point de départ de ce didacticiel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/custom-component-start
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

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution) ou le retirer localement en passant à la branche `React/custom-component-solution`.

## Définir le composant AEM

Un composant AEM est défini comme un noeud et des propriétés. Dans le projet, ces noeuds et propriétés sont représentés sous la forme de fichiers XML dans le `ui.apps` module. Créez ensuite le composant AEM dans le `ui.apps` module.

>[!NOTE]
>
> Il peut être utile [d’actualiser rapidement les](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html)bases des composants AEM.

1. Dans l&#39;IDE de votre choix, ouvrez le `ui.apps` dossier.
2. Accédez à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` et créez un nouveau dossier nommé `custom-component`.
3. Create a new file named `.content.xml` beneath the `custom-component` folder. Populate the `custom-component/.content.xml` with the following:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Créer une définition de composant personnalisée](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifie que ce noeud sera un composant AEM.

   `jcr:title` est la valeur qui sera affichée pour les auteurs de contenu et qui `componentGroup` détermine le regroupement des composants dans l’interface utilisateur de création.

4. Sous le `custom-component` dossier, créez un autre dossier nommé `_cq_dialog`.
5. Sous le `_cq_dialog` dossier, créez un nouveau fichier nommé `.content.xml` et remplissez-le avec les éléments suivants :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
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
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   ![Définition de composant personnalisée](assets/custom-component/dialog-custom-component-defintion.png)

   Le fichier XML ci-dessus génère une boîte de dialogue très simple pour le `Custom Component`. La partie essentielle du fichier est le `<message>` noeud interne. Cette boîte de dialogue contient un simple `textfield` nom `Message` et conserve la valeur du champ textile sur une propriété nommée `message`.

   Un modèle Sling sera créé en regard pour exposer la valeur de la `message` propriété via le modèle JSON.

   >[!NOTE]
   >
   > Vous pouvez vue beaucoup plus d’ [exemples de boîtes de dialogue en affichant les définitions](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)des composants principaux. Vous pouvez également vue d’autres champs de formulaire, comme `select`, `textarea`, `pathfield`, disponibles sous `/libs/granite/ui/components/coral/foundation/form` CRXDE-Lite [](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Avec un composant AEM traditionnel, un script [HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/overview.html) est généralement requis. Puisque l’application d’une seule page effectue le rendu du composant, aucun script HTML n’est nécessaire.

## Création du modèle Sling

Les modèles Sling sont des &quot;POJO&quot; Java pilotés par l’annotation (objets Java ordinaires) qui facilitent le mappage des données du JCR aux variables Java. [Les modèles](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) Sling fonctionnent généralement pour encapsuler une logique métier complexe côté serveur pour les composants AEM.

Dans le contexte de l’éditeur d’applications monopages, les modèles Sling exposent le contenu d’un composant par le biais du modèle JSON à l’aide d’une fonction utilisant l’exportateur [de modèles](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)Sling.

1. Dans l&#39;IDE de votre choix, ouvrez le `core` module. `CustomComponent.java` et `CustomComponentImpl.java` ont déjà été créés et supprimés dans le code de démarrage du chapitre.

   >[!NOTE]
   >
   > Si vous utilisez Visual Studio Code IDE, il peut s’avérer utile d’installer [des extensions pour Java](https://code.visualstudio.com/docs/java/extensions).

2. Ouvrez l’interface Java `CustomComponent.java` à l’adresse `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/CustomComponent.java`:

   ![Interface CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Il s’agit de l’interface Java qui sera implémentée par le modèle Sling.

3. Mettez à jour `CustomComponent.java` afin d’étendre l’ `ComponentExporter` interface :

   ```java
   package com.adobe.aem.guides.wknd.spa.react.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   L’implémentation de l’ `ComponentExporter` interface est une exigence pour que le modèle Sling soit automatiquement récupéré par l’API du modèle JSON.

   L&#39; `CustomComponent` interface comprend une méthode d&#39;obtention unique `getMessage()`. Il s’agit de la méthode qui expose la valeur de la boîte de dialogue de l’auteur via le modèle JSON. Seules les méthodes getter publiques avec des paramètres vides `()` seront exportées dans le modèle JSON.

4. Ouvrez `CustomComponentImpl.java` à `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java`.

   Il s’agit de l’implémentation de l’ `CustomComponent` interface. L&#39; `@Model` annotation identifie la classe Java comme modèle Sling. L’ `@Exporter` annotation permet à la classe Java d’être sérialisée et exportée via Sling Model Exporter.

5. Mettez à jour la variable statique `RESOURCE_TYPE` pour pointer vers le composant AEM `wknd-spa-react/components/custom-component` créé lors de l’exercice précédent.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-react/components/custom-component";
   ```

   Le type de ressource du composant est celui qui lie le modèle Sling au composant AEM et qui, en fin de compte, est mappé au composant Réagir.

6. ajoutez la `getExportedType()` méthode à la `CustomComponentImpl` classe pour renvoyer le type de ressource de composant :

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Cette méthode est requise lors de l&#39;implémentation de l&#39; `ComponentExporter` interface et expose le type de ressource qui permet le mappage au composant Réagir.

7. Mettez à jour la `getMessage()` méthode pour renvoyer la valeur de la `message` propriété conservée par la boîte de dialogue d’auteur. Utilisez l’ `@ValueMap` annotation is map the JCR value `message` to a Java variable :

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   Une autre &quot;logique métier&quot; est ajoutée pour renvoyer la valeur String du message avec toutes les majuscules. Cela nous permettra de voir la différence entre la valeur brute stockée par la boîte de dialogue d&#39;auteur et la valeur exposée par le modèle Sling.

   >[!NOTE]
   >
   > Vous pouvez vue le fichier CustomComponentImpl.java [terminé ici](https://github.com/adobe/aem-guides-wknd-spa/blob/React/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java).

## Mettre à jour le composant Réagir

Le code de réaction du composant personnalisé a déjà été créé. Ensuite, effectuez quelques mises à jour pour mapper le composant Réagir au composant AEM.

1. Dans le `ui.frontend` module, ouvrez le fichier `ui.frontend/src/components/Custom/Custom.js`.
2. Observez la `{this.props.message}` variable dans le cadre de la `render()` méthode :

   ```js
   return (
           <div class="CustomComponent">
               <h2 class="CustomComponent__message">{this.props.message}</h2>
           </div>
       );
   ```

   La valeur en majuscules transformée du modèle Sling devrait être mappée à cette `message` propriété.

3. Importez l’ `MapTo` objet à partir de l’AEM SPA Editor JS SDK et utilisez-le pour le mapper au composant AEM :

   ```diff
   + import {MapTo} from '@adobe/aem-react-editable-components';
   
    ...
    export default class Custom extends Component {
        ...
    }
   
   + MapTo('wknd-spa-react/components/custom-component')(Custom, CustomEditConfig);
   ```

4. Déployez toutes les mises à jour vers un environnement AEM local à partir de la racine du répertoire du projet, en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Mettre à jour la stratégie de modèle

Accédez ensuite à AEM pour vérifier les mises à jour et autoriser leur ajout `Custom Component` à l’application d’une seule page.

1. Vérifiez l’enregistrement du nouveau modèle Sling en accédant à [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl - wknd-spa-react/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl exports 'wknd-spa-react/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Vous devriez voir les deux lignes ci-dessus qui indiquent que le `CustomComponentImpl` composant est associé au `wknd-spa-react/components/custom-component` composant et qu&#39;il est enregistré via l&#39;Exportateur de modèle Sling.

2. Accédez au modèle de page de l’application d’une seule page à l’adresse [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. Mettez à jour la stratégie du Conteneur de mise en page pour ajouter le nouveau `Custom Component` en tant que composant autorisé :

   ![Mettre à jour la stratégie de Conteneur de mise en page](assets/custom-component/custom-component-allowed.png)

   Enregistrez les modifications apportées à la stratégie et observez-les `Custom Component` en tant que composant autorisé :

   ![Composant personnalisé en tant que composant autorisé](assets/custom-component/custom-component-allowed-layout-container.png)

## Création du composant personnalisé

Créez ensuite le fichier `Custom Component` à l’aide de l’éditeur d’applications monopages AEM.

1. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
2. En `Edit` mode, ajoutez le `Custom Component` au `Layout Container`:

   ![Insérer un nouveau composant](assets/custom-component/insert-custom-component.png)

3. Ouvrez la boîte de dialogue du composant et saisissez un message contenant des lettres minuscules.

   ![Configuration du composant personnalisé](assets/custom-component/enter-dialog-message.png)

   Il s’agit de la boîte de dialogue qui a été créée en fonction du fichier XML plus tôt dans le chapitre.

4. Enregistrez les modifications. Observez que le message affiché est en majuscules.

   ![Message affiché dans toutes les zones](assets/custom-component/message-displayed.png)

5. Vue du modèle JSON en accédant à [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Recherchez `wknd-spa-react/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-react/components/custom-component"
   }
   ```

   Notez que la valeur JSON est définie sur toutes les majuscules en fonction de la logique ajoutée au modèle Sling.

## Félicitations ! {#congratulations}

Félicitations, vous avez appris à créer un composant AEM personnalisé à utiliser avec l’éditeur d’applications monopages. Vous avez également appris comment les boîtes de dialogue, les propriétés JCR et les modèles Sling interagissent pour générer le modèle JSON.

Vous pouvez vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution) ou extraire le code localement en passant à la branche `React/custom-component-solution`.

### Étapes suivantes {#next-steps}

[Étendre un composant](extend-component.md) principal - Découvrez comment étendre un composant principal existant à utiliser avec l’éditeur d’applications monopages AEM. Comprendre comment ajouter des propriétés et du contenu à un composant existant est une technique puissante permettant d’étendre les fonctionnalités d’une mise en oeuvre AEM SPA Editor.
