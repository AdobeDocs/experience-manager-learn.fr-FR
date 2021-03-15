---
title: Prise en main de AEM Sites - Pages et modèles
seo-title: Prise en main de AEM Sites - Pages et modèles
description: Découvrez la relation entre un composant de page de base et des modèles modifiables. Comprenez comment les composants principaux sont intégrés au projet et découvrez les configurations de stratégie avancées des modèles modifiables afin de créer un modèle de page d’article bien structuré basé sur une maquette d’Adobe XD.
sub-product: sites
feature: Composants principaux, modèles modifiables
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
topic: gestion de contenu, développement
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: b5b43ae8231bf23e0c53777b1e9c16bcc3fc188a
workflow-type: tm+mt
source-wordcount: '3106'
ht-degree: 1%

---


# Pages et modèles {#pages-and-template}

Ce chapitre porte sur la relation entre un composant de page de base et des modèles modifiables. Nous allons créer un modèle d’article sans style basé sur certaines maquettes de [AdobeXD](https://www.adobe.com/products/xd.html). Dans le processus de création du modèle, les composants principaux et les configurations de stratégie avancées des modèles modifiables sont traités.

## Conditions préalables {#prerequisites}

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Projet de démarrage

>[!NOTE]
>
> Si vous avez terminé avec succès le chapitre précédent, vous pouvez réutiliser le projet et ignorer les étapes permettant d&#39;extraire le projet de démarrage.

Consultez le code de ligne de base sur lequel le didacticiel s&#39;appuie :

1. Consultez la branche `tutorial/pages-templates-start` de [GitHub](https://github.com/adobe/aem-guides-wknd).

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Déployez la base de code sur une instance AEM locale en utilisant vos compétences Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` aux commandes Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) ou vérifier le code localement en passant à la branche `tutorial/pages-templates-solution`.

## Intention

1. Inspect une conception de page créée dans Adobe XD et la mappe aux composants principaux.
1. Comprendre les détails des modèles modifiables et comment les stratégies peuvent être utilisées pour appliquer un contrôle granulaire du contenu de la page.
1. Découvrez comment les modèles et les pages sont liés

## Ce que vous allez créer {#what-you-will-build}

Dans cette partie du didacticiel, vous allez créer un nouveau modèle de page d’article qui pourra être utilisé pour créer de nouvelles pages d’article et s’aligner sur une structure commune. Le modèle de page de l’article sera basé sur des conceptions et un kit d’interface produit dans AdobeXD. Ce chapitre porte uniquement sur la création de la structure ou du squelette du modèle. Aucun style ne sera implémenté mais le modèle et les pages seront fonctionnels.

![Conception de page d’article et version sans style](assets/pages-templates/what-you-will-build.png)

## Planification de l’interface utilisateur avec Adobe XD {#adobexd}

Dans la plupart des cas, la planification d’un nouveau début Web avec des maquettes et des conceptions statiques. [Adobe ](https://www.adobe.com/products/xd.html) XDest un outil de conception qui permet de créer des expériences utilisateur. Ensuite, nous allons examiner un kit d’interface utilisateur et des maquettes pour vous aider à planifier la structure du modèle de page d’article.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Téléchargez le fichier [ de conception d’article ](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** WKND.

>[!NOTE]
>
> Un [kit d&#39;interface utilisateur des composants principaux générique ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) est également disponible comme point de départ pour les projets personnalisés.

## Création d’un modèle de page d’article

Lors de la création d’une page, vous devez sélectionner un modèle. C’est la base pour la création de la page. Le modèle définit la structure de la page résultante, le contenu initial et les composants autorisés.

Il existe 3 zones principales de [Modèles modifiables](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html) :

1. **Structure**  : définit les composants qui font partie du modèle. Ils ne seront pas modifiables par les auteurs de contenu.
1. **Contenu**  initial : définit les composants que le modèle utilisera, qui peuvent être modifiés et/ou supprimés par les auteurs de contenu.
1. **Stratégies**  : définit des configurations sur le comportement des composants et sur les options disponibles pour les auteurs.

Créez ensuite un nouveau modèle dans AEM qui correspond à la structure des maquettes. Cela se produit dans une instance locale d&#39;AEM. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

Procédure de haut niveau pour la vidéo ci-dessous :

### Configurations de la structure

1. Créez un modèle à l’aide du **Type de modèle de page**, nommé **Page d’article**.
1. Passez en mode **Structure**.
1. Ajoutez un composant **Fragment d’expérience** pour faire office d’en-tête **en haut du modèle.**
   * Configurez le composant pour qu’il pointe sur `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Définissez la stratégie sur **En-tête de page** et assurez-vous que l&#39;élément **Par défaut** est défini sur `header`. L’élément `header`sera ciblé avec CSS dans le chapitre suivant.
1. Ajoutez un composant **Fragment d’expérience** pour faire office de **Pied de page** au bas du modèle.
   * Configurez le composant pour qu’il pointe sur `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Définissez la stratégie sur **Pied de page** et assurez-vous que l&#39;élément **Par défaut** est défini sur `footer`. L’élément `footer` sera ciblé avec CSS dans le chapitre suivant.
1. Verrouillez le conteneur **main** inclus lors de la création initiale du modèle.
   * Définissez la stratégie sur **Page principale** et assurez-vous que l&#39;élément **Par défaut** est défini sur `main`. L’élément `main` sera ciblé avec CSS dans le chapitre suivant.
1. Ajoutez un composant **Image** au conteneur **main**.
   * Déverrouillez le composant **Image**.
1. Ajoutez un composant **Chemin de navigation** sous le composant **Image** dans le conteneur principal.
   * Créez une nouvelle stratégie pour le composant **Chemin de navigation** nommé **Page d&#39;article - Chemin de navigation**. Définissez le **niveau du Début de navigation** sur **4**.
1. Ajoutez un composant **Conteneur** sous le composant **Chemin de navigation** et dans le conteneur **main**. Cela agira en tant que **conteneur de contenu** pour le modèle.
   * Déverrouillez le conteneur **Contenu**.
   * Définissez la stratégie sur **Contenu de la page**.
1. Ajoutez un autre composant **Conteneur** sous le **conteneur de contenu**. Cela servira de conteneur **Rail latéral** pour le modèle.
   * Déverrouillez le conteneur **Rail latéral**.
   * Créez une nouvelle stratégie nommée **Page article - Rail latéral**.
   * Configurez les **composants autorisés** sous **WKND Sites Project - Content** pour inclure : **Bouton**, **Télécharger**, **Image**, **Liste**, **Séparateur**, **Partage sur les réseaux sociaux**, &lt;a 6/>Texte **et** Titre **.**
1. Mettez à jour la stratégie du conteneur racine de page. Il s’agit du conteneur le plus à l’extérieur du modèle. Définissez la stratégie sur **Racine de la page**.
   * Sous **Paramètres du Conteneur**, définissez **Disposition** sur **Grille réactive**.
1. Engagez le mode de mise en page pour le **conteneur de contenu**. Faites glisser la poignée de droite à gauche et rétrécissez le conteneur pour qu’il ait une largeur de 8 colonnes.
1. Engagez le mode de mise en page pour le conteneur de rail latéral ****. Faites glisser la poignée de droite à gauche et rétrécissez le conteneur à 4 colonnes de large. Ensuite, faites glisser la poignée de gauche vers la colonne de droite 1 pour faire large le conteneur de 3 colonnes et laissez un espace de 1 colonne entre le **conteneur de contenu**.
1. Ouvrez l’émulateur mobile et passez à un point d’arrêt mobile. Reprenez le mode de mise en page et faites du **conteneur de contenu** et du **conteneur du rail latéral** la largeur complète de la page. Ceci empile les conteneurs verticalement dans le point d’arrêt mobile.
1. Mettez à jour la stratégie du composant **Texte** dans le **conteneur de contenu**.
   * Définissez la stratégie sur **Texte du contenu**.
   * Sous **Plugins** > **Styles de paragraphe**, cochez **Activer les styles de paragraphe** et assurez-vous que le **bloc de devis** est activé.

### Configurations de contenu initial

1. Passez en mode **Contenu initial**.
1. Ajoutez un composant **Title** au **conteneur de contenu**. Il s’agit du titre de l’article. Lorsqu’elle est vide, elle affiche automatiquement le titre de la page active.
1. Ajoutez un second composant **Title** sous le premier composant Title.
   * Configurez le composant avec le texte : &quot;Par l&#39;auteur&quot;. Il s’agira d’un espace réservé de texte.
   * Définissez le type sur `H4`.
1. Ajoutez un composant **Texte** sous le composant **Par auteur** Titre.
1. Ajoutez un composant **Title** au Conteneur de rail latéral **Side Rail**.
   * Configurez le composant avec le texte : &quot;Partagez cette histoire&quot;.
   * Définissez le type sur `H5`.
1. Ajoutez un composant **Partage sur les réseaux sociaux** sous le composant Titre **Partager cet article**.
1. Ajoutez un composant **Séparateur** sous le composant **Partage sur les réseaux sociaux**.
1. Ajoutez un composant **Download** sous le composant **Separator**.
1. Ajoutez un composant **Liste** sous le composant **Télécharger**.
1. Mettez à jour les **Propriétés de page initiales** pour le modèle.
   * Sous **Médias sociaux** > **Partage des réseaux sociaux**, consultez **Facebook** et **Pinterest**

### Activez le modèle et ajoutez une miniature.

1. Vue du modèle dans la console Modèle en accédant à [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **** Activez le modèle Page d’article.
1. Modifiez les propriétés du modèle Page d’article et téléchargez la miniature suivante afin d’identifier rapidement les pages créées à l’aide du modèle Page d’article :

   ![Miniature du modèle de page d’article](assets/pages-templates/article-page-template-thumbnail.png)

## Mettre à jour l’en-tête et le pied de page avec des fragments d’expérience {#experience-fragments}

Une pratique courante lors de la création de contenu global, tel qu’un en-tête ou un pied de page, consiste à utiliser un [fragment d’expérience](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragments d’expérience, permet aux utilisateurs de combiner plusieurs composants afin de créer un composant unique et référencable. Les fragments d’expérience ont l’avantage de prendre en charge la gestion multisite et [la localisation](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

L&#39;archétype de projet AEM a généré un en-tête et un pied de page. Ensuite, mettez à jour les fragments d’expérience pour qu’ils correspondent aux maquettes. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Procédure de haut niveau pour la vidéo ci-dessous :

1. Téléchargez l’exemple de package de contenu **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**.
1. Téléchargez et installez le package de contenu à l’aide de Package Manager à l’adresse [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).
1. Mettez à jour le modèle de variation Web, qui est le modèle utilisé pour les fragments d’expérience à l’adresse [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html).
   * Mettez à jour la stratégie du composant **Conteneur** sur le modèle.
   * Définissez la stratégie sur **racine XF**.
   * Sous **Composants autorisés**, sélectionnez le groupe de composants **WKND Sites Project - Structure** pour inclure les composants **Navigation linguistique**, **Navigation** et **Recherche rapide**.

### Mettre à jour le fragment d’expérience d’en-tête

1. Ouvrez le fragment d’expérience qui affiche l’en-tête à l’adresse [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html).
1. Configurez la racine **Conteneur** du fragment. Il s&#39;agit du **Conteneur le plus externe**.
   * Définissez **Disposition** sur **Grille réactive**.
1. Ajoutez le **logo noir WKND** en tant qu&#39;image au haut du **Conteneur**. Le logo a été inclus dans le paquet installé lors d&#39;une étape précédente.
   * Modifiez la disposition du **logo foncé WKND** pour qu&#39;il soit de **2** largeur des colonnes. Faites glisser les poignées de droite à gauche.
   * Configurez le logo avec **Texte de remplacement** du &quot;logo WKND&quot;.
   * Configurez le logo sur **Lier** à `/content/wknd/us/en` la Page d&#39;accueil.
1. Configurez le composant **Navigation** qui est déjà placé sur la page.
   * Définissez **Exclure les niveaux racine** sur **1**.
   * Définissez **Profondeur de structure de navigation** sur **1**.
   * Modifiez la disposition du composant **Navigation** pour qu&#39;il soit **8** large. Faites glisser les poignées de droite à gauche.
1. Supprimez le composant **Navigation linguistique**.
1. Modifiez la disposition du composant **Rechercher** pour qu&#39;il soit **2** à l&#39;échelle des colonnes. Faites glisser les poignées de droite à gauche. Tous les composants doivent maintenant être alignés horizontalement sur une seule ligne.

### Mettre à jour le fragment d’expérience de pied de page

1. Ouvrez le fragment d’expérience qui affiche le pied de page à l’adresse [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html).
1. Configurez la racine **Conteneur** du fragment. Il s&#39;agit du **Conteneur le plus externe**.
   * Définissez **Disposition** sur **Grille réactive**.
1. Ajoutez le **logo léger WKND** comme image en haut du **Conteneur**. Le logo a été inclus dans le paquet installé lors d&#39;une étape précédente.
   * Modifiez la disposition du **logo de la lumière WKND** pour qu&#39;il soit **2** large. Faites glisser les poignées de droite à gauche.
   * Configurez le logo avec **Texte de remplacement** de &quot;WKND Logo Light&quot;.
   * Configurez le logo sur **Lier** à `/content/wknd/us/en` la Page d&#39;accueil.
1. Ajoutez un composant **Navigation** sous le logo. Configurez le composant **Navigation** :
   * Définissez **Exclure les niveaux racine** sur **1**.
   * Décochez **Collecter toutes les pages enfants**.
   * Définissez **Profondeur de structure de navigation** sur **1**.
   * Modifiez la disposition du composant **Navigation** pour qu&#39;il soit **8** large. Faites glisser les poignées de droite à gauche.

## Création d’une page d’article

Créez ensuite une page à l’aide du modèle Page d’article. Créez le contenu de la page en fonction des maquettes de site. Suivez les étapes de la vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

Procédure de haut niveau pour la vidéo ci-dessous :

1. Accédez à la console Sites à l&#39;adresse [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Créez une nouvelle page sous **WKND** > **US** > **EN** > **Magazine**.
   * Sélectionnez le modèle **Page d’article**.
   * Sous **Properties**, définissez **Title** sur &quot;Ultimate Guide to LA Skateparks&quot;.
   * Définissez le **nom** sur &quot;guide-la-skateparks&quot;.
1. Remplacez **Par auteur** Titre par le texte &quot;Par Stacey Roswells&quot;.
1. Mettez à jour le composant **Texte** pour inclure un paragraphe afin de renseigner l’article. Vous pouvez utiliser le fichier texte suivant comme copie : [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Ajoutez un autre composant **Texte**.
   * Mettez à jour le composant pour inclure le devis : &quot;Il n&#39;y a pas de meilleur endroit pour se raser que Los Angeles.&quot;
   * Modifiez l’éditeur de texte enrichi en mode plein écran et modifiez le devis ci-dessus pour utiliser l’élément **Bloc de devis**.
1. Continuez à remplir le corps de l’article pour qu’il corresponde aux maquettes.
1. Configurez le composant **Download** pour utiliser une version PDF de l’article.
   * Sous **Télécharger** > **Propriétés**, cochez la case **Obtenir le titre de la ressource DAM**.
   * Définissez **Description** sur : &quot;Obtenez l&#39;histoire complète&quot;.
   * Définissez **Action Text** sur : &quot;Télécharger PDF&quot;.
1. Configurez le composant **Liste**.
   * Sous **Paramètres de Liste** > **Créer une Liste à l’aide de**, sélectionnez **Pages enfants**.
   * Définissez **Page parente** sur `/content/wknd/us/en/magazine`.
   * Sous **Paramètres d&#39;élément**, cochez **Lier les éléments** et cochez **Afficher la date**.

## Inspect de la structure de noeud {#node-structure}

A ce stade, la page de l&#39;article n&#39;est clairement pas stylisée. Toutefois, la structure de base est en place. Ensuite, examinez la structure des noeuds de la page d’article pour mieux comprendre le rôle du modèle, de la page et des composants.

Utilisez l’outil CRXDE-Lite sur une instance d’AEM locale pour vue de la structure de noeud sous-jacente.

1. Ouvrez [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) et utilisez l&#39;arborescence de navigation pour accéder à `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Cliquez sur le noeud `jcr:content` situé sous la page `la-skateparks` et vue les propriétés :

   ![Propriétés de contenu JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Notez la valeur de `cq:template`, qui pointe vers `/conf/wknd/settings/wcm/templates/article-page`, le modèle de page d’article que nous avons créé précédemment.

   Notez également la valeur de `sling:resourceType`, qui pointe vers `wknd/components/page`. Il s’agit du composant de page créé par l’archétype du projet AEM et responsable du rendu de la page en fonction du modèle.

1. Développez le noeud `jcr:content` sous `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` et vue la hiérarchie des noeuds :

   ![JCR Contenu JCR LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Vous devriez être en mesure de mapper de manière plus ou moins précise chacun des noeuds aux composants créés. Vérifiez si vous pouvez identifier les différents Conteneurs de mise en page utilisés en examinant les noeuds précédés de `container`.

1. Examinez ensuite le composant de page à l&#39;adresse `/apps/wknd/components/page`. Vue des propriétés du composant dans le CRXDE Lite :

   ![Propriétés du composant de page](assets/pages-templates/page-component-properties.png)

   Notez qu’il n’existe que 2 scripts HTL, `customfooterlibs.html` et `customheaderlibs.html` sous le composant de page. *Comment ce composant génère-t-il la page ?*

   La propriété `sling:resourceSuperType` pointe vers `core/wcm/components/page/v2/page`. Cette propriété permet au composant de page du WKND d&#39;hériter **all** des fonctionnalités du composant de page du composant principal. Il s’agit du premier exemple d’un élément appelé [Modèle de composant proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Vous trouverez plus d’informations[ ici.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect un autre composant dans les composants WKND, le composant `Breadcrumb` situé à l&#39;emplacement suivant : `/apps/wknd/components/breadcrumb`. Notez que la même propriété `sling:resourceSuperType` est disponible, mais cette fois elle pointe sur `core/wcm/components/breadcrumb/v2/breadcrumb`. Voici un autre exemple d&#39;utilisation du modèle de composant Proxy pour inclure un composant principal. En fait, tous les composants de la base de code WKND sont des proxies de AEM Core Components (à l&#39;exception de notre célèbre composant HelloWorld). Il est recommandé d&#39;essayer de réutiliser autant de fonctionnalités des composants principaux que possible *avant* d&#39;écrire du code personnalisé.

1. Examinez ensuite la page du composant principal à `/libs/core/wcm/components/page/v2/page` à l&#39;aide du CRXDE Lite :

   >[!NOTE]
   >
   > Dans AEM 6.5/6.4, les composants principaux sont situés sous `/apps/core/wcm/components`. Dans AEM en tant que Cloud Service, les composants principaux se trouvent sous `/libs` et sont mis à jour automatiquement.

   ![Page Composant principal](assets/pages-templates/core-page-component-properties.png)

   Notez que beaucoup d’autres scripts sont inclus sous cette page. La page Composant principal contient beaucoup de fonctionnalités. Cette fonctionnalité est divisée en plusieurs scripts afin de faciliter la maintenance et la lisibilité. Vous pouvez suivre l’inclusion des scripts HTL en ouvrant `page.html` et en recherchant `data-sly-include` :

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

   L’autre raison de diviser le code HTML en plusieurs scripts est de permettre aux composants proxy de remplacer des scripts individuels pour implémenter une logique métier personnalisée. Les scripts HTL, `customfooterlibs.html` et `customheaderlibs.html`, sont créés dans le but explicite d’être remplacés par la mise en oeuvre de projets.

   Pour en savoir plus sur la manière dont le modèle modifiable prend en compte le rendu de la page de contenu [en lisant cet article](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect est un autre composant principal, comme la barre de navigation à `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Vue du script `breadcrumb.html` pour comprendre comment le balisage du composant Breadcrumb est finalement généré.

## Enregistrement des configurations dans le contrôle de code source {#configuration-persistence}

Dans de nombreux cas, en particulier au début d&#39;un projet AEM, il est important de conserver les configurations, comme les modèles et les stratégies de contenu connexes, pour contrôler la source. Ceci garantit que tous les développeurs travaillent sur le même ensemble de contenu et de configurations et peut garantir une cohérence supplémentaire entre les environnements. Une fois qu&#39;un projet atteint un certain niveau de maturité, la pratique de gestion des modèles peut être transmise à un groupe spécial d&#39;utilisateurs de la puissance.

Pour l&#39;instant, nous traiterons les modèles comme d&#39;autres éléments de code et synchroniserons le **Modèle de page d&#39;article** dans le cadre du projet. Jusqu&#39;à présent, nous avons **poussé** le code de notre projet AEM vers une instance locale d&#39;AEM. Le **modèle de page d&#39;article** a été créé directement sur une instance locale d&#39;AEM, nous devons donc **importer** le modèle dans notre projet AEM. Le module **ui.content** est inclus dans le projet AEM à cet effet spécifique.

Les prochaines étapes se dérouleront à l’aide de l’IDE VSCode à l’aide du module externe [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview), mais elles peuvent être effectuées à l’aide de tout IDE que vous avez configuré pour **importer** ou importer du contenu à partir d’une instance locale d’AEM.

1. Dans le VSCode, ouvrez le projet `aem-guides-wknd`.

1. Développez le module **ui.content** dans l&#39;explorateur de projets. Développez le dossier `src` et accédez à `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Cliquez avec le bouton droit de la souris ] sur le  `templates` dossier et sélectionnez  **Importer à partir du serveur** AEM :

   ![Modèle d&#39;import VSCode](assets/pages-templates/vscode-import-templates.png)

   Les modèles `article-page` doivent être importés et les modèles `page-content`, `xf-web-variation` doivent également être mis à jour.

   ![Modèles mis à jour](assets/pages-templates/updated-templates.png)

1. Répétez les étapes pour importer le contenu, mais sélectionnez le dossier **policies** situé à `/conf/wknd/settings/wcm/policies`.

   ![Stratégies d’importation VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect le fichier `filter.xml` situé à `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Le fichier `filter.xml` est chargé d&#39;identifier les chemins d&#39;accès des noeuds qui seront installés avec le package. Remarquez le `mode="merge"` sur chaque filtres qui indique que le contenu existant ne sera pas modifié, seul le nouveau contenu sera ajouté. Les auteurs de contenu pouvant mettre à jour ces chemins, il est important qu’un déploiement de code ne remplace **pas** le contenu. Pour plus d&#39;informations sur l&#39;utilisation des éléments de filtre, consultez la [documentation FileVault](https://jackrabbit.apache.org/filevault/filter.html).

   Comparez `ui.content/src/main/content/META-INF/vault/filter.xml` et `ui.apps/src/main/content/META-INF/vault/filter.xml` pour comprendre les différents noeuds gérés par chaque module.

   >[!WARNING]
   >
   > Afin d&#39;assurer des déploiements cohérents pour le site de référence WKND, certaines branches du projet sont configurées de telle sorte que `ui.content` remplacera toute modification du JCR. Il s’agit de la conception, c’est-à-dire pour les branches de solution, puisque le code/les styles seront écrits pour des stratégies spécifiques.

## Félicitations! {#congratulations}

Félicitations, vous venez de créer un nouveau modèle et une nouvelle page avec Adobe Experience Manager Sites.

### Étapes suivantes {#next-steps}

A ce stade, la page de l&#39;article n&#39;est clairement pas stylisée. Suivez le didacticiel [Bibliothèques côté client et Workflow frontal](client-side-libraries.md) pour découvrir les meilleures pratiques d&#39;inclusion de CSS et de JavaScript pour appliquer des styles globaux au site et intégrer une version frontale dédiée.

Vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur la brach Git `tutorial/pages-templates-solution`.

1. Cloner le référentiel [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `tutorial/pages-templates-solution`.
