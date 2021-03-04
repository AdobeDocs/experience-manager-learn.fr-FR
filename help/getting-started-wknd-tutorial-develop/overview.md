---
title: Prise en main du développement AEM Sites – Tutoriel WKND
description: Prise en main du développement AEM Sites – Tutoriel WKND. Le tutoriel WKND est un tutoriel en plusieurs parties conçu pour les développeurs qui viennent de découvrir Adobe Experience Manager. Le tutoriel passe en revue la mise en oeuvre d'un site AEM pour une marque de style de vie fictif, le WKND. Ce didacticiel porte sur des sujets fondamentaux tels que la configuration de projet, les archétypes d’expert, les composants principaux, les modèles modifiables, les bibliothèques clientes et le développement de composants.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Composants principaux, Éditeur de page, Modèles modifiables, Archétype de projet AEM
topic: gestion de contenu, développement
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 21%

---


# Prise en main du développement AEM Sites – Tutoriel WKND {#introduction}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui viennent d&#39;apprendre Adobe Experience Manager (AEM). Ce tutoriel présente la mise en oeuvre d&#39;un site AEM pour une marque de style de vie fictif WKND. Ce didacticiel porte sur des sujets fondamentaux tels que la configuration de projets, les composants principaux, les modèles modifiables, les bibliothèques côté client et le développement de composants avec Adobe Experience Manager Sites.

## Présentation {#wknd-tutorial-overview}

Ce didacticiel en plusieurs parties a pour but d’apprendre à un développeur comment mettre en oeuvre un site Web en utilisant les dernières normes et technologies de Adobe Experience Manager (AEM). Après avoir suivi ce tutoriel, un développeur doit comprendre les fondements de la plate-forme et connaître les schémas de conception courants en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

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

## À propos du didacticiel {#about-tutorial}

The WKND est un magazine et blog fictif en ligne qui se concentre sur la vie nocturne, les activités et les événements dans plusieurs villes internationales.

### Kit d’interface utilisateur Adobe XD

Pour rapprocher ce didacticiel d&#39;un scénario réel, des concepteurs UX talentueux du Adobe ont créé les maquettes pour le site à l&#39;aide de [Adobe XD](https://www.adobe.com/products/xd.html). Au cours du tutoriel, divers éléments des conceptions sont mis en oeuvre dans un site d&#39;AEM entièrement créateur. Remerciements particuliers à **Lorenzo Buosi** et **Kilian Amendola** qui ont créé la belle conception pour le site WKND.

Téléchargez les XD kits d’interface utilisateur :

* [Kit d’interface utilisateur des composants principaux AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit d’interface utilisateur WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

Le nom WKND convient car nous espérons qu&#39;un développeur prendra la meilleure part d&#39;un ***week-end*** pour compléter le tutoriel.

### Github {#github}

Tout le code du projet se trouve sur Github dans le AEM Guide repo :

**[GitHub : Projet de sites WKND](https://github.com/adobe/aem-guides-wknd)**

En outre, chaque partie du tutoriel a sa propre branche dans GitHub. Un utilisateur peut commencer le didacticiel à tout moment en vérifiant simplement la branche correspondant à la partie précédente.

>[!NOTE]
>
> Si vous travailliez avec la version précédente de ce didacticiel, vous pouvez toujours trouver les [packages de solution](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) et [code](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) sur GitHub.

## Site de référence {#reference-site}

Une version finale du site WKND est également disponible à titre de référence : [https://wknd.site/](https://wknd.site/)

Le tutoriel couvre les principales compétences de développement requises pour un développeur AEM, mais *ne* ne construira pas l&#39;ensemble du site de bout en bout. Le site de référence terminé est une autre ressource formidable pour explorer et voir plus d&#39;AEM fonctionnalités prêtes à l&#39;emploi.

Pour tester le code le plus récent avant de passer au didacticiel, téléchargez et installez la **[dernière version de GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Proposé par Adobe Stock

La plupart des images du site Web de référence WKND proviennent d&#39;[Adobe Stock](https://stock.adobe.com/) et sont des documents de tierces parties, tels que définis dans les termes supplémentaires des ressources de démonstration à l&#39;adresse [https://www.adobe.com/legal/terms.html](https://www.adobe.com/fr/legal/terms.html). Si vous souhaitez utiliser une image Adobe Stock à d’autres fins que l’affichage de ce site Web de démonstration, par exemple en l’affichant sur un site Web ou dans du matériel marketing, vous pouvez acheter une licence sur Adobe Stock.

Avec Adobe Stock, vous avez accès à plus de 140 millions d&#39;images de haute qualité, libres de droits, dont des photos, des graphiques, des vidéos et des modèles pour lancer vos projets créatifs.

## Étapes suivantes {#next-steps}

Qu&#39;attendez-vous ? ! Début du didacticiel en accédant au chapitre [Configuration du projet](project-setup.md) et en apprenant comment générer un nouveau projet Adobe Experience Manager à l&#39;aide de l&#39;archétype de projet AEM.
