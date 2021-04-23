---
title: Prise en main de AEM Sites - Archétype de projet
description: Prise en main de AEM Sites - Archétype de projet. Le tutoriel WKND est un tutoriel en plusieurs parties conçu pour les développeurs qui viennent de découvrir Adobe Experience Manager. Le tutoriel passe en revue la mise en oeuvre d'un site AEM pour une marque de style de vie fictif, le WKND. Ce didacticiel porte sur des sujets fondamentaux tels que la configuration de projet, les archétypes d’expert, les composants principaux, les modèles modifiables, les bibliothèques clientes et le développement de composants.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
index: y
feature: Composants principaux, Éditeur de page, Modèles modifiables, Archétype de projet AEM
topic: gestion de contenu, développement
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: fb6c56dfc85fbcb36a68210f068fd496849c352e
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 31%

---


# Prise en main de AEM Sites - Archétype de projet {#project-archetype}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui viennent d&#39;apprendre Adobe Experience Manager (AEM). Ce tutoriel présente la mise en oeuvre d&#39;un site AEM pour une marque de style de vie fictif WKND.

Ce didacticiel début en utilisant l&#39;[AEM Archétype de projet](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr) pour générer un nouveau projet.

Le tutoriel est conçu pour fonctionner avec **AEM en tant que Cloud Service** et est rétrocompatible avec **AEM 6.5.5.0+** et **l&#39;AEM 6.4.8.1+**. Le site est mis en oeuvre à l’aide des éléments suivants :

* [Archétype de projet Maven AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/archetype/overview.html)
* [Composants principaux](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/getting-started/getting-started.html)
* Modèles Sling
* [Modèles modifiables](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Système de style](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Estimez 1 à 2 heures pour passer en revue chaque partie du tutoriel.*

## Environnement de développement local {#local-dev-environment}

Un environnement de développement local est nécessaire pour compléter ce tutoriel. Les captures d’écran et la vidéo sont capturées à l’aide de l’AEM en tant que SDK Cloud Service s’exécutant sur un environnement Mac OS avec [Visual Studio Code](https://code.visualstudio.com/) en tant qu’IDE. Sauf indication contraire, les commandes et le code doivent être indépendants du système d&#39;exploitation local.

### Logiciels requis

Les logiciels suivants doivent être installés localement :

* Instance d’AEM locale **Auteur** (SDK Cloud Service, 6.5.5+ ou 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou version ultérieure)
* [Node.js](https://nodejs.org/en/) (LTS - Prise en charge à long terme)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio ](https://code.visualstudio.com/) Codeor équivalent IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Outil utilisé dans le didacticiel

>[!NOTE]
>
> **Vous découvrez AEM as a Cloud Service ?** Consultez le guide  [suivant pour configurer un environnement de développement local à l’aide de l’AEM en tant que SDK](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) Cloud Service.
>
> **Nouveau à AEM 6.5 ?** Consultez le guide  [suivant pour la configuration d&#39;un environnement](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de développement local.

## Github {#github}

Tout le code du projet se trouve sur Github dans le AEM Guide repo :

**[GitHub : Projet de sites WKND](https://github.com/adobe/aem-guides-wknd)**

En outre, chaque partie du tutoriel a sa propre branche dans GitHub. Un utilisateur peut commencer le didacticiel à tout moment en vérifiant simplement la branche correspondant à la partie précédente.

## Étapes suivantes {#next-steps}

Qu&#39;attendez-vous ? ! Début du didacticiel en accédant au chapitre [Configuration du projet](project-setup.md) et en apprenant comment générer un nouveau projet Adobe Experience Manager à l&#39;aide de l&#39;archétype de projet AEM.
