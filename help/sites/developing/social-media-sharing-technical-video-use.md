---
title: Utiliser le partage sur les réseaux sociaux dans AEM Sites
description: Explorez la configuration et l’utilisation du composant Partage sur les réseaux sociaux.
feature: Core Components
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 523
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 100%

---

# Utiliser le partage sur les réseaux sociaux {#using-social-media-sharing-in-aem-sites}

Explorez la configuration et l’utilisation du composant Partage sur les réseaux sociaux.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Cette vidéo explore les fonctionnalités suivantes du composant Partage sur les réseaux sociaux (qui fait partie des [composants principaux d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)) en utilisant l’exemple de site web [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00 - Ajout et configuration du composant Partage sur les réseaux sociaux
* 1:00 - Partage sur Facebook
* 3:10 - Partage sur Pinterest
* 6:25 - Utilisation du composant Partage sur les réseaux sociaux sur une page produit

## Configuration de l’externaliseur {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

L’[externaliseur AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/externalizer.html?lang=fr) doit être configuré sur les services de création et de publication AEM, afin de mapper le mode d’exécution de publication au domaine accessible au public utilisé pour accéder au service de publication AEM.

Dans cette vidéo, nous utilisons `/etc/hosts` pour simuler *www.example.com* pour résoudre vers localhost, et nous utilisons une [configuration de Dispatcher AEM de base](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=fr) pour permettre à www.example.com de donner sur le service de publication AEM.

## Documents annexes {#supporting-materials}

* [Télécharger des composants principaux AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Télécharger de We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installation du Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=fr)
