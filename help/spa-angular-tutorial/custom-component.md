---
title: Créer un composant personnalisé | Prise en main de l’AEM SPA Editor et Angular
description: Découvrez comment créer un composant personnalisé à utiliser avec AEM SPA Editor. Découvrez comment développer des boîtes de dialogue d’auteur et des modèles Sling pour étendre le modèle JSON afin de renseigner un composant personnalisé.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
translation-type: tm+mt
source-git-commit: 1fd4d31770a4eac37a88a7c6960fd51845601bee
workflow-type: tm+mt
source-wordcount: '1505'
ht-degree: 3%

---


# Créer un composant personnalisé {#custom-component}

Découvrez comment créer un composant personnalisé à utiliser avec AEM SPA Editor. Découvrez comment développer des boîtes de dialogue d’auteur et des modèles Sling pour étendre le modèle JSON afin de renseigner un composant personnalisé.

## Intention

1. Comprendre le rôle des modèles Sling dans la manipulation de l’API de modèle JSON fournie par AEM.
2. Découvrez comment créer de nouvelles boîtes de dialogue de composants AEM.
3. Découvrez comment créer un composant **personnalisé** AEM qui sera compatible avec la structure d’éditeur SPA.

## Ce que vous allez créer

Les chapitres précédents ont porté sur l&#39;élaboration de SPA composants et leur mise en correspondance avec *les* éléments de base &lt;a1/> AEM existants. Ce chapitre porte sur la façon de créer et d’étendre *les nouveaux composants* AEM et de manipuler le modèle JSON fourni par AEM.

Un `Custom Component` simple illustre les étapes nécessaires pour créer un composant AEM net-new.

![Message affiché dans toutes les zones](assets/custom-component/message-displayed.png)

## Conditions préalables

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Obtention du code

1. Téléchargez le point de départ de ce didacticiel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
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

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) ou vérifier le code localement en passant à la branche `Angular/custom-component-solution`.

## Définir le composant AEM

Un composant AEM est défini comme un noeud et des propriétés. Dans le projet, ces noeuds et propriétés sont représentés sous la forme de fichiers XML dans le module `ui.apps`. Créez ensuite le composant AEM dans le module `ui.apps`.

>[!NOTE]
>
> Il peut être utile d&#39;actualiser rapidement les [bases des composants AEM](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html).

1. Dans l&#39;IDE de votre choix, ouvrez le dossier `ui.apps`.
2. Accédez à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` et créez un nouveau dossier nommé `custom-component`.
3. Créez un nouveau fichier nommé `.content.xml` sous le dossier `custom-component`. Renseignez `custom-component/.content.xml` comme suit :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Créer une définition de composant personnalisée](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifie que ce noeud sera un composant AEM.

   `jcr:title` est la valeur qui sera affichée pour les auteurs de contenu et qui  `componentGroup` détermine le regroupement des composants dans l’interface utilisateur de création.

4. Sous le dossier `custom-component`, créez un autre dossier nommé `_cq_dialog`.
5. Sous le dossier `_cq_dialog`, créez un nouveau fichier nommé `.content.xml` et renseignez-le avec ce qui suit :

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

   Le fichier XML ci-dessus génère une boîte de dialogue très simple pour `Custom Component`. La partie critique du fichier est le noeud interne `<message>`. Cette boîte de dialogue contient un `textfield` simple nommé `Message` et conserve la valeur du champ textile sur une propriété nommée `message`.

   Un modèle Sling sera créé en regard pour exposer la valeur de la propriété `message` via le modèle JSON.

   >[!NOTE]
   >
   > Vous pouvez vue beaucoup plus [d&#39;exemples de boîtes de dialogue en affichant les définitions de composants principaux](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Vous pouvez également vue d’autres champs de formulaire, tels que `select`, `textarea`, `pathfield`, disponibles sous `/libs/granite/ui/components/coral/foundation/form` dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Avec un composant AEM traditionnel, un script [HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/overview.html) est généralement requis. Comme le SPA va rendre le composant, aucun script HTML n’est nécessaire.

## Création du modèle Sling

Les modèles Sling sont des &quot;POJO&quot; Java pilotés par l’annotation (objets Java ordinaires) qui facilitent le mappage des données du JCR aux variables Java. [Sling ](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) Modelstypics, fonction permettant d’encapsuler une logique métier complexe côté serveur pour les composants AEM.

Dans le contexte de l’éditeur de SPA, les modèles Sling exposent le contenu d’un composant par le biais du modèle JSON à l’aide d’une fonction utilisant l’[Exportateur de modèle Sling](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/foundation/development/develop-sling-model-exporter.html).

1. Dans l&#39;IDE de votre choix, ouvrez le module `core`. `CustomComponent.java` et  `CustomComponentImpl.java` ont déjà été créés et ajoutés au code de démarrage du chapitre.

   >[!NOTE]
   >
   > Si vous utilisez Visual Studio Code IDE, il peut s&#39;avérer utile d&#39;installer des [extensions pour Java](https://code.visualstudio.com/docs/java/extensions).

2. Ouvrez l’interface Java `CustomComponent.java` à l’adresse `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java` :

   ![Interface CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Il s’agit de l’interface Java qui sera implémentée par le modèle Sling.

3. Mettez à jour `CustomComponent.java` de sorte qu&#39;il étende l&#39;interface `ComponentExporter` :

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   L’implémentation de l’interface `ComponentExporter` est une exigence pour que le modèle Sling soit automatiquement récupéré par l’API du modèle JSON.

   L&#39;interface `CustomComponent` comprend une méthode get unique `getMessage()`. Il s’agit de la méthode qui expose la valeur de la boîte de dialogue de l’auteur via le modèle JSON. Seules les méthodes getter avec des paramètres vides `()` seront exportées dans le modèle JSON.

4. Ouvrez `CustomComponentImpl.java` à `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Il s&#39;agit de l&#39;implémentation de l&#39;interface `CustomComponent`. L&#39;annotation `@Model` identifie la classe Java en tant que modèle Sling. L&#39;annotation `@Exporter` permet à la classe Java d&#39;être sérialisée et exportée via Sling Model Exporter.

5. Mettez à jour la variable statique `RESOURCE_TYPE` pour pointer vers le composant AEM `wknd-spa-angular/components/custom-component` créé lors de l&#39;exercice précédent.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   Le type de ressource du composant est celui qui lie le modèle Sling au composant AEM et qui sera finalement mappé au composant Angular.

6. Ajoutez la méthode `getExportedType()` à la classe `CustomComponentImpl` pour renvoyer le type de ressource de composant :

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Cette méthode est requise lors de l&#39;implémentation de l&#39;interface `ComponentExporter` et expose le type de ressource qui permet le mappage au composant Angular.

7. Mettez à jour la méthode `getMessage()` pour renvoyer la valeur de la propriété `message` conservée par la boîte de dialogue d’auteur. Utilisez l&#39;annotation `@ValueMap` pour mapper la valeur JCR `message` à une variable Java :

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

   Une autre &quot;logique métier&quot; est ajoutée pour renvoyer la valeur du message en majuscules. Cela nous permettra de voir la différence entre la valeur brute stockée par la boîte de dialogue d&#39;auteur et la valeur exposée par le modèle Sling.

   >[!NOTE]
   >
   > Vous pouvez vue le fichier [fini CustomComponentImpl.java ici](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Mettre à jour le composant angulaire

Le code angulaire du composant personnalisé a déjà été créé. Ensuite, effectuez quelques mises à jour pour mapper le composant Angular au composant AEM.

1. Dans le module `ui.frontend`, ouvrez le fichier `ui.frontend/src/app/components/custom/custom.component.ts`
2. Observez la ligne `@Input() message: string;`. La valeur en majuscules transformée devrait être mappée à cette variable.
3. Importez l’objet `MapTo` à partir du AEM SPA Editor JS SDK et utilisez-le pour mapper l’objet au composant AEM :

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Ouvrez `cutom.component.html` et observez que la valeur de `{{message}}` s’affichera à côté d’une balise `<h2>`.
5. Ouvrez `custom.component.css` et ajoutez la règle suivante :

   ```css
   :host-context {
       display: block;
   }
   ```

   Pour que l&#39;espace réservé de l&#39;éditeur AEM s&#39;affiche correctement lorsque le composant est vide, la valeur `:host-context` ou une autre valeur `<div>` doit être définie sur `display: block;`.

6. Déployez toutes les mises à jour vers un environnement AEM local à partir de la racine du répertoire du projet, en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Mettre à jour la stratégie de modèle

Accédez ensuite à AEM pour vérifier les mises à jour et autoriser l&#39;ajout de `Custom Component` au SPA.

1. Vérifiez l’enregistrement du nouveau modèle Sling en accédant à [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Vous devriez voir les deux lignes ci-dessus qui indiquent que `CustomComponentImpl` est associé au composant `wknd-spa-angular/components/custom-component` et qu&#39;il est enregistré via l&#39;Exportateur de modèle Sling.

2. Accédez au modèle de page SPA à l’adresse [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Mettez à jour la stratégie du Conteneur de mise en page pour ajouter le nouveau `Custom Component` en tant que composant autorisé :

   ![Mettre à jour la stratégie de Conteneur de mise en page](assets/custom-component/custom-component-allowed.png)

   Enregistrez les modifications apportées à la stratégie et observez `Custom Component` comme composant autorisé :

   ![Composant personnalisé en tant que composant autorisé](assets/custom-component/custom-component-allowed-layout-container.png)

## Création du composant personnalisé

Créez ensuite le `Custom Component` à l’aide de l’AEM SPA Editor.

1. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. En mode `Edit`, ajoutez `Custom Component` à `Layout Container` :

   ![Insérer un nouveau composant](assets/custom-component/insert-custom-component.png)

3. Ouvrez la boîte de dialogue du composant et saisissez un message contenant des lettres minuscules.

   ![Configuration du composant personnalisé](assets/custom-component/enter-dialog-message.png)

   Il s’agit de la boîte de dialogue qui a été créée en fonction du fichier XML précédemment dans le chapitre.

4. Enregistrez les modifications. Observez que le message affiché est en majuscules.

   ![Message affiché dans toutes les zones](assets/custom-component/message-displayed.png)

5. Vue du modèle JSON en accédant à [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Recherchez `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Notez que la valeur JSON est définie sur toutes les majuscules en fonction de la logique ajoutée au modèle Sling.

## Félicitations! {#congratulations}

Félicitations, vous avez appris à créer un composant AEM personnalisé et comment les modèles et boîtes de dialogue Sling fonctionnent avec le modèle JSON.

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) ou vérifier le code localement en passant à la branche `Angular/custom-component-solution`.

### Étapes suivantes {#next-steps}

[Étendre un composant](extend-component.md)  principal - Découvrez comment étendre un composant principal existant à utiliser avec l&#39;AEM SPA Editor. Comprendre comment ajouter des propriétés et du contenu à un composant existant est une technique puissante pour étendre les fonctionnalités d&#39;une mise en oeuvre AEM SPA Editor.
