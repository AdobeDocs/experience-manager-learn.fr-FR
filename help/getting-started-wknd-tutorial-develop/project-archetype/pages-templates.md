---
title: Prise en main d’AEM Sites - Pages et modèles
seo-title: Prise en main d’AEM Sites - Pages et modèles
description: Découvrez la relation entre un composant de page de base et des modèles modifiables. Comprenez comment les composants principaux sont liés par proxy au projet et découvrez les configurations de stratégie avancées des modèles modifiables afin de créer un modèle de page d’article bien structuré basé sur une maquette d’Adobe XD.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Composants principaux, modèles modifiables, éditeur de page
topic: Gestion de contenu, développement
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '3106'
ht-degree: 1%

---


# Pages et modèles {#pages-and-template}

Dans ce chapitre, nous allons explorer la relation entre un composant de page de base et des modèles modifiables. Nous allons créer un modèle d’article sans style basé sur certaines maquettes de [AdobeXD](https://www.adobe.com/products/xd.html). Lors du processus de création du modèle, les composants principaux et les configurations de stratégie avancées des modèles modifiables sont traités.

## Prérequis {#prerequisites}

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Projet de démarrage

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes d’extraction du projet de démarrage.

Consultez le code de ligne de base sur lequel le tutoriel s’appuie :

1. Extrayez la branche `tutorial/pages-templates-start` à partir de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Déployez la base de code sur une instance d’AEM locale à l’aide de vos compétences Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` à toute commande Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) ou extraire le code localement en passant à la branche `tutorial/pages-templates-solution`.

## Intention

1. Inspect une conception de page créée dans Adobe XD et la mappez aux composants principaux.
1. Découvrez les détails des modèles modifiables et comment les stratégies peuvent être utilisées pour appliquer un contrôle granulaire du contenu de la page.
1. Découvrez comment les modèles et les pages sont liés

## Ce que vous allez créer {#what-you-will-build}

Dans cette partie du tutoriel, vous allez créer un modèle de page d’article qui pourra être utilisé pour créer de nouvelles pages d’article et vous aligner sur une structure commune. Le modèle de page d’article sera basé sur des conceptions et un kit d’interface utilisateur produit dans Adobe XD. Ce chapitre se concentre uniquement sur la construction de la structure ou du squelette du modèle. Aucun style ne sera implémenté, mais le modèle et les pages seront fonctionnels.

![Conception de page d’article et version sans style](assets/pages-templates/what-you-will-build.png)

## Planification de l’interface utilisateur avec Adobe XD {#adobexd}

Dans la plupart des cas, la planification d’un nouveau site web commence par des maquettes et des conceptions statiques. [Adobe ](https://www.adobe.com/products/xd.html) XDest un outil de conception qui crée des expériences utilisateur. Ensuite, nous allons examiner un kit d’interface utilisateur et des maquettes pour planifier la structure du modèle de page d’article.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Téléchargez le  [fichier de conception de l’article WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Un [kit d’interface utilisateur d’AEM composants principaux est également disponible ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) comme point de départ pour les projets personnalisés.

## Création du modèle de page d’article

Lors de la création d’une page, vous devez sélectionner un modèle. C’est la base pour la création de la page. Le modèle définit la structure de la page créée, le contenu initial et les composants autorisés.

Il existe trois zones principales de [Modèles modifiables](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html) :

1. **Structure**  : définit les composants qui font partie du modèle. Ils ne seront pas modifiables par les auteurs de contenu.
1. **Contenu initial**  : définit les composants à partir desquels le modèle commencera. Ils peuvent être modifiés et/ou supprimés par les auteurs de contenu.
1. **Stratégies**  : définit des configurations sur le comportement des composants et les options disponibles pour les auteurs.

Créez ensuite un modèle dans AEM qui correspond à la structure des maquettes. Cela se produit dans une instance locale d’AEM. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

Étapes de haut niveau pour la vidéo ci-dessous :

### Configurations de structure

1. Créez un modèle à l’aide de **Type de modèle de page**, nommé **Page d’article**.
1. Passez en mode **Structure** .
1. Ajoutez un composant **Fragment d’expérience** pour agir comme en-tête **** dans la partie supérieure du modèle.
   * Configurez le composant pour qu’il pointe vers `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Définissez la stratégie sur **En-tête de page** et assurez-vous que l’élément **Par défaut** est défini sur `header`. L’élément `header`sera ciblé avec CSS dans le chapitre suivant.
1. Ajoutez un composant **Fragment d’expérience** pour agir comme **Pied de page** au bas du modèle.
   * Configurez le composant pour qu’il pointe vers `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Définissez la stratégie sur **Pied de page** et assurez-vous que l’élément **Par défaut** est défini sur `footer`. L’élément `footer` sera ciblé avec CSS dans le prochain chapitre.
1. Verrouillez le conteneur **main** qui était inclus lors de la création initiale du modèle.
   * Définissez la stratégie sur **Page principale** et assurez-vous que l’élément **Par défaut** est défini sur `main`. L’élément `main` sera ciblé avec CSS dans le prochain chapitre.
1. Ajoutez un composant **Image** au conteneur **main**.
   * Déverrouillez le composant **Image** .
1. Ajoutez un composant **Chemin de navigation** sous le composant **Image** dans le conteneur principal.
   * Créez une nouvelle stratégie pour le composant **Chemin de navigation** nommé **Page d’article - Chemin de navigation**. Définissez le **Niveau de départ de navigation** sur **4**.
1. Ajoutez un composant **Container** sous le composant **Chemin de navigation** et à l’intérieur du conteneur **main**. Cela agit comme **Conteneur de contenu** pour le modèle.
   * Déverrouillez le conteneur **Contenu**.
   * Définissez la stratégie sur **Contenu de la page**.
1. Ajoutez un autre composant **Container** sous le **Conteneur de contenu**. Cela agit comme conteneur **Rail latéral** pour le modèle.
   * Déverrouillez le conteneur **Rail latéral** .
   * Créez une nouvelle stratégie nommée **Page d’article - Rail latéral**.
   * Configurez les **composants autorisés** sous **Projet de sites WKND - Contenu** pour inclure : **Bouton**, **Télécharger**, **Image**, **Liste**, **Séparateur**, **Partage sur les réseaux sociaux**, **Texte** et **Titre**.
1. Mettez à jour la stratégie du conteneur Racine de page. Il s’agit du conteneur le plus à l’extérieur du modèle. Définissez la stratégie sur **Racine de la page**.
   * Sous **Paramètres du conteneur**, définissez la **mise en page** sur **Grille réactive**.
1. Activez le mode Mise en page pour le **conteneur de contenu**. Faites glisser la poignée de droite à gauche et rétrécissez le conteneur pour qu’il s’étende à 8 colonnes.
1. Activez le mode Mise en page pour le conteneur **Rail latéral**. Faites glisser la poignée de droite à gauche et rétrécissez le conteneur pour obtenir 4 colonnes de large. Faites ensuite glisser la poignée de gauche vers la colonne 1 droite pour large le conteneur 3 colonnes et laisser un espace de 1 colonne entre le **conteneur de contenu**.
1. Ouvrez l’émulateur mobile et passez à un point d’arrêt mobile. Réactivez le mode de mise en page et attribuez à **Conteneur de contenu** et au **Conteneur de rail latéral** la largeur complète de la page. Les conteneurs seront ainsi empilés verticalement dans le point d’arrêt mobile.
1. Mettez à jour la stratégie du composant **Texte** dans le **Conteneur de contenu**.
   * Définissez la stratégie sur **Texte du contenu**.
   * Sous **Plugins** > **Styles de paragraphe**, cochez **Activer les styles de paragraphe** et assurez-vous que le **bloc de citations** est activé.

### Configurations initiales du contenu

1. Passez en mode **Contenu initial** .
1. Ajoutez un composant **Titre** au **Conteneur de contenu**. Cela agit comme titre de l’article. Lorsqu’il est vide, il affiche automatiquement le titre de la page active.
1. Ajoutez un second composant **Titre** sous le premier composant Titre.
   * Configurez le composant avec le texte : &quot;Par auteur&quot;. Il s’agit d’un espace réservé de texte.
   * Définissez le type sur `H4`.
1. Ajoutez un composant **Texte** sous le composant **Par auteur** Titre .
1. Ajoutez un composant **Titre** au **conteneur de rail latéral**.
   * Configurez le composant avec le texte : &quot;Partagez cette histoire&quot;.
   * Définissez le type sur `H5`.
1. Ajoutez un composant **Partage sur les médias sociaux** sous le composant **Partager cet article** titre.
1. Ajoutez un composant **Séparateur** sous le composant **Partage sur les réseaux sociaux** .
1. Ajoutez un composant **Télécharger** sous le composant **Séparateur** .
1. Ajoutez un composant **Liste** sous le composant **Télécharger** .
1. Mettez à jour les **Propriétés de page initiales** du modèle.
   * Sous **Médias sociaux** > **Partage sur les réseaux sociaux**, cochez **Facebook** et **Pinterest**

### Activation du modèle et ajout d’une miniature

1. Affichez le modèle dans la console Modèle en accédant à [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **** Activez le modèle Page d’article .
1. Modifiez les propriétés du modèle Page d’article et téléchargez la miniature suivante afin d’identifier rapidement les pages créées à l’aide du modèle Page d’article :

   ![Miniature du modèle de page d’article](assets/pages-templates/article-page-template-thumbnail.png)

## Mise à jour de l’en-tête et du pied de page avec les fragments d’expérience {#experience-fragments}

Une pratique courante lors de la création de contenu global, tel qu’un en-tête ou un pied de page, consiste à utiliser un [fragment d’expérience](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Les fragments d’expérience permettent aux utilisateurs de combiner plusieurs composants afin de créer un seul composant pouvant faire référence. Les fragments d’expérience ont l’avantage de prendre en charge la gestion multisite et la [localisation](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

L’archétype de projet AEM a généré un en-tête et un pied de page. Ensuite, mettez à jour les fragments d’expérience pour qu’ils correspondent aux maquettes. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Étapes de haut niveau pour la vidéo ci-dessous :

1. Téléchargez l’exemple de module de contenu **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**.
1. Téléchargez et installez le module de contenu à l’aide de Package Manager à l’adresse [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Mettez à jour le modèle de variation web, qui est le modèle utilisé pour les fragments d’expérience à l’adresse [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Mettez à jour la stratégie du composant **Container** sur le modèle.
   * Définissez la stratégie sur **racine XF**.
   * Sous **Composants autorisés**, sélectionnez le groupe de composants **Projet de sites WKND - Structure** pour inclure les composants **Navigation par langue**, **Navigation** et **Recherche rapide**.

### Mise à jour du fragment d’expérience d’en-tête

1. Ouvrez le fragment d’expérience qui effectue le rendu de l’en-tête à l’adresse [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configurez la racine **Conteneur** du fragment. Il s’agit de l’extérieur le plus **Container**.
   * Définissez la **mise en page** sur **grille réactive**.
1. Ajoutez le **Logo WKND Dark** en tant qu’image dans la partie supérieure du **Container**. Le logo a été inclus dans le package installé lors d’une étape précédente.
   * Modifiez la mise en page du **logo sombre WKND** pour qu’il corresponde à la largeur des colonnes **2**. Faites glisser les poignées de droite à gauche.
   * Configurez le logo avec **Texte de remplacement** de &quot;Logo WKND&quot;.
   * Configurez le logo sur **Lien** vers `/content/wknd/us/en` la page d’accueil.
1. Configurez le composant **Navigation** qui est déjà placé sur la page.
   * Définissez **Exclure les niveaux racine** sur **1**.
   * Définissez la **Profondeur de la structure de navigation** sur **1**.
   * Modifiez la mise en page du composant **Navigation** pour qu’il soit à l’échelle des colonnes **8**. Faites glisser les poignées de droite à gauche.
1. Supprimez le composant **Navigation par langue** .
1. Modifiez la mise en page du composant **Recherche** pour qu’il soit à l’échelle des colonnes **2**. Faites glisser les poignées de droite à gauche. Tous les composants doivent maintenant être alignés horizontalement sur une seule ligne.

### Mise à jour du fragment d’expérience de pied de page

1. Ouvrez le fragment d’expérience qui effectue le rendu du pied de page à l’adresse [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configurez la racine **Conteneur** du fragment. Il s’agit de l’extérieur le plus **Container**.
   * Définissez la **mise en page** sur **grille réactive**.
1. Ajoutez le **Logo WKND Light** en tant qu’image dans la partie supérieure du **Container**. Le logo a été inclus dans le package installé lors d’une étape précédente.
   * Modifiez la mise en page du **Logo léger WKND** pour qu’il corresponde à la largeur des colonnes **2**. Faites glisser les poignées de droite à gauche.
   * Configurez le logo avec **Texte de remplacement** de &quot;WKND Logo Light&quot;.
   * Configurez le logo sur **Lien** vers `/content/wknd/us/en` la page d’accueil.
1. Ajoutez un composant **Navigation** sous le logo. Configurez le composant **Navigation** :
   * Définissez **Exclure les niveaux racine** sur **1**.
   * Décochez **Collecter toutes les pages enfants**.
   * Définissez la **Profondeur de la structure de navigation** sur **1**.
   * Modifiez la mise en page du composant **Navigation** pour qu’il soit à l’échelle des colonnes **8**. Faites glisser les poignées de droite à gauche.

## Création d’une page d’article

Créez ensuite une page à l’aide du modèle Page d’article . Créez le contenu de la page pour qu’il corresponde aux maquettes du site. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

Étapes de haut niveau pour la vidéo ci-dessous :

1. Accédez à la console Sites à l’adresse [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Créez une nouvelle page sous **WKND** > **US** > **FR** > **Magazine**.
   * Sélectionnez le modèle **Page d’article** .
   * Sous **Propriétés** définissez le **Titre** sur &quot;Guide ultime pour les skateparks&quot;.
   * Définissez le **nom** sur &quot;guide-la-skateparks&quot;.
1. Remplacez **Par auteur** Titre par le texte &quot;Par Stacey Roswells&quot;.
1. Mettez à jour le composant **Texte** pour inclure un paragraphe afin de renseigner l’article. Vous pouvez utiliser le fichier texte suivant comme copie : [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Ajoutez un autre composant **Texte**.
   * Mettez à jour le composant pour inclure le guillemet : &quot;Il n&#39;y a pas de meilleur endroit pour se raser que Los Angeles.&quot;
   * Modifiez l’éditeur de texte enrichi en mode plein écran et modifiez le guillemet ci-dessus pour utiliser l’élément **Bloc de citations** .
1. Continuez à remplir le corps de l’article pour qu’il corresponde aux maquettes.
1. Configurez le composant **Télécharger** pour utiliser une version PDF de l’article.
   * Sous **Télécharger** > **Propriétés**, cochez la case **Obtenir le titre de la ressource DAM**.
   * Définissez la **Description** sur : &quot;Obtenez l&#39;histoire complète&quot;.
   * Définissez le **texte d’action** sur : &quot;Télécharger le PDF&quot;.
1. Configurez le composant **Liste**.
   * Sous **Paramètres de liste** > **Créer la liste à l’aide de**, sélectionnez **Pages enfants**.
   * Définissez la **page parente** sur `/content/wknd/us/en/magazine`.
   * Sous **Paramètres d’élément** cochez **Lier les éléments** et cochez **Afficher la date**.

## Inspect de la structure de noeud {#node-structure}

A ce stade, la page de l&#39;article est clairement déstylisée. Cependant, la structure de base est en place. Ensuite, examinez la structure de noeud de la page de l’article pour mieux comprendre le rôle du modèle, de la page et des composants.

Utilisez l’outil CRXDE-Lite sur une instance d’AEM locale pour afficher la structure de noeud sous-jacente.

1. Ouvrez [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) et utilisez la navigation dans l’arborescence pour accéder à `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Cliquez sur le noeud `jcr:content` sous la page `la-skateparks` et affichez les propriétés :

   ![Propriétés JCR Content](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Notez la valeur de `cq:template`, qui pointe vers `/conf/wknd/settings/wcm/templates/article-page`, le modèle de page d’article que nous avons créé précédemment.

   Notez également la valeur de `sling:resourceType`, qui pointe vers `wknd/components/page`. Il s’agit du composant de page créé par l’archétype de projet AEM et responsable du rendu de la page en fonction du modèle.

1. Développez le noeud `jcr:content` sous `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` et affichez la hiérarchie des noeuds :

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Vous devriez être en mesure de mapper vaguement chacun des noeuds aux composants qui ont été créés. Vérifiez si vous pouvez identifier les différents conteneurs de mise en page utilisés en examinant les noeuds dotés du préfixe `container`.

1. Examinez ensuite le composant de page à l’adresse `/apps/wknd/components/page`. Affichez les propriétés du composant dans CRXDE Lite :

   ![Propriétés du composant Page](assets/pages-templates/page-component-properties.png)

   Notez qu’il n’existe que 2 scripts HTL, `customfooterlibs.html` et `customheaderlibs.html` sous le composant de page. *Alors, comment ce composant effectue-t-il le rendu de la page ?*

   La propriété `sling:resourceSuperType` pointe vers `core/wcm/components/page/v2/page`. Cette propriété permet au composant de page de WKND d’hériter de **all** des fonctionnalités du composant de page des composants principaux. Il s’agit du premier exemple d’un élément appelé [Modèle de composant proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Vous trouverez plus d’informations[ ici.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect d’un autre composant dans les composants WKND, le composant `Breadcrumb` situé à l’adresse : `/apps/wknd/components/breadcrumb`. Notez que la même propriété `sling:resourceSuperType` est disponible, mais qu’elle pointe cette fois vers `core/wcm/components/breadcrumb/v2/breadcrumb`. Voici un autre exemple d’utilisation du modèle de composant proxy pour inclure un composant principal. En fait, tous les composants de la base de code WKND sont des proxies des composants principaux d’AEM (à l’exception de notre célèbre composant HelloWorld). Il est recommandé d’essayer de réutiliser autant de fonctionnalités que possible des composants principaux *avant* d’écrire du code personnalisé.

1. Examinez ensuite la page des composants principaux à l’adresse `/libs/core/wcm/components/page/v2/page` à l’aide de CRXDE Lite :

   >[!NOTE]
   >
   > Dans AEM 6.5/6.4, les composants principaux se trouvent sous `/apps/core/wcm/components`. Dans AEM en tant que Cloud Service, les composants principaux se trouvent sous `/libs` et sont mis à jour automatiquement.

   ![Page des composants principaux](assets/pages-templates/core-page-component-properties.png)

   Notez que de nombreux autres scripts sont inclus sous cette page. La page des composants principaux contient de nombreuses fonctionnalités. Cette fonctionnalité est divisée en plusieurs scripts pour faciliter la maintenance et la lisibilité. Vous pouvez suivre l’inclusion des scripts HTL en ouvrant la balise `page.html` et en recherchant `data-sly-include` :

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   L’autre raison de diviser le HTL en plusieurs scripts est de permettre aux composants proxy de remplacer des scripts individuels pour implémenter une logique métier personnalisée. Les scripts HTL, `customfooterlibs.html` et `customheaderlibs.html`, sont créés dans un but explicite à remplacer par l’implémentation de projets.

   Pour en savoir plus sur la manière dont le modèle modifiable prend en compte le rendu de la page de contenu [en lisant cet article](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect est un autre composant principal, comme le chemin de navigation à l’adresse `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Affichez le script `breadcrumb.html` pour comprendre comment le balisage du composant de chemin de navigation est finalement généré.

## Enregistrement des configurations dans le contrôle de code source {#configuration-persistence}

Dans de nombreux cas, en particulier au début d’un projet AEM, il est utile de conserver les configurations, comme les modèles et les stratégies de contenu associées, pour le contrôle de code source. Cela garantit que tous les développeurs travaillent sur le même ensemble de contenu et de configurations et peut garantir une cohérence supplémentaire entre les environnements. Une fois qu’un projet atteint un certain niveau de maturité, la pratique de gestion des modèles peut être transmise à un groupe spécial d’utilisateurs expérimentés.

Pour l’instant, nous allons traiter les modèles comme d’autres éléments de code et synchroniser le **modèle de page d’article** dans le cadre du projet. Jusqu’à présent, nous avons **envoyé le code** de notre projet AEM vers une instance locale d’AEM. Le **modèle de page d’article** a été créé directement sur une instance locale d’AEM. Nous devons donc **importer** le modèle dans notre projet AEM. Le module **ui.content** est inclus dans le projet AEM à cet effet spécifique.

Les étapes suivantes s’effectuent à l’aide de l’IDE VSCode à l’aide du module externe [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) , mais cela peut se faire à l’aide de n’importe quel IDE que vous avez configuré sur **import** ou à partir d’une instance locale d’AEM.

1. Dans le VSCode, ouvrez le projet `aem-guides-wknd` .

1. Développez le module **ui.content** dans l’explorateur de projets. Développez le dossier `src` et accédez à `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Cliquez avec le bouton droit de la souris ] sur le  `templates` dossier et sélectionnez  **Importer depuis AEM serveur** :

   ![Modèle d’import VSCode](assets/pages-templates/vscode-import-templates.png)

   Les `article-page` doivent être importés et les modèles `page-content`, `xf-web-variation` doivent également être mis à jour.

   ![Modèles mis à jour](assets/pages-templates/updated-templates.png)

1. Répétez les étapes pour importer du contenu, mais sélectionnez le dossier **policies** situé à l’emplacement `/conf/wknd/settings/wcm/policies`.

   ![Stratégies d’importation VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect le fichier `filter.xml` situé à l’emplacement `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   Le fichier `filter.xml` est chargé d’identifier les chemins d’accès des noeuds qui seront installés avec le package. Notez la balise `mode="merge"` de chacun des filtres qui indique que le contenu existant ne sera pas modifié, seul le nouveau contenu est ajouté. Comme les auteurs de contenu peuvent mettre à jour ces chemins, il est important qu’un déploiement de code remplace **et non** le contenu. Pour plus d’informations sur l’utilisation des éléments de filtre, voir la [documentation FileVault](https://jackrabbit.apache.org/filevault/filter.html) .

   Comparez `ui.content/src/main/content/META-INF/vault/filter.xml` et `ui.apps/src/main/content/META-INF/vault/filter.xml` pour comprendre les différents noeuds gérés par chaque module.

   >[!WARNING]
   >
   > Afin d’assurer des déploiements cohérents pour le site de référence WKND, certaines branches du projet sont configurées de sorte que `ui.content` écrasent toute modification dans le JCR. C’est par conception, c’est-à-dire pour les branches de solution, puisque le code/les styles seront écrits pour des stratégies spécifiques.

## Félicitations !  {#congratulations}

Félicitations, vous venez de créer un modèle et une page avec Adobe Experience Manager Sites.

### Étapes suivantes {#next-steps}

A ce stade, la page de l&#39;article est clairement déstylisée. Suivez le tutoriel [Bibliothèques côté client et Processus front-end](client-side-libraries.md) pour découvrir les bonnes pratiques d’inclusion de CSS et de JavaScript afin d’appliquer des styles globaux au site et d’intégrer un build front-end dédié.

Affichez le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue le code et déployez-le localement sur la branche Git `tutorial/pages-templates-solution`.

1. Cloner le référentiel [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `tutorial/pages-templates-solution` .
