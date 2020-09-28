---
title: Chapitre 1 - Configuration et téléchargements de didacticiels
seo-title: Prise en main de AEM Content Services - Chapitre 1 - Configuration des didacticiels
description: Chapitre 1 du didacticiel AEM sans titre sur la configuration de la ligne de base pour l’instance AEM pour le didacticiel.
seo-description: Chapitre 1 du didacticiel AEM sans titre sur la configuration de la ligne de base pour l’instance AEM pour le didacticiel.
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 1%

---


# Configuration du didacticiel

La dernière version des composants principaux de AEM et AEM WCM est toujours recommandée.

* AEM 6.5  ou version ultérieure
* aem composants principaux WCM 2.4.0 ou version ultérieure
   * Inclus dans le package de contenu de l’application [WKND Mobile AEM ci-dessous](#wknd-mobile-application-packages)

Avant de démarrer ce didacticiel, assurez-vous que les instances AEM suivantes sont [installées et en cours d’exécution sur l’ordinateur](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)local :

* **Auteur** AEM sur le **port 4502**
* **Publication** AEM sur le **port 4503**

## Packages d’applications WKND Mobile{#wknd-mobile-application-packages}

Installez les packages de contenu AEM suivants sur **AEM Author et AEM Publish, à l’aide de** [!DNL AEM Package Manager].

* [ui.apps : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Composant proxy pour les composants principaux de AEM WCM
   * [!DNL WKND Mobile] Page CSS des AEM Content Services (pour la mise en forme mineure)
* [ui.content : GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Structure du site
   * [!DNL WKND Mobile] Structure de dossiers DAM
   * [!DNL WKND Mobile] Fichiers d’image

Au [chapitre 7](./chapter-7.md) , nous allons exécuter l’application mobile [!DNL WKND Mobile] Android à l’aide d’ [Android Studio](https://developer.android.com/studio) et de l’APK fourni (package d’application Android) :

* [[ ! Application mobile Android DNL : GitHub > Ressources > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Chapitre AEM Paquets de contenu

Cet ensemble de packages de contenu crée le contenu et la configuration décrits dans le chapitre associé et dans tous les chapitres précédents. Ces packs sont facultatifs mais peuvent accélérer la création de contenu.

* [Chapitre 2 Contenu : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Chapitre 3 Contenu : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Chapitre 4 Contenu : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Chapitre 5 Contenu : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Code source

Le code source pour le projet AEM et le projet [!DNL Android Mobile App] sont disponibles sur les [[ ! DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Le code source n&#39;a pas besoin d&#39;être créé ou modifié pour ce tutoriel, il est fourni pour permettre une transparence complète dans la façon dont tous les aspects du tutoriel sont créés.

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez laisser un problème [](https://github.com/adobe/aem-guides-wknd-mobile/issues)GitHub.

## Passer à la fin

Pour passer à la fin du didacticiel, le package de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) peut être installé sur **** AEM Author et AEM Publish. Notez que le contenu et la configuration ne s’afficheront pas comme publiés dans AEM Author. Cependant, en raison du déploiement manuel, tout le contenu et la configuration requis seront disponibles sur AEM Publish, ce qui permettra [!DNL WKND Mobile App] à l’utilisateur d’accéder au contenu.


## Étape suivante

* [Chapitre 2 - Définition de modèles de fragments de contenu de Événement](./chapter-2.md)
