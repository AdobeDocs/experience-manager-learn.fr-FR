---
title: Prise en charge de la traduction des fragments de contenu AEM
description: Découvrez comment localiser et traduire des fragments de contenu avec Adobe Experience Manager. Les fichiers multimédias associés à un fragment de contenu peuvent également être extraits et traduits.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: Business Practitioner
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 2%

---


# Prise en charge de la traduction des fragments de contenu AEM {#translation-support-content-fragments}

Découvrez comment localiser et traduire des fragments de contenu avec Adobe Experience Manager. Les fichiers multimédias associés à un fragment de contenu peuvent également être extraits et traduits.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Cas d’utilisation de la traduction de fragments de contenu {#content-fragment-translation-use-cases}

Les fragments de contenu sont un type de contenu reconnu qui AEM extractions à envoyer à un service de traduction externe. Plusieurs cas d’utilisation sont pris en charge immédiatement :

1. Un fragment de contenu peut être sélectionné directement dans la console Ressources pour la copie et la traduction de langue.
2. Les fragments de contenu référencés sur une page Sites sont copiés dans le dossier de langue approprié et extraits pour traduction lorsque la page Sites est sélectionnée pour la copie de langue.
3. Les fichiers multimédias intégrés à un fragment de contenu peuvent être extraits et traduits.
4. Les collections de ressources associées à un fragment de contenu peuvent être extraites et traduites.

## Éditeur de règles de traduction {#translation-rules-editor}

Le comportement de traduction du Experience Manager peut être mis à jour à l&#39;aide de l&#39;**éditeur de règles de traduction**. Pour mettre à jour la traduction, accédez à **Outils** > **Général** > **Configuration de traduction** à l’adresse [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Les configurations prêtes à l’emploi référencent Fragments de contenu à `fragmentPath` avec un type de ressource `core/wcm/components/contentfragment/v1/contentfragment`. Tous les composants qui héritent de `v1/contentfragment` sont reconnus par la configuration par défaut.

![Éditeur de règles de traduction](assets/translation-configuration.png)
