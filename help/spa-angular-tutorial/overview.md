---
title: Prise en main de l’Éditeur AEM SPA et d’Angular
description: Créez votre première application angulaire d’une seule page (SPA) modifiable dans Adobe Experience Manager, AEM avec le SPA WKND. Découvrez comment créer un SPA à l’aide de la structure JS angulaire avec AEM Éditeur SPA. Ce tutoriel en plusieurs parties présente la mise en oeuvre d'une application Angular pour une marque de style de vie fictive, le WKND. Le tutoriel couvre la création du SPA de bout en bout et l'intégration avec AEM.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 13%

---


# Créer votre première SPA Angular dans AEM {#introduction}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent la fonction **SPA Editor** de Adobe Experience Manager (AEM). Ce tutoriel passe en revue la mise en oeuvre d&#39;une application Angular pour une marque de style de vie fictive, le WKND. L’application Angular sera développée et conçue pour être déployée avec AEM SPA Editor, qui mappe les composants Angular aux composants AEM. Les SPA terminées, déployées sur AEM, peuvent être créées dynamiquement à l’aide des outils de modification en ligne traditionnels d’AEM.

![SPA final mis en oeuvre](assets/wknd-spa-implementation.png)

*Implémentation de la SPA WKND*

## À propos d’

L’objectif de ce didacticiel en plusieurs parties est d’apprendre à un développeur comment mettre en oeuvre une application Angular pour travailler avec la fonction SPA éditeur d’AEM. Dans un scénario réel, les activités de développement sont ventilées par personne, impliquant souvent un **développeur frontal** et un **développeur dorsal**. Nous pensons qu&#39;il est bénéfique pour tout développeur qui sera impliqué dans un projet AEM SPA Editor de compléter ce tutoriel.

Le tutoriel est conçu pour fonctionner avec **AEM en tant que Cloud Service** et est rétrocompatible avec **AEM 6.5.4+** et **AEM 6.4.8+**. La SPA est mise en oeuvre en utilisant :

* [Archétype de projet Maven AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/developing/archetype/overview.html)
* [aem Éditeur SPA](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Composants principaux](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*Estimez 1 à 2 heures pour passer en revue chaque partie du tutoriel.*

## Dernier code

Vous trouverez tout le code du didacticiel sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [dernière base de code](https://github.com/adobe/aem-guides-wknd-spa/versions) est disponible sous forme de paquets AEM téléchargeables.

## Conditions préalables

Avant de commencer ce didacticiel, vous aurez besoin des éléments suivants :

* Connaissances de base en HTML, CSS et JavaScript
* Connaissance de base de [Angular](https://angular.io/)
* [aem en tant que SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) Cloud Service,  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) ou  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)  (3.3.9 ou version ultérieure)
* [Node.](https://nodejs.org/fr/) jand  [npm](https://www.npmjs.com/)

*Bien qu&#39;il ne soit pas nécessaire, il est bénéfique de bien comprendre de base le  [développement des composantes](https://docs.adobe.com/content/help/fr/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) traditionnelles de l&#39;AEM Sites.*

## Environnement de développement local {#local-dev-environment}

Un environnement de développement local est nécessaire pour compléter ce tutoriel. Les captures d’écran et la vidéo sont capturées à l’aide de l’AEM en tant que SDK Cloud Service s’exécutant sur un environnement Mac OS avec [Visual Studio Code](https://code.visualstudio.com/) en tant qu’IDE. Sauf indication contraire, les commandes et le code doivent être indépendants du système d&#39;exploitation local.

>[!NOTE]
>
> **Vous êtes nouveau à AEM en tant que Cloud Service ?** Consultez le guide  [suivant pour configurer un environnement de développement local à l’aide de l’AEM en tant que SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) Cloud Service.
>
> **Nouveau à AEM 6.5 ?** Consultez le guide  [suivant pour la configuration d&#39;un environnement](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de développement local.

## Étapes suivantes {#next-steps}

Qu&#39;attendez-vous ? ! Début du didacticiel en accédant au chapitre [SPA Editor Project](create-project.md) et en apprenant comment générer un projet SPA Editor activé à l’aide de l’archétype de projet AEM.

## Compatibilité descendante {#compatibility}

Le code de projet de ce didacticiel a été créé pour AEM en tant que Cloud Service. Afin de rendre le code du projet rétrocompatible pour **6.5.4+** et **6.4.8+**, plusieurs modifications ont été apportées.

Le [UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** a été inclus en tant que dépendance :

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

Un autre profil Maven, nommé `classic` a été ajouté pour modifier la version en environnements cible 6.x AEM :

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

Le profil `classic` est désactivé par défaut. Si vous suivez le tutoriel avec AEM 6.x, veuillez ajouter le profil `classic` dès que vous avez reçu l&#39;ordre d&#39;exécuter une compilation Maven :

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Lors de la génération d&#39;un nouveau projet pour une mise en oeuvre AEM, utilisez toujours la dernière version de l&#39;[AEM Archétype de projet ](https://github.com/adobe/aem-project-archetype) et mettez à jour `aemVersion` afin de cible la version que vous souhaitez de l&#39;AEM.
