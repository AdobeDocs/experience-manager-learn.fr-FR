---
title: Créer un composant personnalisé | Prise en main de l’éditeur de SPA d’AEM et d’Angular
description: Découvrez comment créer un composant personnalisé à utiliser avec l’éditeur de SPA d’AEM. Découvrez comment développer des boîtes de dialogue de création et des modèles Sling pour étendre le modèle JSON afin de remplir un composant personnalisé.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
duration: 308
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: ht
source-wordcount: '1321'
ht-degree: 100%

---

# Créer un composant personnalisé {#custom-component}

{{spa-editor-deprecation}}

Découvrez comment créer un composant personnalisé à utiliser avec l’éditeur de SPA d’AEM. Découvrez comment développer des boîtes de dialogue de création et des modèles Sling pour étendre le modèle JSON afin de remplir un composant personnalisé.

## Objectif

1. Comprendre le rôle des modèles Sling dans la manipulation de l’API de modèle JSON fournie par AEM.
2. Découvrir comment créer des boîtes de dialogue de composants AEM.
3. Découvrir comment créer un composant AEM **personnalisé** compatible avec le framework de l’éditeur de SPA.

## Ce que vous allez créer

Les chapitres précédents portaient principalement sur le développement de composants SPA et leur mappage aux composants principaux AEM *existants*. Ce chapitre se concentre sur la création et l’extension de *nouveaux* composants AEM et sur la manipulation du modèle JSON fourni par AEM.

Un `Custom Component` illustre les étapes nécessaires à la création d’un nouveau composant AEM.

![Message affiché en majuscules](assets/custom-component/message-displayed.png)

## Prérequis

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Obtenir le code

1. Téléchargez le point de départ de ce tutoriel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Déployez la base de code sur une instance AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility) ajoutez le profil `classic` :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installez le package terminé pour le [site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) classique. Les images fournies par le [site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) sont réutilisées dans la SPA WKND. Le package peut être installé à l’aide du [Gestionnaire de packages d’AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Installation du gestionnaire de packages wknd.all](./assets/map-components/package-manager-wknd-all.png)

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) ou consulter le code localement en passant à la branche `Angular/custom-component-solution`.

## Définir le composant AEM

Un composant AEM est défini comme un nœud et des propriétés. Dans le projet, ces nœuds et propriétés sont représentés sous la forme de fichiers XML dans le module `ui.apps`. Il suffit ensuite de créer le composant AEM dans le module `ui.apps`.

>[!NOTE]
>
> Un petit rappel sur les [notions de base des composants AEM peut s’avérer utile](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=fr).

1. Ouvrez le dossier `ui.apps` dans l’IDE de votre choix.
2. Accédez à `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` et créez un dossier appelé `custom-component`.
3. Créez un fichier appelé `.content.xml` sous le dossier `custom-component`. Remplissez `custom-component/.content.xml` avec les éléments suivants :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Définition d’un composant personnalisé de création.](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` : identifie que ce nœud est un composant AEM.

   `jcr:title` est la valeur affichée pour les auteurs et autrices de contenu et le `componentGroup` détermine le regroupement des composants dans l’interface utilisateur de création.

4. Sous le dossier `custom-component`, créez un autre dossier appelé `_cq_dialog`.
5. Sous le dossier `_cq_dialog`, créez un fichier appelé `.content.xml` et remplissez-le avec les éléments suivants :

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

   ![Définition d’un composant personnalisé](assets/custom-component/dialog-custom-component-defintion.png)

   Le fichier XML ci-dessus génère une boîte de dialogue simple pour le `Custom Component`. La partie essentielle du fichier est le nœud interne `<message>`. Cette boîte de dialogue contient un simple `textfield` nommé `Message` et conserve la valeur du champ de texte dans une propriété nommée `message`.

   Un modèle Sling est ensuite créé pour exposer la valeur de la propriété `message` via le modèle JSON.

   >[!NOTE]
   >
   > Vous pouvez afficher beaucoup plus [d’exemples de boîtes de dialogue en affichant les définitions des composants principaux](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Vous pouvez également afficher des champs de formulaire supplémentaires, tels que `select`, `textarea`, `pathfield`, disponibles sous `/libs/granite/ui/components/coral/foundation/form` dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Avec un composant d’AEM traditionnel, un script [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=fr) est généralement requis. Étant donné que la SPA effectue le rendu du composant, aucun script HTL n’est nécessaire.

## Créer le modèle Sling

Les modèles Sling sont des objets POJO (Plain Old Java™ Object) Java™ pilotés par des annotations qui facilitent le mappage des données du JCR avec les variables Java™. [Les modèles Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=fr#sling-models) servent en règle générale à encapsuler une logique commerciale complexe côté serveur pour les composants AEM.

Dans le contexte de l’éditeur de SPA, les modèles Sling exposent le contenu d’un composant par le biais du modèle JSON par un fonctionnalité qui utilise l’[Exporteur de modèles Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=fr).

1. Dans l’IDE de votre choix, ouvrez le module `core`. `CustomComponent.java` et `CustomComponentImpl.java` ont déjà été créés et bouchés dans le cadre du code de démarrage de projet.

   >[!NOTE]
   >
   > Si vous utilisez l’IDE Visual Studio Code, il peut s’avérer utile d’installer des [extensions pour Java™](https://code.visualstudio.com/docs/java/extensions).

2. Ouvrez le `CustomComponent.java` d’interface Java™ dans `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java` :

   ![Interface du CustomComponent.java.](assets/custom-component/custom-component-interface.png)

   Il s’agit de l’interface Java™ implémentée par le modèle Sling.

3. Mettez à jour `CustomComponent.java` pour qu’il étende l’interface du `ComponentExporter` :

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   L’implémentation de l’interface du `ComponentExporter` est obligatoire pour que le modèle Sling soit automatiquement récupéré par l’API du modèle JSON.

   L’interface `CustomComponent` comprend une méthode getter unique `getMessage()`. Il s’agit de la méthode qui expose la valeur de la boîte de dialogue de création par le biais du modèle JSON. Seules les méthodes getter avec des paramètres vides `()` sont exportées dans le modèle JSON.

4. Ouvrez le `CustomComponentImpl.java` dans `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Il s’agit de l’implémentation de l’interface du `CustomComponent`. L’annotation `@Model` identifie la classe Java™ en tant que modèle Sling. L’annotation `@Exporter` permet à la classe Java™ d’être sérialisée et exportée via l’exporteur de modèle Sling.

5. Mettez à jour la variable statique `RESOURCE_TYPE` pour pointer vers le composant AEM `wknd-spa-angular/components/custom-component` créé dans l’exercice précédent.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   Le type de ressource du composant est ce qui lie le modèle Sling au composant AEM et qui le mappe finalement au composant Angular.

6. Ajoutez la méthode `getExportedType()` à la classe `CustomComponentImpl` pour renvoyer le type de ressource du composant :

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Cette méthode est requise lors de l’implémentation de l’interface du `ComponentExporter` et expose le type de ressource qui permet le mappage au composant Angular.

7. Mettez à jour la méthode `getMessage()` pour renvoyer la valeur de la propriété `message` conservée par la boîte de dialogue de création. Utilisez l’annotation `@ValueMap` pour mapper la valeur JCR `message` à une variable Java™ :

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

   Une « logique commerciale » supplémentaire est ajoutée pour renvoyer la valeur du message en majuscules. Cela nous permet de voir la différence entre la valeur brute stockée par la boîte de dialogue de création et la valeur exposée par le modèle Sling.

   >[!NOTE]
   >
   > Vous pouvez visualiser ici le [CustomComponentImpl.java terminé](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Mettre à jour le composant Angular

Le code Angular du composant personnalisé a déjà été créé. Effectuez ensuite quelques mises à jour pour mapper le composant Angular au composant AEM.

1. Dans le module `ui.frontend`, ouvrez le fichier `ui.frontend/src/app/components/custom/custom.component.ts`.
2. Observez la ligne `@Input() message: string;`. La valeur transformée en majuscules est normalement mappée à cette variable.
3. Importez l’objet `MapTo` à partir du SDK JS de l’éditeur de SPA d’AEM et utilisez-le pour mapper au composant AEM :

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Ouvrez `cutom.component.html` et observez que la valeur de `{{message}}` s’affiche à côté d’une balise `<h2>`.
5. Ouvrez `custom.component.css` et ajoutez la règle suivante :

   ```css
   :host-context {
       display: block;
   }
   ```

   Pour que l’espace réservé de l’éditeur AEM s’affiche correctement lorsque le composant est vide, le `:host-context` ou un autre `<div>` doit être défini sur `display: block;`.

6. Déployez les mises à jour dans un environnement AEM local à partir de la racine du répertoire du projet, en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Mettre à jour la stratégie des modèles

Ensuite, accédez à AEM pour vérifier les mises à jour et autoriser le `Custom Component` à être ajouté à la SPA.

1. Vérifiez l’enregistrement du nouveau modèle Sling en accédant à [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Vous devriez voir les deux lignes ci-dessus qui indiquent que le `CustomComponentImpl` est associé au composant `wknd-spa-angular/components/custom-component` et qu’il est enregistré via l’exporteur de modèle Sling.

2. Accédez au modèle de page SPA à l’adresse [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Mettez à jour la stratégie du conteneur de disposition pour ajouter le nouveau `Custom Component` en tant que composant autorisé :

   ![Mise à jour de la stratégie du conteneur de disposition.](assets/custom-component/custom-component-allowed.png)

   Enregistrez les modifications apportées à la stratégie et observez le composant `Custom Component` en tant que composant autorisé :

   ![Composant personnalisé en tant que composant autorisé.](assets/custom-component/custom-component-allowed-layout-container.png)

## Créer le composant personnalisé

Ensuite, créez le `Custom Component` à l’aide de l’éditeur de SPA AEM.

1. Accédez à [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. Dans le mode `Edit`, ajoutez le `Custom Component` au `Layout Container` :

   ![Insérer un nouveau composant](assets/custom-component/insert-custom-component.png)

3. Ouvrez la boîte de dialogue du composant et saisissez un message contenant des lettres minuscules.

   ![Configuration du composant personnalisé.](assets/custom-component/enter-dialog-message.png)

   Il s’agit de la boîte de dialogue qui a été créée à partir du fichier XML plus tôt dans le chapitre.

4. Enregistrez les modifications. Notez que le message affiché est en majuscules.

   ![Message affiché en majuscules.](assets/custom-component/message-displayed.png)

5. Affichez le modèle JSON en accédant à [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Recherchez `wknd-spa-angular/components/custom-component` :

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Notez que la valeur JSON est définie en majuscules en fonction de la logique ajoutée au modèle Sling.

## Félicitations ! {#congratulations}

Félicitations, vous avez appris à créer un composant AEM personnalisé et comment les modèles et boîtes de dialogue Sling fonctionnent avec le modèle JSON.

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) ou consulter le code localement en passant à la branche `Angular/custom-component-solution`.

### Étapes suivantes {#next-steps}

[Étendre un composant principal](extend-component.md) - Découvrez comment étendre un composant principal existant à utiliser avec l’éditeur de SPA AEM. Comprendre comment ajouter des propriétés et du contenu à un composant existant est une technique puissante pour étendre les fonctionnalités d’une implémentation d’éditeur de SPA AEM.
