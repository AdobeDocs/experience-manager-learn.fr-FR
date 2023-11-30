---
title: Chapitre 1 - Configuration du tutoriel et téléchargements - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: Le chapitre 1 du tutoriel d’AEM Headless présente la configuration de base pour l’instance AEM du tutoriel.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 100%

---

# Configuration du tutoriel

La dernière version des composants principaux AEM et AEM WCM est toujours recommandée.

* AEM 6.5 ou version ultérieure
* Version 2.4.0 ou ultérieure des composants principaux d’AEM WCM
   * Inclus dans le [package de contenu d’application WKND Mobile AEM ci-dessous](#wknd-mobile-application-packages)

Avant de commencer ce tutoriel, assurez-vous que les instances AEM suivantes sont [installées et en cours d’exécution sur votre ordinateur local](https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install) :

* **Instance de création AEM** sur le **port 4502**
* **Instance de publication AEM** sur le **port 4503**

## Packages d’application mobile WKND{#wknd-mobile-application-packages}

Installez les packages de contenu AEM suivants sur **les instances** de création AEM et de publication AEM, à l’aide du [!DNL AEM Package Manager].

* [ui.apps : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * Composant proxy [!DNL WKND Mobile] pour les composants principaux d’AEM WCM
   * CSS des pages AEM Content Services [!DNL WKND Mobile] (pour le style mineur)
* [ui.content : GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * Structure du site [!DNL WKND Mobile]
   * Structure de dossiers DAM [!DNL WKND Mobile]
   * Ressources d’image [!DNL WKND Mobile]

Dans le [chapitre 7](./chapter-7.md), nous allons exécuter l’application mobile Android [!DNL WKND Mobile] à l’aide d’[Android Studio](https://developer.android.com/studio) et de l’APK (Android Application Package) fourni :

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Packages de contenu dans les chapitres AEM

Cet ensemble de packages de contenu crée le contenu et la configuration décrits dans le chapitre associé, ainsi que tous les chapitres précédents. Ces packages sont facultatifs, mais peuvent accélérer la création de contenu.

* [Contenu du chapitre 2 : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenu du chapitre 3 : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenu du chapitre 4 : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenu du chapitre 5 : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Code source

Le code source pour le projet AEM et l’[!DNL Android Mobile App] sont disponibles sur [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Le code source n’a pas besoin d’être créé ou modifié pour ce tutoriel. Il est fourni pour garantir une transparence totale dans la création de tous les aspects du tutoriel.

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez signaler un [problème GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Passer à la fin

Pour passer à la fin du tutoriel, vous pouvez installer le package de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur **les instances** de création AEM et de publication AEM. Notez que le contenu et la configuration ne s’afficheront pas comme publiés dans l’instance de création AEM. Cependant, en raison du déploiement manuel, tout le contenu et la configuration requis sont disponibles sur l’instance de publication AEM, ce qui permet à l’[!DNL WKND Mobile App] d’accéder au contenu.


## Étape suivante

* [Chapitre 2 - Définition des modèles de fragment de contenu d’événement](./chapter-2.md)
