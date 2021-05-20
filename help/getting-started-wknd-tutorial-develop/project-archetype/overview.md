---
title: Prise en main d’AEM Sites - Archétype de projet
description: Prise en main d’AEM Sites - Archétype de projet. Le tutoriel WKND est un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent Adobe Experience Manager. Le tutoriel décrit la mise en oeuvre d’un site AEM pour une marque de style de vie fictive, WKND. Le tutoriel aborde des sujets fondamentaux tels que la configuration de projet, les archétypes maven, les composants principaux, les modèles modifiables, les bibliothèques clientes et le développement de composants.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Composants principaux, éditeur de page, modèles modifiables, AEM archétype de projet
topic: Gestion de contenu, développement
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 31%

---


# Prise en main d’AEM Sites - Archétype de projet {#project-archetype}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent Adobe Experience Manager (AEM). Ce tutoriel décrit la mise en oeuvre d’un site AEM pour une marque de style de vie fictive WKND.

Ce tutoriel commence par l’utilisation de l’[archétype de projet AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr) pour générer un nouveau projet.

Le tutoriel est conçu pour fonctionner avec **AEM en tant que Cloud Service** et est rétrocompatible avec **AEM 6.5.5.0+** et **la version 6.4.8.1+**. Le site est mis en oeuvre à l’aide des éléments suivants :

* [Archétype de projet Maven AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/archetype/overview.html)
* [Composants principaux](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/getting-started/getting-started.html)
* Modèles Sling
* [Modèles modifiables](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Système de style](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Estimez 1 à 2 heures pour parcourir chaque partie du tutoriel.*

## Environnement de développement local {#local-dev-environment}

Un environnement de développement local est nécessaire pour terminer ce tutoriel. Les captures d’écran et la vidéo sont capturées à l’aide de AEM as a Cloud Service SDK s’exécutant dans un environnement Mac OS avec [Visual Studio Code](https://code.visualstudio.com/) comme IDE. Les commandes et le code doivent être indépendants du système d’exploitation local, sauf indication contraire.

### Logiciels requis

Les logiciels suivants doivent être installés localement :

* Instance d’AEM locale **auteur** (SDK Cloud Service, 6.5.5+ ou 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou version ultérieure)
* [Node.js](https://nodejs.org/en/)  (LTS - Prise en charge à long terme)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio ](https://code.visualstudio.com/) Codeor IDE équivalent
   * [Synchronisation](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  des AEM VSCode : outil utilisé tout au long du tutoriel

>[!NOTE]
>
> **Vous découvrez AEM as a Cloud Service ?** Consultez le guide  [suivant pour configurer un environnement de développement local à l’aide du SDK AEM as a Cloud Service](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Vous découvrez AEM 6.5 ?** Consultez le guide  [suivant pour configurer un environnement de développement local](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Github {#github}

Vous trouverez tout le code du projet sur Github dans le référentiel AEM Guide :

**[GitHub : Projet de sites WKND](https://github.com/adobe/aem-guides-wknd)**

En outre, chaque partie du tutoriel possède sa propre branche dans GitHub. Un utilisateur peut commencer le didacticiel à tout moment en vérifiant simplement la branche correspondant à la partie précédente.

## Étapes suivantes {#next-steps}

Qu&#39;attendez-vous?! Démarrez le tutoriel en accédant au chapitre [Configuration du projet](project-setup.md) et apprenez à générer un nouveau projet Adobe Experience Manager à l’aide de l’archétype de projet AEM.
