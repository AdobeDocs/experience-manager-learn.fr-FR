---
title: Composant personnalisé
description: Couvre la création de bout en bout d’un composant de signature personnalisé qui affiche le contenu créé. Comprend le développement d’un modèle Sling pour encapsuler la logique métier afin de renseigner le composant de ligne guide et le code HTML correspondant pour effectuer le rendu du composant.
sub-product: sites
feature: sling-models
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '4011'
ht-degree: 0%

---


# Composant personnalisé {#custom-component}

Ce didacticiel porte sur la création de bout en bout d’un composant de ligne d’AEM personnalisé qui affiche le contenu créé dans une boîte de dialogue et explore le développement d’un modèle Sling pour encapsuler la logique métier qui renseigne le code HTML du composant.

## Conditions préalables {#prerequisites}

Examiner les outils et les instructions nécessaires à la mise en place d&#39;un environnement [de développement](overview.md#local-dev-environment)local.

### Projet de démarrage

>[!NOTE]
>
> Si vous avez suivi le cours dans les sections précédentes du didacticiel, vous remarquerez que le projet Starter pour ce chapitre accélère la mise en oeuvre. Il comprend quelques modèles supplémentaires et beaucoup plus de contenu. En plus, n’hésitez pas à explorer le nouveau contenu et d’autres zones de l’implémentation, en dehors du développement de composants personnalisés.

Consultez le code de ligne de base sur lequel le didacticiel s&#39;appuie :

1. Clonez le référentiel [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) .
1. Consulter la `custom-component/start` branche

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout custom-component/start
   ```

1. Déployez la base de code sur une instance AEM locale en utilisant vos compétences Maven :

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/custom-component/solution) ou le retirer localement en passant à la branche `custom-component/solution`.

## Intention

1. Comprendre comment créer un composant AEM personnalisé
1. Découvrez comment encapsuler la logique métier avec les modèles Sling
1. Comprendre comment utiliser un modèle Sling dans un script HTML

## Ce que vous allez construire {#byline-component}

>[!VIDEO](https://video.tv.adobe.com/v/30181/?quality=12&learn=on)

Dans cette partie du didacticiel WKND, un composant Byline est créé qui sera utilisé pour afficher les informations créées sur le contributeur d’un article.

![exemple de composant de signature](./assets/custom-component/byline-design.png)

*Conception visuelle des composants de signature fournie par l&#39;équipe de conception WKND*

L’implémentation du composant Signature comprend une boîte de dialogue qui collecte le contenu de la signature et un modèle Sling personnalisé qui récupère les éléments suivants :

* Nom
* Image
* Professions

pour l’affichage par un script HTL, qui effectue le rendu du code HTML que le navigateur affiche en fin de compte.

![décomposition par ligne](./assets/custom-component/byline-decomposition.png)

*Décomposition des composants de signature*

## Créer un composant Signature {#create-byline-component}

Tout d’abord, créez la structure de noeud Composant de signature et définissez une boîte de dialogue. Il représente le composant dans AEM et définit implicitement le type de ressource du composant en fonction de son emplacement dans le JCR.

La boîte de dialogue expose l’interface avec laquelle les auteurs de contenu peuvent fournir. Pour cette implémentation, le composant **Image** du composant principal de gestion de contenu Web AEM sera exploité pour gérer la création et le rendu de l’image de la signature, de sorte qu’elle sera définie comme celle de notre composant `sling:resourceSuperType`.

### Créer un noeud de composant {#create-component-node}

1. Dans le module **ui.apps** , accédez à `/apps/wknd/components/content` et créez un nouveau noeud appelé **signature** de type `cq:Component`.

   ![boîte de dialogue pour créer un noeud](./assets/custom-component/byline-node-creation.png)

1. Add the following properties to the Byline component&#39;s `cq:Component` node.

   ```plain
   jcr:title = Byline
   jcr:description = Displays a contributor's byline.
   componentGroup = WKND.Content
   sling:resourceSuperType =  core/wcm/components/image/v2/image
   ```

   ![Propriétés des composants de signature](./assets/custom-component/byline-component-properties.png)

   Ce qui génère ce `.content.xml` code XML :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND.Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

### Création du script HTML {#create-the-htl-script}

1. Sous le `byline` noeud, ajoutez un nouveau fichier `byline.html`, qui est responsable de la présentation HTML du composant. Il est important de nommer le fichier de la même manière que le `cq:Component` noeud car il devient le script par défaut utilisé par Sling pour effectuer le rendu de ce type de ressource.

1. ajoutez le code suivant sur le `byline.html`.

   ```xml
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` est [revisité ultérieurement](#byline-htl), une fois le modèle Sling créé. L’état actuel du fichier HTML permet au composant de s’afficher dans un état vide, dans l’éditeur de pages des sites AEM, lorsqu’il est glissé-déposé sur la page.

### Créer une définition de boîte de dialogue {#create-the-dialog-definition}

Ensuite, définissez une boîte de dialogue pour le composant Signature avec les champs suivants :

* **Nom**: champ de texte du nom du contributeur.
* **Image**: une référence à la biographie du contributeur.
* **Professions**: une liste de professions attribuées au contributeur. Les professions doivent être triées par ordre alphabétique croissant (a à z).

1. Beneath the `byline` component node create a new node named `cq:dialog` of type `nt:unstructured`.
1. Mettez à jour le fichier `cq:dialog` avec le code XML suivant. Il est plus facile d’ouvrir le fichier `.content.xml` et de copier/coller le XML suivant dans celui-ci.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
           xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
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
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
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

   Ces définitions de noeud utilisent [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) pour contrôler les onglets de dialogue hérités du `sling:resourceSuperType` composant, dans ce cas le composant **Image des composants** principaux.

   ![boîte de dialogue terminée pour la signature](./assets/custom-component/byline-dialog-created.png)

### Boîte de dialogue Créer une stratégie {#create-the-policy-dialog}

Suivant la même approche que lors de la création de la boîte de dialogue, créez une boîte de dialogue Stratégie (anciennement appelée boîte de dialogue de conception) pour masquer les champs indésirables dans la configuration Stratégie héritée du composant Image des composants principaux.

1. Beneath the `byline` `cq:Component` node, create a new node named `cq:design_dialog` of type `nt:unstructured`.
1. Mettez à jour le fichier `cq:design_dialog` avec le code XML suivant. Il est plus facile d’ouvrir le fichier `.content.xml` et de copier/coller le XML ci-dessous.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   Le code XML de la boîte de dialogue **** Stratégie précédente a été obtenu à partir du composant [Image des composants](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml)principaux.

   Comme dans la configuration de la boîte de dialogue, la fusion [de ressources](https://sling.apache.org/documentation/bundles/resource-merger.html) Sling est utilisée pour masquer les champs non pertinents qui sont par ailleurs hérités du `sling:resourceSuperType`, comme le montrent les définitions de noeud avec `sling:hideResource="{Boolean}true"` propriété.

### Déploiement du code {#deploy-the-code}

1. Déployez la base de code mise à jour sur une instance AEM locale à l’aide de vos compétences Maven :

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallPackage
   ```

### Add the component to a page {#add-the-component-to-a-page}

Pour simplifier les choses et se concentrer sur le développement des composants AEM, nous allons ajouter le composant Signature dans son état actuel à une page Article pour vérifier que la définition de `cq:Component` noeud est déployée et correcte, AEM reconnaît la nouvelle définition de composant et la boîte de dialogue du composant fonctionne pour la création.

Comme nous avons [ajouté le composant Byline au groupe **de composants** WKND.Content](#create-component-node), par l&#39;intermédiaire de la propriété `/apps/wknd/components/content/byline@componentGroup=WKND.Content` , il est automatiquement disponible pour tout Conteneur **de** mise en page dont Policy autorise le groupe de composants WKND.Content, que le Conteneur de mise en page de l&#39;article est.********

#### Drag and drop the component onto the page {#drag-and-drop-the-component-onto-the-page}

1. **Modifiez** la page de l&#39;article à l&#39;adresse **AEM > Sites > Site WKND > Principal linguistique > Anglais > Magazine > Guide ultime pour les parcs**&#x200B;à thème LA.
1. Dans la barre latérale gauche, faites glisser un composant **** Byline vers le **bas** du Conteneur de mise en page de la page de l’article ouvert.

   ![ajouter un composant de signature à la page](assets/custom-component/add-to-page.png)

#### Création du composant {#author-the-component}

Les auteurs AEM configurent et créent des composants au moyen des boîtes de dialogue. À ce stade du développement du composant Signature, les boîtes de dialogue sont incluses pour la collecte des données, mais la logique de rendu du contenu créé n’a pas encore été ajoutée.

1. Assurez-vous que la barre latérale **gauche est ouverte** et visible et que l’outil de recherche de **ressources** est sélectionné.

   ![open asset finder](assets/custom-component/open-asset-finder.png)

1. Sélectionnez l’espace réservé **du composant** Byline qui, à son tour, affiche la barre d’action et appuie sur l’icône de **clé à molette** pour ouvrir la boîte de dialogue.

   ![barre d&#39;action du composant](assets/custom-component/action-bar.png)

1. La boîte de dialogue s’ouvre et le premier onglet (Fichier) principal, ouvrez la barre latérale gauche, puis faites glisser une image dans la zone de dépôt Image à partir de l’outil de recherche de ressources. Recherchez &quot;stacey&quot; pour trouver la photo biographique de Stacey Roswells fournie dans le paquet ui.content de WKND.

   **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)**

   ![ajouter une image à la boîte de dialogue](assets/custom-component/add-image.png)

1. Après avoir ajouté une image, cliquez sur l’onglet **Propriétés** pour entrer le **Nom** et les **professions**.

   Lorsque vous entrez dans une profession, entrez-la dans un ordre alphabétique **** inverse, de sorte que la logique alphabétique des affaires que nous mettrons en oeuvre dans le modèle Sling soit facilement visible.

   Appuyez sur le bouton **Terminé** en bas à droite pour enregistrer les modifications.

   ![renseignez les propriétés du composant byline.](assets/custom-component/add-properties.png)

1. Après avoir enregistré la boîte de dialogue, accédez au [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/language-masters/en/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline) et vérifiez comment le contenu du composant est stocké sur le noeud de contenu du composant d’origine sous la page AEM.

   Recherchez le noeud de contenu du composant Byline sous le `jcr:content/root/responsivegrid/responsivegrid` noeud i.e. `/content/wknd/language-masters/en/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline`.

   Notez les noms des propriétés `name`, `occupations`et `fileReference` sont stockés sur le noeud **** de signature.

   Notez également que le noeud `sling:resourceType` du noeud est défini sur `wknd/components/content/byline` lequel est lié ce noeud de contenu à l’implémentation du composant Signature.

   ![propriétés de signature dans CRXDE](assets/custom-component/byline-properties-crxde.png)

   */content/wknd/language-masters/fr/magazine/guide-la-skateparks/jcr:content/root/reponvegrid/revegrid/byline*

## Créer un modèle Sling de signature {#create-sling-model}

Ensuite, nous allons créer un modèle Sling pour agir comme modèle de données et héberger la logique métier du composant Signature.

Les modèles Sling sont des &quot;POJO&quot; Java pilotés par les annotations (objets Java ordinaires) qui facilitent la mise en correspondance des données du JCR avec les variables Java et fournissent un certain nombre d&#39;autres subtilités lors du développement dans le contexte de l&#39;AEM.

### Vérifier les dépendances Maven {#maven-dependency}

Le modèle Byline Sling repose sur plusieurs API Java fournies par AEM. Ces API sont disponibles via la `dependencies` liste figurant dans le fichier POM du `core` module.

1. Ouvrez le `pom.xml` fichier sous `<src>/aem-guides-wknd/core/pom.xml`.
1. Recherchez la dépendance de la `uber-jar` dans la section des dépendances du fichier .pom :

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   L’ [uber-jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contient toutes les API Java publiques exposées par AEM. Notez qu’une version n’est pas spécifiée dans le `core/pom.xml` fichier. La version est plutôt conservée dans le pom du réacteur parent situé à la racine du projet `aem-guides-wknd/pom.xml`.

1. Recherchez la dépendance pour `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Il s’agit de l’ensemble des API Java publiques exposées par AEM Core Components. aem Core Components est un projet géré en dehors de AEM et qui a donc un cycle de publication distinct. C&#39;est pourquoi il s&#39;agit d&#39;une dépendance qui doit être incluse séparément et qui **ne l&#39;est pas** avec l&#39;uber-jar.

   Comme l&#39;uber-jar, la version de cette dépendance est conservée dans le fichier Pom du réacteur parent situé à `aem-guides-wknd/pom.xml`.

   Plus loin dans ce didacticiel, nous utiliserons la classe Image du composant principal pour afficher l&#39;image dans le composant Signature. Il est nécessaire d&#39;avoir la dépendance du composant principal pour construire et compiler notre modèle Sling.

### Interface de signature {#byline-interface}

Créez une interface Java publique pour la signature. `Byline.java` définit les méthodes publiques nécessaires pour piloter le script `byline.html` HTL.

1. Dans le `aem-guides-wknd.core` module sous `src/main/java,` créer une nouvelle interface Java nommée `Byline.java` en cliquant avec le bouton droit sur le `com.adobe.aem.guides.wknd.core.models` package > Nouveau > Interface ****. Saisissez **Signature** comme nom de l’interface, puis cliquez sur Terminer.

   ![créer une interface utilisateur](assets/custom-component/create-byline-interface.png)

1. Mettez à jour `Byline.java` avec les méthodes suivantes :

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   Les deux premières méthodes exposent les valeurs du **nom** et des **professions** du composant Signature.

   La `isEmpty()` méthode est utilisée pour déterminer si le composant a un contenu à rendre ou s’il attend d’être configuré.

   Notez qu’il n’existe aucune méthode pour l’image ; [nous allons examiner pourquoi cela se produira plus tard](#tackling-the-image-problem).

### Implémentation par signature {#byline-implementation}

`BylineImpl.java` est l’implémentation du modèle Sling qui implémente l’ `Byline.java` interface définie précédemment. Le code complet pour `BylineImpl.java` se trouve au bas de cette section.

1. Dans le `core` module sous `src/main/java`, créez un nouveau fichier de classe nommé **BylineImpl.java** en cliquant avec le bouton droit sur le `com.adobe.aem.guides.wknd.core.models.impl` package et en sélectionnant **Nouveau > Classe**.

   Pour le nom, saisissez **BylineImpl**. ajoutez l’interface **** Signature en tant qu’interface d’implémentation.

   ![créer une implémentation par ligne commutée](assets/custom-component/create-byline-impl.png)

1. Ouvrez `BylineImpl.java`. Il est automatiquement renseigné avec toutes les méthodes définies dans l’interface `Byline.java`. ajoutez les annotations de modèle Sling en les mettant à jour `BylineImpl.java` avec les annotations de niveau classe suivantes. Cette `@Model(..)`annotation transforme la classe en modèle Sling.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
       ...
   }
   ```

   Examinons cette annotation et ses paramètres :

   * L&#39; `@Model` annotation enregistre BylineImpl en tant que modèle Sling lors de son déploiement sur AEM.
   * Le `adaptables` paramètre spécifie que ce modèle peut être adapté par la demande.
   * Le `adapters` paramètre permet à la classe d&#39;implémentation d&#39;être enregistrée sous l&#39;interface Byline. Cela permet au script HTL d&#39;appeler le modèle Sling via l&#39;interface (au lieu de l&#39;impl directement). [Vous trouverez plus de détails sur les adaptateurs ici](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * Le `resourceType` pointeur vers le type de ressource de composant Byline (créé précédemment) et aide à résoudre le modèle correct en cas d’implémentations multiples. [Vous trouverez plus de détails sur l&#39;association d&#39;une classe de modèle à un type de ressource ici](https://sling.apache.org/documentation/bundles/application d’une seule pages.html#associating-a-application d’une seule page-class-with-a-resource-type-since-130).

### Implémentation des méthodes du modèle Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

La première méthode que nous allons aborder est `getName()` celle qui renvoie simplement la valeur stockée au noeud de contenu JCR de la signature sous la propriété `name`.

Pour ce faire, l’annotation Modèle `@ValueMapValue` Sling est utilisée pour injecter la valeur dans un champ Java à l’aide de la ValueMap de la ressource Request.

```java
...
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
...
public class BylineImpl implements Byline {
    ...

    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

Dans la mesure où la propriété JCR partage le même nom que le champ Java (les deux sont &quot;name&quot;), résout `@ValueMapValue` automatiquement cette association et injecte la valeur de la propriété dans le champ Java.

#### getOccupations() {#implementing-get-occupations}

La méthode suivante à implémenter est `getOccupations()`. Cette méthode collecte toutes les professions stockées dans la propriété JCR `occupations` et renvoie une collection triée (alphabétique).

L’utilisation de la même technique explorée dans `getName()` la valeur de propriété peut être injectée dans le champ Modèle Sling.

Une fois que les valeurs de propriété JCR sont disponibles dans le modèle Sling via le champ Java injecté `occupations`, la logique de tri métier peut être appliquée dans la `getOccupations()` méthode.

```java
import java.util.ArrayList;
import java.util.Collections;
...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
...
```

#### isEmpty() {#implementing-is-empty}

La dernière méthode publique est `isEmpty()` celle qui détermine à quel moment le composant doit se considérer comme &quot;suffisamment créé&quot; pour être rendu.

Pour ce composant, nous avons des exigences commerciales stipulant que les trois champs, nom, image et professions doivent être remplis *avant* que le composant puisse être rendu.

```java
import org.apache.commons.lang3.StringUtils;
...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```

#### Lutter contre le &quot;problème de l&#39;image&quot; {#tackling-the-image-problem}

La vérification des conditions de nom et d&#39;occupation est triviale (et Apache Commons Lang3 fournit la classe [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) toujours pratique), mais on ne sait pas comment la **présence de l&#39;image** peut être validée puisque le composant Image des composants principaux est utilisé pour refaire la surface de l&#39;image.

Il existe deux façons de s&#39;attaquer à ce problème :

1. Vérifiez si la propriété `fileReference` JCR correspond à une ressource.
1. Convertissez cette ressource en modèle Sling d&#39;image de composant principal et assurez-vous que la `getSrc()` méthode n&#39;est pas vide.

   Nous allons opter pour la **deuxième** approche. La première approche est probablement suffisante, mais dans ce tutoriel, celle-ci sera utilisée pour nous permettre d&#39;explorer d&#39;autres caractéristiques des modèles Sling.

1. Créez une méthode privée qui obtient l’image. Cette méthode est laissée privée, car il n’est pas nécessaire d’exposer l’objet Image dans le fichier HTML lui-même, et elle est utilisée uniquement pour le lecteur. `isEmpty().`

   Méthode privée suivante pour `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Comme nous l&#39;avons mentionné plus haut, il existe deux autres méthodes pour obtenir le modèle **Sling** d&#39;image :

   Le premier utilise l&#39; `@Self` annotation pour adapter automatiquement la requête active au composant principal. `Image.class`

   ```java
   @Self
   private Image image;
   ```

   Le second utilise le service [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi, qui est très pratique, et nous aide à créer des modèles Sling d&#39;autres types dans le code Java.

   Nous allons opter pour la deuxième approche.

   >[!NOTE]
   >
   >Dans une mise en oeuvre réelle, il est préférable d&#39;utiliser &quot;One&quot;, car c&#39; `@Self` est la solution la plus simple et la plus élégante. Dans ce tutoriel, nous utiliserons la deuxième approche, car elle nous demande d&#39;explorer plus de facettes de modèles Sling qui sont extrêmement utiles sont des composants plus complexes !

   Comme les modèles Sling sont des modèles Java POJO et non des services OSGi, les annotations d&#39;injection OSGi habituelles `@Reference` ne **peuvent pas** être utilisées, mais les modèles Sling fournissent une annotation spéciale **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** qui fournit des fonctionnalités similaires.

1. Mettez à jour `BylineImpl.java` pour inclure l’ `OSGiService` annotation afin d’injecter les `ModelFactory`:

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   Avec la `ModelFactory` disponibilité, un modèle Sling d’image de composant principal peut être créé à l’aide des éléments suivants :

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Cependant, cette méthode requiert à la fois une requête et une ressource, qui n&#39;ont pas encore été disponibles dans le modèle Sling. Pour les obtenir, davantage d&#39;annotations de modèle Sling sont utilisées !

   Pour obtenir la requête actuelle, l&#39;annotation **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** peut être utilisée pour injecter le `adaptable` (défini dans la `@Model(..)` sous comme `SlingHttpServletRequest.class`) dans un champ de classe Java.

1. ajoutez l’annotation **@Self** pour obtenir la demande **** SlingHttpServletRequest :

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   N&#39;oubliez pas que l&#39;utilisation `@Self Image image` pour injecter le modèle Sling d&#39;image du composant principal était une option ci-dessus : l&#39; `@Self` annotation tente d&#39;injecter l&#39;objet adaptable (dans notre cas, un SlingHttpServletRequest) et s&#39;adapte au type de champ d&#39;annotation. Puisque le modèle Sling d’image du composant principal est adaptable à partir des objets SlingHttpServletRequest, cela aurait fonctionné et est moins de code que notre approche plus exploratoire.

   Nous avons maintenant injecté les variables nécessaires pour instancier notre modèle d&#39;image via l&#39;API ModelFactory. Nous utiliserons l&#39;annotation **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** de Sling Model pour obtenir cet objet après l&#39;instanciation du modèle Sling.

   `@PostConstruct` est incroyablement utile et agit dans une capacité similaire à un constructeur, cependant, il est appelé après l&#39;instanciation de la classe et tous les champs Java annotés sont injectés. Alors que d&#39;autres annotations de modèle Sling annotent des champs de classe Java (variables), `@PostConstruct` annote une méthode de paramètre void, zero, généralement nommée `init()` (mais peut être nommée tout).

1. Méthode Ajoute **@PostConstruct** :

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   N’oubliez pas que les modèles Sling ne sont **PAS** des services OSGi, il est donc sûr de maintenir l’état de classe. Souvent `@PostConstruct` dérive et configure l&#39;état de classe de modèle Sling pour une utilisation ultérieure, comme le fait un constructeur simple.

   Notez que si la `@PostConstruct` méthode renvoie une exception, le modèle Sling n’instanciera pas (il sera nul).

1. **getImage()** peut désormais être mis à jour pour renvoyer simplement l’objet image.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Retournons à `isEmpty()` la mise en oeuvre et terminons :

   ```java
   @Override
   public boolean isEmpty() {
       ...
       } else if (getImage() == null || StringUtils.isBlank(getImage().getSrc())) {
           // A valid image is required
           return true;
       } else {
       ...
   }
   ```

   Notez que plusieurs appels à `getImage()` n&#39;est pas problématique car renvoie la variable `image` de classe initialisée et n&#39;appelle pas `modelFactory.getModelFromWrappedRequest(...)` ce qui n&#39;est pas trop coûteux, mais vaut la peine d&#39;éviter d&#39;appeler inutilement.

1. La finale `BylineImpl.java` devrait se présenter comme suit :

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   
   import javax.annotation.PostConstruct;
   
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image image = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (image == null || StringUtils.isBlank(image.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```

## Signature HTL {#byline-htl}

Dans le `ui.apps` module, ouvrez `/apps/wknd/components/content/byline/byline.html` nous avons créé dans la configuration précédente du composant AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Examinons ce que ce script HTML fait jusqu’à présent :

* Le `placeholderTemplate` pointeur vers l’espace réservé des composants principaux s’affiche lorsque le composant n’est pas entièrement configuré. Cette opération effectue le rendu dans l’éditeur de page AEM Sites sous la forme d’une zone avec le titre du composant, comme défini ci-dessus dans la `cq:Component`propriété `jcr:title` ’s.

* Le `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` code charge la `placeholderTemplate` valeur définie ci-dessus et transmet une valeur booléenne (actuellement codée en dur `false`) dans le modèle d’espace réservé. Si `isEmpty` la valeur est true, le modèle d’espace réservé effectue le rendu de la zone grise, sans quoi il ne rend rien.

### Mettre à jour la signature HTL

1. Mettez à jour **byline.html** avec la structure HTML squelettique suivante :

   ```xml
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!-- Include the Core Components Image Component -->
           </div>
           <h2 class="cmp-byline__name"><!-- Include the name --></h2>
           <p class="cmp-byline__occupations"><!-- Include the occupations --></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Notez que les classes CSS respectent la convention [d’affectation de nom](https://getbem.com/naming/)BEM. Bien que l’utilisation des conventions BEM ne soit pas obligatoire, BEM est recommandé car il est utilisé dans les classes CSS des composants principaux et donne généralement lieu à des règles CSS propres et lisibles.

#### Instanciation des objets Modèle Sling dans HTL {#instantiating-sling-model-objects-in-htl}

L’instruction [de bloc](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) Use est utilisée pour instancier des objets Modèle Sling dans le script HTL et les affecter à une variable HTL.

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` utilise l’interface Byline (com.adobe.aem.guides.wknd.models.Byline) implémentée par BylineImpl et y adapte la SlingHttpServletRequest actuelle, et le résultat est stocké dans une variable HTL nom par ligne ( `data-sly-use.<variable-name>`).

1. Mettez à jour l’extérieur `div` pour référencer le modèle Sling **Byline** par son interface publique :

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

#### Accès aux méthodes du modèle Sling {#accessing-sling-model-methods}

HTL emprunte à partir de JSTL et utilise le même raccourcissement des noms de méthodes d’accesseur Java getter.

Par exemple, l’appel de la `getName()` méthode du modèle Sling de signature peut être raccourci pour `byline.name`, de la même manière, au lieu de `byline.isEmpty`la raccourcir, en faire `byline.empty`. L’utilisation de noms de méthode complets, `byline.getName` ou `byline.isEmpty`, fonctionne également. Notez que les méthodes ne `()` sont jamais utilisées pour appeler des méthodes dans HTL (comme JSTL).

Les méthodes Java nécessitant un paramètre **ne peuvent pas** être utilisées dans HTL. C&#39;est par conception pour garder la logique dans HTL simple.

1. Le nom de la signature peut être ajouté au composant en appelant la `getName()` méthode sur le modèle de signature tactile ou dans le fichier HTML : `${byline.name}`.

   Mettez à jour la `h2` balise :

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

#### Utilisation des options d&#39;Expression HTML {#using-htl-expression-options}

[Les options](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) d’Expressions HTML agissent comme des modificateurs sur le contenu HTML et vont de la mise en forme de date à la traduction i18n. Les Expressions peuvent également être utilisées pour joindre des listes ou des tableaux de valeurs, ce qui est nécessaire pour afficher les professions dans un format délimité par des virgules.

Les Expressions sont ajoutées via l’ `@` opérateur dans l’expression HTL.

1. Pour joindre la liste des professions avec &quot;, &quot;, le code suivant est utilisé :

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

#### Affichage conditionnel de l’espace réservé {#conditionally-displaying-the-placeholder}

La plupart des scripts HTML pour les composants AEM utilisent le paradigme **de l’** espace réservé pour fournir un indice visuel aux auteurs **indiquant qu’un composant est créé de manière incorrecte et ne s’affichera pas dans AEM Publish**. La convention qui motive cette décision est de mettre en oeuvre une méthode sur le modèle Sling de support du composant, dans notre cas : `Byline.isEmpty()`.

`isEmpty()` est appelé sur le modèle de signature Sling et le résultat (ou plutôt négatif, via l’ `!` opérateur) est enregistré dans une variable HTL nommée `hasContent`:

1. Mettez à jour l&#39;extérieur `div` pour enregistrer une variable HTL nommée `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Notez l’utilisation de `data-sly-test`, le `test` bloc HTML est intéressant dans la mesure où il définit une variable HTL ET effectue/ne rend pas l’élément HTML sur lequel il est basé, selon si le résultat de l’expression HTL est véridique ou non. Si &quot;true&quot;, l’élément HTML est rendu, sinon il n’est pas rendu.

   Cette variable HTL `hasContent` peut désormais être réutilisée pour afficher/masquer de manière conditionnelle l’espace réservé.

1. Mettez à jour l’appel conditionnel vers le `placeholderTemplate` bas du fichier avec ce qui suit :

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

#### Affichage de l’image à l’aide des composants principaux {#using-the-core-components-image}

Le script HTL pour `byline.html` est maintenant presque terminé et ne manque que l’image.

```html
<!--/* current progress of byline.html */-->
<div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!byline.empty}"
     class="cmp-byline">
    <div class="cmp-byline__image">
        <!-- Include the Core Components Image component -->
    </div>
    <h2 class="cmp-byline__name">${byline.name}</h2>
    <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
```

Puisque nous utilisons `sling:resourceSuperType` le composant Image des composants principaux pour créer l’image, nous pouvons également utiliser le composant Image du composant principal pour générer l’image !

Pour ce faire, nous devons inclure la ressource actuelle par ligne, mais forcer le type de ressource du composant Image des composants principaux, en utilisant le type de ressource `core/wcm/components/image/v2/image`. C&#39;est un modèle puissant pour la réutilisation des composants. Pour cela, le `data-sly-resource` bloc HTL est utilisé.

1. Remplacez le par `div` une classe de `cmp-byline__image` par la classe suivante :

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Ceci `data-sly-resource`incluait la ressource actuelle via le chemin d&#39;accès relatif `'.'`et force l&#39;inclusion de la ressource actuelle (ou de la ressource de contenu par ligne) avec le type de ressource `core/wcm/components/image/v2/image`.

   Le type de ressource Composant principal est utilisé directement, et non par le biais d&#39;un proxy, parce qu&#39;il s&#39;agit d&#39;une utilisation en script, et qu&#39;il n&#39;est jamais conservé dans notre contenu.

2. Terminé `byline.html` ci-dessous :

   ```html
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
            data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
           <h2 class="cmp-byline__name">${byline.name}</h2>
           <p class="cmp-byline__occupations">${byline.occupations @ join=','}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Déployez la base de code sur une instance AEM locale. Comme des modifications majeures ont été apportées aux fichiers POM, effectuez une compilation Maven complète à partir du répertoire racine du projet.

   >[!WARNING]
   >
   > Notez que le projet WKND est configuré de telle sorte qu’il `ui.content` remplacera toute modification du JCR. Nous voulons donc nous assurer que nous déployons uniquement le `ui.apps` projet pour éviter d’effacer le composant Signature ajouté à la page d’article plus tôt.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.apps
   $ mvn -PautoInstallPackage clean install
   ...
   Package imported.
   Package installed in 338ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

#### Vérification du composant Byline non stylisé {#reviewing-the-unstyled-byline-component}

1. Après avoir déployé la mise à jour, accédez à la page Guide [final de la page LA Skateparks ](http://localhost:4502/editor.html/content/wknd/language-masters/en/magazine/guide-la-skateparks.html) ou à l’emplacement où vous avez ajouté le composant Signature plus haut dans le chapitre.

1. L&#39; **image**, le **nom** et les **professions** apparaissent maintenant et nous avons un composant Byline sans style, mais fonctionnel.

   ![composant de ligne d’entrée sans style](assets/custom-component/unstyled.png)

#### Vérification de l&#39;enregistrement du modèle Sling {#reviewing-the-sling-model-registration}

La vue [d&#39;état des modèles Sling de la console Web](http://localhost:4502/system/console/status-slingmodels) AEM affiche tous les modèles Sling enregistrés en AEM. Le modèle Sling de signature peut être validé comme étant installé et reconnu en examinant cette liste.

Si le **BylineImpl** n’est pas affiché dans cette liste, il y a probablement un problème avec les annotations du modèle Sling ou si le modèle Sling n’a pas été ajouté au package de modèles Sling enregistrés (com.adobe.aem.guides.wknd.core.models) dans le projet principal.

![Modèle Sling de signature enregistré](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## Styles de signature {#byline-styles}

Le composant Signature doit être mis en forme pour correspondre à la conception créative du composant Signature. Pour ce faire, il utilisera le SCSS, qui AEM fournit une assistance via le sous-projet **ui.frontend** Maven.

Après la mise en forme, le composant Signature doit adopter l’esthétique suivante.

![styles fictifs de ligne](./assets/custom-component/byline-design.png)

*Conception de composant de signature définie par l’équipe de création WKND*

### ajouter un style par défaut

ajoutez les styles par défaut pour le composant Signature. Dans le projet **ui.frontend** sous `/src/main/webpack/components/content`:

1. Create a new folder named `byline`.
1. Créez un dossier sous le `byline` dossier nommé `scss`.
1. Créez un nouveau fichier sous `byline/scss` le dossier nommé `byline.scss`.
1. Créez un dossier sous le `byline/scss` dossier nommé `styles`.
1. Créez un nouveau fichier sous `byline/scss/styles` le dossier nommé `default.scss`.

   ![explorateur de projets de signature](assets/custom-component/byline-style-project-explorer.png)

1. Début en renseignant **byline.scss** pour inclure le style par défaut :

   ```scss
    /* WKND Byline styles */
   @import 'styles/default';
   ```

1. ajoutez le CSS d’implémentation par signature (écrit en tant que SCSS) dans `default.scss`:

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-large;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. Ouvrez le fichier `main.scss` dans le projet **ui.frontend** sous `/src/main/webpack/site` et ajoutez la ligne suivante dans la `/* Components */` section :

   ```scss
   @import '../components/content/byline/scss/byline.scss';
   ```

1. Créez et compilez le `ui.frontend` module à l&#39;aide de NPM :

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend
    $ npm run dev
   ```

1. Créez et déployez le `ui.apps` projet, qui inclura de manière transitoire le `ui.frontend` projet, sur une instance AEM locale à l’aide de Maven :

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.apps
    $ mvn clean install -PautoInstallPackage
   ```

   >[!TIP]
   >
   >Vous devrez peut-être vider le cache du navigateur pour vous assurer que le CSS obsolète n’est pas diffusé et actualiser la page avec le composant Signature pour obtenir le style complet.

## Putting It Together {#putting-it-together}

Vous trouverez ci-dessous ce à quoi doit ressembler le composant Signature entièrement créé et stylisé sur la page AEM.

![composant de signature terminé](assets/custom-component/final-byline-component.png)

Regardez la vidéo ci-dessous pour un aperçu rapide de ce qui a été construit dans ce tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/30174/?quality=12&learn=on)

## Félicitations ! {#congratulations}

Félicitations, vous venez de créer un composant personnalisé à partir de zéro en utilisant Adobe Experience Manager !

### Étapes suivantes {#next-steps}

Continuez à en savoir plus sur le développement de composants AEM en explorant comment écrire des tests JUnit pour le code Java Byline pour vous assurer que tout est développé correctement et que la logique métier implémentée est correcte et complète.

* [Ecriture de tests unitaires ou de composants AEM](unit-testing.md)

Vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur l&#39;embrayage Git `custom-component/solution`.

1. Clonez le référentiel [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) .
1. Consulter la `custom-component/solution` branche

## Résolution des incidents {#troubleshooting}

### Dossiers source manquants

Si vous ne voyez pas le dossier `src/main/java` source dans Eclipse, vous pouvez ajouter les dossiers en cliquant avec le bouton droit de la souris sur src et en ajoutant des dossiers pour main et java. Après avoir ajouté les dossiers, le `src/main/java` package s’affiche.

### Packages non résolus

![résoudre les problèmes liés aux packs non résolus](assets/custom-component/troubleshoot-unresolved-packages.png)

>[!NOTE]
>
> Si vous avez des importations de packages non résolues pour certaines des nouvelles dépendances ajoutées au projet principal, essayez de mettre à jour le projet aem-guides-wknd maven, qui mettra à jour tous les sous-projets. Pour ce faire, cliquez avec le bouton droit de la souris sur **aem-guides-wknd > expert > Mettre à jour le projet**.
