---
title: Présentation de la création et de la publication | Création rapide de site AEM
description: Utilisez l’éditeur de page d’Adobe Experience Manager (AEM) pour mettre à jour le contenu du site web. Découvrez comment les composants sont utilisés pour faciliter la création. Comprenez la différence entre les environnements de création et de publication AEM et découvrez comment publier les modifications sur le site en ligne.
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 350
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 100%

---

# Présentation de la création et de la publication {#author-content-publish}

Il est important de comprendre comment un utilisateur ou une utilisatrice va mettre à jour le contenu du site web. Dans ce chapitre, le rôle de notre persona est de **créer du contenu** et nous apporterons quelques mises à jour éditoriales au site généré dans le chapitre précédent. À la fin du chapitre, nous publierons les modifications pour comprendre comment le site en ligne est mis à jour.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que les étapes décrites dans le chapitre [Créer un site](./create-site.md) sont terminées.

## Objectif {#objective}

1. Comprendre les concepts de **Pages** et **Composants** dans AEM Sites.
1. Découvrez comment mettre à jour le contenu du site web.
1. Découvrez comment publier des modifications sur le site en ligne.

## Créer une page {#create-page}

Un site web est généralement divisé en pages pour former une expérience sur plusieurs pages. AEM structure le contenu de la même manière. Créez ensuite une page de site.

1. Connectez-vous au service de **création** AEM utilisé dans le chapitre précédent.
1. Dans l’écran de démarrage AEM, cliquez sur **Sites** > **Site WKND** > **Anglais** > **Article**
1. Dans le coin supérieur droit, cliquez sur **Créer** > **Page**.

   ![Création de page.](assets/author-content-publish/create-page-button.png)

   L’assistant **Créer une page** s’affiche.

1. Sélectionnez le modèle de **Page d’article** et cliquez sur **Suivant**.

   Les pages d’AEM sont créées à partir d’un modèle de page. Les modèles de page sont détaillés dans le chapitre [Modèles de page](page-templates.md).

1. Sous **Propriétés**, saisissez un **Titre**, « Hello World ».
1. Définissez le **Nom** sur `hello-world` et cliquez sur **Créer**.

   ![Propriétés de page initiales.](assets/author-content-publish/initial-page-properties.png)

1. Dans la boîte de dialogue contextuelle, cliquez sur **Ouvrir** pour ouvrir la page nouvellement créée.

## Créer un composant {#author-component}

Les composants AEM peuvent être considérés comme de petits blocs de création modulaires d’une page Web. En divisant l’interface utilisateur en blocs ou composants logiques, la gestion est facilitée. Pour réutiliser les composants, ceux-ci doivent pouvoir être configurés. Pour ce faire, utilisez la boîte de dialogue de création.

AEM fournit un ensemble de [Composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr) qui sont prêts à l’emploi en production. Les **Composants principaux** vont des éléments de base tels que [Texte](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=fr) et [Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=fr) aux éléments d’IU plus complexes tels qu’un [Carrousel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=fr).

Créez ensuite quelques composants à l’aide de l’éditeur de page AEM.

1. Accédez à la page **Hello World** créée dans l’exercice précédent.
1. Assurez-vous que vous êtes en mode **Modifier** et, dans le rail latéral gauche, cliquez sur l’icône **Composants**.

   ![Rail latéral de l’éditeur de page.](assets/author-content-publish/page-editor-siderail.png)

   Vous ouvrez ainsi la bibliothèque de composants et répertoriez les composants disponibles qui peuvent être utilisés sur la page.

1. Faites défiler vers le bas et effectuez un **Glisser-déposer** d’un composant **Texte (v2)** sur la zone modifiable principale de la page.

   ![Glisser-déposer d’un composant de texte.](assets/author-content-publish/drag-drop-text-cmp.png)

1. Cliquez sur le composant de **texte** pour le mettre en surbrillance, puis cliquez sur l’icône **Clé à molette** ![Icône Clé à molette](assets/author-content-publish/wrench-icon.png) pour ouvrir la boîte de dialogue du composant. Saisissez du texte et enregistrez les modifications dans la boîte de dialogue.

   ![Composant de texte enrichi.](assets/author-content-publish/rich-text-populated-component.png)

   Le composant de **texte** doit maintenant afficher le texte enrichi sur la page.

1. Répétez les étapes ci-dessus, sauf faire glisser une instance du composant **Image(v2)** sur la page. Ouvrez la boîte de dialogue du composant **Image**.

1. Dans le rail de gauche, passez à l’**outil de recherche de ressources** en cliquant sur l’icône **Ressources** ![icône de ressource](assets/author-content-publish/asset-icon.png).
1. **Glissez-déposez** une image dans la boîte de dialogue du composant, puis cliquez sur **Terminé** pour enregistrer les modifications.

   ![Ajout d’une ressource à la boîte de dialogue.](assets/author-content-publish/add-asset-dialog.png)

1. Notez qu’il existe des composants corrigés sur la page, comme le **Titre**, la **Navigation** ou **Search**. Ces domaines sont configurés dans le cadre du modèle de page et ne peuvent pas être modifiés sur une page individuelle. Ce point est abordé plus en détail dans le chapitre suivant.

N’hésitez pas à tester d’autres composants. La documentation de chaque [composant principal se trouve ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr). Une série vidéo détaillée sur [la création de page est disponible ici](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html?lang=fr).

## Publier des mises à jour {#publish-updates}

Les environnements AEM sont répartis entre un **Service de création** et un **Service de publication**. Dans ce chapitre, nous avons apporté plusieurs modifications au site sur le **Service de création**. Pour que les visiteurs et visiteuses du site puissent visualiser les modifications, nous devons les publier dans le **Service de publication**.

![Diagramme de haut niveau.](assets/author-content-publish/author-publish-high-level-flow.png)

*Flux de contenu de haut niveau du service de création au service de publication*

**1.** Les auteurs et autrices de contenu mettent à jour le contenu du site. Les mises à jour peuvent être prévisualisées, examinées et approuvées pour être mises en ligne.

**2.** Le contenu est publié. La publication peut être effectuée à la demande ou programmée pour une date ultérieure.

**3.** Les modifications seront répercutées sur le service de publication pour les visiteurs et visiteuses du site.

### Publier les modifications

Publions ensuite les modifications.

1. Dans l’écran AEM Démarrer, accédez aux **Sites** et sélectionnez le **Site WKND**.
1. Cliquez sur le bouton **Gérer la publication** dans la barre de menus.

   ![Gérer la publication.](assets/author-content-publish/click-manage-publiciation.png)

   Comme il s’agit d’un tout nouveau site, nous voulons publier toutes les pages et nous pouvons utiliser l’assistant de gestion de publication pour définir exactement ce qui doit être publié.

1. Sous **Options**, laissez les paramètres par défaut sur **Publier** et planifiez la publication sur **Maintenant**. Cliquez sur **Suivant**.
1. Sous **Portée**, sélectionnez le **Site WKND** et cliquez sur **Inclure les paramètres enfants**. Dans la boîte de dialogue, cochez **Inclure les enfants**. Décochez les autres cases pour vous assurer que l’intégralité du site est publiée.

   ![Mise à jour de la portée de publication.](assets/author-content-publish/update-scope-publish.png)

1. Cliquez sur le bouton **Références publiées**. Dans la boîte de dialogue, vérifiez que tout est coché. Cela inclut le **Modèle de site standard** et plusieurs configurations générées par le modèle de site. Cliquez sur **Terminé** pour mettre à jour.

   ![Références de publication.](assets/author-content-publish/publish-references.png)

1. Enfin, cochez la case à côté de **Site WKND** et cliquez sur **Suivant** dans le coin supérieur droit.
1. Dans l’étape **Workflows**, spécifiez un **titre de workflow**. Il peut s’agir de n’importe quel texte et peut s’avérer utile ultérieurement dans le cadre d’un journal d’audit. Saisissez « Publication initiale », puis cliquez sur **Publier**.

![Publication initiale de l’étape de workflow.](assets/author-content-publish/workflow-step-publish.png)

## Afficher le contenu publié {#publish}

Ensuite, accédez au service de publication pour afficher les modifications.

1. Pour obtenir facilement l’URL du service de publication, copiez l’URL de création et remplacez le mot `author` par `publish`. Par exemple :

   * **URL de création** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL de publication** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Ajoutez `/content/wknd.html` à l’URL de publication afin que l’URL finale ressemble à : `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Modifiez `wknd.html` pour correspondre au nom de votre site, si vous avez fourni un nom unique au cours de la [création de site](create-site.md).

1. En accédant à l’URL de publication, vous devriez voir le site, sans aucune fonctionnalité de création AEM.

   ![Site publié.](assets/author-content-publish/publish-url-update.png)

1. Dans le menu de **Navigation**, cliquez sur **Article** > **Hello World** pour accéder à la page Hello World créée précédemment.
1. Revenez au **Service de création AEM** et apportez des modifications supplémentaires au contenu dans l’éditeur de page.
1. Publiez ces modifications directement depuis l’éditeur de page en cliquant sur l’icône **Propriétés de la page** > **Publier la page**.

   ![Publication directe.](assets/author-content-publish/page-editor-publish.png)

1. Revenez au **Service de publication AEM** pour afficher les modifications. Il est probable que vous ne puissiez **pas** consulter immédiatement les mises à jour. En effet, le **Service de publication AEM** inclut une [mise en cache via un serveur web Apache et un réseau CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html?lang=fr). Par défaut, le contenu du HTML est mis en cache pendant environ 5 minutes.

1. Pour contourner le cache à des fins de test/débogage, ajoutez simplement un paramètre de requête tel que `?nocache=true`. L’URL ressemble à ceci : `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Plus d’informations sur la stratégie de mise en cache et les configurations disponibles [peuvent être consultées ici](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html?lang=fr).

1. Vous pouvez également trouver l’URL du service de publication dans Cloud Manager. Accédez à **Programme Cloud Manager** > **Environnements** > **Environnement**.

   ![Affichage du service de publication.](assets/author-content-publish/view-environment-segments.png)

   Sous **Segments d’environnement**, vous trouverez des liens vers les services de **création** et de **publication**.

## Félicitations. {#congratulations}

Félicitations, vous venez de créer et de publier des modifications sur votre site AEM.

### Étapes suivantes {#next-steps}

Dans une implémentation réelle, la planification du site avec des maquettes et des conceptions d’interface utilisateur précède généralement sa création. Découvrez comment les Kits d’interface utilisateur d’Adobe XD peuvent être utilisés pour concevoir et accélérer votre implémentation Adobe Experience Manager Sites dans [Planifier l’interface utilisateur avec Adobe XD](./ui-planning-adobe-xd.md).

Vous souhaitez continuer à explorer les fonctionnalités d’AEM Sites ? N’hésitez pas à accéder directement au chapitre sur les [Modèles de page](./page-templates.md) pour comprendre la relation entre un modèle de page et une page.


