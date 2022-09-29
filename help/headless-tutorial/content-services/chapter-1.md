---
title: Chapitre 1 - Configuration et téléchargements de tutoriels - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: Le chapitre 1 du tutoriel AEM sans affichage présente la configuration de ligne de base pour l’instance AEM du tutoriel.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 2%

---

# Configuration du tutoriel

La dernière version des composants principaux de la gestion de contenu web d’AEM et d’AEM est toujours recommandée.

* AEM 6.5  ou version ultérieure
* AEM WCM Core Components 2.4.0 ou version ultérieure
   * Inclus dans la variable [WKND Mobile AEM Application Content Package ci-dessous](#wknd-mobile-application-packages)

Avant de commencer ce tutoriel, assurez-vous que les instances AEM suivantes sont [installé et en cours d’exécution sur votre ordinateur local ;](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **Auteur AEM** on **port 4502**
* **Publication AEM** on **port 4503**

## Packages d’applications mobiles WKND{#wknd-mobile-application-packages}

Installez les packages de contenu AEM suivants sur **both** Auteur AEM et publication AEM, à l’aide de [!DNL AEM Package Manager].

* [ui.apps : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Composant proxy pour les composants principaux de la gestion de contenu web AEM
   * [!DNL WKND Mobile] AEM CSS des pages Content Services (pour le style mineur)
* [ui.content : GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Structure du site
   * [!DNL WKND Mobile] Structure de dossiers DAM
   * [!DNL WKND Mobile] ressources image

Dans [Chapitre 7](./chapter-7.md) nous allons exécuter la variable [!DNL WKND Mobile] Application mobile Android utilisant [Android Studio](https://developer.android.com/studio) et l’APK (Android Application Package) fourni :

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Modules de contenu AEM chapitre

Cet ensemble de packages de contenu crée le contenu et la configuration décrits dans le chapitre associé, ainsi que tous les chapitres précédents. Ces modules sont facultatifs, mais peuvent accélérer la création de contenu.

* [Contenu du chapitre 2 : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenu du chapitre 3 : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenu du chapitre 4 : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenu du chapitre 5 : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Code source

Le code source pour le projet AEM et le [!DNL Android Mobile App] sont disponibles sur la [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Le code source n’a pas besoin d’être créé ou modifié pour ce tutoriel. Il est fourni pour garantir une transparence totale dans la création de tous les aspects du tutoriel.

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez laisser une [Problème GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Passer à la fin

Pour passer à la fin du tutoriel, la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) Le module de contenu peut être installé sur **both** Auteur AEM et Publication AEM. Notez que le contenu et la configuration ne s’afficheront pas comme publié dans l’auteur AEM. Cependant, en raison du déploiement manuel, tout le contenu et la configuration requis sont disponibles sur AEM Publish, ce qui permet à l’ [!DNL WKND Mobile App] pour accéder au contenu.


## Étape suivante

* [Chapitre 2 - Définition des modèles de fragment de contenu d’événement](./chapter-2.md)
