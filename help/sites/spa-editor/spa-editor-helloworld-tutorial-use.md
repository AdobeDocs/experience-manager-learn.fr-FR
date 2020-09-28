---
title: Développer avec l’éditeur d’applications d’une seule page - Didacticiel Hello World
description: aem SPA Editor permet la modification en contexte d’une application monopage ou d’une application d’une seule page. Ce didacticiel présente le développement d’une application d’une seule page à utiliser avec AEM SPA Editor JS SDK. Le didacticiel étendra l'application de Journal We.Retail en ajoutant un composant Hello World personnalisé. Les utilisateurs peuvent compléter le didacticiel à l’aide des structures Réagir ou Angular.
sub-product: sites, content-services
feature: spa-editor
topics: development, single-page-applications
audience: developer
doc-type: tutorial
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '3181'
ht-degree: 2%

---


# Développer avec l’éditeur d’applications d’une seule page - Didacticiel Hello World {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> Ce didacticiel est **obsolète**. Il est recommandé de procéder comme suit : [Prise en main de l’éditeur AEM d’application d’une seule page et de Angular](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-angular-tutorial/overview.html) ou [Prise en main de l’éditeur AEM d’une seule page et réaction](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-react-tutorial/overview.html)

aem SPA Editor permet la modification en contexte d’une application monopage ou d’une application d’une seule page. Ce didacticiel présente le développement d’une application d’une seule page à utiliser avec AEM SPA Editor JS SDK. Le didacticiel étendra l&#39;application de Journal We.Retail en ajoutant un composant Hello World personnalisé. Les utilisateurs peuvent compléter le didacticiel à l’aide des structures Réagir ou Angular.

>[!NOTE]
>
> La fonctionnalité Editeur d’application monopage (SPA) nécessite AEM Service Pack 2 6.4 ou version ultérieure.
>
> L’éditeur d’applications monopages est la solution recommandée pour les projets qui nécessitent un rendu côté client basé sur la structure d’applications monopages (par exemple, Réagir ou Angular).

## Lecture prérequise {#prereq}

Ce didacticiel vise à mettre en évidence les étapes nécessaires pour mapper un composant SPA à un composant AEM afin d’activer la modification en contexte. Les utilisateurs qui commencent ce tutoriel doivent connaître les concepts de base du développement avec Adobe Experience Manager, AEM, ainsi que le développement avec React of Angular framework. Ce didacticiel porte à la fois sur les tâches de développement dorsales et frontales.

Il est recommandé de passer en revue les ressources suivantes avant de commencer ce didacticiel :

* [Vidéo](spa-editor-framework-feature-video-use.md) sur les fonctionnalités de l’éditeur d’applications monopages - Présentation vidéo de l’éditeur d’applications monopages et de l’application de Journal We.Retail.
* [Didacticiel](https://reactjs.org/tutorial/tutorial.html) React.js - Présentation du développement avec la structure React.
* [Didacticiel](https://angular.io/tutorial) angulaire - Présentation du développement avec Angular

## Environnement de développement local {#local-dev}

Ce didacticiel est conçu pour :

[Adobe Experience Manager 6.5](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes.html ) ou [Adobe Experience Manager 6.4](https://helpx.adobe.com/fr/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [Service Pack 5](https://helpx.adobe.com/fr/experience-manager/6-4/release-notes/sp-release-notes.html)

Dans ce didacticiel, les technologies et outils suivants doivent être installés :

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) et npm 5.6.0+ (npm est installé avec node.js)

Le doublon vérifie l&#39;installation des outils ci-dessus en ouvrant un nouveau terminal et en exécutant les opérations suivantes :

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

Le concept de base est de mapper un composant SPA à un composant AEM. aem composants, exécution côté serveur, exportation de contenu sous la forme de JSON. Le contenu JSON est consommé par l’application d’une seule page, exécutant côté client dans le navigateur. Un mappage 1:1 entre les composants SPA et un composant AEM est créé.

![Mappage des composants SPA](assets/spa-editor-helloworld-tutorial-use/mapto.png)

Les cadres populaires [réagissent JS](https://reactjs.org/) et [Angular](https://angular.io/) sont pris en charge. Les utilisateurs peuvent compléter ce tutoriel en angulaire ou en réagissant, quel que soit le cadre dans lequel ils se sentent le plus à l&#39;aise.

## Configuration du projet {#project-setup}

Le développement des applications monopoles a un pied dans le développement AEM, et l&#39;autre dehors. L&#39;objectif est de permettre au développement d&#39;une application d&#39;une seule page, et (principalement) de se produire indépendamment de l&#39;AEM.

* Les projets SPA peuvent fonctionner indépendamment du projet AEM pendant le développement frontal.
* Des outils de création et des technologies de pointe tels que Webpack, NPM [!DNL Grunt] et [!DNL Gulp]continuent d&#39;être utilisés.
* Pour construire pour AEM, le projet SPA est compilé et automatiquement inclus dans le projet AEM.
* Packages AEM standard utilisés pour déployer l’application d’une seule page dans AEM.

![Présentation des artefacts et du déploiement](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*Le développement d&#39;une ZPS a un pied dans le développement AEM, et l&#39;autre dehors - permettant au développement d&#39;une ZPS de se produire indépendamment, et (principalement) agnostique à AEM.*

L&#39;objectif de ce didacticiel est d&#39;étendre l&#39;application de Journal We.Retail à un nouveau composant. Début en téléchargeant le code source de l’application de Journal We.Retail et en le déployant sur un AEM local.

1. **Téléchargez** le dernier code de Journal [We.Retail sur GitHub](https://github.com/adobe/aem-sample-we-retail-journal).

   Ou cloner le référentiel à partir de la ligne de commande :

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >Le tutoriel fonctionnera avec la branche **maître** avec la version **1.2.1-SNAPSHOT** du projet.

2. La structure suivante doit être visible :

   ![Structure du dossier Projet](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   Le projet contient les modules maven suivants :

   * `all`: Incorpore et installe l’ensemble du projet dans un seul package.
   * `bundles`: Contient deux lots OSGi : commons et noyau qui contiennent [!DNL Sling Models] et d&#39;autres codes Java.
   * `ui.apps`: contient les parties /apps du projet, à savoir les clientlibs JS et CSS, les composants, les configurations spécifiques au mode d’exécution.
   * `ui.content`: contient le contenu structurel et les configurations (`/content`, `/conf`)
   * `react-app`: Application Web.Retail Journal React. Il s&#39;agit à la fois d&#39;un module Maven et d&#39;un projet webpack.
   * `angular-app`: Application angulaire du Journal We.Retail. Il s&#39;agit à la fois d&#39;un [!DNL Maven] module et d&#39;un projet webpack.

3. Ouvrez une nouvelle fenêtre de terminal et exécutez la commande suivante pour créer et déployer l’application entière sur une instance AEM locale s’exécutant sur [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > Dans ce projet, le profil Maven pour construire et assembler l&#39;ensemble du projet est `autoInstallSinglePackage`

   >[!CAUTION]
   >
   > Si vous recevez une erreur lors de la création, [assurez-vous que votre fichier Maven settings.xml inclut le référentiel](https://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html)d’artefacts Maven d’Adobe.

4. Accédez à:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   L’application de Journal We.Retail doit être affichée dans l’éditeur AEM Sites.

5. En mode [!UICONTROL Edition] , sélectionnez un composant à modifier et mettez à jour le contenu.

   ![Modification d’un composant](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

6. Sélectionnez l’icône Propriétés [!UICONTROL de la] page pour ouvrir les Propriétés [!UICONTROL de la]page. Sélectionnez [!UICONTROL Modifier le modèle] pour ouvrir le modèle de la page.

   ![Menu Propriétés de page](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

7. Dans la dernière version de l’éditeur d’application d’une seule page, les modèles [](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) modifiables peuvent être utilisés de la même manière que dans les implémentations traditionnelles de sites. Ceci sera réexaminé ultérieurement avec notre composant personnalisé.

   >[!NOTE]
   >
   > Seuls AEM 6.5 et AEM 6.4 + **Service Pack 5** prennent en charge les modèles modifiables.

## Présentation du développement {#development-overview}

![Développement d’une vue](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

Les itérations de développement d’une application d’une seule page se produisent indépendamment de l’AEM. Lorsque l’application d’une seule page est prête à être déployée AEM les étapes de haut niveau suivantes ont lieu (comme illustré ci-dessus).

1. La version du projet AEM est appelée, ce qui déclenche à son tour une version du projet d’application d’une seule page. Le Journal We.Retail utilise le plug-in [****](https://github.com/eirslett/frontend-maven-plugin)frontend-maven.
2. Le générateur [**aem-clientlib**](https://www.npmjs.com/package/aem-clientlib-generator) du projet SPA incorpore l’application d’une seule page comme bibliothèque cliente AEM dans le projet AEM.
3. Le projet AEM génère un module AEM, y compris l’application d’une seule page, ainsi que tout autre code de prise en charge de l’AEM.

## Créer un composant AEM {#aem-component}

**Persona : Développeur AEM**

Un composant AEM sera d’abord créé. Le composant AEM est responsable du rendu des propriétés JSON lues par le composant Réagir. Le composant AEM est également chargé de fournir une boîte de dialogue pour toutes les propriétés modifiables du composant.

A l’aide [!DNL Eclipse]ou d’autres [!DNL IDE], importez le projet Importation de Journal We.Retail.

1. Mettez à jour le réacteur **pom.xml** pour supprimer le [!DNL Apache Rat] module externe. Ce module externe vérifie chaque fichier pour s’assurer qu’il existe un en-tête de licence. Pour nos besoins, nous n&#39;avons pas besoin de nous préoccuper de cette fonctionnalité.

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

2. Dans le module **we-détaillant-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`), créez un nouveau noeud sous le `ui.apps/jcr_root/apps/we-retail-journal/components` nom **helloworld** de type **cq:Component.**
3. ajoutez les propriétés suivantes au composant **helloworld** , représenté en XML (`/helloworld/.content.xml`) ci-dessous :

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
   > Pour illustrer la fonction Modèles modifiables, nous avons délibérément défini le `componentGroup="Custom Components"`. Dans un projet réel, il est préférable de minimiser le nombre de groupes de composants, de sorte qu’un meilleur groupe serait &quot;[!DNL We.Retail Journal]&quot; pour correspondre aux autres composants de contenu.
   >
   > Seuls AEM 6.5 et AEM 6.4 + **Service Pack 5** prennent en charge les modèles modifiables.

4. Ensuite, une boîte de dialogue sera créée pour permettre la configuration d&#39;un message personnalisé pour le composant **Hello World** . Sous `/apps/we-retail-journal/components/helloworld` cet onglet, ajoutez un nom de noeud **cq:dialog** de **nt:unstructured**.
5. La boîte de dialogue **** cq:dialog **[!DNL message]** affiche un seul champ de texte qui conserve le texte d’une propriété nommée. Sous la boîte de dialogue **cq:dialog** nouvellement créée, ajoutez les noeuds et propriétés suivants, représentés en XML ci-dessous (`helloworld/_cq_dialog/.content.xml`) :

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

   ![structure de fichiers](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   La définition de noeud XML ci-dessus crée une boîte de dialogue avec un seul champ de texte qui permet à un utilisateur de saisir un &quot;message&quot;. Notez la propriété `name="./message"` dans le `<message />` noeud. Il s’agit du nom de la propriété qui sera stockée dans le JCR dans AEM.

6. Une boîte de dialogue de stratégie vide sera ensuite créée (`cq:design_dialog`). La boîte de dialogue Stratégie est nécessaire pour afficher le composant dans l’éditeur de modèles. Pour ce cas d&#39;utilisation simple, il s&#39;agira d&#39;une boîte de dialogue vide.

   Sous `/apps/we-retail-journal/components/helloworld` ajouter un nom de noeud `cq:design_dialog` de `nt:unstructured`.

   La configuration est représentée en XML ci-dessous (`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

7. Déployez la base de code vers AEM à partir de la ligne de commande :

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   Dans le [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) , la validation du composant a été déployée en inspectant le dossier sous `/apps/we-retail-journal/components:`

   ![Structure de composants déployée dans le CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Créer un modèle Sling {#create-sling-model}

**Persona : Développeur AEM**

Un élément [!DNL Sling Model] est ensuite créé pour sauvegarder le [!DNL Hello World] composant. Dans un cas d’utilisation WCM traditionnel, la [!DNL Sling Model] méthode implémente toute logique métier et un script de rendu côté serveur (HTL) effectue un appel au [!DNL Sling Model]. Le script de rendu reste ainsi relativement simple.

[!DNL Sling Models] sont également utilisées dans le cas d’utilisation de l’application d’une seule page pour implémenter la logique métier côté serveur. La différence est que dans le cas d’ [!DNL SPA] utilisation, la [!DNL Sling Models] présente ses méthodes comme JSON sérialisé.

>[!NOTE]
>
>En règle générale, les développeurs doivent s’efforcer d’utiliser [AEM composants](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html) principaux lorsque cela est possible. Entre autres fonctionnalités, les composants principaux fournissent [!DNL Sling Models] une sortie JSON &quot;compatible SPA&quot;, ce qui permet aux développeurs de se concentrer davantage sur la présentation frontale.

1. Dans l&#39;éditeur de votre choix, ouvrez le projet **nous-commerce-journal-biens** communs ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
2. Dans le package `com.adobe.cq.sample.spa.commons.impl.models`:
   * Créez une nouvelle classe nommée `HelloWorld`.
   * ajouter une interface d’implémentation pour `com.adobe.cq.export.json.ComponentExporter.`

   ![Assistant Nouvelle classe Java](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   L’ `ComponentExporter` interface doit être implémentée pour que la [!DNL Sling Model] bibliothèque soit compatible avec AEM Content Services.

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

3. ajoutez une variable statique nommée `RESOURCE_TYPE` pour identifier le type de ressource du [!DNL HelloWorld] composant :

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

4. ajoutez les annotations OSGi pour `@Model` et `@Exporter`. L&#39; `@Model` annotation enregistre la classe comme [!DNL Sling Model]une. L’ `@Exporter` annotation expose les méthodes sous forme de JSON sérialisé à l’aide de la [!DNL Jackson Exporter] structure.

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

5. Mettez en oeuvre la méthode `getDisplayMessage()` de renvoi de la propriété JCR `message`. Utilisez l’ [!DNL Sling Model] annotation de `@ValueMapValue` pour faciliter la récupération de la propriété `message` stockée sous le composant. L’ `@Optional` annotation est importante car, lorsque le composant est ajouté pour la première fois à la page, `message` il ne sera pas renseigné.

   Dans le cadre de la logique métier, une chaîne &quot;**Hello**&quot; sera ajoutée au message en préfixe.

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
   > Le nom de la méthode `getDisplayMessage` est important. Lorsque la valeur [!DNL Sling Model] est sérialisée avec [!DNL Jackson Exporter] elle est exposée sous la forme d’une propriété JSON : `displayMessage`. Le [!DNL Jackson Exporter] permet de sérialiser et d’exposer toutes les `getter` méthodes qui ne prennent pas de paramètre (sauf si elles sont explicitement marquées pour ignorer). Plus tard, dans l’application Réagir / Angular, nous lirons cette valeur de propriété et l’afficherons dans le cadre de l’application.

   La méthode `getExportedType` est également importante. La valeur du composant `resourceType` sera utilisée pour &quot;mapper&quot; les données JSON au composant frontal (Angular / React). Nous étudierons cette question dans la section suivante.

6. Implémentez la méthode `getExportedType()` pour renvoyer le type de ressource du `HelloWorld` composant.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   Le code complet de [**HelloWorld.java** se trouve ici.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

7. Déployez le code vers AEM à l’aide d’Apache Maven :

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   Vérifiez le déploiement et l’enregistrement du [!DNL Sling Model] logiciel en accédant à [[!UICONTROL État] > Modèles ](http://localhost:4502/system/console/status-slingmodels) Sling dans la console OSGi.

   Vous devriez voir que le modèle `HelloWorld` Sling est lié au type de ressource `we-retail-journal/components/helloworld` Sling et qu&#39;il est enregistré comme [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## Créer un composant de réaction {#react-component}

**Persona : Développeur frontal**

Ensuite, le composant Réagir sera créé. Ouvrez le module **réagir à l’application** ( `<src>/aem-sample-we-retail-journal/react-app`) à l’aide de l’éditeur de votre choix.

>[!NOTE]
>
> N&#39;hésitez pas à sauter cette section si vous n&#39;êtes intéressé que par le développement [](#angular-component)angulaire.

1. Dans le `react-app` dossier, accédez au dossier src. Développez le dossier de composants pour vue les fichiers de composants de Réagir existants.

   ![structure du fichier composant réactionnel](assets/spa-editor-helloworld-tutorial-use/react-components.png)

2. ajoutez un nouveau fichier sous le dossier de composants nommé `HelloWorld.js`.
3. Ouvrez `HelloWorld.js`. ajoutez une instruction d&#39;importation pour importer la bibliothèque de composants Réagir. ajoutez une deuxième instruction d&#39;importation pour importer l&#39; `MapTo` aide fournie par l&#39;Adobe. L’ `MapTo` assistance fournit un mappage du composant Réagir au fichier JSON du composant AEM.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

4. Sous les importations, créez une nouvelle classe nommée `HelloWorld` qui étend l&#39; `Component` interface Réagir. ajoutez la `render()` méthode requise à la `HelloWorld` classe.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

5. L’ `MapTo` assistant inclut automatiquement un objet nommé `cqModel` dans le cadre des props du composant Réagir. Le `cqModel` comprend toutes les propriétés exposées par le [!DNL Sling Model].

   Souvenez-vous que la méthode [!DNL Sling Model] créée précédemment contient une méthode `getDisplayMessage()`. `getDisplayMessage()` est converti en une clé JSON nommée `displayMessage` lors de la sortie.

   Implémentez la `render()` méthode pour générer une `h1` balise contenant la valeur de `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html), une extension de syntaxe à JavaScript, est utilisé pour renvoyer l’annotation finale du composant.

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

6. Mettez en oeuvre une méthode de modification de la configuration. Cette méthode est transmise via l’ `MapTo` assistant et fournit à l’éditeur AEM des informations pour afficher un espace réservé au cas où le composant serait vide. Cela se produit lorsque le composant est ajouté à l’application d’une seule page mais n’a pas encore été créé. ajoutez les éléments suivants sous la `HelloWorld` classe :

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

7. À la fin du fichier, appelez l&#39; `MapTo` assistance, en transmettant la `HelloWorld` classe et le `HelloWorldEditConfig`. Le composant Réagir sera alors mappé au composant AEM en fonction du type de ressource du composant AEM : `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   Le code complet pour [**HelloWorld.js** se trouve ici.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

8. Open the file `ImportComponents.js`. On peut le trouver à `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   ajoutez une ligne pour exiger que la ligne soit `HelloWorld.js` associée aux autres composants du lot JavaScript compilé :

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

9. Dans le `components` dossier, créez un nouveau fichier nommé `HelloWorld.css` frère de `HelloWorld.js.` Renseigner le fichier avec ce qui suit pour créer une mise en forme de base pour le `HelloWorld` composant :

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

10. rouvrez `HelloWorld.js` et mettez à jour sous les instructions d&#39;importation pour exiger `HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

11. Déployez le code vers AEM à l’aide d’Apache Maven :

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

12. Dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) , ouvrez `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. Effectuez une recherche rapide de HelloWorld dans app.js pour vérifier que le composant React a été inclus dans l’application compilée.

   >[!NOTE]
   >
   > **app.js** est l’application React fournie. Le code n&#39;est plus lisible par l&#39;homme. La `npm run build` commande a déclenché une génération optimisée qui génère du code JavaScript compilé qui peut être interprété par les navigateurs modernes.


## Créer un composant angulaire {#angular-component}

**Persona : Développeur frontal**

>[!NOTE]
>
> N&#39;hésitez pas à sauter cette section si vous souhaitez uniquement développer React.

Ensuite, le composant angulaire sera créé. Ouvrez le module d’application **** angulaire (`<src>/aem-sample-we-retail-journal/angular-app`) à l’aide de l’éditeur de votre choix.

1. Dans le `angular-app` dossier, accédez à son `src` dossier. Développez le dossier de composants pour vue des fichiers de composants angulaires existants.

   ![Structure de fichier angulaire](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

2. ajoutez un nouveau dossier sous le dossier de composants nommé `helloworld`. Sous le `helloworld` dossier, ajoutez de nouveaux fichiers nommés `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

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

3. Ouvrez `helloworld.component.ts`. ajoutez une instruction import pour importer les classes Angular `Component` et `Input` classes. Créez un nouveau composant, en pointant le `styleUrls` et `templateUrl` sur `helloworld.component.css` et `helloworld.component.html`. Enfin, exportez la classe `HelloWorldComponent` avec l&#39;entrée attendue de `displayMessage`.

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
   > Si vous vous souvenez de la méthode [!DNL Sling Model] créée précédemment, il y avait une méthode **getDisplayMessage()**. Le JSON sérialisé de cette méthode sera **displayMessage**, que nous lisons maintenant dans l’application Angular.

4. Ouvrez `helloworld.component.html` pour inclure une `h1` balise qui imprime la `displayMessage` propriété :

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

5. Mettez à jour `helloworld.component.css` pour inclure certains styles de base pour le composant.

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

6. Mettez à jour `helloworld.component.spec.ts` avec le banc d&#39;essai suivant :

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

7. Prochaine mise à jour `src/components/mapping.ts` pour inclure le `HelloWorldComponent`. ajoutez une balise `HelloWorldEditConfig` qui marquera l’espace réservé dans l’éditeur AEM avant que le composant n’ait été configuré. Enfin, ajoutez une ligne pour mapper le composant AEM au composant Angular avec l’ `MapTo` assistant.

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

   Le code complet de [**mapping.ts** se trouve ici.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

8. Mettre à jour `src/app.module.ts` pour mettre à jour le **NgModule**. ajoutez la **`HelloWorldComponent`** comme une **déclaration** qui appartient à **AppModule**. Ajoutez également le `HelloWorldComponent` en tant que **entryComponent** afin qu’il soit compilé et dynamiquement inclus dans l’application au fur et à mesure que le modèle JSON est traité.

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

   Le code complet pour [**app.module.ts** se trouve ici.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

9. Déployez le code vers AEM à l’aide de Maven :

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

10. Dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) , ouvrez `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. Effectuez une recherche rapide de **HelloWorld** dans `main.js` pour vérifier que le composant Angular a été inclus.

   >[!NOTE]
   >
   > **main.js** est l’application Angular intégrée. Le code n&#39;est plus lisible par l&#39;homme. La commande npm run build a déclenché une compilation optimisée qui génère du code JavaScript compilé qui peut être interprété par les navigateurs modernes.

## Mise à jour du modèle {#template-update}

1. Accédez au modèle modifiable pour les versions de réaction et/ou angulaire :

   * (angulaire) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (Réaction) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

2. Sélectionnez le Conteneur  de mise en page principal et sélectionnez l’icône [!UICONTROL Stratégie] pour ouvrir sa stratégie :

   ![Sélectionner la stratégie de mise en page](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   Sous **[!UICONTROL Propriétés]** > Composants **** autorisés, recherchez **[!DNL Custom Components]**. Vous devriez voir le **[!DNL Hello World]** composant, le sélectionner. Enregistrez vos modifications en cochant la case située dans le coin supérieur droit.

   ![Configuration de la stratégie de Conteneur de mise en page](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

3. Après l’enregistrement, vous devriez voir le **[!DNL HelloWorld]** composant comme un composant autorisé dans le Conteneur [!UICONTROL de]mise en page.

   ![Composants autorisés mis à jour](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > Seuls AEM 6.5 et AEM 6.4.5 prennent en charge la fonction Modèle modifiable de l’éditeur d’applications monopages. Si vous utilisez AEM 6.4, vous devez configurer manuellement la stratégie pour les composants autorisés par le biais du CRXDE Lite : `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` ou `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite présentant les configurations de stratégie mises à jour pour les composants  autorisés dans le Conteneur [!UICONTROL de]mise en page :

   ![CRXDE Lite présentant les configurations de stratégie mises à jour pour les composants autorisés dans le Conteneur de mise en page](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## Assemblage {#putting-together}

1. Accédez aux pages Angular ou React :

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

2. Recherchez le **[!DNL Hello World]** composant et faites-le glisser et déposez-le **[!DNL Hello World]** sur la page.

   ![salut monde drag + drop](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   L’espace réservé doit apparaître.

   ![Bonjour, titulaire de la place dans le monde](assets/spa-editor-helloworld-tutorial-use/fig10.png)

3. Sélectionnez le composant et ajoutez un message dans la boîte de dialogue, par exemple &quot;Monde&quot; ou &quot;Votre nom&quot;. Enregistrez les modifications.

   ![composant rendu](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   Notez que la chaîne &quot;Hello&quot; est toujours précédée du message. C&#39;est le résultat de la logique dans le `HelloWorld.java`[!DNL Sling Model].

## Étapes suivantes {#next-steps}

[Solution terminée pour le composant HelloWorld](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* Code source complet pour [[!DNL We.Retail Journal] GitHub](https://github.com/adobe/aem-sample-we-retail-journal)
* Consultez un didacticiel plus détaillé sur le développement de React with [[ ! DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## Résolution des incidents {#troubleshooting}

### Impossible de créer le projet dans Eclipse {#unable-to-build-project-in-eclipse}

**Erreur :** Erreur lors de l’importation du [!DNL We.Retail Journal] projet dans Eclipse pour les exécutions d’objectifs non reconnues :

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![assistant d&#39;erreur eclipse](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**Résolution**: Cliquez sur Terminer pour les résoudre ultérieurement. Cela ne doit pas empêcher l&#39;achèvement du tutoriel.

**Erreur**: Le module React `react-app`ne se construit pas correctement lors d&#39;une génération Maven.

**Résolution :** Essayez de supprimer le `node_modules` dossier situé sous l&#39;application **réagir**. Réexécutez la commande Apache Maven `mvn  clean install -PautoInstallSinglePackage` à partir de la racine du projet.

### Dépendances insatisfaites dans AEM {#unsatisfied-dependencies-in-aem}

![Erreur de dépendance du gestionnaire de packages](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

Si une dépendance AEM n’est pas satisfaite, que ce soit dans le gestionnaire de packages **** AEM ou dans la console **[!UICONTROL Web]** AEM (console Felix), cela indique que la fonction de l’éditeur d’applications monopages n’est pas disponible.

### Le composant ne s&#39;affiche pas

**Erreur**: Même après un déploiement réussi et la vérification que les versions compilées des applications React/Angular comportent le `helloworld` composant mis à jour, mon composant ne s’affiche pas lorsque je le fais glisser sur la page. Je peux voir le composant dans l’interface utilisateur AEM.

**Résolution**: Effacez l&#39;historique/le cache de votre navigateur et/ou ouvrez un nouveau navigateur ou utilisez le mode Incognito. Si cela ne fonctionne pas, invalidez le cache de bibliothèque client sur l’instance d’AEM locale. aem tente de mettre en cache de grandes bibliothèques clientes afin d&#39;être efficace. Il peut arriver que l’invalidation manuelle du cache soit nécessaire pour résoudre les problèmes de mise en cache du code obsolète.

Accédez à : [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) et cliquez sur Invalider le cache. Revenez à votre page Réagir/Angulaire et actualisez la page.

![Reconstruire la bibliothèque cliente](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
