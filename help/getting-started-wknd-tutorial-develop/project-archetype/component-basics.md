---
title: Prise en main d’AEM Sites - Principes de base des composants
description: Découvrez la technologie sous-jacente d’un composant Sites Adobe Experience Manager (AEM) à l’aide d’un simple exemple "HelloWorld". Des rubriques sur HTL, les modèles Sling, les bibliothèques côté client et les boîtes de dialogue de création sont explorées.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
source-git-commit: 68a7f263284fdf9cfcf82572b8e1e1c0c01e4b55
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 3%

---

# Principes de base des composants {#component-basics}

Dans ce chapitre, explorons la technologie sous-jacente d’un composant Sites Adobe Experience Manager (AEM) au moyen d’une `HelloWorld` par exemple. De petites modifications sont apportées à un composant existant, couvrant des rubriques de création, HTL, modèles Sling, bibliothèques côté client.

## Prérequis {#prerequisites}

Examinez les outils et les instructions requis pour configurer une [environnement de développement local](./overview.md#local-dev-environment).

L’IDE utilisé dans les vidéos est : [Visual Studio Code](https://code.visualstudio.com/) et le [Synchronisation des AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) module externe

## Objectif {#objective}

1. Découvrez le rôle des modèles HTL et des modèles Sling pour effectuer dynamiquement le rendu du HTML.
1. Découvrez comment les boîtes de dialogue sont utilisées pour faciliter la création de contenu.
1. Découvrez les principes de base des bibliothèques côté client pour inclure CSS et JavaScript afin de prendre en charge un composant.

## Ce que vous allez construire {#what-build}

Dans ce chapitre, vous apportez plusieurs modifications à une `HelloWorld` composant. Lors de la mise à jour des `HelloWorld` vous découvrez les principaux domaines du développement de composants d’AEM.

## Projet de démarrage de chapitre {#starter-project}

Ce chapitre s’appuie sur un projet générique généré par la fonction [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype). Regardez la vidéo ci-dessous et passez en revue la [conditions préalables](#prerequisites) pour commencer !

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes d’extraction du projet de démarrage.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Ouvrez un nouveau terminal de ligne de commande et effectuez les actions suivantes.

1. Dans un répertoire vide, clonez la variable [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) référentiel :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Vous pouvez éventuellement continuer à utiliser le projet généré dans le chapitre précédent, [Configuration du projet](./project-setup.md).

1. Accédez au  `aem-guides-wknd` dossier.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Créez et déployez le projet sur une instance locale d’AEM avec la commande suivante :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM version 6.5 ou 6.4, ajoutez la variable `classic` profile à n’importe quelle commande Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importez le projet dans l’IDE de votre choix en suivant les instructions pour configurer une [environnement de développement local](overview.md#local-dev-environment).

## Création de composants {#component-authoring}

Les composants peuvent être considérés comme de petits blocs de création modulaires d’une page web. Pour réutiliser des composants, ceux-ci doivent pouvoir être configurés. Pour ce faire, utilisez la boîte de dialogue de création. Créons ensuite un composant simple et examinons comment les valeurs de la boîte de dialogue sont conservées dans AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes générales effectuées dans la vidéo ci-dessus.

1. Créez une page nommée **Principes de base des composants** sous **Site WKND** `>` **US** `>` **en**.
1. Ajoutez la variable **Composant Hello World** sur la page nouvellement créée.
1. Ouvrez la boîte de dialogue du composant et saisissez du texte. Enregistrez les modifications pour afficher le message affiché sur la page.
1. Passez en mode Développeur, affichez le chemin d’accès au contenu dans CRXDE-Lite et examinez les propriétés de l’instance de composant.
1. Utilisez CRXDE-Lite pour afficher la variable `cq:dialog` et `helloworld.html` script à partir de `/apps/wknd/components/content/helloworld`.

## HTL (langage de modèle de HTML) et boîtes de dialogue {#htl-dialogs}

Langue du modèle de HTML ou **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)** est un langage de modèle léger côté serveur utilisé par AEM composants pour effectuer le rendu du contenu.

**Boîtes de dialogue** définissez les configurations disponibles qui peuvent être créées pour un composant.

Nous allons maintenant mettre à jour le `HelloWorld` Script HTL pour afficher un message de bienvenue supplémentaire avant le message texte.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes générales effectuées dans la vidéo ci-dessus.

1. Basculez vers l’IDE et ouvrez le projet sur la `ui.apps` module .
1. Ouvrez le `helloworld.html` et mettez à jour HTML Markup.
1. Utilisez les outils IDE tels que [Synchronisation des AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) pour synchroniser la modification du fichier avec l’instance d’AEM locale.
1. Revenez au navigateur et observez que le rendu du composant a changé.
1. Ouvrez le `.content.xml` fichier qui définit la boîte de dialogue pour la variable `HelloWorld` à l’adresse :

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Mettre à jour la boîte de dialogue pour ajouter un champ de texte supplémentaire nommé **Titre** avec le nom `./title`:

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

1. Réouverture du fichier `helloworld.html`, qui représente le script HTL principal responsable du rendu de la variable `HelloWorld` Composant situé sous le chemin d’accès :

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Mettre à jour `helloworld.html` pour effectuer le rendu de la valeur de la variable **Salutations** textfield dans le cadre d’un `H1` tag :

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Déployez les modifications sur une instance locale d’AEM à l’aide du module externe Développeur ou en utilisant vos compétences Maven.

## Modèles Sling {#sling-models}

Les modèles Sling sont des objets POJO (Plain Old Java™ Object) Java™ pilotés par les annotations qui facilitent le mappage des données du JCR aux variables Java™. Ils apportent aussi plusieurs autres subtilités lorsqu&#39;ils se développent dans le contexte de l&#39;AEM.

Ensuite, mettons à jour les `HelloWorldModel` Modèle Sling afin d’appliquer une logique métier aux valeurs stockées dans le JCR avant de les générer sur la page.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Ouvrir le fichier `HelloWorldModel.java`, qui est le modèle Sling utilisé avec la variable `HelloWorld` composant.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Ajoutez les instructions d’importation suivantes :

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Mettez à jour le `@Model` annotation pour utiliser une `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Ajoutez les lignes suivantes au `HelloWorldModel` pour mapper les valeurs des propriétés JCR du composant. `title` et `text` aux variables Java™ :

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

1. Ajoutez la méthode suivante : `getTitle()` au `HelloWorldModel` qui renvoie la valeur de la propriété nommée `title`. Cette méthode ajoute une logique supplémentaire pour renvoyer une valeur de chaîne &quot;Default Value here&quot; (Valeur par défaut ici). si la propriété `title` est nul ou vide :

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Ajoutez la méthode suivante : `getText()` au `HelloWorldModel` qui renvoie la valeur de la propriété nommée `text`. Cette méthode transforme la chaîne en caractères majuscules.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Créez et déployez le lot à partir de la `core` module :

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Pour AEM 6.4/6.5, utilisez `mvn clean install -PautoInstallBundle -Pclassic`

1. Mettre à jour le fichier `helloworld.html` at `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` pour utiliser les méthodes nouvellement créées de la variable `HelloWorld` model :

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

Bibliothèques côté client, `clientlibs` fournit un mécanisme d’organisation et de gestion des fichiers CSS et JavaScript nécessaires à une mise en oeuvre d’AEM Sites. Les bibliothèques côté client sont la méthode standard pour inclure du code CSS et JavaScript sur une page dans AEM.

Le [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=fr) est un module découplé. [webpack](https://webpack.js.org/) qui est intégré au processus de génération. Cela permet l’utilisation de bibliothèques front-end populaires telles que Sass, LESS et TypeScript. Le `ui.frontend` est étudié plus en détail dans la section [Chapitre Bibliothèques côté client](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md).

Ensuite, mettez à jour les styles CSS pour le `HelloWorld` composant.

>[!VIDEO](https://video.tv.adobe.com/v/340750/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes générales effectuées dans la vidéo ci-dessus.

1. Ouvrez une fenêtre de terminal et accédez au `ui.frontend` directory

1. Être dans `ui.frontend` Exécution du répertoire `npm install npm-run-all --save-dev` pour installer la commande [npm-run-all](https://www.npmjs.com/package/npm-run-all) module Noeud. Cette étape est **requis sur le projet AEM généré par Archetype 39**, dans la version à venir de l’archétype, cela n’est pas obligatoire.

1. Exécutez ensuite le `npm run watch` command :

   ```shell
   $ npm run watch
   ```

1. Basculez vers l’IDE et ouvrez le projet sur la `ui.frontend` module .
1. Ouvrez le fichier `ui.frontend/src/main/webpack/components/_helloworld.scss`.
1. Mettez à jour le fichier pour afficher un titre rouge :

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. Dans le terminal, l’activité qui indique que la variable `ui.frontend` module compile et synchronise les modifications avec l’instance locale d’AEM.

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. Revenez au navigateur et notez que la couleur du titre a changé.

   ![Mise à jour des bases des composants](assets/component-basics/color-update.png)

## Félicitations ! {#congratulations}

Félicitations, vous avez appris les bases du développement de composants dans Adobe Experience Manager !

### Étapes suivantes {#next-steps}

Familiarisez-vous avec les pages et les modèles Adobe Experience Manager dans le chapitre suivant [Pages et modèles](pages-templates.md). Comprenez comment les composants principaux sont liés par proxy au projet et découvrez les configurations de stratégie avancées des modèles modifiables afin de créer un modèle de page d’article bien structuré.

Afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou revoir et déployer le code localement sur la branche Git `tutorial/component-basics-solution`.
