---
title: Prise en main d’AEM Sites - Principes de base des composants
description: Découvrez la technologie sous-jacente d’un composant Sites Adobe Experience Manager (AEM) à l’aide d’un simple exemple "HelloWorld". Des rubriques sur HTL, les modèles Sling, les bibliothèques côté client et les boîtes de dialogue de création sont explorées.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Composants principaux, outils de développement
topic: Gestion de contenu, développement
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
source-git-commit: 32320905786682a852baf7d777cb06de0072c439
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 4%

---


# Notions de base des composants {#component-basics}

Dans ce chapitre, nous allons explorer la technologie sous-jacente d’un composant Sites Adobe Experience Manager (AEM) par le biais d’un simple exemple `HelloWorld`. De petites modifications seront apportées à un composant existant, couvrant les rubriques de création, HTL, modèles Sling, bibliothèques côté client.

## Prérequis {#prerequisites}

Examinez les outils et instructions requis pour configurer un [environnement de développement local](./overview.md#local-dev-environment).

L’IDE utilisé dans les vidéos est [Visual Studio Code](https://code.visualstudio.com/) et le module externe [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) .

## Intention {#objective}

1. Découvrez le rôle des modèles HTL et des modèles Sling pour effectuer dynamiquement le rendu HTML.
1. Découvrez comment les boîtes de dialogue sont utilisées pour faciliter la création de contenu.
1. Découvrez les principes de base des bibliothèques côté client pour inclure CSS et JavaScript afin de prendre en charge un composant.

## Ce que vous allez créer {#what-you-will-build}

Dans ce chapitre, vous allez effectuer plusieurs modifications sur un composant `HelloWorld` très simple. Au cours du processus de mise à jour du composant `HelloWorld`, vous découvrirez les principaux domaines du développement des composants d’AEM.

## Projet de démarrage de chapitre {#starter-project}

Ce chapitre s’appuie sur un projet générique généré par l’[archétype de projet AEM](https://github.com/adobe/aem-project-archetype). Regardez la vidéo ci-dessous et passez en revue les [conditions préalables](#prerequisites) pour commencer !

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes d’extraction du projet de démarrage.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Ouvrez un nouveau terminal de ligne de commande et effectuez les actions suivantes.

1. Dans un répertoire vide, clonez le référentiel [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Vous pouvez éventuellement continuer à utiliser le projet généré dans le chapitre précédent, [Configuration du projet](./project-setup.md).

1. Accédez au dossier `aem-guides-wknd` .

   ```shell
   $ cd aem-guides-wknd
   ```

1. Créez et déployez le projet sur une instance locale d’AEM avec la commande suivante :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` à toute commande Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importez le projet dans l’IDE de votre choix en suivant les instructions pour configurer un [environnement de développement local](overview.md#local-dev-environment).

## Création de composants {#component-authoring}

Les composants peuvent être considérés comme de petits blocs de création modulaires d’une page web. Pour réutiliser les composants, ceux-ci doivent pouvoir être configurés. Pour ce faire, utilisez la boîte de dialogue de création. Ensuite, nous allons créer un composant simple et examiner comment les valeurs de la boîte de dialogue sont conservées dans AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. Créez une page nommée **Éléments de base** sous **Site WKND** `>` **US** `>` **en**.
1. Ajoutez le **composant Hello World** à la page nouvellement créée.
1. Ouvrez la boîte de dialogue du composant et saisissez du texte. Enregistrez les modifications pour afficher le message affiché sur la page.
1. Passez en mode Développeur, affichez le chemin d’accès au contenu dans CRXDE-Lite et examinez les propriétés de l’instance de composant.
1. Utilisez CRXDE-Lite pour afficher le script `cq:dialog` et `helloworld.html` situé à l’emplacement `/apps/wknd/components/content/helloworld`.

## HTL (HTML Template Language) et boîtes de dialogue {#htl-dialogs}

Le langage de modèle HTML ou **[HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/getting-started/getting-started.html)** est un langage de modèle léger côté serveur utilisé par les composants d’AEM pour effectuer le rendu du contenu.

**** Les boîtes de dialogue définissent les configurations disponibles pouvant être créées pour un composant.

Nous allons maintenant mettre à jour le script HTL `HelloWorld` afin d’afficher un message de bienvenue supplémentaire avant le message texte.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. Passez à l’IDE et ouvrez le projet dans le module `ui.apps`.
1. Ouvrez le fichier `helloworld.html` et apportez une modification au balisage HTML.
1. Utilisez les outils IDE tels que [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) pour synchroniser le changement de fichier avec l’instance AEM locale.
1. Revenez au navigateur et observez que le rendu du composant a changé.
1. Ouvrez le fichier `.content.xml` qui définit la boîte de dialogue pour le composant `HelloWorld` à l’adresse :

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Mettez à jour la boîte de dialogue pour ajouter un champ de texte supplémentaire nommé **Titre** avec le nom `./title` :

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
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
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

1. Ouvrez à nouveau le fichier `helloworld.html`, qui représente le script HTL principal responsable du rendu du composant `HelloWorld`, situé à l’adresse :

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Mettez à jour `helloworld.html` pour afficher la valeur du champ de texte **Salutations** dans le cadre d’une balise `H1` :

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module externe Développeur ou en utilisant vos compétences Maven.

## Modèles Sling {#sling-models}

Les modèles Sling sont des objets POJO (Plain Old Java Object) Java pilotés par les annotations. Ils facilitent le mappage des données du JCR aux variables Java et fournissent un certain nombre d’autres détails lors du développement dans le contexte d’AEM.

Nous allons ensuite apporter quelques mises à jour au modèle Sling `HelloWorldModel` afin d’appliquer une logique métier aux valeurs stockées dans le JCR avant de les générer sur la page.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Ouvrez le fichier `HelloWorldModel.java`, qui est le modèle Sling utilisé avec le composant `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Ajoutez les instructions d’importation suivantes :

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Mettez à jour l’annotation `@Model` pour utiliser une balise `DefaultInjectionStrategy` :

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Ajoutez les lignes suivantes à la classe `HelloWorldModel` pour mapper les valeurs des propriétés JCR du composant `title` et `text` aux variables Java :

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. Ajoutez la méthode suivante `getTitle()` à la classe `HelloWorldModel`, qui renvoie la valeur de la propriété nommée `title`. Cette méthode ajoute une logique supplémentaire pour renvoyer une valeur de chaîne &quot;Default Value here&quot; (Valeur par défaut ici). si la propriété `title` est nulle ou vide :

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Ajoutez la méthode suivante `getText()` à la classe `HelloWorldModel`, qui renvoie la valeur de la propriété nommée `text`. Cette méthode transforme la chaîne en caractères majuscules.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Créez et déployez le lot à partir du module `core` :

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.4/6.5, utilisez `mvn clean install -PautoInstallBundle -Pclassic`

1. Mettez à jour le fichier `helloworld.html` à `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` pour utiliser les méthodes nouvellement créées du modèle `HelloWorld` :

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld"
   data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module externe de développement Eclipse ou en utilisant vos compétences Maven.

## Bibliothèques côté client {#client-side-libraries}

Les bibliothèques côté client (clientlibs, pour résumer) offrent un mécanisme d’organisation et de gestion des fichiers CSS et JavaScript nécessaires à une mise en oeuvre d’AEM Sites. Les bibliothèques côté client sont la méthode standard pour inclure du code CSS et JavaScript sur une page dans AEM.

Ensuite, nous allons inclure certains styles CSS pour le composant `HelloWorld` afin de comprendre les principes de base des bibliothèques côté client.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. sous `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` créez un dossier nommé `clientlib-helloworld`.
1. Créez une structure de dossiers et de fichiers comme celle-ci sous `clientlibs`

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. Remplissez `helloworld.css` avec les éléments suivants :

   ```css
   .cmp-helloworld .cmp-helloworld__title {
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
   console.log("Hello World from Javascript!");
   ```

1. Remplissez `helloworld/clientlibs/js.txt` avec les éléments suivants :

   ```plain
   #base=js
   helloworld.js
   ```

1. Mettez à jour le fichier `clientlib-helloworld/.content.xml` pour inclure les propriétés suivantes :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. Mettez à jour le fichier `clientlibs/clientlib-base/.content.xml` vers **embed** la catégorie `wknd.helloworld` :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module externe Développeur ou en utilisant vos compétences Maven.

   >[!NOTE]
   >
   > Pour des raisons de performances, CSS et JavaScript sont fréquemment mis en cache par le navigateur. Si vous ne voyez pas immédiatement la modification de la bibliothèque cliente, effectuez une actualisation stricte et effacez le cache du navigateur. Il peut s’avérer utile d’utiliser une fenêtre d’incognito pour garantir un nouveau cache.

## Félicitations !  {#congratulations}

Félicitations, vous venez d’apprendre les bases du développement de composants dans Adobe Experience Manager !

### Étapes suivantes {#next-steps}

Familiarisez-vous avec les pages et les modèles Adobe Experience Manager dans le chapitre suivant [Pages et modèles](pages-templates.md). Comprenez comment les composants principaux sont liés par proxy au projet et découvrez les configurations de stratégie avancées des modèles modifiables afin de créer un modèle de page d’article bien structuré.

Affichez le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur la branche Git `tutorial/component-basics-solution`.
