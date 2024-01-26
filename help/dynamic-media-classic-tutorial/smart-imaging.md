---
title: Imagerie dynamique
description: L’imagerie dynamique de Dynamic Media Classic permet d’améliorer les performances de diffusion des images en optimisant automatiquement le format et la qualité de celles-ci en fonction des fonctionnalités du navigateur client. Pour ce faire, l’imagerie dynamique tire parti des fonctionnalités d’IA d’Adobe Sensei et utilise les paramètres d’image prédéfinis existants. Découvrez l’imagerie dynamique et apprenez à l’utiliser afin d’offrir de meilleures expériences à votre clientèle grâce à des chargements de page plus rapides.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
duration: 158
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 100%

---

# Imagerie dynamique {#smart-imaging}

Les temps de chargement des pages ont un impact décisif sur l’expérience client sur votre site web, mobile ou sur votre application. Les visiteurs et visiteuses n’hésiteront pas à abandonner un site ou une application si le chargement d’une page prend trop de temps. Le temps de chargement d’une page dépend en grande partie des images présentes sur celle-ci. L’imagerie dynamique de Dynamic Media Classic permet d’améliorer les performances de diffusion des images en optimisant automatiquement le format et la qualité de celles-ci en fonction des fonctionnalités du navigateur client. Pour ce faire, l’imagerie dynamique tire parti des fonctionnalités d’IA d’Adobe Sensei et utilise les paramètres d’image prédéfinis existants. L’imagerie dynamique réduit les tailles d’image de 30 % ou plus, ce qui se traduit par des chargements de page plus rapides et de meilleures expériences pour votre clientèle.

L’imagerie dynamique tire également parti d’une parfaite intégration dans un service haut de gamme d’Adobe pour offrir un gain de performance accru. Ce service recherche l’itinéraire Internet optimal entre les serveurs, réseaux et points d’appairage ; c’est-à-dire l’itinéraire ayant une latence et/ou un taux de perte de paquets plus faibles que l’itinéraire par défaut sur Internet.

En savoir plus sur l’[Imagerie dynamique](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=fr).

## Avantages de l’imagerie dynamique

Les images sont les éléments qui demandent le plus de temps lors du chargement d’une page. Aussi une amélioration des performances grâce à l’imagerie dynamique peut-elle avoir une incidence considérable sur les KPI d’entreprise, tels qu’un taux de conversion plus élevé, une augmentation du temps passé sur le site et un taux de rebond moindre.

![image](assets/smart-imaging/smart-imaging-1.png)

## Fonctionnement de l’imagerie dynamique

Comme nous l’avons vu précédemment, l’imagerie dynamique tire parti des fonctionnalités d’IA d’Adobe Sensei et utilise les paramètres d’image prédéfinis existants pour convertir automatiquement les images dans les formats d’image optimaux de nouvelle génération, tels que WebP, tout en conservant la fidélité visuelle.

En savoir plus sur le [Fonctionnement de l’imagerie dynamique](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=fr#how-does-smart-imaging-work), notamment les formats d’image pris en charge (et ce qui se passe si vous n’utilisez pas ces formats) et son impact sur les paramètres d’image prédéfinis existants qui sont utilisés.

## Impacts de l’imagerie dynamique

Pour utiliser l’imagerie dynamique, vous devrez probablement apporter certaines modifications aux éléments de votre site, tels que les URL, les paramètres d’image prédéfinis et le code. Si vous remplissez les conditions préalables à l’utilisation de l’imagerie dynamique et que vous n’utilisez que les formats d’image JPEG et PNG pris en charge, vous n’avez pas à apporter de modification.

L’imagerie dynamique prend en charge les images diffusées sur HTTP, HTTPS et HTTP/2.

>[!NOTE]
>
>L’utilisation de l’imagerie dynamique efface votre cache sur le réseau CDN. Le cache du réseau CDN est généralement reconstitué en l’espace d’un ou deux jours.

L’imagerie dynamique est incluse dans votre licence Dynamic Media Classic existante. Cette fonctionnalité n’entraîne aucun frais supplémentaire. Pour l’utiliser, vous devez répondre aux deux conditions suivantes : posséder un réseau CDN avec Adobe et un domaine dédié. Vous devez ensuite l’activer pour votre compte, car l’imagerie dynamique n’est pas activée automatiquement.

Pour activer l’imagerie dynamique, procédez comme suit : envoyez une demande d’assistance technique en |créant un cas d’assistance| [https://helpx.adobe.com/fr/enterprise/using/support-for-experience-cloud.html](https://helpx.adobe.com/fr/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). L’assistance Adobe vous contactera pour configurer un domaine personnalisé réservé à l’imagerie dynamique. Vous devrez ensuite modifier un paramètre associé à la mise en cache (durée de vie ou TTL) et l’assistance Adobe effacera le cache. Si vous le souhaitez, vous pouvez tester la fonctionnalité dans un environnement d’évaluation avant de passer en production. Lorsque l’imagerie dynamique est activée, vous diffuserez des images de plus petite taille à vos clientes et clients, tout en conservant la qualité d’image requise. À la clé, des temps de chargement de page plus rapides. Cerise sur le gâteau, le processus est automatique, car Adobe Sensei aide à choisir la taille la plus efficace.

Une fois l’imagerie dynamique activée, assurez-vous de son fonctionnement correct.

Vous avez probablement d’autres questions à propos de l’imagerie dynamique. Pour y répondre de manière efficace, nous avons dressé une liste de questions fréquentes. Consultez la section [Questions fréquentes](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=fr).

## Ressources supplémentaires

Regardez le webinaire à la demande [Accroître ses compétences sur l’optimisation des performances des pages dans Dynamic Media Classic](https://seminars.adobeconnect.com/pzc1gw0cihpv) pour en savoir plus sur l’imagerie dynamique.
