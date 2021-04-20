---
title: Configuration de la publication sur les réseaux sociaux avec AEM fragments d’expérience
description: Les fragments d’expérience permettent aux spécialistes du marketing de publier des expériences créées dans AEM sur les plateformes de médias sociaux. La vidéo ci-dessous détaille la configuration et la configuration nécessaires pour publier des fragments d’expérience sur Facebook et Pinterest.
sub-product: sites, content-services
feature: Experience Fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Content Management
role: Administrator, Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 3%

---


# Configuration de la publication sur les réseaux sociaux avec des fragments d’expérience {#set-up-social-posting-with-experience-fragments}

Les fragments d’expérience permettent aux spécialistes du marketing de publier des expériences créées dans AEM sur les plateformes de médias sociaux. La vidéo ci-dessous détaille la configuration et la configuration nécessaires pour publier des fragments d’expérience sur Facebook et Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Fragments]  d’expérience - Configuration et configuration pour la publication sur les réseaux sociaux sur Facebook et Pinterest*

## Liste de contrôle permettant de configurer des fragments d’expérience pour une publication sur Facebook et Pinterest

1. L’instance d’auteur AEM s’exécute sur HTTPS.
2. Compte Facebook + application de développement Facebook
3. Compte Pinterest + application Pinterest Developer
4. [!UICONTROL AEM Cloud ] ServicesConfiguration - Facebook
5. [!UICONTROL AEM Cloud ] ServicesConfiguration - Pinterest
6. AEM Fragment d’expérience avec des Cloud Services pour Facebook + Pinterest
7. Variation de fragment d’expérience à l’aide d’un modèle Facebook
8. Variation du fragment d’expérience à l’aide du modèle Pinterest

## URI de redirection de fragment d’expérience

Cet URI est utilisé pour les applications Facebook et Pinterest dans le cadre du flux Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

