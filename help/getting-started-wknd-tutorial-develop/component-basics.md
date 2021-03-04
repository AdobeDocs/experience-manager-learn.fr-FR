---
title: Prise en main de AEM Sites - Concepts de base des composants
description: Comprendre la technologie sous-jacente d'un composant de sites Adobe Experience Manager (AEM) à travers un simple exemple "HelloWorld". Des sujets tels que HTL, les modèles Sling, les bibliothèques côté client et les boîtes de dialogue d’auteur sont explorés.
sub-product: sites
feature: '"Composants principaux, outils de développement"'
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
topic: '"Gestion de contenu, développement"'
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 4%

---


# Notions de base des composants {#component-basics}

Dans ce chapitre, nous étudierons la technologie sous-jacente d&#39;un composant Sites Adobe Experience Manager (AEM) à l&#39;aide d&#39;un simple `HelloWorld` exemple. De petites modifications seront apportées à un composant existant, couvrant des sujets de création, HTL, modèles Sling, bibliothèques côté client.

## Conditions préalables {#prerequisites}

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

L&#39;IDE utilisé dans les vidéos est [Code Visual Studio](https://code.visualstudio.com/) et le module externe [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync).

## Intention {#objective}

1. Découvrez le rôle des modèles HTML et des modèles Sling pour effectuer un rendu dynamique du code HTML.
1. Comprendre comment les boîtes de dialogue sont utilisées pour faciliter la création de contenu.
1. Découvrez les principes de base des bibliothèques côté client pour inclure CSS et JavaScript afin de prendre en charge un composant.

## Ce que vous allez créer {#what-you-will-build}

Dans ce chapitre, vous allez effectuer plusieurs modifications à un composant très simple `HelloWorld`. Dans le processus de mise à jour du composant `HelloWorld`, vous découvrirez les principaux domaines du développement des composants AEM.

## Projet de démarrage de chapitre {#starter-project}

Ce chapitre s&#39;appuie sur un projet générique généré par l&#39;[AEM Archétype de projet](https://github.com/adobe/aem-project-archetype). Regardez la vidéo ci-dessous et passez en revue les [conditions préalables](#prerequisites) pour commencer !

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes permettant d&#39;extraire le projet de démarrage.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Ouvrez un nouveau terminal de ligne de commande et effectuez les actions suivantes.

1. Dans un répertoire vide, cloner le référentiel [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Vous pouvez éventuellement continuer à utiliser le projet généré dans le chapitre précédent, [Configuration du projet](./project-setup.md).

1. Accédez au dossier `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Créez et déployez le projet sur une instance locale d’AEM avec la commande suivante :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` aux commandes Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importez le projet dans votre environnement IDE préféré en suivant les instructions pour configurer un [environnement de développement local](overview.md#local-dev-environment).

## Création de composant {#component-authoring}

Les composants peuvent être considérés comme de petits blocs de construction modulaires d’une page Web. Pour réutiliser les composants, ceux-ci doivent pouvoir être configurés. Pour ce faire, ouvrez la boîte de dialogue de l’auteur. Ensuite, nous allons créer un composant simple et examiner comment les valeurs de la boîte de dialogue sont conservées en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. Créez une nouvelle page nommée **Concepts de base des composants** sous **Site WKND** `>` **US** `>` **en**.
1. Ajoutez le **composant Hello World** sur la page nouvellement créée.
1. Ouvrez la boîte de dialogue du composant et saisissez du texte. Enregistrez les modifications pour afficher le message affiché sur la page.
1. Passez en mode développeur et vue du chemin d’accès au contenu dans CRXDE-Lite et examinez les propriétés de l’instance de composant.
1. Utilisez CRXDE-Lite pour vue des scripts `cq:dialog` et `helloworld.html` situés à `/apps/wknd/components/content/helloworld`.

## HTML (HTML Template Language) et boîtes de dialogue {#htl-dialogs}

Le langage de modèle HTML ou **[HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/getting-started/getting-started.html)** est un langage de modèle côté serveur et à poids léger utilisé par les composants AEM pour générer le contenu.

**Les** boîtes de dialogue définissent les configurations disponibles qui peuvent être créées pour un composant.

Ensuite, nous allons mettre à jour le script HTL `HelloWorld` pour afficher un message de bienvenue supplémentaire avant le message texte.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. Accédez à l&#39;IDE et ouvrez le projet dans le module `ui.apps`.
1. Ouvrez le fichier `helloworld.html` et apportez une modification au balisage HTML.
1. Utilisez les outils IDE tels que [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) pour synchroniser la modification du fichier avec l’instance AEM locale.
1. Revenez au navigateur et observez que le rendu du composant a changé.
1. Ouvrez le fichier `.content.xml` qui définit la boîte de dialogue du composant `HelloWorld` à l&#39;adresse suivante :

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

1. Ouvrez de nouveau le fichier `helloworld.html`, qui représente le script HTL principal responsable du rendu du composant `HelloWorld`, situé à l’emplacement suivant :

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Mettez à jour `helloworld.html` pour afficher la valeur du champ de texte **Salutation** dans le cadre d’une balise `H1` :

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module de développement ou en utilisant vos compétences Maven.

## Modèles Sling {#sling-models}

Les modèles Sling sont des &quot;POJO&quot; Java pilotés par les annotations (objets Java ordinaires) qui facilitent la mise en correspondance des données du JCR avec les variables Java et fournissent un certain nombre d&#39;autres subtilités lors du développement dans le contexte de l&#39;AEM.

Ensuite, nous apporterons quelques mises à jour au modèle Sling `HelloWorldModel` afin d’appliquer une logique métier aux valeurs stockées dans le JCR avant de les envoyer à la page.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Ouvrez le fichier `HelloWorldModel.java`, qui est le modèle Sling utilisé avec le composant `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Ajoutez les instructions d&#39;importation suivantes :

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Mettez à jour l&#39;annotation `@Model` pour utiliser une `DefaultInjectionStrategy` :

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

1. Ajoutez la méthode suivante `getTitle()` à la classe `HelloWorldModel`, qui renvoie la valeur de la propriété nommée `title`. Cette méthode ajoute une logique supplémentaire pour renvoyer une valeur de chaîne &quot;Default Value here !&quot; (Valeur par défaut ici !). si la propriété `title` est nulle ou vide :

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

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module externe Eclipse Developer ou en utilisant vos compétences Maven.

## Bibliothèques côté client {#client-side-libraries}

Les bibliothèques côté client, clientlibs for short, offrent un mécanisme d’organisation et de gestion des fichiers CSS et JavaScript nécessaires à une mise en oeuvre AEM Sites. Les bibliothèques côté client constituent la méthode standard pour inclure CSS et JavaScript sur une page dans AEM.

Ensuite, nous allons inclure certains styles CSS pour le composant `HelloWorld` afin de comprendre les bases des bibliothèques côté client.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. sous `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` créez un nouveau dossier nommé `clientlib-helloworld`.
1. Créez une structure de dossiers et de fichiers semblable à celle qui suit sous `clientlibs`

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

1. Mettez à jour le fichier `clientlib-helloworld/.conten.xml` pour inclure les propriétés suivantes :

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

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module de développement ou en utilisant vos compétences Maven.

   >[!NOTE]
   >
   > Pour des raisons de performances, CSS et JavaScript sont fréquemment mis en cache par le navigateur. Si vous ne voyez pas immédiatement la modification de la bibliothèque cliente, effectuez une actualisation stricte et effacez le cache du navigateur. Il peut s’avérer utile d’utiliser une fenêtre incognito pour assurer un nouveau cache.

## Félicitations! {#congratulations}

Félicitations, vous venez d&#39;apprendre les bases du développement de composants à Adobe Experience Manager !

### Étapes suivantes {#next-steps}

Familiarisez-vous avec les pages et modèles Adobe Experience Manager dans le chapitre suivant [Pages et modèles](pages-templates.md). Découvrez comment les composants principaux sont intégrés au projet et comment des configurations de stratégie avancées de modèles modifiables sont appliquées pour créer un modèle de page d’article bien structuré.

Vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur la branche Git `tutorial/component-basics-solution`.

