---
title: Créer du contenu et publier les modifications
seo-title: Prise en main d’AEM Sites - Création de modifications de contenu et de publication
description: Utilisez l’éditeur de page d’Adobe Experience Manager AEM pour mettre à jour le contenu du site web. Découvrez comment les composants sont utilisés pour faciliter la création. Comprenez la différence entre les environnements d’auteur et de publication AEM et découvrez comment publier les modifications sur le site en direct.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Gestion de contenu
feature: Composants principaux, éditeur de page
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 3%

---


# Créer du contenu et publier des modifications {#author-content-publish}

>[!CAUTION]
>
> Les fonctionnalités de création rapide de site présentées ici seront publiées au deuxième semestre 2021. La documentation associée est disponible à des fins d’aperçu.

Il est important de comprendre comment un utilisateur va mettre à jour le contenu du site web. Dans ce chapitre, nous allons adopter le personnage d’un **auteur de contenu** et apporter quelques mises à jour éditoriales au site généré dans le chapitre précédent. À la fin du chapitre, nous publierons les modifications pour comprendre comment le site actif est mis à jour.

## Prérequis {#prerequisites}

Ce tutoriel en plusieurs parties est supposé que les étapes décrites dans le chapitre [Créer un site](./create-site.md) sont terminées.

## Intention {#objective}

1. Découvrez les concepts de **Pages** et **Composants** dans AEM Sites.
1. Découvrez comment mettre à jour le contenu du site web.
1. Découvrez comment publier des modifications sur le site actif.

## Créer une page {#create-page}

Un site web est généralement divisé en pages pour former une expérience multi-page. AEM structure le contenu de la même manière. Créez ensuite une page pour le site.

1. Connectez-vous au service **Auteur** AEM utilisé dans le chapitre précédent.
1. Dans l’écran AEM Démarrer, cliquez sur **Sites** > **Site WKND** > **Anglais** > **Article**
1. Dans le coin supérieur droit, cliquez sur **Créer** > **Page**.

   ![Créer une page](assets/author-content-publish/create-page-button.png)

   L’assistant **Créer une page** s’affiche alors.

1. Choisissez le modèle **Page d’article** et cliquez sur **Suivant**.

   Les pages d’AEM sont créées à partir d’un modèle de page. Les modèles de page seront détaillés dans le chapitre [Modèles de page](page-templates.md).

1. Sous **Propriétés**, saisissez un **Titre** de &quot;Hello World&quot;.
1. Définissez le **nom** sur `hello-world` et cliquez sur **Créer**.

   ![Propriétés de page initiales](assets/author-content-publish/initial-page-properties.png)

1. Dans la fenêtre contextuelle de la boîte de dialogue, cliquez sur **Ouvrir** pour ouvrir la page nouvellement créée.

## Création d’un composant {#author-component}

AEM Les composants peuvent être considérés comme de petits blocs de création modulaires d’une page web. En divisant l’interface utilisateur en blocs ou composants logiques, elle facilite la gestion. Pour réutiliser les composants, ceux-ci doivent pouvoir être configurés. Pour ce faire, utilisez la boîte de dialogue de création.

AEM fournit un ensemble de [composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr) prêts à l’emploi en production. Les **composants principaux** vont des éléments de base tels que [Texte](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) et [Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) à des éléments d’IU plus complexes tels qu’un [Carrousel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

Créons ensuite quelques composants à l’aide de l’éditeur de page AEM.

1. Accédez à la page **Hello World** créée lors de l’exercice précédent.
1. Vérifiez que vous êtes en mode **Édition** et, dans le rail latéral gauche, cliquez sur l’icône **Composants** .

   ![Rail latéral de l’éditeur de pages](assets/author-content-publish/page-editor-siderail.png)

   Cela ouvre la bibliothèque de composants et répertorie les composants disponibles qui peuvent être utilisés sur la page.

1. Faites défiler l’écran vers le bas et **Faites glisser un composant** **Texte (v2)** vers la région principale modifiable de la page.

   ![Faire glisser et déposer le composant de texte](assets/author-content-publish/drag-drop-text-cmp.png)

1. Cliquez sur le composant **Texte** pour le mettre en surbrillance, puis sur l’icône **clé à molette** ![Icône de clé à molette](assets/author-content-publish/wrench-icon.png) pour ouvrir la boîte de dialogue du composant. Entrez du texte et enregistrez les modifications dans la boîte de dialogue.

   ![Composant de texte enrichi](assets/author-content-publish/rich-text-populated-component.png)

   Le composant **Texte** doit maintenant afficher le texte enrichi sur la page.

1. Répétez les étapes ci-dessus, sauf faites glisser une instance du composant **Image(v2)** sur la page. Ouvrez la boîte de dialogue du composant **Image** .

1. Dans le rail de gauche, passez à l’**outil de recherche de ressources** en cliquant sur l’icône **Ressources** ![icône de ressource](assets/author-content-publish/asset-icon.png).
1. **Faites glisser et** déposez l’image dans la boîte de dialogue du composant, puis cliquez sur  **** Ne pas enregistrer les modifications.

   ![Ajouter une ressource à la boîte de dialogue](assets/author-content-publish/add-asset-dialog.png)

1. Notez qu’il existe des composants sur la page, tels que le **titre**, la **navigation**, la **recherche** qui sont corrigés. Ces zones sont configurées dans le cadre du modèle de page et ne peuvent pas être modifiées sur une page individuelle. Ce point sera abordé plus en détail dans le prochain chapitre.

N’hésitez pas à tester d’autres composants. Vous trouverez une documentation sur chaque [composant principal ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). Une série vidéo détaillée sur la [création de pages est disponible ici](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Publier les mises à jour {#publish-updates}

Les environnements AEM sont répartis entre un **service d’auteur** et un **service de publication**. Dans ce chapitre, nous avons apporté plusieurs modifications au site sur le **service d’auteur**. Pour que les visiteurs du site puissent afficher les modifications, nous devons les publier dans le **service de publication**.

![Diagramme de haut niveau](assets/author-content-publish/author-publish-high-level-flow.png)

*Flux de contenu de haut niveau de l’auteur à la publication*

**1.** Les auteurs de contenu mettent à jour le contenu du site. Les mises à jour peuvent être prévisualisées, examinées et approuvées pour publication.

**2.** Le contenu est publié. La publication peut être effectuée à la demande ou programmée pour une date ultérieure.

**3.** Les modifications seront répercutées sur le service de publication pour les visiteurs du site.

### Publier les modifications

Publions ensuite les modifications.

1. Dans l’écran AEM Démarrer , accédez à **Sites** et sélectionnez le **site WKND**.
1. Cliquez sur **Gérer la publication** dans la barre de menus.

   ![Gérer la publication](assets/author-content-publish/click-manage-publiciation.png)

   Comme il s’agit d’un tout nouveau site, nous voulons publier toutes les pages et nous pouvons utiliser l’assistant Gérer la publication pour définir exactement ce qui doit être publié.

1. Sous **Options**, laissez les paramètres par défaut sur **Publier** et planifiez-les pour **Maintenant**. Cliquez sur **Suivant**.
1. Sous **Portée**, sélectionnez le **site WKND** et cliquez sur **Inclure les enfants**. Dans la boîte de dialogue, décochez toutes les cases. Nous voulons publier le site complet.

   ![Mettre à jour le périmètre de publication](assets/author-content-publish/update-scope-publish.png)

1. Cliquez sur le bouton **Références publiées** . Dans la boîte de dialogue, vérifiez que tout est coché. Cela inclut le **modèle de site de base AEM** et plusieurs configurations générées par le modèle de site. Cliquez sur **Terminé** pour mettre à jour.

   ![Publication de références](assets/author-content-publish/publish-references.png)

1. Enfin, cliquez sur **Publier** dans le coin supérieur droit pour publier le contenu.

## Affichage du contenu publié {#publish}

Ensuite, accédez au service de publication pour afficher les modifications.

1. Pour obtenir facilement l’URL du service de publication, copiez l’URL de création et remplacez le mot `author` par `publish`. Par exemple :

   * **URL de l&#39;auteur** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL de publication**  -  `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Ajoutez `/content/wknd.html` à l’URL de publication afin que l’URL finale ressemble à : `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Modifiez `wknd.html` pour qu’il corresponde au nom de votre site, si vous avez fourni un nom unique lors de la [création du site](create-site.md).

1. En accédant à l’URL de publication, vous devriez voir le site, sans aucune fonctionnalité de création AEM.

   ![Site publié](assets/author-content-publish/publish-url-update.png)

1. À l’aide du menu **Navigation**, cliquez sur **Article** > **Hello World** pour accéder à la page Hello World créée précédemment.
1. Revenez au **service d’auteur AEM** et apportez des modifications de contenu supplémentaires dans l’éditeur de page.
1. Publiez ces modifications directement dans l’éditeur de page en cliquant sur l’icône **Propriétés de page** > **Publier la page**

   ![publication directe](assets/author-content-publish/page-editor-publish.png)

1. Revenez à **AEM Publish Service** pour afficher les modifications. **et** ne verrez probablement pas immédiatement les mises à jour. En effet, le **service de publication AEM** inclut la [mise en cache via un serveur web Apache et un CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). Par défaut, le contenu HTML est mis en cache pendant ~5 minutes.

1. Pour contourner le cache à des fins de test/débogage, ajoutez simplement un paramètre de requête tel que `?nocache=true`. L’URL ressemblerait à `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Vous trouverez plus d’informations sur la stratégie de mise en cache et les configurations disponibles [ici](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Vous pouvez également trouver l’URL du service de publication dans Cloud Manager. Accédez au **Programme Cloud Manager** > **Environnements** > **Environnement**.

   ![Afficher le service de publication](assets/author-content-publish/view-environment-segments.png)

   Sous **Segments d’environnement**, vous trouverez des liens vers les services **Auteur** et **Publier**.

## Félicitations !  {#congratulations}

Félicitations, vous venez de créer et de publier des modifications sur votre site AEM !

### Étapes suivantes {#next-steps}

Découvrez comment créer et modifier des [modèles de page](./page-templates.md). Comprendre la relation entre un modèle de page et une page. Découvrez comment configurer les stratégies d’un modèle de page afin de garantir une gouvernance granulaire et une cohérence de la marque pour le contenu.  Un modèle d’article de magazine bien structuré sera créé à partir d’une maquette provenant d’Adobe XD.
