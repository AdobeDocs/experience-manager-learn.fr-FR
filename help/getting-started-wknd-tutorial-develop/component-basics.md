---
title: Prise en main de AEM Sites - Concepts de base des composants
description: Comprendre la technologie sous-jacente d'un composant de sites Adobe Experience Manager (AEM) à travers un simple exemple "HelloWorld". Des sujets tels que HTL, les modèles Sling, les bibliothèques côté client et les boîtes de dialogue d’auteur sont explorés.
sub-product: sites
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 6%

---


# Notions de base des composants {#component-basics}

Dans ce chapitre, nous étudierons la technologie sous-jacente d&#39;un composant Sites Adobe Experience Manager (AEM) à l&#39;aide d&#39;un simple `HelloWorld` exemple. De petites modifications seront apportées à un composant existant, couvrant des sujets de création, HTL, modèles Sling, bibliothèques côté client.

## Conditions préalables {#prerequisites}

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

## Intention {#objective}

1. Découvrez le rôle des modèles HTML et des modèles Sling pour effectuer un rendu dynamique du code HTML.
1. Comprendre comment les boîtes de dialogue sont utilisées pour faciliter la création de contenu.
1. Découvrez les principes de base des bibliothèques côté client pour inclure CSS et JavaScript afin de prendre en charge un composant.

## Ce que vous allez créer {#what-you-will-build}

Dans ce chapitre, vous allez effectuer plusieurs modifications à un composant très simple `HelloWorld`. Dans le processus de mise à jour du composant `HelloWorld`, vous découvrirez les principaux domaines du développement des composants AEM.

## Projet de démarrage de chapitre {#starter-project}

Ce chapitre s&#39;appuie sur un projet générique généré par l&#39;[AEM Archétype de projet](https://github.com/adobe/aem-project-archetype). Regardez la vidéo ci-dessous et passez en revue les [conditions préalables](#prerequisites) pour commencer !

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

Ouvrez un nouveau terminal de ligne de commande et effectuez les actions suivantes.

1. Dans un répertoire vide, cloner le référentiel [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > Vous pouvez éventuellement télécharger directement la branche [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip).

1. Accédez au répertoire `aem-guides-wknd` :

   ```shell
   $ cd aem-guides-wknd
   ```

1. Accédez à la branche `component-basics/start` :

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. Créez et déployez le projet sur une instance locale d’AEM avec la commande suivante :

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. Importez le projet dans votre environnement IDE préféré en suivant les instructions pour configurer un [environnement de développement local](overview.md#local-dev-environment).

## Création de composant {#component-authoring}

Les composants peuvent être considérés comme de petits blocs de construction modulaires d’une page Web. Pour réutiliser les composants, ceux-ci doivent pouvoir être configurés. Pour ce faire, ouvrez la boîte de dialogue de l’auteur. Ensuite, nous allons créer un composant simple et examiner comment les valeurs de la boîte de dialogue sont conservées en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. Créez une nouvelle page nommée **Concepts de base des composants** sous **Site WKND** `>` **US** `>` **en**.
1. Ajoutez le **composant Hello World** sur la page nouvellement créée.
1. Ouvrez la boîte de dialogue du composant et saisissez du texte. Enregistrez les modifications pour afficher le message affiché sur la page.
1. Passez en mode développeur et vue du chemin d’accès au contenu dans CRXDE-Lite et examinez les propriétés de l’instance de composant.
1. Utilisez CRXDE-Lite pour vue des scripts `cq:dialog` et `helloworld.html` situés à `/apps/wknd/components/content/helloworld`.

## Langage de modèle HTML (HTL) {#htl-templates}

Le langage de modèle HTML ou [HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/getting-started/getting-started.html) est un langage de modèle côté serveur à poids léger utilisé par les composants AEM pour générer le contenu.

Ensuite, nous allons mettre à jour le script HTL `HelloWorld` pour afficher un message de bienvenue supplémentaire avant le message texte.

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. Passez à l&#39;Eclipse IDE et ouvrez le projet dans le module `ui.apps`.
1. Ouvrez le fichier `.content.xml` qui définit la boîte de dialogue du composant `HelloWorld` à l&#39;adresse suivante :

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. Mettez à jour la boîte de dialogue pour ajouter un champ de texte supplémentaire nommé **Salutation** avec le nom `./greeting` :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. Ouvrez le fichier `helloworld.html`, qui représente le script HTL principal responsable du rendu du composant `HelloWorld`, situé à l&#39;emplacement suivant :

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. Mettez à jour `helloworld.html` pour afficher la valeur du champ de texte **Salutation** dans le cadre d’une balise `H1` :

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module externe Eclipse Developer ou en utilisant vos compétences Maven.

## Modèles Sling {#sling-models}

Les modèles Sling sont des &quot;POJO&quot; Java pilotés par les annotations (objets Java ordinaires) qui facilitent la mise en correspondance des données du JCR avec les variables Java et fournissent un certain nombre d&#39;autres subtilités lors du développement dans le contexte de l&#39;AEM.

Ensuite, nous apporterons quelques mises à jour au modèle Sling `HelloWorldModel` afin d’appliquer une logique métier aux valeurs stockées dans le JCR avant de les envoyer à la page.

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. Ouvrez le fichier `HelloWorldModel.java`, qui est le modèle Sling utilisé avec le composant `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Ajoutez les lignes suivantes à la classe `HelloWorldModel` pour mapper les valeurs des propriétés JCR du composant `greeting` et `text` aux variables Java :

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. Ajoutez la méthode suivante `getGreeting()` à la classe `HelloWorldModel`, qui renvoie la valeur de la propriété nommée `greeting`. Cette méthode ajoute une logique supplémentaire pour renvoyer une valeur String de &quot;Hello&quot; si la propriété `greeting` est nulle ou vide :

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. Ajoutez la méthode suivante `getTextUpperCase()` à la classe `HelloWorldModel`, qui renvoie la valeur de la propriété nommée `text`. Cette méthode transforme la chaîne en caractères majuscules.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Mettez à jour le fichier `helloworld.html` à `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` pour utiliser les méthodes nouvellement créées du modèle `HelloWorld` :

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module externe Eclipse Developer ou en utilisant vos compétences Maven.

## Bibliothèques côté client {#client-side-libraries}

Les bibliothèques côté client, clientlibs for short, offrent un mécanisme d’organisation et de gestion des fichiers CSS et JavaScript nécessaires à une mise en oeuvre AEM Sites. Les bibliothèques côté client constituent la méthode standard pour inclure CSS et JavaScript sur une page dans AEM.

Ensuite, nous allons inclure certains styles CSS pour le composant `HelloWorld` afin de comprendre les bases des bibliothèques côté client.

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. sous `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` créez un nouveau noeud nommé `clientlibs` avec un type de noeud `cq:ClientLibraryFolder`.
1. Créez une structure de dossiers et de fichiers semblable à celle qui suit sous `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. Remplissez `helloworld/clientlibs/css/helloworld.css` avec les éléments suivants :

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. Remplissez `helloworld/clientlibs/css.txt` avec les éléments suivants :

   ```plain
   #base=css
   helloworld.css
   ```

1. Remplissez `helloworld/clientlibs/js/helloworld.js` avec les éléments suivants :

   ```js
   console.log("hello world!");
   ```

1. Remplissez `helloworld/clientlibs/js.txt` avec les éléments suivants :

   ```plain
   #base=js
   helloworld.js
   ```

1. Mettez à jour les propriétés de noeud `clientlibs` pour inclure les deux propriétés suivantes :

   | Nom | Type | Valeur |
   |------|------|-------|
   | categories | Chaîne | wknd.base |
   | allowProxy | Booléen | true |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module externe Eclipse Developer ou en utilisant vos compétences Maven.

## Félicitations! {#congratulations}

Félicitations, vous venez d&#39;apprendre les bases du développement de composants à Adobe Experience Manager !

### Étapes suivantes {#next-steps}

Familiarisez-vous avec les pages et modèles Adobe Experience Manager dans le chapitre suivant [Pages et modèles](pages-templates.md). Découvrez comment les composants principaux sont intégrés au projet et comment des configurations de stratégie avancées de modèles modifiables sont appliquées pour créer un modèle de page d’article bien structuré.

Vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur la brach Git `component-basics/solution`.

1. Cloner le référentiel [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `component-basics/solution`
