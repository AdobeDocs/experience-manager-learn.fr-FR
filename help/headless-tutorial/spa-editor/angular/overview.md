---
title: Prise en main de l’Éditeur AEM SPA et d’Angular
description: Créez votre première application d’une seule page (SPA) d’Angular modifiable dans Adobe Experience Manager, AEM avec la version WKND. Découvrez comment créer un SPA à l’aide de la structure JS d’Angular avec AEM Éditeur. Ce tutoriel en plusieurs parties décrit la mise en oeuvre d’une application d’Angular pour une marque de style de vie fictive, WKND. Le tutoriel couvre la création de bout en bout de la SPA et l’intégration à AEM.
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 12%

---

# Créer votre première SPA Angular dans AEM {#introduction}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent la fonction **SPA Éditeur** d’Adobe Experience Manager (AEM). Ce tutoriel décrit la mise en oeuvre d’une application d’Angular pour une marque de style de vie fictive, WKND. L’application d’Angular sera développée et conçue pour être déployée avec AEM SPA Editor, qui mappe les composants d’Angular aux composants d’AEM. Les SPA terminées, déployées sur AEM, peuvent être créées dynamiquement à l’aide des outils de modification en ligne traditionnels d’.

![SPA finale implémentée](assets/wknd-spa-implementation.png)

*Implémentation SPA WKND*

## À propos

L’objectif de ce tutoriel en plusieurs parties est d’apprendre à un développeur comment mettre en oeuvre une application d’Angular pour travailler avec la fonctionnalité d’éditeur SPA d’AEM. Dans un scénario réel, les activités de développement sont ventilées par persona, impliquant souvent un **développeur front-end** et un **développeur back-end**. Nous pensons qu’il est bénéfique pour tout développeur qui sera impliqué dans un projet AEM SPA Editor de suivre ce tutoriel.

Le tutoriel est conçu pour fonctionner avec **AEM en tant que Cloud Service** et est rétrocompatible avec **AEM 6.5.4+** et **la version 6.4.8+**. La SPA est mise en oeuvre à l’aide des éléments suivants :

* [Archétype de projet Maven AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr)
* [AEM Éditeur SPA](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [Angular](https://angular.io/)

*Estimez 1 à 2 heures pour parcourir chaque partie du tutoriel.*

## Dernier code

Vous trouverez tout le code du tutoriel sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [dernière base de code](https://github.com/adobe/aem-guides-wknd-spa/releases) est disponible en tant que packages d’AEM téléchargeables.

## Prérequis

Avant de commencer ce tutoriel, vous aurez besoin des éléments suivants :

* Connaissances de base en HTML, CSS et JavaScript
* Familiarité de base avec [Angular](https://angular.io/)
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk),  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65)  ou  [6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou version ultérieure)
* [Node.](https://nodejs.org/en/) jand  [npm](https://www.npmjs.com/)

*Bien que cela ne soit pas nécessaire, il est préférable de posséder une compréhension de base du  [développement des composants](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) AEM Sites traditionnels.*

## Environnement de développement local {#local-dev-environment}

Un environnement de développement local est nécessaire pour terminer ce tutoriel. Les captures d’écran et la vidéo sont capturées à l’aide de AEM as a Cloud Service SDK s’exécutant dans un environnement Mac OS avec [Visual Studio Code](https://code.visualstudio.com/) comme IDE. Les commandes et le code doivent être indépendants du système d’exploitation local, sauf indication contraire.

>[!NOTE]
>
> **Vous découvrez AEM as a Cloud Service ?** Consultez le guide  [suivant pour configurer un environnement de développement local à l’aide du SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Vous découvrez AEM 6.5 ?** Consultez le guide  [suivant pour configurer un environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr).

## Étapes suivantes {#next-steps}

Qu&#39;attendez-vous?! Commencez le tutoriel en accédant au chapitre [SPA Projet de l’éditeur](create-project.md) et apprenez à générer un projet activé SPA Éditeur à l’aide de l’archétype de projet d’AEM.

## Compatibilité descendante {#compatibility}

Le code de projet de ce tutoriel a été créé pour AEM en tant que Cloud Service. Afin de rendre le code de projet rétrocompatible pour **6.5.4+** et **6.4.8+**, plusieurs modifications ont été apportées.

La balise [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** a été incluse en tant que dépendance :

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

Un autre profil Maven, nommé `classic` a été ajouté pour modifier la version afin de cibler AEM environnements 6.x :

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

Le profil `classic` est désactivé par défaut. Si vous suivez le tutoriel avec AEM 6.x, ajoutez le profil `classic` chaque fois que vous êtes invité à exécuter une version de Maven :

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Lors de la génération d’un nouveau projet pour une mise en oeuvre d’AEM, utilisez toujours la dernière version de [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) et mettez à jour `aemVersion` pour cibler la version d’AEM prévue.
