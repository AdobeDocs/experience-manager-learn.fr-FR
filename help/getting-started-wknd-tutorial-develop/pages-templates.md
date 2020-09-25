---
title: Prise en main de AEM Sites - Pages et modèles
seo-title: Prise en main de AEM Sites - Pages et modèles
description: Découvrez la relation entre un composant de page de base et des modèles modifiables. Comprenez comment les composants principaux sont intégrés au projet et découvrez les configurations de stratégie avancées des modèles modifiables afin de créer un modèle de page d’article bien structuré basé sur une maquette d’Adobe XD.
sub-product: sites
feature: template-editor, core-components
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2226'
ht-degree: 3%

---


# Pages et modèles {#pages-and-template}

Ce chapitre porte sur la relation entre un composant de page de base et des modèles modifiables. Nous allons créer un modèle d’article sans style basé sur certaines maquettes d’ [AdobeXD](https://www.adobe.com/products/xd.html). Dans le processus de création du modèle, les composants principaux et les configurations de stratégie avancées des modèles modifiables sont traités.

## Conditions préalables {#prerequisites}

Examiner les outils et les instructions nécessaires à la mise en place d&#39;un environnement [de développement](overview.md#local-dev-environment)local.

### Projet de démarrage

Consultez le code de ligne de base sur lequel le didacticiel s&#39;appuie :

1. Clonez le référentiel [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) .
1. Regardez la `pages-templates/start` branche.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout pages-templates/start
   ```

1. Déployez la base de code sur une instance AEM locale en utilisant vos compétences Maven :

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Vous pouvez toujours vue le code fini sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) ou le retirer localement en passant à la branche `pages-templates/solution`.

## Intention

1. inspect une conception de page créée dans Adobe XD et la mappe aux composants principaux.
1. Comprendre les détails des modèles modifiables et comment les stratégies peuvent être utilisées pour appliquer un contrôle granulaire du contenu de la page.
1. Découvrez comment les modèles et les pages sont liés

## Ce que vous allez construire {#what-you-will-build}

Dans cette partie du didacticiel, vous allez créer un nouveau modèle de page d’article qui pourra être utilisé pour créer de nouvelles pages d’article et s’aligner sur une structure commune. Le modèle de page de l’article sera basé sur des conceptions et un kit d’interface produit dans AdobeXD. Ce chapitre porte uniquement sur la création de la structure ou du squelette du modèle. Aucun style ne sera implémenté mais le modèle et les pages seront fonctionnels.

![Conception de page d’article et version sans style](assets/pages-templates/what-you-will-build.png)

## Planification de l’interface utilisateur avec Adobe XD {#adobexd}

Dans la plupart des cas, la planification d’un nouveau début Web avec des maquettes et des conceptions statiques. [adobe xd](https://www.adobe.com/products/xd.html) est un outil de conception qui permet de créer des expériences utilisateur. Ensuite, nous allons examiner un kit d’interface utilisateur et des maquettes pour vous aider à planifier la structure du modèle de page d’article.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

Téléchargez le fichier [de conception d’article](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)WKND.

## Création d’un en-tête et d’un pied de page avec des fragments d’expérience {#experience-fragments}

Lors de la création de contenu global, tel qu’un en-tête ou un pied de page, l’utilisation d’un fragment [d’expérience est une pratique courante](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragments d’expérience, nous permettent de combiner plusieurs composants afin de créer un composant unique, pouvant être référencé. Les fragments d’expérience ont l’avantage de prendre en charge la gestion multisite et nous permettent de gérer différents en-têtes/pieds de page par langue.

Ensuite, nous allons mettre à jour le fragment d’expérience destiné à être utilisé comme en-tête et pied de page pour ajouter le logo WKND.

>[!VIDEO](https://video.tv.adobe.com/v/30215/?quality=12&learn=on)

>[!NOTE]
>
> Vos fragments d’expérience ont-ils un aspect différent de celui de la vidéo ? Essayez de les supprimer et réinstallez la base de code du projet de démarrage.

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. Mettez à jour l’en-tête du fragment d’expérience situé à l’adresse [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html) pour inclure le logo WKND Dark (Noir de WKND).

   ![Logo noir WKND](assets/pages-templates/wknd-logo-dk.png)

   *Logo WKND Dark*

1. Mettez à jour l’en-tête du fragment d’expérience situé à l’emplacement [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html) pour inclure le logo de l’éclairage WKND.

   ![Logo léger WKND](assets/pages-templates/wknd-logo-light.png)

   *Logo WKND Light*

## Création d’un modèle de page d’article

Lors de la création d’une page, vous devez sélectionner un modèle. C’est la base pour la création de la page. Le modèle définit la structure de la page résultante, le contenu initial et les composants autorisés.

Les modèles [](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html)modifiables comportent trois zones principales :

1. **Structure** : définit les composants qui font partie du modèle. Ils ne seront pas modifiables par les auteurs de contenu.
1. **Contenu** initial : définit les composants que le modèle utilisera, qui peuvent être modifiés et/ou supprimés par les auteurs de contenu.
1. **Stratégies** : définit des configurations sur le comportement des composants et les options disponibles pour les auteurs.

Nous allons ensuite créer le modèle de page d’article. Cela se produit dans une instance locale d&#39;AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30217/?quality=12&learn=on)

Vous trouverez ci-dessous les étapes de haut niveau effectuées dans la vidéo ci-dessus.

1. Accédez au dossier WKND Sites Template : **Outils** > **Général** > **Modèles** > Site **WKND**
1. Créez un modèle à l’aide du type de modèle de page **vide du site** WKND avec le titre Modèle de page d’ **article**
1. En mode **Structure** , configurez le modèle pour inclure les éléments suivants :

   * En-tête du fragment d’expérience
   * Image
   * Chemin de navigation
   * Conteneur : 8 colonnes large Bureau, 12 colonnes large Tablette, Mobile
   * Conteneur : 4 colonnes de large, 12 colonnes de large Tablette, Mobile
   * Pied de page du fragment d’expérience

   ![Modèle de page d’article du mode Structure](assets/pages-templates/article-page-template-structure.png)

   *Structure - Modèle de page d’article*

1. Passez en mode Contenu **** initial et ajoutez les composants suivants comme contenu de démarrage :

   * **Conteneur principal**
      * Titre - taille par défaut de H1
      * Titre - *&quot;Par nom d’auteur&quot;* avec une taille H4
      * Texte - vide
   * **Conteneur latéral**
      * Titre - *&quot;Partager cet article&quot;* avec une taille H5
      * Partage sur les réseaux sociaux
      * Séparateur
      * Téléchargement
      * Liste

   ![Modèle de page d’article du mode Contenu initial](assets/pages-templates/article-page-template-initialcontent.png)

   *Contenu initial - Modèle de page d’article*

1. Mettez à jour les propriétés **** initiales de la page pour activer le partage des utilisateurs pour **Facebook** et **Pinterest**.
1. Téléchargez une image dans les **propriétés du modèle de page d’** article afin de l’identifier facilement :

   ![Miniature du modèle de page d’article](assets/pages-templates/article-page-template-thumbnail.png)

   *Miniature du modèle de page d’article*

1. Activez les **modèles de page d’** article dans le dossier [Modèles de site](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd/settings/wcm/templates)WKND.

## Création d’une page d’article

Maintenant que nous avons un modèle, créons une nouvelle page en utilisant ce modèle.

1. Téléchargez le package zip suivant, [WKND-PagesTemplates-DAM-Assets.zip](assets/pages-templates/WKND-PagesTemplates-DAM-Assets.zip) et installez-le via [CRX Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   Le package ci-dessus installe plusieurs images et ressources sous `/content/dam/wknd/en/magazine/la-skateparks` lesquelles seront utilisées pour remplir une page d’article par la suite.

   *Les images et les ressources du paquet ci-dessus sont libres de toute licence avec l&#39;aimable autorisation de[Unsplash.com](https://unsplash.com/).*

   ![Exemples de ressources DAM](assets/pages-templates/sample-assets-la-skatepark.png)

1. Créez une nouvelle page, sous **WKND** > **US** > **en, nommée****Magazine.** Utilisez le modèle de page **** de contenu.

   Cette page ajoutera une structure à notre site et nous permettra de voir le composant Chemin de navigation en action.

1. Créez ensuite une nouvelle page, sous **WKND** > **US** > **en** > **Magazine.** Utilisez le modèle Page **** article. Utilisez un titre de guide **Ultimate pour les parcs** de la Skateparks et un nom de **guide-la-skateparks**.

   ![Page d’article initialement créée](assets/pages-templates/create-article-page-nocontent.png)

1. Renseignez la page avec le contenu afin de correspondre aux maquettes inspectées dans la partie [UI Planning avec AdobeXD](#adobexd) . Un exemple de texte d&#39;article peut être [téléchargé ici](assets/pages-templates/la-skateparks-copy.txt). Vous devriez être en mesure de créer quelque chose de similaire à ceci :

   ![Page d’article renseignée](assets/pages-templates/article-page-unstyled.png)

   >[!NOTE]
   >
   > Le composant Image situé en haut de la page peut être modifié mais pas supprimé. Le composant de chemin de navigation s’affiche sur la page mais ne peut pas être modifié ou supprimé.

## inspect de la structure des noeuds {#node-structure}

A ce stade, la page de l&#39;article n&#39;est clairement pas stylisée. Toutefois, la structure de base est en place. Ensuite, nous allons examiner la structure des noeuds de la page d’article afin de mieux comprendre le rôle du modèle et du composant de page responsable du rendu du contenu.

Nous pouvons le faire à l&#39;aide de l&#39;outil CRXDE-Lite sur une instance d&#39;AEM locale.

1. Ouvrez [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) et utilisez l’arborescence de navigation pour naviguer jusqu’à `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Cliquez sur le `jcr:content` noeud sous la `la-skateparks` page et vue des propriétés :

   ![Propriétés de contenu JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Notez la valeur pour `cq:template`, qui pointe vers `/conf/wknd/settings/wcm/templates/article-page`, le modèle de page d’article que nous avons créé précédemment.

   Notez également la valeur de `sling:resourceType`, qui pointe vers `wknd/components/structure/page`. Il s’agit du composant de page créé par l’archétype du projet AEM et responsable du rendu de la page en fonction du modèle.

1. Développez le `jcr:content` noeud sous `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` la hiérarchie des noeuds et en vue :

   ![JCR Contenu JCR LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Vous devriez être en mesure de mapper de manière plus ou moins précise chacun des noeuds aux composants créés. Voyez si vous pouvez identifier les différents Conteneurs de mise en page utilisés en examinant les noeuds avec préfixe `responsivegrid`.

1. Examinez ensuite le composant de page dans `/apps/wknd/components/structure/page`. Vue des propriétés du composant dans le CRXDE Lite :

   ![Propriétés du composant de page](assets/pages-templates/page-component-properties.png)

   Notez que le composant de page se trouve sous un dossier nommé **structure**. Il s’agit d’une convention qui correspond au mode de structure de l’éditeur de modèles et qui est utilisée pour indiquer que le composant de page n’est pas un composant avec lequel les auteurs interagiront directement.

   Notez qu’il n’existe que 2 scripts HTML `customfooterlibs.html` et `customheaderlibs.html` sous le composant de page. Comment ce composant génère-t-il la page ?

   Notez la `sling:resourceSuperType` propriété et la valeur de `core/wcm/components/page/v2/page`. Cette propriété permet au composant de page du WKND d’hériter de toutes les fonctionnalités du composant de page Composant principal. C&#39;est le premier exemple de quelque chose appelé le modèle [de composant](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)proxy. Vous trouverez plus d’informations[ ici.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. inspect un autre composant dans les composants WKND, le `Breadcrumb` composant se trouvant à l’emplacement suivant : `/apps/wknd/components/content/breadcrumb`. Notez que la même `sling:resourceSuperType` propriété peut être trouvée, mais cette fois il pointe vers `core/wcm/components/breadcrumb/v2/breadcrumb`. Voici un autre exemple d&#39;utilisation du modèle de composant Proxy pour inclure un composant principal. En fait, tous les composants de la base de code WKND sont des proxies de AEM Core Components (à l&#39;exception de notre célèbre composant HelloWorld). Il est recommandé d’essayer de réutiliser autant de fonctionnalités des composants principaux que possible *avant* d’écrire du code personnalisé.

1. Examinez ensuite la page Composant principal à l’ `/apps/core/wcm/components/page/v2/page` aide du CRXDE Lite :

   ![Page Composant principal](assets/pages-templates/core-page-component-properties.png)

   Notez que beaucoup d’autres scripts sont inclus sous cette page. La page Composant principal contient beaucoup de fonctionnalités. Cette fonctionnalité est divisée en plusieurs scripts afin de faciliter la maintenance et la lisibilité. Vous pouvez suivre l’inclusion des scripts HTL en ouvrant la page `page.html` et en recherchant `data-sly-include`:

   ```html
   <!--/* /apps/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
           data-sly-use.head="head.html"
           data-sly-use.footer="footer.html"
           data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}">
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   L’autre raison de diviser le code HTML en plusieurs scripts est de permettre aux composants proxy de remplacer des scripts individuels pour implémenter une logique métier personnalisée. Les scripts HTL, `customfooterlibs.html` et `customheaderlibs.html`, sont créés dans le but explicite d’être remplacés par la mise en oeuvre de projets.

   Vous pouvez en savoir plus sur la manière dont le modèle modifiable prend en compte le rendu de la page de [contenu en lisant cet article](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html#resultant-content-pages).

1. inspect un autre composant principal, comme la barre de navigation sur `/apps/core/wcm/components/breadcrumb/v2/breadcrumb`. Vue du `breadcrumb.html` script pour comprendre comment le balisage du composant Chemin de navigation est généré en fin de compte.

## Enregistrement des configurations dans le contrôle de code source {#configuration-persistence}

Dans de nombreux cas, en particulier au début d&#39;un projet AEM, il est important de conserver les configurations, comme les modèles et les stratégies de contenu connexes, pour contrôler la source. Ceci garantit que tous les développeurs travaillent sur le même ensemble de contenu et de configurations et peut garantir une cohérence supplémentaire entre les environnements. Une fois qu&#39;un projet atteint un certain niveau de maturité, la pratique de gestion des modèles peut être transmise à un groupe spécial d&#39;utilisateurs de la puissance.

Pour l&#39;instant, nous traiterons les modèles comme d&#39;autres éléments de code et synchroniserons le modèle **de page de l&#39;** article dans le cadre du projet. Jusqu&#39;à présent, nous avons **repoussé** le code de notre projet AEM vers une instance locale d&#39;AEM. Le modèle **de page de l’** article a été créé directement sur une instance locale de l’AEM. Nous devons donc **extraire** ou importer le modèle dans notre projet AEM. Le module **ui.content** est inclus dans le projet AEM à cet effet spécifique.

Les prochaines étapes se dérouleront à l&#39;aide de l&#39;Eclipse IDE, mais elles peuvent être effectuées à l&#39;aide de tout IDE que vous avez configuré pour **extraire** ou importer du contenu à partir d&#39;une instance locale d&#39;AEM.

1. Dans l&#39;Eclipse IDE, assurez-vous qu&#39;un serveur connecté à l&#39;instance locale d&#39;AEM a démarré le module d&#39;outil de développement AEM et que le module **ui.content** a été ajouté à la configuration du serveur.

   ![Connexion au serveur Eclipse](assets/pages-templates/eclipse-server-started.png)

1. Développez le module **ui.content** dans l&#39;explorateur de projets. Développez le `src` dossier (celui qui contient la petite icône de gobelet) et accédez à `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Cliquez avec le bouton droit de la souris sur] le `templates` noeud et sélectionnez **Importer à partir du serveur...**:

   ![Modèle d&#39;import Eclipse](assets/pages-templates/eclipse-import-templates.png)

   Confirmez la boîte de dialogue **Importer à partir du référentiel** et cliquez sur **Terminer**. Vous devriez maintenant voir le `article-page-template` sous le `templates` dossier.

1. Répétez les étapes pour importer du contenu, mais sélectionnez le noeud **policies** situé dans `/conf/wknd/settings/wcm/policies`.

   ![Stratégies d’importation Eclipse](assets/pages-templates/policies-article-page-template.png)

1. inspect le `filter.xml` fichier situé dans `src/main/content/META-INF/vault/filter.xml`.

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

   Le `filter.xml` fichier est chargé d&#39;identifier les chemins d&#39;accès des noeuds qui seront installés avec le package. Notez que `mode="merge"` sur chacun des filtres indique que le contenu existant ne sera pas modifié, seul le nouveau contenu sera ajouté. Les auteurs de contenu pouvant mettre à jour ces chemins, il est important qu’un déploiement de code **ne remplace pas** le contenu. Pour plus d&#39;informations sur l&#39;utilisation des éléments de filtre, consultez la documentation [](https://jackrabbit.apache.org/filevault/filter.html) FileVault.

   Comparer `ui.content/src/main/content/META-INF/vault/filter.xml` et `ui.apps/src/main/content/META-INF/vault/filter.xml` comprendre les différents noeuds gérés par chaque module.

   >[!WARNING]
   >
   > Afin d&#39;assurer des déploiements cohérents pour le site de référence WKND, certaines branches du projet sont configurées de manière à `ui.content` remplacer toute modification du JCR. Il s’agit de la conception, c’est-à-dire pour les branches de solution, puisque le code/les styles seront écrits pour des stratégies spécifiques.

## Félicitations ! {#congratulations}

Félicitations, vous venez de créer un nouveau modèle et une nouvelle page avec Adobe Experience Manager Sites.

### Étapes suivantes {#next-steps}

A ce stade, la page de l&#39;article n&#39;est clairement pas stylisée. Suivez les didacticiels des bibliothèques côté [client et du flux de travail](client-side-libraries.md) frontal pour découvrir les meilleures pratiques d’inclusion de CSS et de JavaScript pour appliquer des styles globaux au site et intégrer une version frontale dédiée.

Vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur l&#39;embrayage Git `pages-templates/solution`.

1. Clonez le référentiel [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) .
1. Regardez la `pages-templates/solution` branche.
