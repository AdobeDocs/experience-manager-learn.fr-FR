---
title: Chapitre 1 - Installation et téléchargements de didacticiels - Content Services
seo-title: Prise en main de AEM Content Services - Chapitre 1 - Configuration des didacticiels
description: Chapitre 1 du didacticiel AEM sans titre sur la configuration de la ligne de base pour l’instance AEM pour le didacticiel.
seo-description: Chapitre 1 du didacticiel AEM sans titre sur la configuration de la ligne de base pour l’instance AEM pour le didacticiel.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 1%

---


# Configuration du didacticiel

La dernière version des composants principaux de AEM et AEM WCM est toujours recommandée.

* AEM 6.5  ou version ultérieure
* aem composants principaux WCM 2.4.0 ou version ultérieure
   * Inclus dans le [package de contenu de l&#39;application d&#39;AEM WKND Mobile ci-dessous](#wknd-mobile-application-packages)

Avant de démarrer ce didacticiel, assurez-vous que les instances d&#39;AEM suivantes sont [installées et en cours d&#39;exécution sur votre ordinateur local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install) :

* **aem** port  **d&#39;autorisation 4502**
* **aem** Publier  **port 4503**

## Packages d’applications mobiles WKND{#wknd-mobile-application-packages}

Installez les packages de contenu AEM suivants sur **AEM Author** et AEM Publish, à l’aide de [!DNL AEM Package Manager].

* [ui.apps : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Composant proxy pour les composants principaux de AEM WCM
   * [!DNL WKND Mobile] Page CSS des AEM Content Services (pour la mise en forme mineure)
* [ui.content : GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Structure du site
   * [!DNL WKND Mobile] Structure de dossiers DAM
   * [!DNL WKND Mobile] Fichiers d’image

Dans le [chapitre 7](./chapter-7.md), nous allons exécuter l&#39;application mobile [!DNL WKND Mobile] Android à l&#39;aide de [Android Studio](https://developer.android.com/studio) et l&#39;application APK fournie (package d&#39;application Android) :

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Chapitre AEM Paquets de contenu

Cet ensemble de packages de contenu crée le contenu et la configuration décrits dans le chapitre associé et dans tous les chapitres précédents. Ces packs sont facultatifs mais peuvent accélérer la création de contenu.

* [Chapitre 2 Contenu : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Chapitre 3 Contenu : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Chapitre 4 Contenu : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Chapitre 5 Contenu : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Code source

Le code source du projet AEM et du projet [!DNL Android Mobile App] est disponible sur le [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Le code source n&#39;a pas besoin d&#39;être créé ou modifié pour ce tutoriel, il est fourni pour permettre une transparence complète dans la façon dont tous les aspects du tutoriel sont créés.

Si vous rencontrez un problème avec le tutoriel ou le code, veuillez laisser un [problème GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Passer à la fin

Pour passer à la fin du didacticiel, le package de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) peut être installé sur **AEM Author et AEM Publish.** Notez que le contenu et la configuration ne s’afficheront pas comme publiés dans AEM Author. Cependant, en raison du déploiement manuel, tout le contenu et la configuration requis seront disponibles sur AEM Publish, ce qui permettra à [!DNL WKND Mobile App] d’accéder au contenu.


## Étape suivante

* [Chapitre 2 - Définition de modèles de fragments de contenu de Événement](./chapter-2.md)
