---
title: Analyse des données avec Analysis Workspace
description: Découvrez comment mapper les données capturées d’un site Adobe Experience Manager aux mesures et aux dimensions dans les suites de rapports Adobe Analytics. Découvrez comment créer un tableau de bord de création de rapports détaillé à l’aide de la fonctionnalité Analysis Workspace d’Adobe Analytics.
feature: analyses
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6409
thumbnail: KT-6296.jpg
topic: Intégrations
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2204'
ht-degree: 1%

---


# Analyse des données avec Analysis Workspace

Découvrez comment mapper les données capturées d’un site Adobe Experience Manager aux mesures et aux dimensions dans les suites de rapports Adobe Analytics. Découvrez comment créer un tableau de bord de création de rapports détaillé à l’aide de la fonctionnalité Analysis Workspace d’Adobe Analytics.

## Ce que vous allez créer

L’équipe marketing WKND souhaite déterminer les boutons CTA (Appel à l’action) les plus performants sur la page d’accueil. Dans ce tutoriel, nous allons créer un projet dans Analysis Workspace afin de visualiser les performances de différents boutons CTA et de comprendre le comportement des utilisateurs sur le site. Les informations suivantes sont capturées à l’aide d’Adobe Analytics lorsqu’un utilisateur clique sur un bouton CTA (Appel à l’action) sur la page d’accueil WKND.

**Variables Analytics**

Vous trouverez ci-dessous les variables Analytics actuellement suivies :

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

![CTA Cliquez sur Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Objectifs {#objective}

1. Créez une suite de rapports ou utilisez une suite existante.
1. Configurez les [Variables de conversion (eVars)](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html) et [Événements de succès (Événements)](https://docs.adobe.com/help/en/analytics/admin/admin-tools/success-events/success-event.html) dans la suite de rapports.
1. Créez un [projet Analysis Workspace](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/home.html) pour analyser les données à l’aide d’outils qui vous permettent de créer, d’analyser et de partager rapidement des informations.
1. Partagez le projet Analysis Workspace avec d’autres membres de l’équipe.

## Prérequis

Ce tutoriel est la suite du composant [Suivi des clics avec Adobe Analytics](./track-clicked-component.md) et suppose que vous avez :

* Une **propriété Launch** avec l’extension [Adobe Analytics](https://docs.adobe.com/content/help/fr-FR/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) activée
* **Adobe de l’identifiant de suite de rapports** Analytics/dev et du serveur de suivi. Consultez la documentation suivante pour [créer une suite de rapports](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Extension de navigateur ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) DebuggerExperience Platform configurée avec votre propriété Launch chargée sur  [https://wknd.site/us/en.](https://wknd.site/us/en.html) htmlor un site AEM avec la couche de données d’Adobe activée.

## Variables de conversion (eVars) et événements de succès (événement)

La variable de conversion Custom Insight (ou eVar) est placée dans le code d’Adobe sur les pages web sélectionnées de votre site. Son Principal objectif est de segmenter les mesures de succès de conversion dans les rapports marketing personnalisés. Un eVar peut être basé sur les visites et fonctionner comme un cookie. Les valeurs transmises aux variables d’eVar suivent l’utilisateur pendant une période prédéfinie.

Lorsqu’un eVar est défini sur la valeur d’un visiteur, l’Adobe mémorise automatiquement cette valeur jusqu’à son expiration. Tous les événements de succès qu’un visiteur rencontre alors que la valeur de l’eVar est principale sont comptabilisés dans la valeur de l’eVar.

Les eVars sont mieux utilisées pour mesurer les causes et les effets, par exemple :

* Quelles sont les campagnes internes qui ont influencé les recettes ?
* Quelles bannières publicitaires ont finalement abouti à une inscription ?
* Nombre d’utilisations d’une recherche interne avant de passer une commande

Les événements de succès sont des actions dont le suivi peut être effectué. Vous déterminez ce qu’est un événement de succès. Par exemple, si un visiteur clique sur un bouton CTA, l’événement click peut être considéré comme un événement de succès.

### Configuration des eVars

1. Sur la page d’accueil de Adobe Experience Cloud, sélectionnez votre organisation et lancez Adobe Analytics.

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Dans la barre d’outils Analytics, cliquez sur **Admin** > **Suites de rapports** et recherchez votre suite de rapports.

   ![Suite de rapports Analytics](assets/create-analytics-workspace/select-report-suite.png)

1. Sélectionnez Suite de rapports > **Modifier les paramètres** > **Conversion** > **Variables de conversion**

   ![Variables de conversion Analytics](assets/create-analytics-workspace/conversion-variables.png)

1. À l’aide de l’option **Ajouter**, créons des variables de conversion pour mapper le schéma comme ci-dessous :

   * `eVar5` -   `Page Template`
   * `eVar6` -  `Page ID`
   * `eVar7` -  `Last Modified Date`
   * `eVar8` -  `Button Id`
   * `eVar9` -  `Page Name`

   ![Ajout de nouvelles eVars](assets/create-analytics-workspace/add-new-evars.png)

1. Attribuez un nom et une description appropriés à chaque eVar et **enregistrez** vos modifications. Nous allons utiliser ces eVars pour créer un projet Analysis Workspace dans la section suivante. Ainsi, un nom convivial rend les variables facilement détectables.

   ![eVars](assets/create-analytics-workspace/evars.png)

### Configuration des événements de succès

Ensuite, créons un événement pour suivre le clic sur le bouton CTA.

1. Dans la fenêtre **Gestionnaire de Report Suites** , sélectionnez l’**ID de Report Suite** et cliquez sur **Modifier les paramètres**.
1. Cliquez sur **Conversion** > **Événements de succès**
1. À l’aide de l’option **Ajouter nouveau** , créez un événement de succès personnalisé pour effectuer le suivi du clic sur le bouton CTA, puis **Enregistrez** vos modifications.
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVars](assets/create-analytics-workspace/add-success-event.png)

## Créez un projet dans Analysis Workspace {#workspace-project}

Analysis Workspace est un outil de navigation flexible qui vous permet de créer des analyses et de partager rapidement des informations. Grâce à l’interface par glisser-déposer, vous pouvez concevoir votre analyse, ajouter des visualisations pour donner vie aux données, organiser un jeu de données, partager et planifier des projets avec toute personne de votre entreprise.

Créez ensuite un nouveau [projet](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/t-freeform-project.html) pour créer un tableau de bord afin d’analyser les performances des boutons CTA sur l’ensemble du site.

1. Dans la barre d’outils Analytics, sélectionnez **Workspace** et cliquez sur **Créer un projet**.

   ![Workspace](assets/create-analytics-workspace/create-workspace.png)

1. Choisissez de commencer à partir d’un **projet vierge** ou sélectionnez l’un des modèles prédéfinis, fournis par Adobe ou des modèles personnalisés créés par votre organisation. Plusieurs modèles sont disponibles, selon l’analyse ou le cas d’utilisation que vous avez en tête. [En savoir ](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) plus sur les différentes options de modèle disponibles.

   Dans votre projet Workspace, les panneaux, les tableaux, les visualisations et les composants sont accessibles à partir du rail de gauche. Ce sont les blocs de construction de votre projet.

   * **[Composants](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)**  : les composants sont des dimensions, des mesures, des segments ou des périodes. Ils peuvent tous être combinés dans un tableau à structure libre pour commencer à répondre à la question que vous vous posez. Veillez à vous familiariser avec chaque type de composant avant de poursuivre votre analyse. Une fois que vous avez maîtrisé la terminologie des composants, vous pouvez commencer à faire glisser et à déposer vos analyses dans un tableau à structure libre.
   * **[Visualisations](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)**  : les visualisations, telles qu’un graphique à barres ou en courbes, sont ensuite ajoutées au-dessus des données pour les rendre visibles. Sur le rail de l’extrême gauche, sélectionnez l’icône Visualisations du milieu pour afficher la liste complète des visualisations disponibles.
   * **[Panneaux](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/panels/panels.html)**  : un panneau est un ensemble de tableaux et de visualisations. Vous pouvez accéder aux panneaux à partir de l’icône supérieure gauche dans Workspace. Les panneaux s’avèrent utiles lorsque vous souhaitez organiser vos projets en fonction des périodes, des suites de rapports ou des cas pratiques d’analyse. Les types de panneau suivants sont disponibles dans Analysis Workspace :

   ![Sélection de modèle](assets/create-analytics-workspace/workspace-tools.png)

### Ajout d’une visualisation de données avec Analysis Workspace

Créez ensuite un tableau afin de créer une représentation visuelle de la façon dont les utilisateurs interagissent avec les boutons CTA (Appel à l’action) sur la page d’accueil du site WKND. Pour créer une telle représentation, utilisons les données collectées dans le composant [Suivi des clics avec Adobe Analytics](./track-clicked-component.md). Vous trouverez ci-dessous un résumé rapide des données suivies pour les interactions des utilisateurs avec les boutons d’appel à l’action du site WKND.

* `eVar5` -   `Page template`
* `eVar6` -  `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

1. Faites glisser et déposez le composant de dimension **Page** sur le tableau à structure libre. Vous devriez maintenant pouvoir afficher une visualisation qui affiche le nom de page (eVar9) et les pages vues correspondantes (occurrences) affichées dans le tableau.

   ![Dimension de page](assets/create-analytics-workspace/evar9-dimension.png)

1. Faites glisser la mesure **Cliquer CTA** (event8) sur la mesure d’occurrences et remplacez-la. Vous pouvez désormais afficher une visualisation qui affiche le nom de page (eVar9) et un nombre correspondant d’événements de clic CTA sur une page.

   ![Mesure de page - Clics CTA](assets/create-analytics-workspace/evar8-cta-click.png)

1. Ventilez la page par type de modèle. Sélectionnez la mesure de modèle de page à partir de composants, puis faites glisser-déposez la mesure Modèle de page sur la dimension Nom de page . Vous pouvez désormais afficher le nom de la page ventilé selon son type de modèle.

   * **Avant**

      ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **Après**

      ![Mesures eVar5](assets/create-analytics-workspace/evar5-metrics.png)

1. Pour comprendre comment les utilisateurs interagissent avec les boutons CTA lorsqu’ils se trouvent sur les pages du site WKND, nous devons ventiler davantage la mesure Modèle de page en ajoutant la mesure ID de bouton (eVar8).

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. Vous trouverez ci-dessous une représentation visuelle du site WKND ventilée par modèle de page et plus précisément par interaction de l’utilisateur avec les boutons Cliquer pour agir (CTA) du site WKND.

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Vous pouvez remplacer la valeur ID de bouton par un nom plus convivial à l’aide des classifications Adobe Analytics. Vous pouvez en savoir plus sur la création d’une classification pour une mesure spécifique [ici](https://docs.adobe.com/content/help/en/analytics/components/classifications/c-classifications.html). Dans ce cas, nous avons une mesure de classification `Button Section (Button ID)` configurée pour `eVar8` qui mappe l’ID de bouton à un nom convivial.

   ![Section Bouton](assets/create-analytics-workspace/button-section.png)

## Ajouter une classification à une variable Analytics

### Classifications des conversions

La classification d’Analytics permet de classer les données de variable d’Analytics, puis de les afficher de différentes manières lors de la génération des rapports. Pour améliorer l’affichage de l’identifiant de bouton dans le rapport Analysis Workspace, créons une variable de classification pour l’identifiant de bouton (eVar8). Lors de la classification, vous établissez une relation entre la variable et les métadonnées associées à cette variable.

Créez ensuite une classification pour la variable Analytics.

1. Dans le menu de la barre d’outils **Admin**, sélectionnez **Suites de rapports**
1. Sélectionnez **Identifiant de suite de rapports** dans la fenêtre **Gestionnaire de suites de rapports** et cliquez sur **Modifier les paramètres** > **Conversion** > **Classifications des conversions**

   ![Classification des conversions](assets/create-analytics-workspace/conversion-classification.png)

1. Dans la liste déroulante **Sélectionner le type de classification** , sélectionnez la variable (ID de bouton eVar8) pour ajouter une classification.
1. Cliquez sur la flèche située à droite de la variable Classification répertoriée sous la section Classifications pour ajouter une nouvelle classification.

   ![Type de classification de conversion](assets/create-analytics-workspace/select-classification-variable.png)

1. Dans la boîte de dialogue **Modifier une classification**, indiquez un nom approprié pour la classification de texte. Un composant de dimension portant le nom Classification de texte est créé.

   ![Type de classification de conversion](assets/create-analytics-workspace/new-classification.png)

1. **Enregistrez vos modifications.**

### Importateur de classifications

Utilisez l’importateur pour télécharger des classifications dans Adobe Analytics. Vous pouvez également exporter les données pour les mettre à jour avant un import. Les données que vous importez à l’aide de l’outil d’importation doivent être dans un format spécifique. Adobe vous offre la possibilité de télécharger un modèle de données contenant tous les détails d’en-tête appropriés dans un fichier de données délimité par des tabulations. Vous pouvez ajouter vos nouvelles données à ce modèle, puis importer le fichier de données dans le navigateur par FTP.

#### Modèle de classification

Avant d’importer des classifications dans des rapports marketing, vous pouvez télécharger un modèle qui vous aide à créer un fichier de données de classification. Le fichier de données utilise les classifications voulues sous forme d’en-têtes de colonne, puis classe le jeu de données de rapport sous les en-têtes de classification appropriés.

Téléchargeons ensuite le modèle de classification pour la variable d’identifiant de bouton (eVar8).

1. Accédez à **Admin** > **Importateur de classifications**
1. Téléchargeons un modèle de classification pour la variable de conversion à partir de l’onglet **Télécharger le modèle**.
   ![Type de classification de conversion](assets/create-analytics-workspace/classification-importer.png)

1. Dans l’onglet Télécharger le modèle , spécifiez la configuration du modèle de données.
   * **Sélectionner une suite de rapports**  : Sélectionnez la suite de rapports à utiliser dans le modèle. La suite de rapports et le jeu de données doivent correspondre.
   * **Données à classifier**  : Sélectionnez le type de données pour le fichier de données. Le menu comprend tous les rapports des suites de rapports qui sont configurés pour les classifications.
   * **Encodage**  : Sélectionnez le codage des caractères pour le fichier de données. Le format de codage par défaut est UTF-8.

1. Cliquez sur **Télécharger** et enregistrez le fichier de modèle sur votre système local. Le fichier de modèle est un fichier de données délimité par des tabulations (extension de nom de fichier .tab ) qui est pris en charge par la plupart des tableurs.
1. Ouvrez le fichier de données délimité par des tabulations à l’aide d’un éditeur de votre choix.
1. Ajoutez l’ ID de bouton (eVar9) et un nom de bouton correspondant au fichier délimité par des tabulations pour chaque valeur eVar9 de l’ Étape 9 de la section .

   ![Valeur clé](assets/create-analytics-workspace/key-value.png)

1. **** Enregistrez le fichier délimité par des tabulations.
1. Accédez à l’onglet **Importer un fichier** .
1. Configurez la destination de l’importation du fichier.
   * **Sélectionner une suite de rapports**  : AEM de site WKND (suite de rapports)
   * **Données à classer**  : Id De Bouton (Variable De Conversion eVar8)
1. Cliquez sur l’option **Choisir un fichier** pour télécharger le fichier délimité par des tabulations depuis votre système, puis cliquez sur **Importer un fichier**.

   ![Importateur de fichiers](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Une importation réussie affiche immédiatement les modifications appropriées dans une exportation. Toutefois, les modifications des données dans les rapports peuvent prendre jusqu’à quatre heures lors de l’utilisation d’une importation de navigateur et jusqu’à 24 heures lors de l’utilisation d’une importation FTP.

#### Remplacer la variable de conversion par la variable de classification

1. Dans la barre d’outils Analytics, sélectionnez **Workspace** et ouvrez l’espace de travail que nous avons créé dans la section [Créer un projet dans Analysis Workspace](#workspace-project) de ce tutoriel.

   ![ID de bouton de l’espace de travail](assets/create-analytics-workspace/workspace-report-button-id.png)

1. Ensuite, remplacez la mesure **Id de bouton** de votre espace de travail qui affiche l’identifiant d’un bouton CTA (Appel à l’action) par le nom de classification créé à l’étape précédente.

1. Dans l’outil de recherche de composant, recherchez **Boutons CTA WKND** et faites glisser et déposez la dimension **Boutons CTA WKND (ID de bouton)** sur la mesure ID de bouton et remplacez-la.

   * **Avant**

      ![Bouton Espace de travail avant](assets/create-analytics-workspace/wknd-button-before.png)
   * **Après**

      ![Bouton Espace de travail après](assets/create-analytics-workspace/wknd-button-after.png)

1. Vous pouvez remarquer que la mesure d’identifiant de bouton qui contenait l’identifiant de bouton d’un bouton CTA (Appel à l’action) est désormais remplacée par un nom correspondant fourni dans le modèle de classification.
1. Comparons le tableau de l’espace de travail Analytics à la page d’accueil WKND et comprenons le nombre de clics sur le bouton CTA et son analyse. Sur la base des données de tableau à structure libre de l’espace de travail, il est clair que 22 fois les utilisateurs ont cliqué sur le bouton **SKI NOW** et quatre fois pour le bouton Campagne de page d’accueil WKND en Australie occidentale **En savoir plus** .

   ![Rapport CTA](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Veillez à enregistrer votre projet Adobe Analytics Workspace et fournissez un nom et une description appropriés. Vous pouvez éventuellement ajouter des balises à un projet Workspace.

   ![Enregistrer le projet](assets/create-analytics-workspace/save-project.png)

1. Une fois votre projet enregistré, vous pouvez le partager avec d’autres collègues ou collègues à l’aide de l’option Partager .

   ![Partager le projet](assets/create-analytics-workspace/share.png)

## Félicitations ! 

Vous venez d’apprendre à mapper les données capturées d’un site Adobe Experience Manager aux mesures et dimensions des suites de rapports Adobe Analytics, à effectuer une classification pour les mesures et à créer un tableau de bord de création de rapports détaillé à l’aide de la fonction Analysis Workspace d’Adobe Analytics.

