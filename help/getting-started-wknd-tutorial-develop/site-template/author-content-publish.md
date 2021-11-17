---
title: Présentation de la création et de la publication | Création AEM site rapide
description: Utilisez l’éditeur de page d’Adobe Experience Manager AEM pour mettre à jour le contenu du site web. Découvrez comment les composants sont utilisés pour faciliter la création. Comprenez la différence entre les environnements d’auteur et de publication AEM et découvrez comment publier les modifications sur le site en direct.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '1364'
ht-degree: 3%

---

# Présentation de la création et de la publication {#author-content-publish}

>[!CAUTION]
>
> L’outil de création de site rapide est actuellement un aperçu technique. Il est mis à disposition à des fins de test et d’évaluation et n’est pas destiné à un usage en production sauf accord avec le support Adobe.

Il est important de comprendre comment un utilisateur va mettre à jour le contenu du site web. Dans ce chapitre, nous adopterons le personnage d’une **Auteur de contenu** et apporter quelques mises à jour éditoriales au site généré dans le chapitre précédent. À la fin du chapitre, nous publierons les modifications pour comprendre comment le site actif est mis à jour.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que les étapes décrites dans la section [Création d’un site](./create-site.md) Le chapitre a été terminé.

## Objectif {#objective}

1. Comprendre les concepts de **Pages** et **Composants** dans AEM Sites.
1. Découvrez comment mettre à jour le contenu du site web.
1. Découvrez comment publier des modifications sur le site actif.

## Créer une page {#create-page}

Un site web est généralement divisé en pages pour former une expérience multi-page. AEM structure le contenu de la même manière. Créez ensuite une page pour le site.

1. Connectez-vous à l’AEM **Auteur** Service utilisé dans le chapitre précédent.
1. Dans l’écran AEM Démarrer , cliquez sur **Sites** > **Site WKND** > **Anglais** > **Article**
1. Dans le coin supérieur droit, cliquez sur **Créer** > **Page**.

   ![Créer une page](assets/author-content-publish/create-page-button.png)

   Cela permettra d’augmenter le **Créer une page** assistant.

1. Choisissez la **Page Article** modèle et cliquez sur **Suivant**.

   Les pages d’AEM sont créées à partir d’un modèle de page. Les modèles de page seront détaillés dans la section [Modèles de page](page-templates.md) chapitre.

1. Sous **Propriétés** saisir une **Titre** de &quot;Hello World&quot;.
1. Définissez la variable **Nom** to `hello-world` et cliquez sur **Créer**.

   ![Propriétés de page initiales](assets/author-content-publish/initial-page-properties.png)

1. Dans la fenêtre contextuelle de la boîte de dialogue, cliquez sur **Ouvrir** pour ouvrir la page nouvellement créée.

## Création d’un composant {#author-component}

AEM Les composants peuvent être considérés comme de petits blocs de création modulaires d’une page web. En divisant l’interface utilisateur en blocs ou composants logiques, elle facilite la gestion. Pour réutiliser les composants, ceux-ci doivent pouvoir être configurés. Pour ce faire, utilisez la boîte de dialogue de création.

AEM fournit un ensemble de [Composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr) qui sont prêts à l’emploi en production. Le **Composants principaux** à partir des éléments de base tels que [Texte](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=fr) et [Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=fr) pour des éléments d’IU plus complexes comme une [Carrousel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=fr).

Créez ensuite quelques composants à l’aide de l’éditeur de page AEM.

1. Accédez au **Hello World** page créée dans l’exercice précédent.
1. Assurez-vous que vous êtes dans **Modifier** et, dans le rail latéral gauche, cliquez sur l’icône **Composants** icône .

   ![Rail latéral de l’éditeur de pages](assets/author-content-publish/page-editor-siderail.png)

   Cela ouvre la bibliothèque de composants et répertorie les composants disponibles qui peuvent être utilisés sur la page.

1. Faire défiler vers le bas et **Glisser-déposer** a **Texte (v2)** sur la zone principale modifiable de la page.

   ![Faire glisser et déposer le composant de texte](assets/author-content-publish/drag-drop-text-cmp.png)

1. Cliquez sur le bouton **Texte** pour mettre en surbrillance le composant, puis cliquez sur **clé à molette** icon ![Icône Forme](assets/author-content-publish/wrench-icon.png) pour ouvrir la boîte de dialogue du composant. Entrez du texte et enregistrez les modifications dans la boîte de dialogue.

   ![Composant de texte enrichi](assets/author-content-publish/rich-text-populated-component.png)

   Le **Texte** doit maintenant afficher le texte enrichi sur la page.

1. Répétez les étapes ci-dessus, sauf faites glisser une instance de la fonction **Image(v2)** sur la page. Ouvrez le **Image** de la boîte de dialogue du composant.

1. Dans le rail de gauche, passez à la **Outil de recherche de ressources** en cliquant sur le bouton **Ressources** icon ![icône de ressource](assets/author-content-publish/asset-icon.png).
1. **Glisser-déposer** une image dans la boîte de dialogue du composant, puis cliquez sur **Terminé** pour enregistrer les modifications.

   ![Ajouter une ressource à la boîte de dialogue](assets/author-content-publish/add-asset-dialog.png)

1. Notez qu’il existe des composants sur la page, comme la variable **Titre**, **Navigation**, **Rechercher** qui sont corrigés. Ces zones sont configurées dans le cadre du modèle de page et ne peuvent pas être modifiées sur une page individuelle. Ce point sera abordé plus en détail dans le prochain chapitre.

N’hésitez pas à tester d’autres composants. La documentation de chaque [Le composant principal se trouve ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). Une série vidéo détaillée sur [La création de pages est disponible ici](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Publier les mises à jour {#publish-updates}

Les environnements AEM sont fractionnés en un **Service de création** et un **Service de publication**. Dans ce chapitre, nous avons apporté plusieurs modifications au site sur la page **Service de création**. Pour que les visiteurs du site puissent afficher les modifications, nous devons les publier dans le **Service de publication**.

![Diagramme de haut niveau](assets/author-content-publish/author-publish-high-level-flow.png)

*Flux de contenu de haut niveau de l’auteur à la publication*

**1.** Les auteurs de contenu mettent à jour le contenu du site. Les mises à jour peuvent être prévisualisées, examinées et approuvées pour publication.

**2.** Le contenu est publié. La publication peut être effectuée à la demande ou programmée pour une date ultérieure.

**3.** Les modifications seront répercutées sur le service de publication pour les visiteurs du site.

### Publier les modifications

Publions ensuite les modifications.

1. Dans l’écran AEM Démarrer , accédez à **Sites** et sélectionnez la variable **Site WKND**.
1. Cliquez sur le bouton **Gérer la publication** dans la barre de menus.

   ![Gérer la publication](assets/author-content-publish/click-manage-publiciation.png)

   Comme il s’agit d’un tout nouveau site, nous voulons publier toutes les pages et nous pouvons utiliser l’assistant Gérer la publication pour définir exactement ce qui doit être publié.

1. Sous **Options** laissez les paramètres par défaut sur **Publier** et planifiez-le pour **Maintenant**. Cliquez sur **Suivant**.
1. Sous **Portée**, sélectionnez la variable **Site WKND** et cliquez sur **Paramètres d’inclusion des enfants**. Dans la boîte de dialogue, cochez **Inclure les enfants**. Décochez les autres cases pour vous assurer que l’intégralité du site est publiée.

   ![Mettre à jour le périmètre de publication](assets/author-content-publish/update-scope-publish.png)

1. Cliquez sur le bouton **Références publiées** bouton . Dans la boîte de dialogue, vérifiez que tout est coché. Cela inclut la variable **Modèle de site standard** et plusieurs configurations générées par le modèle de site. Cliquez sur **Terminé** pour mettre à jour.

   ![Publication de références](assets/author-content-publish/publish-references.png)

1. Enfin, cochez la case en regard de **Site WKND** et cliquez sur **Suivant** dans le coin supérieur droit.
1. Dans le **Workflows** , saisissez une **Titre du workflow**. Il peut s’agir de n’importe quel texte et s’avérer utile ultérieurement dans le cadre d’un suivi. Saisissez &quot;Publication initiale&quot;, puis cliquez sur **Publier**.

![Publication initiale de l’étape de workflow](assets/author-content-publish/workflow-step-publish.png)

## Affichage du contenu publié {#publish}

Ensuite, accédez au service de publication pour afficher les modifications.

1. Pour obtenir facilement l’URL du service de publication, copiez l’URL de création et remplacez la variable `author` word avec `publish`. Par exemple :

   * **URL de l&#39;auteur** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL de publication** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Ajouter `/content/wknd.html` à l’URL de publication afin que l’URL finale ressemble à : `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Modifier `wknd.html` pour correspondre au nom de votre site, si vous avez fourni un nom unique au cours de la [création de site](create-site.md).

1. En accédant à l’URL de publication, vous devriez voir le site, sans aucune fonctionnalité de création AEM.

   ![Site publié](assets/author-content-publish/publish-url-update.png)

1. En utilisant la variable **Navigation** menu **Article** > **Hello World** pour accéder à la page Hello World créée précédemment.
1. Revenez au **Service AEM Author** et apporter des modifications supplémentaires au contenu dans l’éditeur de page.
1. Publiez ces modifications directement depuis l’éditeur de page en cliquant sur le bouton **Propriétés de la page** icon > **Publier la page**

   ![publication directe](assets/author-content-publish/page-editor-publish.png)

1. Revenez au **Service de publication AEM** pour afficher les modifications. Il est probable que vous **not** consultez immédiatement les mises à jour. En effet, la variable **Service de publication AEM** inclut [mise en cache via un serveur web Apache et un réseau de diffusion de contenu](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). Par défaut, le contenu du HTML est mis en cache pendant ~5 minutes.

1. Pour contourner le cache à des fins de test/débogage, ajoutez simplement un paramètre de requête comme `?nocache=true`. L’URL ressemblerait à `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Plus d’informations sur la stratégie de mise en cache et les configurations disponibles [peut être consulté ici](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Vous pouvez également trouver l’URL du service de publication dans Cloud Manager. Accédez au **Programme Cloud Manager** > **Environnements** > **Environnement**.

   ![Afficher le service de publication](assets/author-content-publish/view-environment-segments.png)

   Sous **Segments d’environnement** vous trouverez des liens vers le **Auteur** et **Publier** services.

## Félicitations ! {#congratulations}

Félicitations, vous venez de créer et de publier des modifications sur votre site AEM !

### Étapes suivantes {#next-steps}

Dans une mise en oeuvre réelle, la planification d’un site avec des maquettes et des conceptions d’interface utilisateur précède généralement la création du site. Découvrez comment les Kits d’interface utilisateur d’Adobe XD peuvent être utilisés pour concevoir et accélérer votre mise en oeuvre Adobe Experience Manager Sites dans [Planification de l’interface utilisateur avec Adobe XD](./ui-planning-adobe-xd.md).

Vous souhaitez continuer à explorer les fonctionnalités d’AEM Sites ? N’hésitez pas à accéder directement au chapitre sur [Modèles de page](./page-templates.md) pour comprendre la relation entre un modèle de page et une page.


