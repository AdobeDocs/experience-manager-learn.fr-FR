---
title: Imagerie dynamique
description: L’imagerie dynamique dans Dynamic Media Classic améliore les performances de diffusion d’images en optimisant automatiquement le format et la qualité des images en fonction des fonctionnalités du navigateur client. Pour ce faire, il exploite les fonctionnalités d’Adobe Sensei AI et utilise les paramètres d’image prédéfinis existants. Découvrez l’imagerie dynamique et comment l’utiliser pour offrir de meilleures expériences client grâce à des chargements de page plus rapides.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Gestion de contenu
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 6%

---


# Imagerie dynamique {#smart-imaging}

Le temps de chargement des pages est l’un des aspects les plus importants de l’expérience client sur votre site web ou mobile ou sur votre application. Les clients abandonnent souvent un site ou une application si le chargement d’une page prend trop de temps. Les images constituent la majeure partie du temps de chargement de la page. L’imagerie dynamique dans Dynamic Media Classic améliore les performances de diffusion d’images en optimisant automatiquement le format et la qualité des images en fonction des fonctionnalités du navigateur client. Pour ce faire, il exploite les fonctionnalités d’Adobe Sensei AI et utilise les paramètres d’image prédéfinis existants. L’imagerie dynamique réduit les tailles d’image de 30 % ou plus, ce qui se traduit par des chargements de page plus rapides et de meilleures expériences client.

L’imagerie dynamique bénéficie également de l’amélioration des performances grâce à une intégration complète avec le service haut de gamme haut de gamme d’Adobe. Ce service recherche l’itinéraire Internet optimal entre les serveurs, réseaux et points d’appairage ; c’est-à-dire l’itinéraire ayant une latence et/ou un taux de perte de paquets plus faibles que l’itinéraire par défaut sur Internet.

En savoir plus sur [l’imagerie dynamique](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html).

## Avantages de l’imagerie dynamique

Comme les images constituent la majeure partie du temps de chargement d’une page, l’amélioration des performances de l’imagerie dynamique peut avoir un impact profond sur les indicateurs clés de performance métier, tels qu’une conversion plus élevée, la durée de la visite du site et un taux de rebond plus faible.

![image](assets/smart-imaging/smart-imaging-1.png)

## Fonctionnement de l’imagerie dynamique

Comme nous l’avons vu précédemment, l’imagerie dynamique tire parti des fonctionnalités d’Adobe Sensei AI et fonctionne avec les paramètres d’image prédéfinis existants pour convertir automatiquement les images en formats d’image de nouvelle génération optimaux, tels que WebP, tout en conservant la fidélité visuelle.

En savoir plus sur le [fonctionnement de l’imagerie dynamique](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), y compris des détails tels que les formats d’image pris en charge (et ce qui se passe si vous n’utilisez pas ces formats) et son impact sur les paramètres d’image prédéfinis existants en cours d’utilisation.

## Impacts de l’imagerie dynamique

Il est probable que vous ayez à apporter des modifications à vos URL, paramètres d’image prédéfinis et code sur votre site pour tirer parti de l’imagerie dynamique. Si vous remplissez les conditions préalables requises pour utiliser l’imagerie dynamique et que vous utilisez uniquement des images dans les formats d’image JPEG et PNG pris en charge, vous n’avez pas à apporter de modifications.

L’imagerie dynamique fonctionne avec les images diffusées sur HTTP, HTTPS et HTTP/2.

>[!NOTE]
>
>Le passage à l’imagerie dynamique efface votre cache sur le réseau de diffusion de contenu. Le cache du réseau de diffusion de contenu est généralement reconstitué dans un ou deux jours.

L’imagerie dynamique est incluse dans votre licence Dynamic Media Classic existante. Cette fonctionnalité n’engendre aucuns frais supplémentaires. Pour en tirer parti, vous devez répondre à deux exigences : posséder un réseau de diffusion de contenu avec Adobe et un domaine dédié. Vous devez ensuite l’activer pour votre compte, car il n’est pas activé automatiquement.

L’activation de l’imagerie dynamique commence par l’envoi d’une demande d’assistance technique en |création d’un cas de support| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/fr/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). La prise en charge de vous permet de configurer un domaine personnalisé que vous associerez à l’imagerie dynamique. Vous allez modifier un paramètre associé à la mise en cache (durée de vie ou TTL) et la prise en charge va effacer le cache. Vous pouvez également effectuer une étape d’évaluation facultative si vous le souhaitez avant de passer en production. Ensuite, lorsque l’imagerie dynamique est activée, vous proposez aux clients des images de petite taille, mais avec la même qualité que celle requise. Cela signifie qu’ils ont des temps de chargement de page plus rapides — et tout cela est fait automatiquement parce qu’Adobe Sensei aide à choisir la taille la plus efficace.

Une fois que vous avez activé l’imagerie dynamique, vous souhaiterez vérifier qu’elle fonctionne comme prévu.

Vous avez probablement d’autres questions sur l’imagerie dynamique. Nous avons compilé une liste de questions fréquentes avec des réponses. Lisez la [FAQ](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html).

## Ressources supplémentaires

Regardez le [Webinaire sur les performances de la page d’optimisation de Dynamic Media Classic](https://seminars.adobeconnect.com/pzc1gw0cihpv) dédié au créateur de compétences à la demande pour en savoir plus sur l’imagerie dynamique.
