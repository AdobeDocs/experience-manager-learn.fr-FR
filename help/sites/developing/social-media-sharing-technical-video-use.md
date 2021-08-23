---
title: Utilisation du partage sur les réseaux sociaux dans AEM Sites
description: Explorez la configuration et l’utilisation du composant Partage sur les réseaux sociaux .
feature: Composants principaux
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
topic: Gestion de contenu
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 10%

---


# Utilisation du partage sur les réseaux sociaux {#using-social-media-sharing-in-aem-sites}

Explorez la configuration et l’utilisation du composant Partage sur les réseaux sociaux .

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

Cette vidéo explore les fonctionnalités suivantes du composant Partage sur les réseaux sociaux (qui fait partie de [AEM Composants principaux](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)) à l’aide de l’exemple de site web [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00 - Ajout et configuration du composant Partage sur les réseaux sociaux
* 1:00 - Partage vers Facebook
* 3:10 - Partage vers Pinterest
* 6:25 - Utilisation du composant Partage sur les réseaux sociaux sur une page de produit

## Configuration de l’externaliseur {#externalizer-setup}

![Externalisateur de lien Day CQ](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) externaliseur doit être configuré sur AEM Author et AEM Publish, afin de mapper le mode d’exécution de publication au domaine accessible au public utilisé pour accéder à la publication AEM.

Dans cette vidéo, nous utilisons `/etc/hosts` pour parodier *www.example.com* afin de résoudre le problème de localhost et nous utilisons une [configuration de base du Dispatcher AEM](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) pour permettre à www.example.com d’anticiper la publication AEM.

## Documents annexes {#supporting-materials}

* [Téléchargement des composants principaux AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Téléchargement de We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installation de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
