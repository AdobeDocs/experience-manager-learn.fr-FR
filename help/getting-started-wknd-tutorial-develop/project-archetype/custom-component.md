---
title: Custom Component (composant personnalisé)
description: Couvre la création de bout en bout d’un composant de signature personnalisé qui affiche le contenu créé. Inclut le développement d’un modèle Sling pour encapsuler la logique commerciale afin de renseigner le composant de signature et le HTL correspondant pour effectuer le rendu du composant.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: 68a7f263284fdf9cfcf82572b8e1e1c0c01e4b55
workflow-type: tm+mt
source-wordcount: '4066'
ht-degree: 99%

---

# Custom Component (composant personnalisé) {#custom-component}

Ce tutoriel décrit la création de bout en bout d’un composant AEM de `Byline` personnalisé qui affiche le contenu créé dans une boîte de dialogue et explore le développement d’un modèle Sling pour encapsuler la logique commerciale qui renseigne le HTL du composant.

## Prérequis {#prerequisites}

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Projet de démarrage

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes de consultation du projet de démarrage.

Consultez le code de ligne de base sur lequel le tutoriel s’appuie :

1. Consultez la branche `tutorial/custom-component-start` à partir de [GitHub](https://github.com/adobe/aem-guides-wknd).

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Déployez la base de code sur une instance locale d’AEM à l’aide de vos compétences Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` à n’importe quelle commande Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) ou consulter le code localement en passant à la branche `tutorial/custom-component-solution`.

## Objectif

1. Comprendre comment créer un composant AEM personnalisé
1. Découvrir comment encapsuler la logique commerciale avec les modèles Sling
1. Comprendre comment utiliser un modèle Sling dans un script HTL

## Ce que vous allez construire {#what-build}

Dans cette partie du tutoriel WKND, un composant de signature est créé afin d’afficher les informations créées sur le contributeur ou la contributrice d’un article.

![exemple de composant de signature](assets/custom-component/byline-design.png)

*Composant de signature*

L’implémentation du composant de signature comprend une boîte de dialogue qui collecte le contenu de la signature et un modèle Sling personnalisé qui récupère les détails tels que :

* Nom
* Image
* Professions

## Créer un composant de signature {#create-byline-component}

Créez d’abord la structure de nœud du composant de signature et définissez une boîte de dialogue. Cela représente le composant dans AEM et définit implicitement le type de ressource du composant en fonction de son emplacement dans le JCR.

La boîte de dialogue expose l’interface que les auteurs et autrices de contenu peuvent fournir. Pour cette implémentation, le composant **Image** du composant principal de la gestion du contenu web AEM est utilisé pour gérer la création et le rendu de l’image de la signature. Il doit donc être défini comme le `sling:resourceSuperType` de ce composant.

### Créer une définition de composant {#create-component-definition}

1. Dans le module **ui.apps**, accédez à `/apps/wknd/components` et créez un dossier nommé `byline`.
1. Dans le dossier `byline`, ajoutez un fichier nommé `.content.xml`.

   ![boîte de dialogue pour créer le nœud](assets/custom-component/byline-node-creation.png)

1. Remplissez le fichier `.content.xml` avec les éléments suivants :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   Le fichier XML ci-dessus fournit la définition du composant, y compris le titre, la description et le groupe. `sling:resourceSuperType` pointe vers `core/wcm/components/image/v2/image`, qui est le [composant Image principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html?lang=fr).

### Créer le script HTL {#create-the-htl-script}

1. Dans le dossier `byline`, ajoutez un fichier `byline.html`, qui est responsable de la présentation HTML du composant. Il est important d’attribuer au fichier le même nom que le dossier, car il devient le script par défaut utilisé par Sling pour effectuer le rendu de ce type de ressource.

1. Ajoutez le code suivant à `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

Le fichier `byline.html` est [revu plus tard](#byline-htl), une fois le modèle Sling créé. L’état actuel du fichier HTL permet au composant de s’afficher dans un état vide dans l’éditeur de page d’AEM Sites lorsqu’il est glissé-déposé sur la page.

### Créer la définition de boîte de dialogue {#create-the-dialog-definition}

Définissez ensuite une boîte de dialogue pour le composant Signature avec les champs suivants :

* **Nom** : champ de texte contenant le nom du contributeur.
* **Image** : référence à la photo de la biographie du contributeur ou de la contributrice.
* **Fonctions** : liste des fonctions attribuées au contributeur ou à la contributrice. Les fonctions doivent être classées par ordre alphabétique (de a à z).

1. Dans le dossier `byline`, créez un dossier appelé `_cq_dialog`.
1. Dans `byline/_cq_dialog`, ajoutez un fichier appelé `.content.xml`. Il s’agit de la définition XML de la boîte de dialogue. Ajoutez le code XML suivant :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
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

   Ces définitions de nœud de boîte de dialogue utilisent [Sling Resourc Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) pour contrôler quels onglets de boîte de dialogue sont hérités du composant `sling:resourceSuperType`, dans ce cas, le **composant Image des composants principaux**.

   ![Boîte de dialogue terminée pour la signature](assets/custom-component/byline-dialog-created.png)

### Créer la boîte de dialogue Politique {#create-the-policy-dialog}

En suivant la même approche que pour la création de la boîte de dialogue, créez une boîte de dialogue Politique (anciennement appelée Boîte de dialogue de conception) pour masquer les champs indésirables dans la configuration de la politique héritée du composant Image des composants principaux.

1. Dans le dossier `byline`, créez un dossier appelé `_cq_design_dialog`.
1. Dans `byline/_cq_design_dialog`, créez un fichier appelé `.content.xml`. Mettez à jour le fichier avec le code XML ci-après. Il est plus facile d’ouvrir `.content.xml` et de copier/coller le code XML ci-dessous dedans.

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

   La base du code XML précédent de la **boîte de dialogue Politique** a été obtenue à partir du [composant Image des composants principaux](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Comme dans la configuration de la boîte de dialogue, [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) est utilisé pour masquer les champs non pertinents qui sont autrement hérités de `sling:resourceSuperType`, comme le montrent les définitions de nœuds avec la propriété `sling:hideResource="{Boolean}true"`.

### Déployer le code {#deploy-the-code}

1. Synchronisez les modifications dans `ui.apps` avec votre IDE ou en utilisant vos compétences Maven.

   ![Exporter vers le composant Signature de serveur AEM](assets/custom-component/export-byline-component-aem.png)

## Ajouter le composant à une page {#add-the-component-to-a-page}

Pour garder les choses simples et se concentrer sur le développement de composants AEM, ajoutons le composant Signature dans son état actuel à une page Article pour vérifier que la définition du nœud `cq:Component` est correcte. Cela permet également de vérifier qu’AEM reconnaît la nouvelle définition de composant et que la boîte de dialogue du composant fonctionne pour la création.

### Ajouter une image à AEM Assets

Tout d’abord, chargez un exemple de portrait dans AEM Assets pour remplir l’image dans le composant Signature.

1. Accédez au dossier LA Skateparks dans AEM Assets : [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Chargez le portrait pour **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** dans le dossier.

   ![Portrait chargé dans AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### Créer le composant {#author-the-component}

Ajoutez ensuite le composant Signature à une page dans AEM. Comme le composant Signature est ajouté au groupe de composants **Projet de sites WKND - Contenu** via la définition `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml`, il est automatiquement disponible pour tous les **conteneurs** dont la **politique** autorise le groupe de composants **Projet de sites WKND - Contenu**. Il est donc disponible dans le Conteneur de dispositions de la page d’article.

1. Accédez à l’article LA Skatepark à l’adresse : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Depuis la barre latérale gauche, effectuez un glisser-déposer d’un **composant Signature** au **bas** du Conteneur de dispositions de la page d’article ouverte.

   ![ajouter un composant Signature à la page](assets/custom-component/add-to-page.png)

1. Vérifiez que la barre latérale gauche est ouverte **et visible et que** l’outil de recherche de ressources** est sélectionné.

1. Sélectionnez l’**espace réservé du composant Signature** qui affiche la barre d’actions, puis appuyez sur l’icône représentant une **clé à molette** pour ouvrir la boîte de dialogue.

1. Une fois la boîte de dialogue ouverte et le premier onglet (Ressource) actif, ouvrez la barre latérale gauche. Depuis l’outil de recherche de ressources, faites glisser une image jusqu’à la zone de dépôt Image. Recherchez &quot;stacey&quot; pour trouver l’image de profil de Stacey Roswells fournie dans le package WKND ui.content.

   ![ajouter une image à la boîte de dialogue](assets/custom-component/add-image.png)

1. Après avoir ajouté une image, cliquez sur l’onglet **Propriétés** pour saisir le **Nom** et les **Professions**.

   Lorsque vous saisissez un emploi, entrez-les dans l’ordre **alphabétique inverse** afin que la logique de gestion de l’ordre alphabétique mise en œuvre dans le modèle Sling soit vérifiée.

   Appuyez sur le bouton **Terminé** en bas à droite pour enregistrer les modifications.

   ![renseignez les propriétés du composant signature](assets/custom-component/add-properties.png)

   Les créateurs et créatrices AEM configurent et créent des composants via les boîtes de dialogue. À ce stade, dans le développement du composant Signature, les boîtes de dialogue sont incluses pour la collecte des données, mais la logique pour rendre le contenu créé n’a pas encore été ajoutée. Par conséquent, seul l’espace réservé s’affiche.

1. Après avoir enregistré la boîte de dialogue, accédez à [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) et passez en revue la manière dont le contenu du composant est stocké sur le nœud de contenu du composant Signature sous la page AEM.

   Recherchez le nœud de contenu du composant Signature sous la page LA Skate Parks, c’est-à-dire `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Notez les noms des propriétés. `name`, `occupations`, et `fileReference` sont stockées sur le **nœud Signature**.

   Veuillez également noter que le code `sling:resourceType` du nœud est défini sur `wknd/components/content/byline` et lie ce nœud de contenu à l’implémentation du composant Signature.

   ![propriétés de signature dans CRXDE](assets/custom-component/byline-properties-crxde.png)

## Créer un modèle Sling de signature {#create-sling-model}

Ensuite, nous allons créer un modèle Sling qui servira de modèle de données et abritera la logique commerciale du composant Signature.

Les modèles Sling sont des POJO Java™ axés sur les annotations (objets Java™ simples) qui facilitent le mappage des données du JCR aux variables Java™ et offrent une efficacité lors du développement dans le contexte AEM.

### Vérifier les dépendances Maven {#maven-dependency}

Le modèle Sling de signature repose sur plusieurs API Java™ fournies par AEM. Ces API sont mises à disposition via le code `dependencies` répertorié dans le fichier POM du module `core`. Le projet utilisé pour ce tutoriel a été créé pour AEM as a Cloud Service. Cependant, il est unique, car il est rétrocompatible avec AEM 6.5/6.4. Par conséquent, les deux dépendances pour Cloud Service et AEM 6.x sont incluses.

1. Ouvrez le fichier `pom.xml` sous `<src>/aem-guides-wknd/core/pom.xml`.
1. Recherchez la dépendance pour `aem-sdk-api` - **AEM as a Cloud Service uniquement**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   Le [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=fr) contient toutes les API Java™ publiques exposées par AEM. Le `aem-sdk-api` est utilisé par défaut lors de la création de ce projet. La version est maintenue dans le fichier pom Reactor parent de la racine du projet à `aem-guides-wknd/pom.xml`.

1. Recherchez la dépendance du `uber-jar` - **AEM 6.5/6.4 uniquement**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   Le `uber-jar` n’est inclus que lorsque le profil `classic` est appelé, c’est-à-dire `mvn clean install -PautoInstallSinglePackage -Pclassic`. Encore une fois, cela ne concerne que ce projet. Dans un projet réel, généré à partir de l’archétype de projet AEM, le `uber-jar` est la valeur par défaut si la version d’AEM spécifiée est 6.5 ou 6.4.

   Le [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=fr) contient toutes les API Java™ publiques exposées par AEM 6.x. La version est maintenue dans le fichier pom Reactor parent de la racine du projet `aem-guides-wknd/pom.xml`.

1. Recherchez la dépendance pour `core.wcm.components.core` :

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Il s’agit de l’ensemble des API publiques Java™ exposées par les composants principaux AEM. Les composants principaux AEM sont un projet conservé en dehors d&#39;AEM et ont donc un cycle de publication distinct. C’est pourquoi il s’agit d’une dépendance qui doit être incluse séparément et qui n&#39;est **pas** incluse dans `uber-jar` ou `aem-sdk-api`.

   Comme pour l’uber-jar, la version de cette dépendance est conservée dans le fichier pom Reactor parent de `aem-guides-wknd/pom.xml`.

   Plus loin dans ce tutoriel, la classe Image Core Component est utilisée pour afficher l’image dans le composant Signature. Il est nécessaire de disposer de la dépendance Core Component pour construire et compiler le modèle Sling.

### Interface de signature {#byline-interface}

Créez une interface Java™ publique pour la signature. Le `Byline.java` définit les méthodes publiques nécessaires pour piloter le script HTL `byline.html`.

1. À l’intérieur, le module `core` dans le dossier `core/src/main/java/com/adobe/aem/guides/wknd/core/models` crée un fichier nommé `Byline.java`.

   ![créer une interface de signature](assets/custom-component/create-byline-interface.png)

1. Mettre à jour `Byline.java` en utilisant les méthodes suivantes :

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

   Les deux premières méthodes exposent les valeurs des **nom** et **professions** pour le composant de signature.

   La méthode `isEmpty()` est utilisée pour déterminer si le composant a du contenu à rendre ou s’il est en attente de configuration.

   Notez qu’il n’existe aucune méthode pour l’image ; [ceci est vérifié ultérieurement](#tackling-the-image-problem).

1. Les versions des packages Java™ qui contiennent des classes Java™ publiques, dans ce cas un modèle Sling, doivent être gérées à l’aide du fichier `package-info.java` du package.

   Étant donné que le package Java™ `com.adobe.aem.guides.wknd.core.models` de la source WKND déclare la version de `1.0.0`, et qu’une interface publique et des méthodes avec des modifications mineures sont ajoutées, la version doit passer à `1.1.0`. Ouvrez le fichier à `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` et modifiez `@Version("1.0.0")` en `@Version("2.1.0")`.

       ```
     @Version(&quot;2.1.0&quot;)
     package com.adobe.aem.guides.wknd.core.models;
     
     import org.osgi.annotation.versioning.Version;
     ```
   
Lorsqu’une modification est apportée aux fichiers de ce package, [la version du package doit être ajustée sémantiquement](https://semver.org/). Si ce n’est pas le cas, le [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) du projet Maven détecte une version de package non valide et rompt la version. Heureusement, en cas d’échec, le plug-in Maven signale la version non valide du package Java™ et la version devant être utilisée. Mettez à jour la déclaration `@Version("...")` dans le `package-info.java` du package Java™ en infraction à la version recommandée par le plug-in à corriger.

### Implémentation par signature {#byline-implementation}

Le `BylineImpl.java` est l’implémentation du modèle Sling mettant en œuvre l’interface `Byline.java` définie précédemment. Le code complet de `BylineImpl.java` se trouve au bas de cette section.

1. Créez un dossier nommé `impl` sous `core/src/main/java/com/adobe/aem/guides/core/models`.
1. Dans le dossier `impl`, créez un fichier `BylineImpl.java`.

   ![Fichier Impl de signature](assets/custom-component/byline-impl-file.png)

1. Ouvrez `BylineImpl.java`. Spécifiez qu’il implémente l’interface `Byline`. Utilisez les fonctionnalités de saisie semi-automatique de l’IDE ou mettez à jour manuellement le fichier afin d’inclure les méthodes nécessaires à l’implémentation de l’interface `Byline` :

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. Ajoutez des annotations de modèle Sling en mettant à jour `BylineImpl.java` avec les annotations au niveau de la classe suivantes. Cette annotation `@Model(..)` transforme la classe en modèle Sling.

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
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   Examinons cette annotation et ses paramètres :

   * L’annotation `@Model` enregistre BylineImpl en tant que modèle Sling lorsqu’il est déployé sur AEM.
   * Le paramètre `adaptables` indique que ce modèle peut être adapté par la requête.
   * Le paramètre `adapters` permet à la classe d’implémentation d’être enregistrée sous l’interface de signature. Cela permet au script HTL d’appeler le modèle Sling via l’interface (au lieu de l’implémentation directe). [Vous trouverez plus d’informations sur les adaptateurs ici](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * Le `resourceType` pointe vers le type de ressource du composant Signature (créé précédemment) et aide à résoudre le modèle correct s’il existe plusieurs mises en œuvre. [Vous trouverez plus d’informations sur l’association d’une classe de modèle à un type de ressource ici](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implémentation des méthodes de modèle Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

La première méthode implémentée est `getName()`, qui renvoie simplement la valeur stockée dans le nœud de contenu JCR de la signature sous la propriété `name`.

Pour ce faire, lœannotation de modèle Sling `@ValueMapValue` est utilisée pour injecter la valeur dans un champ Java™ à lœaide de la ValueMap de la ressource de la requête.


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

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

Dans la mesure où la propriété JCR partage le nom en tant que champ Java™ (« nom » pour les deux), `@ValueMapValue` résout automatiquement cette association et injecte la valeur de la propriété dans le champ Java™.

#### getOccupations() {#implementing-get-occupations}

La méthode suivante à mettre en œuvre est `getOccupations()`. Cette méthode charge les professions stockées dans la propriété JCR `occupations` et renvoie une collection triée (par ordre alphabétique).

Avec la même technique explorée dans `getName()`, la valeur de propriété peut être injectée dans le champ du modèle Sling.

Une fois que les valeurs de propriété JCR sont disponibles dans le modèle Sling via le champ Java™ injecté `occupations`, la logique commerciale de tri peut être appliquée dans la méthode `getOccupations()` .


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
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

La dernière méthode publique est `isEmpty()`, qui détermine à quel moment le composant doit se considérer comme « suffisamment créé » pour effectuer le rendu.

Pour ce composant, les exigences métier sont les trois champs suivants, `name, image and occupations` qui doivent être renseignés *avant* que le composant soit rendu.


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


#### Lutter contre le « problème d’image » {#tackling-the-image-problem}

La vérification du nom et des conditions d’occupation est simple. Apache Commons Lang3 fournit la classe [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) pratique à utiliser. Cependant, il est difficile de savoir comment la **présence de l’image** peut être validée, car le composant Image des composants principaux est utilisé pour surfacer l’image.

Il existe deux façons de procéder :

Vérifiez si la propriété JCR `fileReference` est résolue sur une ressource. *OU* convertissez cette ressource en modèle Sling d’image de composant principal et assurez-vous que la méthode `getSrc()` n’est pas vide.

Utilisons la **deuxième** approche. La première approche est probablement suffisante, mais dans ce tutoriel, la deuxième est utilisée pour nous permettre d’explorer d’autres fonctionnalités des modèles Sling.

1. Créez une méthode privée qui récupère l’image. Cette méthode reste privée, car il n’est pas nécessaire d’exposer l’objet Image dans le HTL lui-même et elle est uniquement utilisée pour transmettre `isEmpty().`.

   Ajoutez la méthode privée suivante pour `getImage()` :

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Comme indiqué ci-dessus, il existe deux autres approches pour obtenir le **Modèle Sling d’image** :

   La première utilise l’annotation `@Self` afin d’adapter automatiquement la requête active à l’`Image.class` du composant principal.

   La seconde utilise le service OSGi [Sling ModelFactory Apache](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html). Pratique, il permet de créer des modèles Sling d’autres types dans le code Java™.

   Utilisons la seconde approche.

   >[!NOTE]
   >
   >Dans une implémentation réelle, la première approche, à savoir l’utilisation de `@Self`, est préférable, car c’est la solution la plus simple et la plus élégante. Dans ce tutoriel, la deuxième approche est utilisée, car elle nécessite d’explorer plus de facettes des modèles Sling qui sont utiles pour des composants plus complexes.

   Étant donné que les modèles Sling sont des POJO Java™, et non des services OSGi, les annotations d’injection OSGi habituelles `@Reference` **ne peuvent pas** être utilisées. À leur place, les modèles Sling fournissent une annotation spéciale **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** qui offre une fonctionnalité similaire.

1. Mettez à jour `BylineImpl.java` pour inclure l’annotation `OSGiService` pour injecter le `ModelFactory` :

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

   Avec le `ModelFactory` disponible, un modèle Sling d’image de composant principal peut être créé en utilisant :

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Toutefois, cette méthode nécessite à la fois une requête et une ressource qui ne sont pas encore disponibles dans le modèle Sling. Pour les obtenir, il faut utiliser davantage d’annotations du modèle Sling.

   Pour obtenir la requête active, l’annotation **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** peut être utilisée pour injecter l’élément `adaptable` (qui est défini dans le `@Model(..)` comme `SlingHttpServletRequest.class`, dans un champ de classe Java™).

1. Ajoutez l’annotation **@Self** pour obtenir la **requête SlingHttpServletRequest** :

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Rappelez-vous que l’utilisation de `@Self Image image` pour injecter le modèle Sling d’image de composant principal était une option ci-dessus. L’annotation `@Self` essaie d’injecter l’objet adaptable (dans ce cas une SlingHttpServletRequest), et de s’adapter au type de champ de l’annotation. Puisque le modèle Sling d’image de composant principal est adaptable à partir des objets SlingHttpServletRequest, cela aurait fonctionné et représente moins de code que l’approche `modelFactory` plus exploratoire.

   Les variables nécessaires à l’instanciation du modèle d’image via l’API ModelFactory sont maintenant injectées. Utilisons l’annotation **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** du modèle Sling pour obtenir cet objet après l’instanciation du modèle Sling.

   `@PostConstruct` est incroyablement utile et agit dans une capacité similaire à celle d’un constructeur, cependant, il est invoqué après l’instanciation de la classe et l’injection de tous les champs Java™ annotés. Tandis que les autres annotations du modèle Sling annotent des champs de classe Java™ (variables), `@PostConstruct` annote une méthode vide à zéro paramètre, typiquement nommée `init()` (mais pouvant être nommée comme bon vous semble).

1. Ajoutez la méthode **@PostConstruct** :

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

   Rappelez-vous, les modèles Sling ne sont **PAS** des services OSGi, ce qui signifie que le maintien de l’état des classes est sûr. Souvent, `@PostConstruct` dérive et met en place l’état de la classe du modèle Sling pour une utilisation ultérieure, de manière similaire à ce que fait un constructeur ordinaire.

   Si la méthode `@PostConstruct` déclenche une exception, le modèle Sling n’est pas instancié et sa valeur est nulle.

1. Vous pouvez désormais mettre à jour **getImage()** pour renvoyer simplement l’objet image.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Retournons à `isEmpty()` et terminons l’implémentation :

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   Veuillez noter que les appels multiples à `getImage()` ne posent pas de problème, car ils renvoient la variable de classe initialisée `image` et n’invoquent pas `modelFactory.getModelFromWrappedRequest(...)`, qui sans être trop coûteuse gagne tout de même à ne pas être appelée inutilement.

1. La `BylineImpl.java` finale doit ressembler à :


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
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
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
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
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

Dans le module `ui.apps`, ouvrez `/apps/wknd/components/byline/byline.html` qui a été créé dans la configuration précédente du composant AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Passons en revue ce que fait ce script HTL jusqu’à présent :

* Le `placeholderTemplate` pointe vers l’espace réservé des composants principaux, qui s’affiche lorsque le composant n’est pas entièrement configuré. Cela est rendu dans l’éditeur de page d’AEM Sites par une boîte avec le titre du composant, comme défini ci-dessus dans la propriété `jcr:title` du `cq:Component`.

* Le `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` charge le `placeholderTemplate` défini ci-dessus et passe une valeur booléenne (actuellement codée de manière irréversible sur `false`) dans le modèle de l’espace réservé. Lorsque `isEmpty` est true, le modèle d’espace réservé rend la boîte grise, sinon il ne rend rien.

### Mettre à jour la signature HTL

1. Mettez à jour **byline.html** avec la structure HTML squelettique suivante :

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Veuillez noter que les classes CSS suivent la [convention de nommage BEM](https://getbem.com/naming/). Bien que l’utilisation des conventions BEM ne soit pas obligatoire, elle est recommandée, car elle est utilisée dans les classes CSS des composants principaux et aboutit généralement à des règles CSS propres et lisibles.

### Instancier des objets de modèle Sling en HTL {#instantiating-sling-model-objects-in-htl}

L’[instruction Use block](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) est utilisée pour instancier les objets du modèle Sling dans le script HTL et l’affecter à une variable HTL.

Le `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` utilise l’interface de signature (com.adobe.aem.guides.wknd.models.Byline) implémentée par BylineImpl et y adapte la SlingHttpServletRequest en cours, et le résultat est stocké dans une signature de nom variable HTL (`data-sly-use.<variable-name>`).

1. Mettez à jour le `div` extérieur pour référencer le modèle Sling de **signature** par son interface publique :

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Accès aux méthodes de modèle Sling {#accessing-sling-model-methods}

HTL emprunte à JSTL, et utilise la même façon de raccourcir les noms des méthodes getter Java™.

Par exemple, invoquer la méthode `getName()` du modèle Sling de signature peut être raccourci en `byline.name`, de même `byline.isEmpty` peut être raccourci en `byline.empty`. L’utilisation des noms complets des méthodes, `byline.getName` ou `byline.isEmpty`, fonctionne également. Veuillez noter que les `()` ne sont jamais utilisés pour invoquer des méthodes en HTL (similaire au JSTL).

Les méthodes Java™ qui nécessitent un paramètre **ne peuvent pas** être utilisées en HTL. Il en est ainsi pour que la logique du HTL reste simple.

1. Le nom Signature peut être ajouté au composant en invoquant la méthode `getName()` sur le modèle Sling de signature, ou en HTL : `${byline.name}`.

   Mettez à jour la balise `h2` :

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Utilisation des options d’expression HTL {#using-htl-expression-options}

[Les options d&#39;expressions HTL](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) agissent comme des modificateurs sur le contenu en HTL, et vont du formatage de la date à la traduction i18n. Les expressions peuvent également être utilisées pour joindre des listes ou des tableaux de valeurs, ce qui est nécessaire pour afficher les professions dans un format délimité par des virgules.

Les expressions sont ajoutées via l’opérateur `@` dans l’expression HTL.

1. Pour joindre la liste des professions avec &quot;, &quot;, le code suivant est utilisé :

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Affichage conditionnel de l’espace réservé {#conditionally-displaying-the-placeholder}

La plupart des scripts HTL pour les composants AEM utilisent le **paradigme d’espace réservé** pour fournir un indice visuel aux auteurs **indiquant qu&#39;un composant est incorrectement authentifié et qu’il ne s’affiche pas sur AEM Publish**. La convention pour guider cette décision est d’implémenter une méthode sur le modèle Sling de soutien du composant, dans ce cas : `Byline.isEmpty()`.

La méthode `isEmpty()` est invoquée sur le modèle Sling de signature et le résultat (ou plutôt son négatif, via l’opérateur `!`) est enregistré dans une variable HTL nommée `hasContent` :

1. Mettez à jour le code `div` extérieur pour sauvegarder une variable HTL nommée `hasContent` :

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Veuillez noter l’utilisation de `data-sly-test`, le bloc HTL `test` est essentiel, il définit une variable HTL et rend/ne rend pas l’élément HTML sur lequel il se trouve. Il est basé sur le résultat de l’évaluation de l’expression HTL. Si la valeur est &quot;true&quot;, l’élément HTML est rendu, sinon il ne l’est pas.

   Cette variable HTL `hasContent` peut maintenant être réutilisée pour afficher/masquer l’espace réservé de manière conditionnelle.

1. Mettez à jour l’appel conditionnel au `placeholderTemplate` au bas du fichier avec les éléments suivants :

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Afficher l’image à l’aide des composants principaux {#using-the-core-components-image}

Le script HTL pour `byline.html` est maintenant presque complet et il ne manque que l’image.

Comme le `sling:resourceSuperType` pointe vers le composant Image des composants principaux pour créer l&#39;image, le composant Image des composants principaux peut être utilisé pour rendre l’image.

Pour cela, incluons la ressource de signature actuelle, mais forçons le type de ressource du composant Image des composants principaux, en utilisant le type de ressource `core/wcm/components/image/v2/image`. Il s’agit d’un modèle puissant pour la réutilisation des composants. Pour cela, le bloc `data-sly-resource` de HTL est utilisé.

1. Remplacez le `div` par une classe de `cmp-byline__image` avec ce qui suit :

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Ce `data-sly-resource`, inclut la ressource courante via le chemin relatif `'.'`, et force l’inclusion de la ressource courante (ou de la ressource de contenu de la signature) avec le type de ressource de `core/wcm/components/image/v2/image`.

   Le type de ressource Composants principaux est utilisé directement, et non par l’intermédiaire d’un proxy, car il s’agit d’une utilisation in-script et elle n’est jamais conservée dans le contenu.

2. `byline.html` complété ci-dessous :

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Déployez la base de code sur une instance locale d’AEM. Étant donné que des modifications ont été apportées à `core` et `ui.apps`, les deux modules doivent être déployés.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   Pour déployer vers AEM 6.5/6.4, invoquez le profil `classic` :

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > Vous pourriez également compiler le projet entier à partir de la racine en utilisant le profil Maven `autoInstallSinglePackage` mais cela pourrait écraser les changements de contenu sur la page. Cela est dû au fait que le `ui.content/src/main/content/META-INF/vault/filter.xml` a été modifié pour le code de démarrage du tutoriel afin de remplacer proprement le contenu AEM existant. Dans un scénario en situation réelle, ce n’est pas un problème.

### Révision du composant de signature non stylisé {#reviewing-the-unstyled-byline-component}

1. Après avoir déployé la mise à jour, accédez à la page [Ultimate Guide to LA Skateparks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html), ou à l’endroit où vous avez ajouté le composant de signature plus tôt dans le chapitre.

1. Les **image**, **nom**, et **professions** apparaissent désormais et un composant de signature non stylisé, mais fonctionnel, est présent.

   ![composant de signature non stylisé](assets/custom-component/unstyled.png)

### Révision de l’enregistrement du modèle Sling {#reviewing-the-sling-model-registration}

La [vue Statut des modèles Sling de la console Web d’AEM](http://localhost:4502/system/console/status-slingmodels) affiche tous les modèles Sling enregistrés dans AEM. Le modèle Sling de signature peut être validé comme étant installé et reconnu en examinant cette liste.

Si le **BylineImpl** ne s’affiche pas dans cette liste, il s’agit probablement d’un problème avec les annotations du modèle Sling ou bien le modèle n’a pas été ajouté au bon package (`com.adobe.aem.guides.wknd.core.models`) dans le projet de base.

![Modèle Sling de signature enregistré](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## Styles de signature {#byline-styles}

Pour aligner le composant de signature sur le design créatif fourni, nous lui donnons un style. Ceci est réalisé en utilisant un fichier SCSS et en mettant à jour le fichier dans le module **ui.frontend**.

### Ajouter un style par défaut

Ajoutez des styles par défaut pour le composant de signature.

1. Revenez à l’IDE et au projet **ui.frontend** sous `/src/main/webpack/components` :
1. Créez un fichier nommé `_byline.scss`.

   ![Explorateur de projet de signature.](assets/custom-component/byline-style-project-explorer.png)

1. Ajoutez le CSS des implémentations de signature (écrit en SCSS) dans le `_byline.scss` :

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
           font-size: $font-size-medium;
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

1. Ouvrez un terminal et accédez au module `ui.frontend`.
1. Démarrez le processus `watch` avec la commande npm suivante :

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Retournez au navigateur et accédez à [l’article LA SkateParks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Vous devriez voir les styles mis à jour pour le composant.

   ![Composant de signature terminé.](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Vous pouvez vider la mémoire cache du navigateur pour vous assurer que le fichier CSS obsolète n’est pas diffusé et actualiser la page avec le composant de signature pour obtenir le style complet.

## Félicitations. {#congratulations}

Félicitations, vous avez créé entièrement un composant personnalisé à l’aide d’Adobe Experience Manager.

### Étapes suivantes {#next-steps}

Continuez votre apprentissage du développement des composants d’AEM en explorant la manière d’écrire des tests JUnit pour le code Java™ de signature. Vous aurez ainsi l’assurance que tout est développé correctement et que la logique commerciale implémentée est correcte et complète.

* [Écrire des tests unitaires ou des composants AEM](unit-testing.md)

Affichez le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou révisez et déployez le code localement sur la branche Git `tutorial/custom-component-solution`.

1. Clonez le référentiel [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `tutorial/custom-component-solution` 
