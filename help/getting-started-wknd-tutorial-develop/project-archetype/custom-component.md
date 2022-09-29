---
title: Custom Component (composant personnalisé)
description: Couvre la création de bout en bout d’un composant d’signature personnalisé qui affiche le contenu créé. Inclut le développement d’un modèle Sling pour encapsuler la logique commerciale afin de renseigner le composant de signature et le code HTL correspondant pour effectuer le rendu du composant.
sub-product: sites
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '4131'
ht-degree: 1%

---

# Custom Component (composant personnalisé) {#custom-component}

Ce tutoriel porte sur la création de bout en bout d’un composant de ligne d’AEM personnalisé qui affiche le contenu créé dans une boîte de dialogue et explore le développement d’un modèle Sling pour encapsuler la logique commerciale qui renseigne le code HTL du composant.

## Prérequis {#prerequisites}

Examinez les outils et les instructions requis pour configurer une [environnement de développement local](overview.md#local-dev-environment).

### Projet de démarrage

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes d’extraction du projet de démarrage.

Consultez le code de ligne de base sur lequel le tutoriel s’appuie :

1. Consultez la section `tutorial/custom-component-start` branche à partir de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Déployez la base de code sur une instance d’AEM locale à l’aide de vos compétences Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM version 6.5 ou 6.4, ajoutez la variable `classic` profile à n’importe quelle commande Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) ou extraire le code localement en passant à la branche `tutorial/custom-component-solution`.

## Objectif

1. Comprendre comment créer un composant d’AEM personnalisé
1. Découvrez comment encapsuler la logique commerciale avec les modèles Sling
1. Comprendre comment utiliser un modèle Sling dans un script HTL

## Ce que vous allez créer {#byline-component}

Dans cette partie du tutoriel WKND, un composant de signature est créé afin d’afficher les informations créées sur le contributeur d’un article.

![exemple de composant signature](assets/custom-component/byline-design.png)

*Composant Byline*

L’implémentation du composant Signature comprend une boîte de dialogue qui collecte le contenu de la signature et un modèle Sling personnalisé qui récupère celui de la signature :

* Nom
* Image
* Professions

## Création d’un composant Byline {#create-byline-component}

Créez tout d’abord la structure de noeud Composant signature et définissez une boîte de dialogue. Cela représente le composant dans AEM et définit implicitement le type de ressource du composant en fonction de son emplacement dans le JCR.

La boîte de dialogue expose l’interface que les auteurs de contenu peuvent fournir. Pour cette implémentation, le composant principal de la gestion du contenu web AEM **Image** Le composant est utilisé pour gérer la création et le rendu de l’image de la signature. Il doit donc être défini comme `sling:resourceSuperType`.

### Création d’une définition de composant {#create-component-definition}

1. Dans le **ui.apps** , accédez à `/apps/wknd/components` et créez un dossier nommé `byline`.
1. Sous la `byline` Ajoutez un nouveau fichier nommé `.content.xml`

   ![boîte de dialogue pour créer le noeud](assets/custom-component/byline-node-creation.png)

1. Renseignez la variable `.content.xml` avec les éléments suivants :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   Le fichier XML ci-dessus fournit la définition du composant, y compris le titre, la description et le groupe. Le `sling:resourceSuperType` pointe vers `core/wcm/components/image/v2/image`, qui est la variable [Composant d’image principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=fr).

### Création du script HTL {#create-the-htl-script}

1. Sous la `byline` dossier, ajouter un nouveau fichier `byline.html`, qui est responsable de la présentation par HTML du composant. Il est important de nommer le fichier de la même manière que le dossier, car il devient le script par défaut que Sling utilisera pour effectuer le rendu de ce type de ressource.

1. Ajoutez le code suivant au `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` is [revisité ultérieurement](#byline-htl), une fois le modèle Sling créé. L’état actuel du fichier HTL permet au composant de s’afficher dans un état vide dans l’éditeur de page d’AEM Sites lorsqu’il est glissé-déposé sur la page.

### Création de la définition de boîte de dialogue {#create-the-dialog-definition}

Définissez ensuite une boîte de dialogue pour le composant Byline avec les champs suivants :

* **Nom**: un champ de texte du nom du contributeur.
* **Image**: une référence à la biographie du contributeur.
* **Professions**: une liste des emplois attribués au contributeur. Les professions doivent être classées par ordre alphabétique croissant (de a à z).

1. Sous la `byline` créer un dossier nommé `_cq_dialog`.
1. Sous `byline/_cq_dialog` ajouter un nouveau fichier nommé `.content.xml`. Il s’agit de la définition XML de la boîte de dialogue. Ajoutez le code XML suivant :

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

   Ces définitions de noeud de boîte de dialogue utilisent la propriété [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) pour contrôler quels onglets de boîte de dialogue sont hérités du `sling:resourceSuperType` , dans ce cas, la variable **Composant Image des composants principaux**.

   ![Boîte de dialogue terminée pour la signature](assets/custom-component/byline-dialog-created.png)

### Boîte de dialogue Créer une stratégie {#create-the-policy-dialog}

Selon la même approche que pour la création de boîte de dialogue, créez une boîte de dialogue Stratégie (anciennement appelée boîte de dialogue de conception) pour masquer les champs indésirables dans la configuration Stratégie héritée du composant Image des composants principaux.

1. Sous la `byline` créer un dossier nommé `_cq_design_dialog`.
1. Sous `byline/_cq_design_dialog` créer un nouveau fichier nommé `.content.xml`. Mettez à jour le fichier avec ce qui suit : avec le code XML suivant. Il est plus facile d’ouvrir le `.content.xml` et copiez/collez le code XML ci-dessous dans celui-ci.

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

   La base du précédent **Boîte de dialogue Stratégie** Le code XML a été obtenu à partir de la fonction [Composant Image des composants principaux](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Comme dans la configuration de la boîte de dialogue, [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) est utilisé pour masquer les champs non pertinents qui sont par ailleurs hérités du `sling:resourceSuperType`, comme le montrent les définitions de noeud avec `sling:hideResource="{Boolean}true"` .

### Déployer le code {#deploy-the-code}

1. Synchroniser les modifications dans `ui.apps` avec votre IDE ou en utilisant vos compétences Maven.

   ![Exporter vers AEM composant de ligne d’entrée de serveur](assets/custom-component/export-byline-component-aem.png)

## Ajouter le composant à une page {#add-the-component-to-a-page}

Pour simplifier les choses et se concentrer sur le développement AEM composant, nous allons ajouter le composant Byline dans son état actuel à une page Article pour vérifier la variable `cq:Component` La définition de noeud est déployée et correcte, AEM reconnaît la nouvelle définition de composant et la boîte de dialogue du composant fonctionne pour la création.

### Ajout d’une image à AEM Assets

Tout d’abord, téléchargez un exemple de capture d’écran vers AEM Assets à utiliser pour remplir l’image dans le composant de signature.

1. Accédez au dossier LA Skateparks dans AEM Assets : [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Téléchargez le plan de tête pour  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** dans le dossier .

   ![Capture d’écran téléchargée dans AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### Création du composant {#author-the-component}

Ajoutez ensuite le composant Byline à une page dans AEM. Parce que nous avons ajouté le composant Byline au **Projet de sites WKND - Contenu** Groupe de composants, via `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` définition, elle est automatiquement disponible pour tous les **Conteneur** who **Stratégie** autorise le **Projet de sites WKND - Contenu** groupe de composants, qui est le conteneur de mise en page de la page d’article.

1. Accédez à l’article LA Skatepark à l’adresse : [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Dans la barre latérale gauche, effectuez un glisser-déposer d’une **Composant Byline** à **bottom** du conteneur de mises en page de la page de l’article ouvert.

   ![ajouter un composant signature à la page](assets/custom-component/add-to-page.png)

1. Assurez-vous que la variable **la barre latérale gauche est ouverte** et visibles, et la variable **Outil de recherche de ressources** est sélectionnée.

1. Sélectionnez la **Espace réservé du composant Byline**, qui à son tour affiche la barre d’actions et appuyez sur la **clé à molette** pour ouvrir la boîte de dialogue.

1. Une fois la boîte de dialogue ouverte et le premier onglet (Ressource) principal, ouvrez la barre latérale gauche. Faites glisser une image dans la zone de dépôt Image à partir de l’outil de recherche de ressources. Recherchez &quot;stacey&quot; pour trouver l’image biographique de Stacey Roswells fournie dans le package ui.content WKND.

   ![ajouter une image à la boîte de dialogue](assets/custom-component/add-image.png)

1. Après avoir ajouté une image, cliquez sur l’icône **Propriétés** pour saisir la variable **Nom** et **Professions**.

   Lorsque vous entrez dans un emploi, entrez-les dans un emploi **alphabétique inverse** Par ordre de priorité, la logique d’ordre alphabétique que nous allons mettre en oeuvre dans le modèle Sling est facilement visible.

   Appuyez sur le bouton **Terminé** en bas à droite pour enregistrer les modifications.

   ![renseignez les propriétés du composant signature](assets/custom-component/add-properties.png)

   AEM les auteurs configurent et créent des composants via les boîtes de dialogue. À ce stade du développement du composant Byline, les boîtes de dialogue sont incluses pour la collecte des données, mais la logique de rendu du contenu créé n’a pas encore été ajoutée. Par conséquent, seul l’espace réservé s’affiche.

1. Après avoir enregistré la boîte de dialogue, accédez à [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) et passez en revue la manière dont le contenu du composant est stocké sur le noeud de contenu du composant byline sous la page AEM.

   Recherchez le noeud de contenu du composant Byline sous la page LA Skate Parks, c’est-à-dire `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Notez les noms des propriétés. `name`, `occupations`, et `fileReference` sont stockées sur la variable **noeud byline**.

   Notez également que la variable `sling:resourceType` du noeud est défini sur `wknd/components/content/byline` qui lie ce noeud de contenu à l’implémentation du composant de signature.

   ![propriétés de signature dans CRXDE](assets/custom-component/byline-properties-crxde.png)

## Création d’un modèle Sling de signature {#create-sling-model}

Ensuite, nous allons créer un modèle Sling pour agir comme modèle de données et héberger la logique commerciale du composant Signature.

Les modèles Sling sont des objets POJO (Plain Old Java Object) Java pilotés par les annotations. Ils facilitent le mappage des données du JCR aux variables Java et fournissent un certain nombre d’autres détails lors du développement dans le contexte d’AEM.

### Vérification des dépendances Maven {#maven-dependency}

Le modèle Sling de signature repose sur plusieurs API Java fournies par AEM. Ces API sont mises à disposition via le `dependencies` répertorié dans le `core` fichier POM du module. Le projet utilisé pour ce tutoriel a été créé pour AEM as a Cloud Service. Cependant, il est unique dans la mesure où il est compatible avec AEM 6.5/6.4. Par conséquent, les deux dépendances pour Cloud Service et AEM 6.x sont incluses.

1. Ouvrez le `pom.xml` fichier sous `<src>/aem-guides-wknd/core/pom.xml`.
1. Recherchez la dépendance pour `aem-sdk-api` - **AEM as a Cloud Service uniquement**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   Le [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=fr#building-for-the-sdk) contient toutes les API Java publiques exposées par AEM. Le `aem-sdk-api` est utilisé par défaut lors de la création de ce projet. La version est conservée dans le modèle pom du réacteur parent situé à la racine du projet à l’adresse `aem-guides-wknd/pom.xml`.

1. Recherchez la dépendance de la variable `uber-jar` - **AEM 6.5/6.4 uniquement**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   Le `uber-jar` n’est inclus que lorsque la variable `classic` Le profil est appelé, c’est-à-dire `mvn clean install -PautoInstallSinglePackage -Pclassic`. Encore une fois, c&#39;est unique à ce projet. Dans un projet réel, généré à partir de l’archétype de projet AEM le `uber-jar` est la valeur par défaut si la version de l’AEM spécifiée est 6.5 ou 6.4.

   Le [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contient toutes les API Java publiques exposées par AEM 6.x. La version est conservée dans le modèle pom du réacteur parent situé à la racine du projet. `aem-guides-wknd/pom.xml`.

1. Recherchez la dépendance pour `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Il s’agit de l’ensemble des API Java publiques exposées par AEM Core Components. AEM Core Components est un projet géré en dehors d’AEM et a donc un cycle de publication distinct. C’est pourquoi il s’agit d’une dépendance qui doit être incluse séparément et qui est **not** inclus dans `uber-jar` ou `aem-sdk-api`.

   Comme l’uber-jar, la version de cette dépendance est conservée dans le fichier pom du réacteur parent situé à l’emplacement suivant : `aem-guides-wknd/pom.xml`.

   Plus loin dans ce tutoriel, nous utiliserons la classe Image des composants principaux pour afficher l’image dans le composant Byline. Il est nécessaire d’avoir la dépendance de composant principal pour créer et compiler notre modèle Sling.

### Interface de signature {#byline-interface}

Créez une interface Java publique pour la signature. `Byline.java` définit les méthodes publiques nécessaires pour piloter la variable `byline.html` Script HTL.

1. Dans le `aem-guides-wknd.core` module sous `core/src/main/java/com/adobe/aem/guides/wknd/core/models` créer un nouveau fichier nommé `Byline.java`

   ![créer une interface de signature](assets/custom-component/create-byline-interface.png)

1. Mettre à jour `Byline.java` avec les méthodes suivantes :

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

   Les deux premières méthodes exposent les valeurs de la variable **name** et **occupations** pour le composant Byline.

   Le `isEmpty()` est utilisée pour déterminer si le contenu du composant doit être rendu ou s’il attend d’être configuré.

   Notez qu’il n’existe aucune méthode pour l’image ; [nous allons examiner pourquoi c&#39;est plus tard.](#tackling-the-image-problem).

1. Les modules Java qui contiennent des classes Java publiques, dans ce cas un modèle Sling, doivent être versionnés à l’aide de la variable  `package-info.java` fichier .

   Depuis le package Java de la source WKND `com.adobe.aem.guides.wknd.core.models` déclare être la version de `1.0.0`, et nous ajoutons une interface publique et des méthodes sans rupture, la version doit être augmentée à `1.1.0`. Ouvrez le fichier à l’adresse `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` et mettre à jour `@Version("1.0.0")` to `@Version("1.1.0")`.

       &quot;
       @Version(&quot;2.1.0&quot;)
       package com.adobe.aem.guides.wknd.core.models;
       
       import org.osgi.annotation.versioning.Version;
       &quot;
   
Lorsqu’une modification est apportée aux fichiers de ce module, la variable [la version du package doit être ajustée sémantiquement.](https://semver.org/). Si ce n’est pas le cas, le projet Maven [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) détectera une version de package non valide et rompra la version créée. Heureusement, en cas d’échec, le module externe Maven signale la version non valide du package Java ainsi que la version qu’il doit être. Mettez simplement à jour la variable `@Version("...")` dans la déclaration violant le package Java `package-info.java` à la version recommandée par le module externe pour corriger.

### Implémentation par signature {#byline-implementation}

`BylineImpl.java` est l’implémentation du modèle Sling qui implémente le `Byline.java` l’interface définie précédemment. Le code complet pour `BylineImpl.java` se trouve au bas de cette section.

1. Créez un dossier nommé `impl` sous `core/src/main/java/com/adobe/aem/guides/core/models`.
1. Dans le `impl` créer un dossier `BylineImpl.java`.

   ![Fichier Impl de signature](assets/custom-component/byline-impl-file.png)

1. Ouvrez `BylineImpl.java`. Spécifiez qu’il implémente le `Byline` . Utilisez les fonctionnalités de saisie semi-automatique de l’IDE ou mettez à jour manuellement le fichier afin d’inclure les méthodes nécessaires à l’implémentation de la variable `Byline` interface :

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

1. Ajout des annotations de modèle Sling en mettant à jour `BylineImpl.java` avec les annotations au niveau de la classe suivantes. Ceci `@Model(..)`annotation est ce qui transforme la classe en modèle Sling.

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

   Examinons cette annotation et ses paramètres :

   * Le `@Model` L’annotation enregistre BylineImpl en tant que modèle Sling lorsqu’il est déployé sur AEM.
   * Le `adaptables` indique que ce modèle peut être adapté par la requête.
   * Le `adapters` permet à la classe d’implémentation d’être enregistrée sous l’interface de signature. Cela permet au script HTL d’appeler le modèle Sling via l’interface (au lieu de l’impl directement). [Vous trouverez plus d’informations sur les adaptateurs ici](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * Le `resourceType` pointe vers le type de ressource de composant Byline (créé précédemment) et aide à résoudre le modèle correct s’il existe plusieurs mises en oeuvre. [Vous trouverez plus d’informations sur l’association d’une classe de modèle à un type de ressource ici](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implémentation des méthodes de modèle Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

La première méthode que nous allons aborder est la suivante : `getName()` qui renvoie simplement la valeur stockée dans le noeud de contenu JCR de la signature sous la propriété . `name`.

Pour ce faire, la variable `@ValueMapValue` L’annotation de modèle Sling est utilisée pour injecter la valeur dans un champ Java à l’aide de la ValueMap de la ressource de requête.


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

Dans la mesure où la propriété JCR partage le même nom que le champ Java (les deux sont &quot;name&quot;), `@ValueMapValue` résout automatiquement cette association et injecte la valeur de la propriété dans le champ Java.

#### getOccupations() {#implementing-get-occupations}

La méthode suivante à mettre en oeuvre est : `getOccupations()`. Cette méthode collecte toutes les occupations stockées dans la propriété JCR. `occupations` et renvoyer une collection triée (par ordre alphabétique).

Utilisation de la même technique explorée dans `getName()` la valeur de propriété peut être injectée dans le champ du modèle Sling.

Une fois que les valeurs de propriété JCR sont disponibles dans le modèle Sling via le champ Java injecté `occupations`, la logique métier de tri peut être appliquée dans la variable `getOccupations()` .


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

La dernière méthode publique est la suivante : `isEmpty()` qui détermine à quel moment le composant doit se considérer comme &quot;suffisamment créé&quot; pour effectuer le rendu.

Pour ce composant, nous avons des exigences de fonctionnement stipulant que les trois champs, nom, image et métiers doivent être remplis. *before* le composant peut être rendu.


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

La vérification du nom et des conditions d&#39;occupation est triviale (et Apache Commons Lang3 fournit le [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) ), cependant, on ne sait pas comment le **présence de l’image** peut être validé, car le composant Image des composants principaux est utilisé pour faire surface de l’image.

Il existe deux façons de s&#39;y attaquer :

Vérifiez si la variable `fileReference` La propriété JCR est résolue sur une ressource. *OU* Convertissez cette ressource en modèle Sling d’image de composant principal et assurez-vous que la variable `getSrc()` n’est pas vide.

Nous acceptons le **second** approche. La première approche est probablement suffisante, mais dans ce tutoriel, cette dernière est utilisée pour nous permettre d’explorer d’autres fonctionnalités des modèles Sling.

1. Créez une méthode privée qui récupère l’image. Cette méthode est laissée privée car nous n’avons pas besoin d’exposer l’objet Image dans le HTL lui-même, et il est utilisé uniquement pour le lecteur. `isEmpty().`

   Ajoutez la méthode privée suivante pour `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Comme indiqué ci-dessus, il existe deux autres méthodes pour obtenir la variable **Modèle Sling d’image**:

   Le premier utilise la variable `@Self` annotation, afin d’adapter automatiquement la requête active au `Image.class`

   La seconde utilise la variable [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) Le service OSGi, qui est très pratique, nous aide à créer des modèles Sling d’autres types dans le code Java.

   Nous allons opter pour la seconde approche.

   >[!NOTE]
   >
   >Dans une mise en oeuvre concrète, approchez &quot;One&quot;, en utilisant `@Self` est préférable car il s’agit de la solution la plus simple et la plus élégante. Dans ce tutoriel, nous utiliserons la deuxième approche, car elle nous oblige à explorer d’autres facettes des modèles Sling qui sont extrêmement utiles, c’est-à-dire des composants plus complexes !

   Comme les modèles Sling sont des POJO Java et non des services OSGi, les annotations d’injection OSGi habituelles `@Reference` **cannot** à utiliser, les modèles Sling fournissent une **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** qui fournit des fonctionnalités similaires.

1. Mettre à jour `BylineImpl.java` pour inclure la variable `OSGiService` annotation pour laquelle injecter `ModelFactory`:

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

   Avec le `ModelFactory` Disponible, un modèle Sling d’image de composant principal peut être créé à l’aide de :

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Toutefois, cette méthode nécessite une requête et une ressource, qui ne sont pas encore disponibles dans le modèle Sling. Pour les obtenir, d’autres annotations de modèle Sling sont utilisées.

   Pour obtenir la requête actuelle, procédez comme suit : **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** annotation peut être utilisée pour injecter la variable `adaptable` (qui est défini dans la variable `@Model(..)` as `SlingHttpServletRequest.class`, dans un champ de classe Java.

1. Ajoutez la variable **@Self** annotation pour obtenir la variable **Requête SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   À retenir, en utilisant `@Self Image image` L’injection du modèle Sling d’image des composants principaux était une option ci-dessus : `@Self` annotation tente d’injecter l’objet adaptable (dans notre cas, SlingHttpServletRequest) et de s’adapter au type de champ d’annotation. Étant donné que le modèle Sling d’image des composants principaux est adaptable à partir des objets SlingHttpServletRequest , cela aurait fonctionné et est moins de code que notre approche plus exploratoire.

   Nous avons maintenant injecté les variables nécessaires pour instancier notre modèle d’image via l’API ModelFactory. Nous utiliserons le modèle Sling **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** annotation pour obtenir cet objet après l’instanciation du modèle Sling.

   `@PostConstruct` est incroyablement utile et agit de la même manière qu’un constructeur. Cependant, il est appelé une fois la classe instanciée et tous les champs Java annotés injectés. tandis que les autres annotations de modèle Sling annotent les champs de classe Java (variables), `@PostConstruct` Annote une méthode de paramètre void, zero, généralement nommée `init()` (mais peut être nommé n’importe quel).

1. Ajouter **@PostConstruct** method :

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

   N’oubliez pas que les modèles Sling sont **NOT** Services OSGi, de sorte qu’il soit sûr de maintenir l’état de classe. Souvent `@PostConstruct` permet de créer et de configurer l’état de classe de modèle Sling pour une utilisation ultérieure, comme le fait un constructeur brut.

   Notez que si la variable `@PostConstruct` renvoie une exception, le modèle Sling n’instanciera pas (il est nul).

1. **getImage()** peut désormais être mis à jour pour renvoyer simplement l’objet image.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Revenons à `isEmpty()` et terminez la mise en oeuvre :

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

   Notez que plusieurs appels à `getImage()` n’est pas problématique, car renvoie la valeur initialisée. `image` et n’appelle pas `modelFactory.getModelFromWrappedRequest(...)` ce qui n&#39;est pas trop coûteux, mais vaut la peine d&#39;éviter d&#39;appeler inutilement.

1. La finale `BylineImpl.java` doit ressembler à :


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


## Byline HTL {#byline-htl}

Dans le `ui.apps` module, ouvrir `/apps/wknd/components/byline/byline.html` Nous avons créé dans la configuration précédente du composant AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Examinons ce que ce script HTL fait jusqu’à présent :

* Le `placeholderTemplate` pointe vers l’espace réservé des composants principaux, qui s’affiche lorsque le composant n’est pas entièrement configuré. Dans l’éditeur de page d’AEM Sites, cette option est affichée sous la forme d’une zone avec le titre du composant, comme défini ci-dessus dans la variable `cq:Component`&#39;s  `jcr:title` .

* Le `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` charge la variable `placeholderTemplate` défini ci-dessus et transmet une valeur booléenne (actuellement codée en dur pour `false`) dans le modèle d’espace réservé. When `isEmpty` est définie sur true, le modèle d’espace réservé effectue le rendu de la zone grise, sinon il ne rend rien.

### Mettre à jour le protocole HTL

1. Mettre à jour **byline.html** avec la structure de HTML squelettique suivante :

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

   Notez que les classes CSS suivent le [Convention de dénomination BEM](https://getbem.com/naming/). Bien que l’utilisation des conventions BEM ne soit pas obligatoire, BEM est recommandée, car elle est utilisée dans les classes CSS des composants principaux et donne généralement lieu à des règles CSS claires et lisibles.

### Instanciation d’objets de modèle Sling dans HTL {#instantiating-sling-model-objects-in-htl}

Le [Instruction block](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) est utilisé pour instancier des objets Sling Model dans le script HTL et les affecter à une variable HTL.

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` utilise l’interface de signature (com.adobe.aem.guides.wknd.models.Byline) implémentée par BylineImpl et s’adapte à la SlingHttpServletRequest actuelle, et le résultat est stocké dans une variable HTL nommée par ligne ( `data-sly-use.<variable-name>`).

1. Mettre à jour l’extérieur `div` pour référencer la variable **Byline** Modèle Sling par son interface publique :

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Accès aux méthodes de modèle Sling {#accessing-sling-model-methods}

HTL emprunte à partir de JSTL et utilise le même raccourcissement des noms de méthodes getter Java.

Par exemple, en appelant le modèle Byline Sling `getName()` peut être raccourcie en `byline.name`, de manière similaire au lieu de `byline.isEmpty`, le raccourci peut être défini sur `byline.empty`. Utilisation de noms complets de méthodes, `byline.getName` ou `byline.isEmpty`, fonctionne également. Notez que `()` ne sont jamais utilisées pour appeler des méthodes dans HTL (comme JSTL).

Méthodes Java nécessitant un paramètre **cannot** être utilisé dans HTL. C’est par conception que la logique reste simple en HTL.

1. Le nom de la signature peut être ajouté au composant en appelant la fonction `getName()` sur le modèle Sling de signature, ou dans HTL : `${byline.name}`.

   Mettez à jour le `h2` tag :

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Utilisation des options d’expression HTL {#using-htl-expression-options}

[Options des expressions HTL](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) agir comme modificateurs sur le contenu dans HTL et aller de la mise en forme de date à la traduction i18n ; Les expressions peuvent également être utilisées pour joindre des listes ou des tableaux de valeurs, ce qui est nécessaire pour afficher les occupations dans un format délimité par des virgules.

Les expressions sont ajoutées à partir du `@` dans l’expression HTL.

1. Pour joindre la liste des occupations avec &quot;, &quot;, le code suivant est utilisé :

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Affichage conditionnel de l’espace réservé {#conditionally-displaying-the-placeholder}

La plupart des scripts HTL pour les composants AEM utilisent la variable **paradigme d&#39;espace réservé** fournir un indice visuel aux auteurs ; **indiquant qu’un composant n’est pas correctement créé et ne s’affiche pas sur AEM Publish**. La convention qui motive cette décision consiste à mettre en oeuvre une méthode sur le modèle Sling de support du composant, dans notre cas : `Byline.isEmpty()`.

`isEmpty()` est appelé sur le modèle Sling de signature et le résultat (ou plutôt son négatif), via la fonction `!` ) est enregistrée dans une variable HTL nommée `hasContent`:

1. Mettre à jour l’extérieur `div` pour enregistrer une variable HTL nommée `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Notez l’utilisation de `data-sly-test`, le HTL `test` block est intéressant dans la mesure où il définit une variable HTL ET effectue/ne rend pas l’élément de HTML sur lequel il est basé, selon si le résultat de l’expression HTL est vrai ou non. Si &quot;true&quot;, l’élément de HTML est rendu, sinon il n’est pas rendu.

   Cette variable HTL `hasContent` peut désormais être réutilisé pour afficher/masquer de manière conditionnelle l’espace réservé.

1. Mettez à jour l’appel conditionnel vers la fonction `placeholderTemplate` au bas du fichier avec les éléments suivants :

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Affichage de l’image à l’aide des composants principaux {#using-the-core-components-image}

Le script HTL pour `byline.html` est maintenant presque terminée et ne manque que l’image.

Puisque nous utilisons `sling:resourceSuperType` Le composant d’image des composants principaux permet de créer l’image. Nous pouvons également utiliser le composant d’image des composants principaux pour effectuer le rendu de l’image.

Pour ce faire, nous devons inclure la ressource d’origine actuelle, mais forcer le type de ressource du composant Image des composants principaux, à l’aide du type de ressource `core/wcm/components/image/v2/image`. Il s’agit d’un modèle puissant pour la réutilisation des composants. Pour ce faire, HTL `data-sly-resource` block est utilisé.

1. Remplacez la variable `div` avec une classe de `cmp-byline__image` avec ce qui suit :

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Ceci `data-sly-resource`, inclus la ressource actuelle via le chemin relatif `'.'`et force l’inclusion de la ressource actuelle (ou de la ressource de contenu par ligne) avec le type de ressource de `core/wcm/components/image/v2/image`.

   Le type de ressource des composants principaux est utilisé directement, et non via un proxy, car il s’agit d’une utilisation en script, qui n’est jamais conservée dans notre contenu.

2. Terminé `byline.html` ci-dessous :

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

3. Déployez la base de code sur une instance d’AEM locale. Puisque les modifications ont été apportées à `core` et `ui.apps` les deux modules doivent être déployés.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   Si vous déployez vers AEM 6.5/6.4, appelez la variable `classic` profile:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > Vous pouvez également créer le projet entier à partir de la racine à l’aide du profil Maven. `autoInstallSinglePackage` mais cela peut remplacer les modifications de contenu sur la page. En effet, la variable `ui.content/src/main/content/META-INF/vault/filter.xml` a été modifié pour que le code de démarrage du tutoriel remplace correctement le contenu AEM existant. Dans un scénario réel, ce ne sera pas un problème.

### Vérification du composant Byline non stylisé {#reviewing-the-unstyled-byline-component}

1. Après avoir déployé la mise à jour, accédez à la [Guide suprême pour les skateparks de la Californie ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) ou à l’emplacement où vous avez ajouté le composant Byline plus tôt dans le chapitre.

1. Le **image**, **name**, et **occupations** apparaît et nous disposons désormais d’un composant Byline sans style, mais qui fonctionne.

   ![composant d’objet sans style](assets/custom-component/unstyled.png)

### Vérification de l’enregistrement du modèle Sling {#reviewing-the-sling-model-registration}

Le [AEM vue d’état des modèles Sling de la console web](http://localhost:4502/system/console/status-slingmodels) affiche tous les modèles Sling enregistrés dans AEM. Le modèle Sling de signature peut être validé comme étant installé et reconnu en consultant cette liste.

Si la variable **BylineImpl** ne s’affiche pas dans cette liste, il y a probablement un problème avec les annotations du modèle Sling ou le modèle Sling n’a pas été ajouté au module Modèles Sling enregistré (`com.adobe.aem.guides.wknd.core.models`) dans le projet principal.

![Modèle Sling de signature enregistré](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## Styles de trait {#byline-styles}

Le composant Signature doit être mis en forme pour correspondre à la conception créative du composant Signature. Pour ce faire, utilisez SCSS, qui AEM la prise en charge via l’ **ui.frontend** Sous-projet Maven.

### Ajouter un style par défaut

Ajoutez des styles par défaut pour le composant Byline.

1. Revenez à l’IDE et au **ui.frontend** projet sous `/src/main/webpack/components`:
1. Créez un fichier nommé `_byline.scss`.

   ![explorateur de projets par défaut](assets/custom-component/byline-style-project-explorer.png)

1. Ajoutez la signature des mises en oeuvre CSS (écrite en tant que SCSS) dans le `_byline.scss`:

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
1. Ouvrez un terminal et accédez au `ui.frontend` module .
1. Démarrez le `watch` traitez avec la commande npm suivante :

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Revenez au navigateur et accédez au [Article de LA SkateParks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Vous devriez voir les styles mis à jour dans le composant.

   ![composant d’signature terminé](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Vous devrez peut-être vider le cache du navigateur pour vous assurer que le fichier CSS obsolète n’est pas diffusé et actualiser la page avec le composant signature pour obtenir le style complet.

## Félicitations ! {#congratulations}

Félicitations, vous venez de créer entièrement un composant personnalisé à l’aide d’Adobe Experience Manager !

### Étapes suivantes {#next-steps}

Continuez à en savoir plus sur le développement des composants d’AEM en explorant la manière d’écrire des tests JUnit pour le code Java signature afin de vous assurer que tout est développé correctement et que la logique commerciale implémentée est correcte et complète.

* [Écriture de tests unitaires ou de composants AEM](unit-testing.md)

Afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passer en revue et déployer le code localement sur la branche Git `tutorial/custom-component-solution`.

1. Cloner le [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) référentiel.
1. Consultez la section `tutorial/custom-component-solution` branche
