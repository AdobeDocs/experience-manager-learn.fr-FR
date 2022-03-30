---
title: Développement avec AEM SPA Editor - Tutoriel Hello World
description: AEM Éditeur SPA prend en charge la modification contextuelle d’une ou de plusieurs applications d’une seule page. Ce tutoriel présente le développement SPA à utiliser avec AEM SDK JS de l’éditeur SPA. Le tutoriel étend l’application We.Retail Journal en ajoutant un composant Hello World personnalisé. Les utilisateurs peuvent suivre le tutoriel à l’aide des structures React ou Angular.
version: 6.3, 6.4, 6.5
topic: SPA
feature: SPA Editor
role: Developer
level: Beginner
exl-id: e900301d-411c-4c02-8443-2a0fa56b65b5
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '3144'
ht-degree: 2%

---

# Développement avec AEM SPA Editor - Tutoriel Hello World {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> Ce tutoriel est **obsolète**. Il est recommandé de procéder comme suit : [Prise en main de l’éditeur et de l’Angular d’AEM SPA](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/angular/overview.html) ou [Prise en main de l’éditeur SPA d’AEM et de React](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/react/overview.html)

AEM Éditeur SPA prend en charge la modification contextuelle d’une ou de plusieurs applications d’une seule page. Ce tutoriel présente le développement SPA à utiliser avec AEM SDK JS de l’éditeur SPA. Le tutoriel étend l’application We.Retail Journal en ajoutant un composant Hello World personnalisé. Les utilisateurs peuvent suivre le tutoriel à l’aide des structures React ou Angular.

>[!NOTE]
>
> La fonction Éditeur d’application sur une seule page (SPA) requiert AEM Service Pack 2 ou version ultérieure.
>
> L’éditeur SPA est la solution recommandée pour les projets qui nécessitent SPA rendu côté client basé sur une structure (par exemple, React ou Angular).

## Lecture requise {#prereq}

Ce tutoriel est conçu pour mettre en évidence les étapes nécessaires pour mapper un composant SPA à un composant AEM afin d’activer la modification contextuelle. Les utilisateurs qui commencent ce tutoriel doivent connaître les concepts de base du développement avec Adobe Experience Manager, AEM, ainsi que le développement avec React des structures d’Angular. Le tutoriel couvre les tâches de développement front-end et back-end.

Il est recommandé de passer en revue les ressources suivantes avant de commencer ce tutoriel :

* [Vidéo sur les fonctionnalités de l’éditeur de SPA](spa-editor-framework-feature-video-use.md) - Présentation vidéo de l’éditeur SPA et de l’application We.Retail Journal.
* [Tutoriel React.js](https://reactjs.org/tutorial/tutorial.html) - Une introduction au développement avec le framework React.
* [Tutoriel sur l’Angular](https://angular.io/tutorial) - Une introduction au développement avec Angular

## Environnement de développement local {#local-dev}

Ce tutoriel est conçu pour les éléments suivants :

[Adobe Experience Manager 6.5](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes.html ) ou [Adobe Experience Manager 6.4](https://helpx.adobe.com/fr/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [Service Pack 5](https://helpx.adobe.com/fr/experience-manager/6-4/release-notes/sp-release-notes.html)

Dans ce tutoriel, les technologies et outils suivants doivent être installés :

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) et npm 5.6.0+ (npm est installé avec node.js)

Vérifiez deux fois l’installation des outils ci-dessus en ouvrant un nouveau terminal et en exécutant les opérations suivantes :

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## Présentation {#overview}

Le concept de base consiste à mapper un composant SPA à un composant AEM. AEM des composants, exécution côté serveur, exportation de contenu sous la forme de JSON. Le contenu JSON est consommé par le SPA, en exécutant côté client dans le navigateur. Un mappage 1:1 entre les composants SPA et un composant AEM est créé.

![Mappage des composants SPA](assets/spa-editor-helloworld-tutorial-use/mapto.png)

Cadres populaires [React JS](https://reactjs.org/) et [Angular](https://angular.io/) sont prises en charge par défaut. Les utilisateurs peuvent suivre ce tutoriel dans Angular ou React, quel que soit le framework avec lequel ils se sentent le plus à l’aise.

## Configuration du projet {#project-setup}

SPA développement a un pied dans AEM développement, et l&#39;autre dehors. L&#39;objectif est de permettre SPA développement de se produire de manière indépendante, et (principalement) indépendante d&#39;AEM.

* SPA projets peuvent fonctionner indépendamment du projet AEM lors du développement frontal.
* Outils de génération front-end et technologies tels que Webpack, NPM, [!DNL Grunt] et [!DNL Gulp]continuer à être utilisée.
* Pour créer pour AEM, le projet SPA est compilé et automatiquement inclus dans le projet AEM.
* Packages d’AEM standard utilisés pour déployer le SPA dans l’.

![Présentation des artefacts et déploiement](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA développement a un pied dans AEM développement, et l&#39;autre dans le développement - permettant à un développement plus proche de se produire de manière indépendante, et (surtout) agnostique envers l&#39;.*

L’objectif de ce tutoriel est d’étendre l’application We.Retail Journal avec un nouveau composant. Commencez par télécharger le code source de l’application We.Retail Journal et par le déployer sur une AEM locale.

1. **Télécharger** la plus récente [Code journal We.Retail de GitHub](https://github.com/adobe/aem-sample-we-retail-journal).

   Ou clonez le référentiel à partir de la ligne de commande :

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >Le tutoriel va travailler sur la **master** branche avec **1.2.1-SNAPSHOT** version du projet.

1. La structure suivante doit être visible :

   ![Structure du dossier du projet](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   Le projet contient les modules Maven suivants :

   * `all`: Incorpore et installe l’ensemble du projet dans un seul module.
   * `bundles`: Contient deux lots OSGi : les biens communs et le noyau qui contiennent [!DNL Sling Models] et tout autre code Java.
   * `ui.apps`: contient les parties /apps du projet, par exemple les bibliothèques client JS et CSS, les composants, les configurations spécifiques au mode d’exécution.
   * `ui.content`: contient le contenu structurel et les configurations (`/content`, `/conf`)
   * `react-app`: Application We.Retail Journal React. Il s’agit à la fois d’un module Maven et d’un projet webpack.
   * `angular-app`: Application d’Angular We.Retail Journal. Il s’agit d’une [!DNL Maven] et un projet webpack.

1. Ouvrez une nouvelle fenêtre de terminal et exécutez la commande suivante pour créer et déployer l’application entière sur une instance d’AEM locale s’exécutant sur [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > Dans ce projet, le profil Maven pour créer et empaqueter l’ensemble du projet est `autoInstallSinglePackage`

1. Accédez à :

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   L’application We.Retail Journal doit s’afficher dans l’éditeur AEM Sites.

1. Dans [!UICONTROL Modifier] , sélectionnez un composant à modifier et effectuez une mise à jour du contenu.

   ![Modification d’un composant](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. Sélectionnez la [!UICONTROL Propriétés de la page] pour ouvrir la [!UICONTROL Propriétés de la page]. Sélectionner [!UICONTROL Modifier le modèle] pour ouvrir le modèle de la page.

   ![Menu Propriétés de la page](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. Dans la dernière version de SPA Editor, [Modèles modifiables](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) peut être utilisé de la même manière qu’avec les implémentations Sites traditionnelles. Il sera réexaminé ultérieurement avec notre composant personnalisé.

   >[!NOTE]
   >
   > Uniquement AEM 6.5 et AEM 6.4 + **Service Pack 5** prennent en charge les modèles modifiables.

## Présentation du développement {#development-overview}

![Développement d’une vue](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA itérations de développement se produisent indépendamment d’AEM. Lorsque le SPA est prêt à être déployé dans AEM, les étapes de haut niveau suivantes ont lieu (comme illustré ci-dessus).

1. La version AEM du projet est appelée, ce qui déclenche à son tour une version du projet SPA. Le journal We.Retail utilise la variable [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. Le projet SPA [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) incorpore le SPA compilé en tant que bibliothèque cliente AEM dans le projet d’AEM.
1. Le projet AEM génère un module AEM, y compris le fichier de données compilé, ainsi que tout autre code d’ pris en charge.

## Création d’un composant AEM {#aem-component}

**Persona : AEM Développeur**

Un composant AEM sera d’abord créé. Le composant AEM est responsable du rendu des propriétés JSON lues par le composant React. Le composant AEM est également chargé de fournir une boîte de dialogue pour toutes les propriétés modifiables du composant.

Utilisation [!DNL Eclipse]ou autre [!DNL IDE]Importez le projet Maven We.Retail Journal.

1. Mettre à jour le réacteur **pom.xml** pour supprimer la variable [!DNL Apache Rat] module externe Ce module externe vérifie chaque fichier pour s’assurer qu’il existe un en-tête de licence. Pour nos besoins, cette fonctionnalité n’est pas nécessaire.

   Dans **aem-sample-we-retail-journal/pom.xml** remove **apache-rate-plugin**:

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. Dans le **we-retail-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) crée un noeud sous `ui.apps/jcr_root/apps/we-retail-journal/components` named **helloworld** de type **cq:Component**.
1. Ajoutez les propriétés suivantes au **helloworld** composant, représenté en XML (`/helloworld/.content.xml`) ci-dessous :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Composant Hello World](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > Pour illustrer la fonction Modèles modifiables, nous avons délibérément défini la variable `componentGroup="Custom Components"`. Dans un projet réel, il est préférable de minimiser le nombre de groupes de composants, de sorte qu’un meilleur groupe soit &quot;[!DNL We.Retail Journal]&quot; pour correspondre aux autres composants de contenu.
   >
   > Uniquement AEM 6.5 et AEM 6.4 + **Service Pack 5** prennent en charge les modèles modifiables.

1. Une boîte de dialogue s’ouvre ensuite pour permettre la configuration d’un message personnalisé pour la variable **Hello World** composant. Sous `/apps/we-retail-journal/components/helloworld` ajouter un nom de noeud **cq:dialog** de **nt:unstructured**.
1. Le **cq:dialog** affiche un seul champ de texte qui conserve le texte dans une propriété nommée **[!DNL message]**. Sous le **cq:dialog** ajoutez les noeuds et propriétés suivants, représentés en XML ci-dessous (`helloworld/_cq_dialog/.content.xml`) :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
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
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
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

   ![structure de fichier](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   La définition de noeud XML ci-dessus crée une boîte de dialogue avec un seul champ de texte qui permet à un utilisateur de saisir un &quot;message&quot;. Notez la propriété `name="./message"` dans le `<message />` noeud . Il s’agit du nom de la propriété qui sera stockée dans le JCR dans AEM.

1. Une boîte de dialogue de stratégie vide sera alors créée (`cq:design_dialog`). La boîte de dialogue Stratégie est nécessaire pour afficher le composant dans l’éditeur de modèles. Pour ce cas d’utilisation simple, il s’agira d’une boîte de dialogue vide.

   Sous `/apps/we-retail-journal/components/helloworld` ajouter un nom de noeud `cq:design_dialog` de `nt:unstructured`.

   La configuration est représentée en XML ci-dessous (`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. Déployez la base de code pour AEM à partir de la ligne de commande :

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   Dans [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) valider le déploiement du composant en examinant le dossier sous `/apps/we-retail-journal/components:`

   ![Structure de composants déployée dans CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Création d’un modèle Sling {#create-sling-model}

**Persona : AEM Développeur**

Suivant un [!DNL Sling Model] est créé pour sauvegarder la variable [!DNL Hello World] composant. Dans le cas d’utilisation WCM classique, la variable [!DNL Sling Model] met en oeuvre toute logique commerciale et un script de rendu côté serveur (HTL) effectue un appel à la fonction [!DNL Sling Model]. Le script de rendu reste ainsi relativement simple.

[!DNL Sling Models] sont également utilisés dans le cas d’utilisation SPA pour implémenter la logique commerciale côté serveur. La différence est que dans la variable [!DNL SPA] le cas d’utilisation, [!DNL Sling Models] expose ses méthodes en tant que JSON sérialisé.

>[!NOTE]
>
>Il est recommandé aux développeurs d’utiliser [Composants principaux AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr) si possible. Entre autres fonctionnalités, les composants principaux fournissent les [!DNL Sling Models] avec une sortie JSON &quot;SPA&quot;, ce qui permet aux développeurs de se concentrer davantage sur la présentation frontale.

1. Dans l’éditeur de votre choix, ouvrez le **we-retail-journal-commons** project ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. Dans le package `com.adobe.cq.sample.spa.commons.impl.models`:
   * Créez une nouvelle classe nommée `HelloWorld`.
   * Ajout d’une interface d’implémentation pour `com.adobe.cq.export.json.ComponentExporter.`

   ![Assistant Nouvelle classe Java](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   Le `ComponentExporter` doit être implémentée pour que la variable [!DNL Sling Model] pour être compatible avec AEM Content Services.

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. Ajouter une variable statique nommée `RESOURCE_TYPE` pour identifier la variable [!DNL HelloWorld] type de ressource du composant :

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. Ajout d’annotations OSGi pour `@Model` et `@Exporter`. Le `@Model` annotation enregistre la classe en tant que [!DNL Sling Model]. Le `@Exporter` annotation expose les méthodes sous la forme d’un fichier JSON sérialisé à l’aide de la fonction [!DNL Jackson Exporter] framework.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. Mise en oeuvre de la méthode `getDisplayMessage()` pour renvoyer la propriété JCR `message`. Utilisez la variable [!DNL Sling Model] annotation de `@ValueMapValue` pour faciliter la récupération de la propriété `message` stocké sous le composant. Le `@Optional` l’annotation est importante, car lorsque le composant est ajouté pour la première fois à la page,  `message`  ne sera pas renseignée.

   Dans le cadre de la logique métier, une chaîne &quot;**Hello**&quot;, sera ajouté en préfixe au message.

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > Nom de la méthode `getDisplayMessage` est important. Lorsque la variable [!DNL Sling Model] est sérialisé avec la variable [!DNL Jackson Exporter] il sera exposé en tant que propriété JSON : `displayMessage`. Le [!DNL Jackson Exporter] sérialisera et présentera tous les `getter` méthodes qui ne prennent pas de paramètre (sauf si elles sont explicitement marquées pour ignorer). Plus loin dans l’application React/Angular, nous allons lire cette valeur de propriété et l’afficher dans le cadre de l’application.

   La méthode `getExportedType` est également important. La valeur du composant `resourceType` sera utilisé pour &quot;mapper&quot; les données JSON au composant front-end (Angular/React). Nous allons l’explorer dans la section suivante.

1. Mise en oeuvre de la méthode `getExportedType()` pour renvoyer le type de ressource de la fonction `HelloWorld` composant.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   Le code complet pour [**HelloWorld.java** se trouve ici.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Déployez le code pour AEM à l’aide d’Apache Maven :

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   Vérification du déploiement et de l’enregistrement de la variable [!DNL Sling Model] en accédant à [[!UICONTROL État] > [!UICONTROL Modèles Sling]](http://localhost:4502/system/console/status-slingmodels) dans la console OSGi.

   Vous devriez constater que la variable `HelloWorld` Le modèle Sling est lié au `we-retail-journal/components/helloworld` Type de ressource Sling et qu’il est enregistré en tant que [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## Créer un composant React {#react-component}

**Persona : Développeur front-end**

Ensuite, le composant React sera créé. Ouvrez le **response-app** module ( `<src>/aem-sample-we-retail-journal/react-app`) à l’aide de l’éditeur de votre choix.

>[!NOTE]
>
> N’hésitez pas à ignorer cette section si vous souhaitez uniquement [Développement des Angulars](#angular-component).

1. Dans le `react-app` accédez au dossier src. Développez le dossier des composants pour afficher les fichiers de composant React existants.

   ![structure du fichier de composant react](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. Ajoutez un nouveau fichier sous le dossier de composants nommé `HelloWorld.js`.
1. Ouvrez `HelloWorld.js`. Ajoutez une instruction d’importation pour importer la bibliothèque de composants React. Ajoutez une deuxième instruction d’importation pour importer la variable `MapTo` assistance fournie par Adobe. Le `MapTo` L’assistant fournit un mappage du composant React sur le fichier JSON du composant AEM.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. Sous les importations, créez une nouvelle classe nommée `HelloWorld` qui étend React `Component` . Ajoutez les `render()` à la méthode `HelloWorld` classe .

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. Le `MapTo` L’assistant inclut automatiquement un objet nommé `cqModel` dans le cadre des props du composant React. Le `cqModel` inclut toutes les propriétés exposées par la variable [!DNL Sling Model].

   Mémoriser [!DNL Sling Model] created précédemment contient une méthode `getDisplayMessage()`. `getDisplayMessage()` est traduit en tant que clé JSON nommée `displayMessage` lors de la sortie.

   Mettez en oeuvre le `render()` pour générer une `h1` qui contient la valeur de `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html), une extension de syntaxe à JavaScript, est utilisée pour renvoyer le balisage final du composant.

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. Mettez en oeuvre une méthode de configuration de modification. Cette méthode est transmise via la méthode `MapTo` aide et fournit à l’éditeur d’AEM des informations pour afficher un espace réservé dans le cas où le composant est vide. Cela se produit lorsque le composant est ajouté à la SPA mais n’a pas encore été créé. Ajoutez ce qui suit sous le `HelloWorld` Classe :

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. À la fin du fichier, appelez la fonction `MapTo` assistance, transmission de `HelloWorld` et la variable `HelloWorldEditConfig`. Le composant React est mappé au composant AEM en fonction du type de ressource du composant AEM : `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   Le code terminé pour [**HelloWorld.js** se trouve ici.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Ouvrez le fichier `ImportComponents.js`. Vous pouvez le trouver à l’adresse `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   Ajoutez une ligne pour que la variable `HelloWorld.js` avec les autres composants du lot JavaScript compilé :

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. Dans le  `components`  créer un dossier nommé `HelloWorld.css` comme frère de `HelloWorld.js.` Renseignez le fichier avec ce qui suit pour créer un style de base pour le `HelloWorld` component :

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. rouvrir `HelloWorld.js` et mettre à jour sous les instructions d’importation pour exiger `HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. Déployez le code pour AEM à l’aide d’Apache Maven :

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. Dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) open `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. Recherchez rapidement HelloWorld dans app.js pour vérifier que le composant React a été inclus dans l’application compilée.

   >[!NOTE]
   >
   > **app.js** est l’application React groupée. Le code n’est plus lisible à l’oeil. Le `npm run build` a déclenché une version optimisée qui génère du code JavaScript compilé pouvant être interprété par les navigateurs modernes.


## Création d’un composant d’Angular {#angular-component}

**Persona : Développeur front-end**

>[!NOTE]
>
> N’hésitez pas à ignorer cette section si vous n’êtes intéressé que par le développement React.

Ensuite, le composant Angular sera créé. Ouvrez le **angular-app** module (`<src>/aem-sample-we-retail-journal/angular-app`) à l’aide de l’éditeur de votre choix.

1. Dans le `angular-app` accéder à son dossier ; `src` dossier. Développez le dossier de composants pour afficher les fichiers de composants d’Angular existants.

   ![Angular la structure du fichier](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. Ajoutez un nouveau dossier sous le dossier de composants nommé `helloworld`. Sous la `helloworld` Ajoutez de nouveaux fichiers nommés `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. Ouvrez `helloworld.component.ts`. Ajouter une instruction d’importation pour importer l’Angular `Component` et `Input` classes. Créez un composant, en pointant le `styleUrls` et `templateUrl` to `helloworld.component.css` et `helloworld.component.html`. Enfin exporter la classe `HelloWorldComponent` avec l’entrée attendue de `displayMessage`.

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > Si vous vous souvenez du [!DNL Sling Model] créé précédemment, il existait une méthode **getDisplayMessage()**. Le fichier JSON sérialisé de cette méthode sera **displayMessage**, que nous lisons maintenant dans l’application Angular.

1. Ouvrir `helloworld.component.html` pour inclure un `h1` qui imprime la balise `displayMessage` property:

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. Mettre à jour `helloworld.component.css` pour inclure certains styles de base pour le composant.

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Mettre à jour `helloworld.component.spec.ts` avec le banc d&#39;essai suivant :

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. Mise à jour suivante `src/components/mapping.ts` pour inclure la variable `HelloWorldComponent`. Ajouter un `HelloWorldEditConfig` qui marquera l’espace réservé dans l’éditeur d’AEM avant la configuration du composant. Enfin, ajoutez une ligne pour mapper le composant AEM au composant Angular avec le `MapTo` assistance.

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   Le code complet pour [**mapping.ts** se trouve ici.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. Mettre à jour `src/app.module.ts` pour mettre à jour la variable **NgModule**. Ajoutez la variable **`HelloWorldComponent`** as a **déclaration** qui appartient à la variable **AppModule**. Ajoutez également la variable `HelloWorldComponent` as a **entryComponent** afin qu’il soit compilé et inclus dynamiquement dans l’application au fur et à mesure du traitement du modèle JSON.

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   Le code terminé pour [**app.module.ts** se trouve ici.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Déployez le code pour AEM à l’aide de Maven :

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. Dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) open `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. Recherchez rapidement des **HelloWorld** in `main.js` pour vérifier que le composant Angular a été inclus.

   >[!NOTE]
   >
   > **main.js** est l’application d’Angular groupée. Le code n’est plus lisible à l’oeil. La commande npm run build a déclenché une version optimisée qui génère du code JavaScript compilé pouvant être interprété par les navigateurs modernes.

## Mise à jour du modèle {#template-update}

1. Accédez au modèle modifiable pour les versions React et/ou Angular :

   * (Angular) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. Sélectionnez la principale [!UICONTROL Conteneur de mises en page] et sélectionnez la variable [!UICONTROL Stratégie] pour ouvrir sa stratégie :

   ![Sélectionner la stratégie de mise en page](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   Sous **[!UICONTROL Propriétés]** > **[!UICONTROL Composants autorisés]**, effectuez une recherche pour **[!DNL Custom Components]**. Vous devriez voir la variable **[!DNL Hello World]** , sélectionnez-le. Enregistrez vos modifications en cochant la case située dans le coin supérieur droit.

   ![Configuration de la stratégie de conteneur de mises en page](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. Après l’enregistrement, le **[!DNL HelloWorld]** en tant que composant autorisé dans [!UICONTROL Conteneur de mises en page].

   ![Composants autorisés mis à jour](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > Seuls AEM 6.5 et AEM 6.4.5 prennent en charge la fonction de modèle modifiable de l’éditeur de ressources. Si vous utilisez AEM 6.4, vous devez configurer manuellement la stratégie pour les composants autorisés via CRXDE Lite : `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` ou `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite présentant les configurations de stratégie mises à jour pour [!UICONTROL Composants autorisés] dans le [!UICONTROL Conteneur de mises en page]:

   ![CRXDE Lite présentant les configurations de stratégie mises à jour pour les composants autorisés dans le conteneur de mises en page](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## Assemblage {#putting-together}

1. Accédez aux pages Angular ou React :

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. Recherchez le **[!DNL Hello World]** et faites glisser et déposez le composant **[!DNL Hello World]** sur la page.

   ![salut world drag+drop](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   L’espace réservé doit s’afficher.

   ![Hello world place holder](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. Sélectionnez le composant et ajoutez un message dans la boîte de dialogue, par exemple &quot;World&quot; (Monde) ou &quot;Your Name&quot; (Votre nom). Enregistrez les modifications.

   ![composant rendu](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   Notez que la chaîne &quot;Hello&quot; est toujours précédée du message. Cela est le résultat de la logique dans la variable `HelloWorld.java` [!DNL Sling Model].

## Étapes suivantes {#next-steps}

[Solution terminée pour le composant HelloWorld](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* Code source complet pour [[!DNL We.Retail Journal] sur GitHub](https://github.com/adobe/aem-sample-we-retail-journal)
* Consultez un tutoriel plus détaillé sur le développement de React avec [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/fr/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## Résolution des problèmes {#troubleshooting}

### Impossible de créer le projet dans Eclipse {#unable-to-build-project-in-eclipse}

**Erreur :** Erreur lors de l’importation du [!DNL We.Retail Journal] dans Eclipse pour les exécutions d’objectifs non reconnues :

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![assistant d’erreur eclipse](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**Résolution**: Cliquez sur Terminer pour les résoudre ultérieurement. Cela ne doit pas empêcher la fin du tutoriel.

**Erreur**: le module React, `react-app`, ne se crée pas correctement lors d’une génération Maven.

**Résolution :** Essayez de supprimer la variable `node_modules` sous le dossier **response-app**. Exécutez à nouveau la commande Apache Maven. `mvn  clean install -PautoInstallSinglePackage` à partir de la racine du projet.

### Dépendances insatisfaites dans AEM {#unsatisfied-dependencies-in-aem}

![Erreur de dépendance du gestionnaire de modules](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

Si une dépendance AEM n’est pas satisfaite, dans l’un des **[!UICONTROL AEM Gestionnaire de modules]** ou dans la variable **[!UICONTROL Console web d’AEM]** (Console Felix), cela indique que SPA fonction d’éditeur n’est pas disponible.

### Le composant ne s’affiche pas

**Erreur**: Même après un déploiement réussi et la vérification que les versions compilées des applications React/Angular ont été mises à jour `helloworld` n’est pas affiché lorsque je le fais glisser sur la page. Je peux voir le composant dans l’interface utilisateur d’AEM.

**Résolution**: Effacez l’historique/le cache de votre navigateur et/ou ouvrez un nouveau navigateur ou utilisez le mode incognito. Si cela ne fonctionne pas, invalidez le cache de la bibliothèque cliente sur l’instance d’AEM locale. AEM tente de mettre en cache de grandes bibliothèques clientes afin d’être efficace. Parfois, l’invalidation manuelle du cache est nécessaire pour résoudre les problèmes de mise en cache du code obsolète.

Accédez à : [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) et cliquez sur Invalider le cache. Revenez à la page React/Angular et actualisez-la.

![Reconstruire la bibliothèque cliente](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
