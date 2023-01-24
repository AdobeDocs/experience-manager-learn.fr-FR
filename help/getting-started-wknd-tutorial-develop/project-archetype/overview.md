---
title: Prise en main d’AEM Sites - Archétype de projet
description: Prise en main d’AEM Sites - Archétype de projet. Le tutoriel WKND est un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent Adobe Experience Manager. Le tutoriel décrit la mise en oeuvre d’un site AEM pour une marque de style de vie fictive, WKND. Le tutoriel aborde des sujets fondamentaux tels que la configuration de projet, les archétypes maven, les composants principaux, les modèles modifiables, les bibliothèques clientes et le développement de composants.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 32%

---

# Prise en main d’AEM Sites - Archétype de projet {#project-archetype}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent Adobe Experience Manager (AEM). Ce tutoriel décrit l’implémentation d’un site AEM pour une marque de style de vie fictive, WKND.

Ce tutoriel commence par utiliser la méthode [AEM Archétype de projet](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr) pour générer un nouveau projet.

Le tutoriel est conçu pour fonctionner avec **AEM as a Cloud Service** et est rétrocompatible avec **AEM 6.5.14+**. Le site est mis en oeuvre à l’aide des éléments suivants :

* [Archétype de projet Maven AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr)
* [Composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Modèles Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Modèles modifiables](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=fr)
* [Système de style](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=fr)

*Estimez 1 à 2 heures pour parcourir chaque partie du tutoriel.*

## Environnement de développement local {#local-dev-environment}

Un environnement de développement local est nécessaire pour terminer ce tutoriel. Les captures d’écran et les vidéos sont capturées à l’aide AEM SDK as a Cloud Service s’exécutant dans un environnement macOS avec [Visual Studio Code](https://code.visualstudio.com/) comme IDE. Les commandes et le code doivent être indépendants du système d’exploitation local, sauf indication contraire.

### Logiciels requis

Les logiciels suivants doivent être installés localement :

* [AEM locale **Auteur** instance](https://experience.adobe.com/#/downloads) (SDK Cloud Service ou version 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou version ultérieure)
* [Node.js](https://nodejs.org/en/) (LTS - Prise en charge à long terme)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) ou IDE équivalent
   * [Synchronisation des AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Outil utilisé tout au long du tutoriel

>[!NOTE]
>
> **Vous découvrez AEM as a Cloud Service ?** Consultez la section [le guide suivant pour configurer un environnement de développement local à l’aide du SDK as a Cloud Service AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr).
>
> **Vous découvrez AEM 6.5 ?** Consultez la section [guide de configuration d’un environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr).

## GitHub {#github}

Le code de ce tutoriel se trouve sur GitHub dans le référentiel AEM Guide :

**[GitHub : Projet de sites WKND](https://github.com/adobe/aem-guides-wknd)**

En outre, chaque partie du tutoriel possède sa propre branche dans GitHub. Un utilisateur peut commencer le didacticiel à tout moment en vérifiant simplement la branche correspondant à la partie précédente.

## Étapes suivantes {#next-steps}

Qu&#39;attends-tu ? Démarrez le tutoriel en accédant à la [Configuration du projet](project-setup.md) chapitres et découvrez comment générer un nouveau projet Adobe Experience Manager à l’aide de l’archétype de projet AEM.
