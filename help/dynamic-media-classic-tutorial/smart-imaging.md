---
title: Imagerie dynamique
description: Smart Imaging dans Dynamic Media Classic améliore les performances de la diffusion d’images en optimisant automatiquement le format et la qualité d’image en fonction des fonctionnalités du navigateur client. Pour ce faire, il utilise les fonctionnalités d’IA Adobe Sensei et travaille avec les paramètres d’image prédéfinis existants. En savoir plus sur l’imagerie intelligente et comment l’utiliser pour offre de meilleures expériences client grâce à des chargements de page plus rapides.
sub-product: dynamic-media
feature: smart-crop
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 5%

---


# Imagerie dynamique {#smart-imaging}

L’un des aspects les plus importants de l’expérience client sur votre site Web ou site ou application mobile est le temps de chargement des pages. Les clients abandonnent souvent un site ou une application si le chargement d’une page prend trop de temps. Les images représentent la majeure partie du temps de chargement des pages. Smart Imaging dans Dynamic Media Classic améliore les performances de la diffusion d’images en optimisant automatiquement le format et la qualité d’image en fonction des fonctionnalités du navigateur client. Pour ce faire, il utilise les fonctionnalités d’IA Adobe Sensei et travaille avec les paramètres d’image prédéfinis existants. L’imagerie intelligente réduit les tailles d’images de 30 % ou plus, ce qui se traduit par des chargements de page plus rapides et de meilleures expériences client.

Smart Imaging bénéficie également de l&#39;amélioration des performances grâce à son intégration complète au meilleur service haut de gamme de l&#39;Adobe. Ce service recherche l’itinéraire Internet optimal entre les serveurs, réseaux et points d’appairage ; c’est-à-dire l’itinéraire ayant une latence et/ou un taux de perte de paquets plus faibles que l’itinéraire par défaut sur Internet.

En savoir plus sur [Smart Imaging](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Avantages de l&#39;imagerie intelligente

Les images constituant la majeure partie du temps de chargement d’une page, l’amélioration des performances de l’imagerie intelligente peut avoir un impact profond sur les indicateurs de performance clés de l’entreprise, tels qu’une conversion plus élevée, la durée de consultation du site et un taux de rebonds du site plus faible.

![image](assets/smart-imaging/smart-imaging-1.png)

## Fonctionnement de l’imagerie intelligente

Comme nous l’avons vu plus haut, Smart Imaging tire parti des fonctionnalités d’IA Adobe Sensei et travaille avec les paramètres d’image prédéfinis existants pour convertir automatiquement les images en formats d’image de nouvelle génération optimaux, tels que WebP, tout en conservant la fidélité visuelle.

En savoir plus sur [le fonctionnement](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)de l’imagerie intelligente, y compris les détails tels que les formats d’image pris en charge (et ce qui se passe si vous n’utilisez pas ces formats) et son impact sur les paramètres d’image prédéfinis existants en cours d’utilisation.

## Impacts de l&#39;imagerie intelligente

Il est probable que vous ayez à modifier vos URL, paramètres d’image prédéfinis et code sur votre site pour tirer parti de l’imagerie intelligente. Si vous remplissez les conditions requises pour l’utilisation de l’imagerie intelligente et que vous travaillez uniquement avec des images aux formats d’image JPEG et PNG pris en charge, vous n’avez pas à apporter de modifications.

L’imagerie intelligente fonctionne avec les images diffusées sur HTTP, HTTPS et HTTP/2.

>[!NOTE]
>
>Le passage à l’imagerie intelligente efface votre cache sur le réseau de diffusion de contenu. En règle générale, le cache du réseau de diffusion de contenu est reconstitué dans un ou deux jours.

Smart Imaging est inclus dans votre licence existante de Dynamic Media Classic. Cette fonction n’entraîne aucun coût supplémentaire. Pour en profiter, vous devez satisfaire deux exigences : possèdent un CDN intégré à un Adobe et un domaine dédié. Vous devez ensuite l’activer pour votre compte, car il n’est pas automatiquement activé.

Activation des débuts Smart Imaging pour envoyer une demande d&#39;assistance technique en envoyant un courrier électronique à l&#39;adresse [s7support@adobe.com](mailto:s7support@adobe.com). Ils collaboreront avec vous pour configurer un domaine personnalisé que vous associerez à Smart Imaging. Vous allez modifier un paramètre lié à la mise en cache (durée de vie ou TTL) et la prise en charge va effacer le cache. Vous pouvez également effectuer une étape intermédiaire facultative si vous le souhaitez avant de passer en production. Ensuite, lorsque Smart Imaging est activé, vous proposez aux clients des images de petite taille, mais avec la même qualité que celle demandée. Cela signifie qu&#39;ils connaissent des temps de chargement de page plus rapides — et tout cela est fait automatiquement parce que Adobe Sensei aide à choisir la taille la plus efficace possible.

Une fois que vous avez activé Smart Imaging, vous devez vérifier qu’il fonctionne comme prévu.

Vous avez probablement d’autres questions sur Smart Imaging. Nous avons compilé une liste de questions fréquentes (FAQ) avec des réponses. Lisez les [FAQ](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Ressources supplémentaires

Regardez le webinaire à la demande du créateur [de compétences en optimisation des performances des pages de](https://seminars.adobeconnect.com/pzc1gw0cihpv) Dynamic Media Classic pour en savoir plus sur l’imagerie intelligente.
