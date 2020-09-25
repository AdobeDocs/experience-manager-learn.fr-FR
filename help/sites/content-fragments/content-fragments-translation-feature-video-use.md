---
title: Utilisation de la traduction avec AEM fragments de contenu
description: aem 6.3 permet de traduire des fragments de contenu. Les ressources multimédias et les collections de ressources associées à un fragment de contenu peuvent également être extraites et traduites.
sub-product: sites, ressources, services de contenu
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# Utilisation de la traduction avec AEM fragments de contenu{#using-translation-with-aem-content-fragments}

aem 6.3 permet de traduire des fragments de contenu. Les ressources multimédias et les collections de ressources associées à un fragment de contenu peuvent également être extraites et traduites.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## Cas d’utilisation de la traduction de fragments de contenu {#content-fragment-translation-use-cases}

Les fragments de contenu sont un type de contenu reconnu que AEM extraira pour être envoyé à un service de traduction externe. Plusieurs cas d’utilisation sont pris en charge immédiatement :

1. Un fragment de contenu peut être sélectionné directement dans la console Ressources pour la copie et la traduction de langue.
2. Les fragments de contenu référencés sur une page Sites sont copiés dans le dossier de langue approprié et extraits pour traduction lorsque la page Sites est sélectionnée pour la copie de langue.
3. Les fichiers multimédias intégrés à un fragment de contenu peuvent être extraits et traduits.
4. Les collections de ressources associées à un fragment de contenu peuvent être extraites et traduites.

## Options de configuration de traduction {#translation-config-options}

La configuration de traduction prête à l’emploi prend en charge plusieurs options de traduction de fragments de contenu. Par défaut, les ressources multimédias intégrées et les collections de ressources associées ne sont PAS traduites. Pour mettre à jour la configuration de traduction, accédez à [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html).

Il existe quatre options pour traduire des ressources de fragment de contenu :

1. **Ne pas traduire (par défaut)**
2. **Ressources multimédias intégrées seulement**
3. **Collectes de ressources associées seulement**
4. **Ressources multimédias intégrées et collections associées**

![Configuration de traduction](assets/classic-ui-dialog.png)
