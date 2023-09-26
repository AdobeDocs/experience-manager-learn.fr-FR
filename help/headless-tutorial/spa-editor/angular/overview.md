---
title: Prise en main de l’Éditeur de SPA d’AEM et d’Angular
description: Créez votre première application monopage (SPA) Angular modifiable dans Adobe Experience Manager (AEM) avec la SPA WKND.
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 100%

---

# Créer votre première SPA Angular dans AEM {#introduction}

{{edge-delivery-services}}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs et développeuses qui découvrent l’**éditeur de SPA** dans Adobe Experience Manager (AEM). Ce tutoriel décrit l’implémentation d’une application Angular pour une marque de style de vie fictive, WKND. L’application Angular est développée et conçue pour être déployée avec l’éditeur de SPA d’AEM, qui mappe les composants d’Angular à ceux d’AEM. Les SPA terminées, déployées sur AEM, peuvent être créées dynamiquement à l’aide des outils de modification en ligne traditionnels d’AEM.

![SPA finale implémentée.](assets/wknd-spa-implementation.png)

*Implémentation de la SPA WKND*

## À propos

Ce tutoriel en plusieurs parties a pour objectif d’apprendre aux développeurs et développeuses comment implémenter une application Angular pour travailler avec la fonctionnalité d’éditeur de SPA d’AEM. Dans un scénario réel, les activités de développement sont réparties en rôles, comprenant souvent un développeur ou une développeuse **front-end** et **back-end**. Nous pensons qu’il est bénéfique pour toute personne développeuse chargée d’un projet d’éditeur de SPA d’AEM de suivre ce tutoriel.

Le tutoriel est conçu pour fonctionner avec **AEM as a Cloud Service** et est rétrocompatible avec **AEM 6.5.4+** et **AEM 6.4.8+**. La SPA est implémentée à l’aide des éléments suivants :

* [Archétype de projet Maven AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr)
* [Éditeur de SPA AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=fr#content-editing-experience-with-spa)
* [Composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [Angular.](https://angular.io/)

*Comptez 1 à 2 heures pour parcourir chaque partie du tutoriel.*

## Dernier code

Vous trouverez tout le code du tutoriel sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base de code la plus récente](https://github.com/adobe/aem-guides-wknd-spa/releases) est disponible sous forme de packages AEM téléchargeables.

## Prérequis

Avant de commencer ce tutoriel, il vous faut :

* Des connaissances de base en HTML, CSS et JavaScript
* Des compétences de base avec [Angular](https://angular.io/)
* [Le SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/fr/experience-manager/aem-releases-updates.html#65) ou [AEM 6.4.8+](https://helpx.adobe.com/fr/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou version ultérieure)
* [Node.js](https://nodejs.org/en/) et [npm](https://www.npmjs.com/)

*Bien que cela ne soit pas une nécessité, il est préférable de posséder des connaissances élémentaires en [développement de composants traditionnels d’AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr).*

## Environnement de développement local {#local-dev-environment}

Pour suivre ce tutoriel, un environnement de développement local est nécessaire. Les captures d’écran et les vidéos sont enregistrées à l’aide du SDK AEM as a Cloud Service exécuté dans un environnement macOS avec [Visual Studio Code](https://code.visualstudio.com/) comme IDE. Sauf indication contraire, les commandes et le code doivent être indépendants du système d’exploitation local.

>[!NOTE]
>
> **Vous découvrez AEM as a Cloud Service ?** Consultez le [guide suivant pour configurer un environnement de développement local à l’aide du SDK d’AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr).
>
> **Vous découvrez AEM 6.5 ?** Consultez le [guide suivant pour configurer un environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr).

## Étapes suivantes {#next-steps}

Qu’attendez-vous ? Commencez le tutoriel en accédant au chapitre du [projet d’éditeur de SPA](create-project.md) et découvrez comment générer un projet où l’éditeur de SPA est activé à l’aide de l’archétype de projet AEM.

## Rétrocompatibilité {#compatibility}

Le code de projet de ce tutoriel a été créé pour AEM as a Cloud Service. Pour rendre le code du projet rétrocompatible avec les versions **6.5.4+** et **6.4.8+**, plusieurs modifications ont été apportées.

L’[UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=fr#what-is-the-uberjar) **v6.4.4** a été inclus comme dépendance :

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

Un profil Maven supplémentaire nommé `classic` a été ajouté pour modifier la version afin de cibler les environnements AEM 6.x :

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

Le profil `classic` est désactivé par défaut. Si vous suivez le tutoriel avec AEM 6.x, ajoutez le profil `classic` lorsqu’indiqué pour créer une version Maven :

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Lors de la génération d’un nouveau projet pour une implémentation AEM, utilisez toujours la dernière version de l’[archétype de projet d’AEM](https://github.com/adobe/aem-project-archetype) et mettez à jour `aemVersion` pour cibler la version d’AEM prévue.
