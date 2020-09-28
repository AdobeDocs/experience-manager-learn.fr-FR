---
title: Utilisation du partage sur les réseaux sociaux en AEM Sites
description: Explorez la configuration et l’utilisation du composant Partage sur les réseaux sociaux.
feature: core-components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 9%

---


# Utilisation du partage sur les réseaux sociaux {#using-social-media-sharing-in-aem-sites}

Explorez la configuration et l’utilisation du composant Partage sur les réseaux sociaux.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

Cette vidéo explore les fonctionnalités suivantes du composant Partage sur les réseaux sociaux (qui fait partie des composants [](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)principaux AEM) à l’aide de l’exemple de site Web [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) .

* 0:00 - Ajoute et configuration du composant Partage sur les réseaux sociaux
* 1:00 - Partage sur Facebook
* 3:10 - Partage sur Pinterest
* 6:25 - Utilisation du composant Partage sur les réseaux sociaux sur une page de produit

## Configuration de l’extériorisateur {#externalizer-setup}

![Externalisateur de liens Day CQ](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[aem externaliseur](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) doit être configuré sur AEM Author et AEM Publish, afin de mapper le mode d’exécution de publication au domaine accessible au public utilisé pour accéder à AEM Publish.

Dans cette vidéo, nous utilisons `/etc/hosts` pour fausser *www.example.com* pour résoudre le problème de localhost, et nous utilisons une configuration [](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) de base du répartiteur d’AEM pour permettre à www.example.com de traiter AEM Publish.

## Documents de support {#supporting-materials}

* [Téléchargement des composants principaux AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Télécharger We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installation de Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
