---
title: Analyser des données avec Analysis Workspace
description: Découvrez comment mapper les données capturées d’un site Adobe Experience Manager aux mesures et aux dimensions dans les suites de rapports Adobe Analytics. Découvrez comment créer un tableau de bord de rapports détaillé à l’aide de la fonctionnalité Analysis Workspace d’Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
jira: KT-6409
thumbnail: KT-6296.jpg
doc-type: Tutorial
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="Intégration" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
duration: 601
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2072'
ht-degree: 100%

---

# Analyser des données avec Analysis Workspace

Découvrez comment mapper les données capturées d’un site Adobe Experience Manager aux mesures et aux dimensions dans les suites de rapports Adobe Analytics. Découvrez comment créer un tableau de bord de rapports détaillé à l’aide de la fonctionnalité Analysis Workspace d’Adobe Analytics.

## Ce que vous allez créer {#what-build}

L’équipe marketing WKND souhaite savoir quels boutons `Call to Action (CTA)` sont les plus performants sur la page d’accueil. Dans ce tutoriel, créez un projet dans **Analysis Workspace** pour visualiser les performances des différents boutons CTA et comprendre le comportement des utilisateurs et utilisatrices sur le site. Les informations suivantes sont capturées à l’aide d’Adobe Analytics lorsqu’un utilisateur ou une utilisatrice clique sur un bouton CTA (Appel à l’action) sur la page d’accueil WKND.

**Variables Analytics**

Vous trouverez ci-dessous les variables Analytics qui font l’objet d’un suivi :

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![Clic CTA Adobe Analytics.](assets/create-analytics-workspace/page-analytics.png)

### Objectifs {#objective}

1. Créez une suite de rapports ou utilisez-en une existante.
1. Configurez les [variables de conversion (eVars)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=fr) et les [événements de succès (événements)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html?lang=fr) dans la suite de rapports.
1. Créez un [projet Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=fr) pour analyser les données à l’aide d’outils qui vous permettent de créer, d’analyser et de partager rapidement des informations.
1. Partagez le projet Analysis Workspace avec les autres personnes membres de l’équipe.

## Conditions préalables

Ce tutoriel est la suite de [Suivi des composants cliqués avec Adobe Analytics](./track-clicked-component.md) et suppose que vous disposez des éléments suivants :

* **Propriété de balise** avec l’[extension Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=fr) activée.
* Identifiant de suite de rapports test/dev et serveur de suivi **Adobe Analytics**. Consultez la documentation suivante pour la [création d’une suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=fr).
* Extension de navigateur [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=fr) configurée avec une propriété de balise chargée sur le [site WKND](https://wknd.site/fr/fr.html) ou un site AEM avec la couche de données Adobe activée.

## Variables de conversion (eVars) et événements de succès (événement)

La variable de conversion (ou eVar) Custom Insight est placée dans le code d’Adobe sur les pages web sélectionnées de votre site. Son principal objectif est de segmenter les mesures de succès de conversion dans les rapports marketing personnalisés. Une eVar peut être basée sur les visites et fonctionne comme un cookie. Les valeurs transmises aux variables eVar suivent l’utilisateur ou l’utilisatrice pendant une période prédéfinie.

Lorsqu’une eVar est définie sur la valeur d’un visiteur ou d’une visiteuse, Adobe mémorise automatiquement cette valeur jusqu’à son expiration. Tous les événements de succès qu’un visiteur ou une visiteuse rencontre alors que la valeur de l’eVar est active sont comptabilisés dans la valeur de l’eVar.

Les eVars servent principalement à mesurer les causes et les effets, par exemple :

* Quelles campagnes internes ont influencé les recettes
* Quelles bannières publicitaires ont finalement abouti à une inscription
* Nombre d’utilisations d’une recherche interne avant de passer une commande

Les événements de succès sont des actions qui peuvent faire l’objet d’un suivi. Vous déterminez ce qu’est un événement de succès. Par exemple, si un visiteur ou une visiteuse clique sur un bouton CTA, l’événement de clic peut être considéré comme un événement de succès.

### Configurer les eVars

1. Sur la page d’accueil d’Adobe Experience Cloud, sélectionnez votre organisation et lancez Adobe Analytics.

   ![AEP Analytics.](assets/create-analytics-workspace/analytics-aep.png)

1. Dans la barre d’outils Analytics, cliquez sur **Admin** > **Suites de rapports** et recherchez votre suite de rapports.

   ![Suite de rapports Analytics.](assets/create-analytics-workspace/select-report-suite.png)

1. Sélectionnez Suite de rapports > **Modifier les paramètres** > **Conversion** > **Variables de conversion**.

   ![Variables de conversion Analytics.](assets/create-analytics-workspace/conversion-variables.png)

1. Avec l’option **Ajouter**, créons des variables de conversion pour mapper le schéma comme ci-dessous :

   * `eVar5` - `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![Ajout de nouvelles eVars.](assets/create-analytics-workspace/add-new-evars.png)

1. Attribuez un nom et une description appropriés à chaque eVar et cliquez sur **Enregistrer**. Dans le projet Analysis Workspace, on utilise les eVars portant les noms appropriés. Par conséquent, un nom convivial rend les variables facilement détectables.

   ![eVars.](assets/create-analytics-workspace/evars.png)

### Configurer des événements de succès

Créez ensuite un événement pour suivre les clics sur le bouton CTA.

1. Dans la fenêtre du **Gestionnaire de suites de rapports**, sélectionnez l’**Identifiant de suite de rapports** et cliquez sur **Modifier les paramètres**.
1. Cliquez sur **Conversion** > **Événements de succès**.
1. En utilisant l’option **Ajouter**, créez un événement de succès personnalisé pour effectuer le suivi des clics sur le bouton CTA, puis cliquez sur **Enregistrer**.
   * `Event` : `event8`
   * `Name` :`CTA Click`
   * `Type` :`Counter`

   ![eVars.](assets/create-analytics-workspace/add-success-event.png)

## Créer un projet dans Analysis Workspace {#workspace-project}

Analysis Workspace est un outil de navigation flexible qui vous permet de créer des analyses et de partager rapidement des informations. Grâce à l’interface par glisser-déposer, vous pouvez concevoir votre analyse, ajouter des visualisations pour donner vie aux données, organiser un jeu de données, partager et planifier des projets avec toute personne de votre entreprise.

Créez ensuite un [projet](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html?lang=fr) pour créer un tableau de bord pour analyser les performances des boutons CTA sur l’ensemble du site.

1. Dans la barre d’outils Analytics, sélectionnez **Workspace** et cliquez sur **Créer un projet**.

   ![Workspace.](assets/create-analytics-workspace/create-workspace.png)

1. Commencez à partir d’un **Projet vierge** ou sélectionnez l’un des modèles prédéfinis fournis par Adobe ou l’un des modèles personnalisés créés par votre entreprise. Plusieurs modèles sont disponibles, selon l’analyse ou votre cas d’utilisation. [En savoir plus](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html?lang=fr) à propos des différentes options de modèle disponibles.

   Dans votre projet Workspace, les panneaux, tableaux, visualisations et composants sont accessibles à partir du rail de gauche. Ils constituent des blocs de création pour votre projet.

   * **[Composants](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html?lang=fr)** : les composants sont des dimensions, des mesures, des segments ou des périodes. Ils peuvent tous être combinés dans un tableau à structure libre pour répondre à vos besoins professionnels. Veillez à vous familiariser avec chaque type de composant avant de poursuivre votre analyse. Une fois que vous maîtrisez la terminologie des composants, vous pouvez commencer à effectuer des actions de glisser-déposer pour créer votre analyse dans un tableau à structure libre.
   * **[Visualisations](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html?lang=fr)** : les visualisations, telles qu’un histogramme ou un graphique linéaire, sont ensuite ajoutées au-dessus des données pour leur donner vie. Sur le rail tout à gauche, sélectionnez l’icône Visualisations du milieu pour afficher la liste complète des visualisations disponibles.
   * **[Panneaux](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html?lang=fr)** : un panneau est un ensemble de tableaux et de visualisations. Vous pouvez accéder aux panneaux à partir de l’icône supérieure gauche dans Workspace. Les panneaux s’avèrent utiles lorsque vous souhaitez organiser vos projets en fonction des périodes, des suites de rapports ou des cas d’utilisation d’analyse. Les types de panneau suivants sont disponibles dans Analysis Workspace :

   ![Sélection de modèle.](assets/create-analytics-workspace/workspace-tools.png)

### Ajouter une visualisation de données avec Analysis Workspace

Créez ensuite un tableau pour créer une représentation visuelle de la manière dont les utilisateurs et utilisatrices interagissent avec les boutons `Call to Action (CTA)` sur la page d’accueil du site WKND. Pour créer une telle représentation, utilisons les données collectées dans le [composant Suivre les éléménts cliqués avec Adobe Analytics](./track-clicked-component.md). Vous trouverez ci-dessous un résumé rapide des données suivies pour les interactions des utilisateurs et utilisatrices avec les boutons d’appel à l’action du site WKND.

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. Faites glisser le composant de dimension **Page** sur le tableau à structure libre. Vous devriez maintenant pouvoir afficher une visualisation qui affiche le nom de page (eVar9) et les pages vues correspondantes (occurrences) affichées dans le tableau.

   ![Dimension Page.](assets/create-analytics-workspace/evar9-dimension.png)

1. Faites glisser la mesure **Clics CTA** (event8) sur la mesure des occurrences et remplacez-la. Vous pouvez désormais afficher une visualisation qui affiche le nom de page (eVar9) et un nombre correspondant d’événements de clics CTA.

   ![Mesure de page - Clics CTA.](assets/create-analytics-workspace/evar8-cta-click.png)

1. Ventilez la page selon son type de modèle. Sélectionnez la mesure de modèle de page à partir de composants, puis faites glisser et déposez la mesure Modèle de page sur la dimension Nom de page. Vous pouvez désormais afficher le nom de la page ventilé selon son type de modèle.

   * **Avant**
     ![eVar5.](assets/create-analytics-workspace/evar5.png)

   * **Après**
     ![Mesures eVar5.](assets/create-analytics-workspace/evar5-metrics.png)

1. Pour comprendre comment les personnes interagissent avec les boutons CTA lorsqu’elles se trouvent sur les pages du site WKND, une ventilation plus détaillée en ajoutant la mesure ID de bouton (eVar8) est nécessaire.

   ![eVar8.](assets/create-analytics-workspace/evar8.png)

1. Vous trouverez ci-dessous une représentation visuelle du site WKND ventilée selon son modèle de page et plus précisément par interaction des personnes avec les boutons CTA (appel à l’action) du site WKND.

   ![eVar8.](assets/create-analytics-workspace/evar8-metric.png)

1. Vous pouvez remplacer la valeur ID de bouton par un nom plus convivial à l’aide des classifications Adobe Analytics. Vous pouvez en apprendre plus sur la création d’une classification pour une mesure spécifique [ici](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html?lang=fr). Dans ce cas, nous avons une mesure de classification `Button Section (Button ID)` pour `eVar8` qui mappe l’identifiant du bouton à un nom convivial.

   ![Section Bouton.](assets/create-analytics-workspace/button-section.png)

## Ajouter une classification à une variable Analytics

### Classifications des conversions

La classification d’Analytics permet de classer les données de variable d’Analytics, puis de les afficher de différentes manières lors de la génération des rapports. Pour améliorer l’affichage de l’identifiant de bouton dans le rapport Analysis Workspace, créons une variable de classification pour l’identifiant de bouton (eVar8). Lors de la classification, vous établissez une relation entre la variable et les métadonnées associées à cette variable.

Créez ensuite une classification pour la variable Analytics.

1. Dans le menu de la barre d’outils **Administration**, sélectionnez **Suites de rapports**.
1. Sélectionnez l’**Identifiant de suite de rapports** de la fenêtre **Gestionnaire de suites de rapports**, puis cliquez sur **Modifier les paramètres** > **Conversion** > **Classifications des conversions**.

   ![Classification des conversions.](assets/create-analytics-workspace/conversion-classification.png)

1. Dans la liste déroulante **Sélectionner le type de classification**, sélectionnez la variable (eVar8-Identifiant de bouton) pour ajouter une classification.
1. Cliquez sur la flèche située à droite de la variable Classification répertoriée sous la section Classifications pour ajouter une nouvelle classification.

   ![Type de classification de conversion.](assets/create-analytics-workspace/select-classification-variable.png)

1. Dans la boîte de dialogue **Modifier une classification**, attribuez un nom approprié à la classification de texte. Un composant de dimension portant le nom Classification de texte est créé.

   ![Type de classification de conversion.](assets/create-analytics-workspace/new-classification.png)

1. **Enregistrez** vos modifications.

### Importateur de classifications

Utilisez l’importateur pour charger des classifications dans Adobe Analytics. Vous pouvez également exporter les données pour les mettre à jour avant un import. Les données que vous importez à l’aide de l’outil d’import doivent être dans un format spécifique. Adobe vous offre la possibilité de télécharger un modèle de données contenant tous les détails d’en-tête appropriés dans un fichier de données délimité par des tabulations. Vous pouvez ajouter vos nouvelles données à ce modèle, puis importer le fichier de données dans le navigateur à l’aide de FTP.

#### Modèle de classification

Avant d’importer des classifications dans des rapports marketing, vous pouvez télécharger un modèle qui vous aide à créer un fichier de données de classification. Le fichier de données utilise les classifications voulues sous forme d’en-têtes de colonne, puis classe le jeu de données de rapport sous les en-têtes de classification appropriés.

Téléchargeons ensuite le modèle de classification pour la variable d’identifiant de bouton (eVar8).

1. Accédez à **Admin** > **Importateur de classifications**.
1. Téléchargeons un modèle de classification pour la variable de conversion à partir de l’onglet **Télécharger le modèle**.
   ![Type de classification de conversion.](assets/create-analytics-workspace/classification-importer.png)

1. Dans l’onglet Télécharger le modèle, spécifiez la configuration du modèle de données.
   * **Sélectionner une suite de rapports** : sélectionnez la suite de rapports à utiliser dans le modèle. La suite de rapports et le jeu de données doivent correspondre.
   * **Jeu de données à classer** : sélectionnez le type de données pour le fichier de données. Le menu comprend tous les rapports des suites de rapports qui sont configurés pour les classifications.
   * **Encodage** : sélectionnez le codage des caractères pour le fichier de données. Le format de codage par défaut est UTF-8.

1. Cliquez sur **Télécharger** et enregistrez le fichier de modèle sur votre système local. Le fichier de modèle est un fichier de données délimité par des tabulations (extension de nom de fichier .tab) qui est pris en charge par la plupart des tableurs.
1. Ouvrez le fichier de données délimité par des tabulations à l’aide d’un éditeur de votre choix.
1. Ajoutez l’identifiant de bouton (eVar9) et un nom de bouton correspondant au fichier délimité par des tabulations pour chaque valeur eVar9 de l’étape 9 de la section.

   ![Valeur de clé.](assets/create-analytics-workspace/key-value.png)

1. **Enregistrez** le fichier délimité par des tabulations.
1. Accédez à l’onglet **Importer un fichier**.
1. Configurez la destination de l’import du fichier.
   * **Sélectionner une suite de rapports** : WKND Site AEM (suite de rapports).
   * **Jeu de données à classer** : identifiant de bouton (variable de conversion eVar8).
1. Cliquez sur l’option **Choisir un fichier** pour télécharger le fichier délimité par des tabulations depuis votre système, puis cliquez sur **Importer un fichier**.

   ![Importateur de fichiers.](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Un import réussi affiche immédiatement les modifications appropriées dans un export. Toutefois, les modifications des données dans les rapports peuvent prendre jusqu’à quatre heures lors de l’utilisation d’un import de navigateur et jusqu’à 24 heures lors de l’utilisation d’un import FTP.

#### Remplacer la variable de conversion par la variable de classification

1. Dans la barre d’outils Analytics, sélectionnez **Workspace** et ouvrez l’espace de travail créé dans la section [Création d’un projet dans Analysis Workspace](#create-a-project-in-analysis-workspace) de ce tutoriel.

   ![Identifiant de bouton Workspace.](assets/create-analytics-workspace/workspace-report-button-id.png).

1. Ensuite, remplacez la mesure **Identifiant de bouton** dans votre espace de travail qui affiche l’identifiant d’un bouton CTA (appel à l’action) avec le nom de classification créé à l’étape précédente.

1. Dans l’outil de recherche de composant, recherchez **Boutons CTA WKND** et faites glisser **Boutons CTAWKND (identifiant de bouton)** sur la mesure Identifiant de bouton et remplacez-la.

   * **Avant**
     ![Bouton Workspace avant.](assets/create-analytics-workspace/wknd-button-before.png)
   * **Après**
     ![Bouton Workspace après.](assets/create-analytics-workspace/wknd-button-after.png)

1. Vous pouvez remarquer que la mesure d’identifiant de bouton qui contenait l’identifiant de bouton d’un bouton CTA (appel à l’action) est désormais remplacée par un nom correspondant fourni dans le modèle de classification.
1. Comparons le tableau d’Analytics Workspace à la page d’accueil WKND et étudions le nombre de clics sur le bouton CTA et son analyse. Selon les données du tableau à structure libre de Workspace, il apparaît clairement que les utilisateurs et utilisatrices ont cliqué 22 fois sur le bouton **SKI NOW** et quatre fois sur le bouton **En savoir plus** de Camping in Western Australia de la page d’accueil WKND.

   ![Rapport CTA.](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Veillez à enregistrer votre projet Adobe Analytics Workspace et à fournir un nom et une description appropriés. Vous pouvez éventuellement ajouter des balises à un projet Workspace.

   ![Enregistrement du projet.](assets/create-analytics-workspace/save-project.png)

1. Une fois votre projet enregistré, vous pouvez partager votre projet Workspace avec d’autres personens avec qui vous travaillez ou collaborez à l’aide de l’option Partager.

   ![Partage du projet.](assets/create-analytics-workspace/share.png)

## Félicitations.

Vous venez d’apprendre à mapper les données capturées d’un site Adobe Experience Manager aux mesures et aux dimensions dans les suites de rapports Adobe Analytics. Vous avez également appris à exécuter une classification pour les mesures et à créer un tableau de bord de création de rapports détaillés à l’aide de la fonction Analysis Workspace d’Adobe Analytics.
