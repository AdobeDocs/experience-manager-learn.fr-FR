---
title: Utilisation du partage sur les réseaux sociaux dans AEM Sites
description: Explorez la configuration et l’utilisation du composant Partage sur les réseaux sociaux .
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 10%

---

# Utilisation du partage sur les réseaux sociaux {#using-social-media-sharing-in-aem-sites}

Explorez la configuration et l’utilisation du composant Partage sur les réseaux sociaux .

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Cette vidéo explore les fonctionnalités suivantes du composant Partage sur les réseaux sociaux (qui fait partie de [Composants principaux AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)) en utilisant la variable [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) exemple de site web.

* 0:00 - Ajout et configuration du composant Partage sur les réseaux sociaux
* 1:00 - Partage vers Facebook
* 3:10 - Partage vers Pinterest
* 6:25 - Utilisation du composant Partage sur les réseaux sociaux sur une page de produit

## Configuration de l’externaliseur {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[Externaliseur AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) doit être configuré sur AEM Author et AEM Publish, afin de mapper le mode d’exécution de publication au domaine accessible au public utilisé pour accéder à AEM Publish.

Dans cette vidéo, nous utilisons `/etc/hosts` à la parodie *www.example.com* pour résoudre le problème en hôte local et utiliser une [configuration de Dispatcher AEM de base](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) pour permettre à www.example.com de préparer la publication AEM.

## Documents annexes {#supporting-materials}

* [Téléchargement des composants principaux AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Téléchargement de We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installation de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
