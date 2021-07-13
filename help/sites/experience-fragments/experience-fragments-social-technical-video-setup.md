---
title: Configuration de la publication sur les réseaux sociaux avec AEM fragments d’expérience
description: Les fragments d’expérience permettent aux spécialistes du marketing de publier des expériences créées dans AEM sur les plateformes de médias sociaux. La vidéo ci-dessous décrit la configuration et la configuration nécessaires pour publier des fragments d’expérience dans Facebook et Pinterest.
sub-product: sites, content-services
feature: Fragments d’expérience
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Gestion de contenu
role: Admin, Developer
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 2%

---


# Configuration de la publication sur les réseaux sociaux avec des fragments d’expérience {#set-up-social-posting-with-experience-fragments}

Les fragments d’expérience permettent aux spécialistes du marketing de publier des expériences créées dans AEM sur les plateformes de médias sociaux. La vidéo ci-dessous décrit la configuration et la configuration nécessaires pour publier des fragments d’expérience dans Facebook et Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Fragments d’expérience]  : configuration et configuration pour la publication sur les réseaux sociaux sur Facebook et Pinterest.*

## Liste de contrôle pour la configuration des fragments d’expérience à publier dans Facebook et Pinterest

1. L’instance d’auteur AEM s’exécute sur HTTPS
2. Compte facebook + application de développement Facebook
3. Compte pinterest + application de développement Pinterest
4. [!UICONTROL Configuration des ] services cloud AEM - Facebook
5. [!UICONTROL Configuration des ] services cloud AEM - Pinterest
6. AEM de fragments d’expérience avec des Cloud Services pour Facebook + Pinterest
7. Variation de fragment d’expérience à l’aide du modèle Facebook
8. Variation de fragment d’expérience à l’aide du modèle Pinterest

## URI de redirection de fragment d’expérience

Cet URI est utilisé pour les applications Facebook et Pinterest dans le cadre du flux Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

