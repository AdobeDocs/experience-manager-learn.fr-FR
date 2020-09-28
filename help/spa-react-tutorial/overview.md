---
title: Prise en main de l’éditeur AEM d’applications monopages et réaction
description: Créez votre première application SPA (React Single Page Application) modifiable dans Adobe Experience Manager AEM avec l’application SPA WKND. Découvrez comment créer une application d’une seule page à l’aide de la structure React JS avec AEM SPA Editor. Ce tutoriel en plusieurs parties présente la mise en oeuvre d'une application React pour une marque de style de vie fictive, le WKND. Le tutoriel couvre la création de bout en bout du SPA et l'intégration avec AEM.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 8%

---


# Créez votre première application d’une seule réponse dans AEM {#overview}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent la fonctionnalité **SPA Editor** de Adobe Experience Manager (AEM). Ce tutoriel présente la mise en oeuvre d&#39;une application React pour une marque fictive de style de vie, le WKND. L’application React sera développée et conçue pour être déployée avec AEM SPA Editor, qui mappe les composants React aux composants AEM. L’application d’une seule page, déployée sur AEM, peut être créée dynamiquement à l’aide des outils d’édition en ligne traditionnels d’AEM.

![Mise en oeuvre de l’application d’une seule page](assets/wknd-spa-implementation.png)

*Implémentation de l’application d’une seule page WKND*

## À propos

L’objectif de ce didacticiel en plusieurs parties est d’apprendre à un développeur comment mettre en oeuvre une application React pour travailler avec la fonction d’éditeur d’applications monopages d’AEM. Dans un scénario réel, les activités de développement sont ventilées par personne, impliquant souvent un développeur **** frontal et un développeur **** dorsal. Nous pensons qu&#39;il est bénéfique pour tout développeur qui sera impliqué dans un projet AEM SPA Editor de compléter ce tutoriel.

Le tutoriel est conçu pour fonctionner avec **AEM comme Cloud Service** et est rétrocompatible avec **AEM 6.5.4+** et **AEM 6.4.8+**. L’application d’une seule page est mise en oeuvre à l’aide des éléments suivants :

* [Archétype de projet Maven AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [Éditeur d’application d’une seule page](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Composants principaux](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)
* [Réagir JS](https://reactjs.org/)
* [Créer une application réactive](https://create-react-app.dev/)

*Estimez 1 à 2 heures pour passer en revue chaque partie du tutoriel.*

## Dernier code

Tous les didacticiels sont disponibles sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [dernière base](https://github.com/adobe/aem-guides-wknd-spa/releases) de code est disponible sous forme de packs d’AEM téléchargeables.

## Conditions préalables

Avant de commencer ce didacticiel, vous aurez besoin des éléments suivants :

* Connaissances de base en HTML, CSS et JavaScript
* Connaissance de base de [React](https://reactjs.org/tutorial/tutorial.html)
* [aem en tant que SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)Cloud Service, [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) ou [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (version 3.3.9 ou ultérieure)
* [Node.js](https://nodejs.org/en/) et [npm](https://www.npmjs.com/)

*Bien qu&#39;il ne soit pas nécessaire, il est bénéfique de bien comprendre de base le[développement des composantes](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)traditionnelles de l&#39;AEM Sites.*

## Environnement de développement local {#local-dev-environment}

Un environnement de développement local est nécessaire pour compléter ce tutoriel. Les captures d’écran et la vidéo sont capturées à l’aide de l’AEM en tant que SDK Cloud Service s’exécutant sur un environnement Mac OS avec [Visual Studio Code](https://code.visualstudio.com/) comme IDE. Sauf indication contraire, les commandes et le code doivent être indépendants du système d&#39;exploitation local.

>[!NOTE]
>
> **Vous êtes nouveau à AEM en tant que Cloud Service ?** Consultez le guide [suivant pour configurer un environnement de développement local à l’aide de l’AEM en tant que SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)Cloud Service.
>
> **Nouveau à AEM 6.5 ?** Consultez le guide [suivant pour la configuration d&#39;un environnement](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)de développement local.

## Étapes suivantes {#next-steps}

Qu&#39;attendez-vous ? ! Début du didacticiel en accédant au chapitre Projet [de l’éditeur](create-project.md) d’applications monopages et en apprenant à générer un projet compatible avec l’éditeur d’applications monopages à l’aide de l’archétype de projets AEM.

## Compatibilité ascendante {#compatibility}

Le code de projet de ce didacticiel a été créé pour AEM en tant que Cloud Service. Afin de rendre le code du projet rétrocompatible avec les versions **6.5.4+** et **6.4.8+** , plusieurs modifications ont été apportées aux fichiers POM du tutoriel.

La [version](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) 6.4.4 **d’UberJar** a été incluse en tant que dépendance :

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

Un autre profil Maven, nommé `classic` , a été ajouté pour modifier la version en environnements cible 6.x AEM :

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

The `classic` profile is disabled by default. Si vous suivez le tutoriel avec AEM 6.x, veuillez ajouter le `classic` profil chaque fois que vous avez reçu l&#39;ordre d&#39;exécuter une compilation Maven, c&#39;est-à-dire :

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Lors de la génération d&#39;un nouveau projet pour une mise en oeuvre AEM utilisez toujours la dernière version de l&#39;archétype [de projet](https://github.com/adobe/aem-project-archetype) AEM et mettez à jour le `aemVersion` pour mettre en cible votre version d&#39;AEM prévue.
