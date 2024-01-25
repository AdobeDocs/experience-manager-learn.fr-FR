---
title: Prendre en main AEM Sites - Archétype de projet
description: Prendre en main AEM Sites - Archétype de projet. Le tutoriel WKND est en plusieurs parties et a été conçu pour les développeurs et développeuses qui découvrent Adobe Experience Manager. Le tutoriel présente l’implémentation d’un site AEM pour une marque de style de vie fictive, WKND. Le tutoriel aborde des sujets fondamentaux tels que la configuration de projet, les archétypes Maven, les composants principaux, les modèles modifiables, les bibliothèques clientes et le développement de composants.
version: 6.5, Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 84
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 100%

---

# Prendre en main AEM Sites - Archétype de projet {#project-archetype}

{{edge-delivery-services-and-page-editor}}

Bienvenue dans ce tutoriel en plusieurs parties conçu pour les développeurs et développeuses qui découvrent Adobe Experience Manager (AEM). Ce tutoriel décrit l’implémentation d’un site AEM pour une marque de style de vie fictive, WKND.

Ce tutoriel commence par utiliser l’[archétype de projet d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr) pour générer un nouveau projet.

Le tutoriel est conçu pour fonctionner avec **AEM as a Cloud Service** et est rétrocompatible avec **AEM 6.5.14+**. Le site est implémenté à l’aide des éléments suivants :

* [Archétype de projet Maven AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr)
* [Composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=fr)
* [Modèles Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Modèles modifiables](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=fr)
* [Système de style](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=fr)

*Comptez 1 à 2 heures pour terminer chaque partie du tutoriel.*

## Environnement de développement local {#local-dev-environment}

Pour suivre ce tutoriel, un environnement de développement local est nécessaire. Les captures d’écran et les vidéos sont faites avec le SDK d’AEM as a Cloud Service qui fonctionne sur un environnement macOS avec [Visual Studio Code](https://code.visualstudio.com/) comme IDE. Sauf indication contraire, les commandes et le code doivent être indépendants du système d’exploitation local.

### Logiciels requis

Les logiciels suivants doivent être installés localement :

* [Instance **de création** locale d’AEM](https://experience.adobe.com/#/downloads) (SDK Cloud Service ou version 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou version ultérieure)
* [Node.js](https://nodejs.org/en/) (LTS - Prise en charge à long terme)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) ou IDE équivalent
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Outil utilisé tout au long du tutoriel

>[!NOTE]
>
> **Vous découvrez AEM as a Cloud Service ?** Consultez le [guide suivant pour configurer un environnement de développement local à l’aide du SDK d’AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr).
>
> **Vous découvrez AEM 6.5 ?** Consultez le [guide de configuration suivant pour mettre en place un environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr).

## GitHub {#github}

Le code de ce tutoriel se trouve sur GitHub dans le référentiel du guide AEM :

**[GitHub : projet Sites WKND](https://github.com/adobe/aem-guides-wknd)**.

De plus, chaque partie du tutoriel dispose de sa propre branche dans GitHub. Il est possible de commencer le tutoriel à tout endroit en vérifiant simplement la branche correspondant à la partie précédente.

## Étapes suivantes {#next-steps}

Qu’attendez-vous ? Démarrez le tutoriel en accédant au chapitre sur la [configuration du projet](project-setup.md) et découvrez comment générer un nouveau projet Adobe Experience Manager à l’aide de l’archétype de projet d’AEM.
