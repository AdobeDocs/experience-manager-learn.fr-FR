---
title: Déboguer AEM avec l’explorateur de référentiels
description: L’explorateur de référentiels est un outil puissant qui offre une visibilité sur l’entrepôt de données AEM sous-jacent, ce qui permet de déboguer facilement l’environnement d’AEM as a Cloud Service.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Déboguer AEM as a Cloud Service avec l’explorateur de référentiels

L’explorateur de référentiels est un outil puissant qui offre une visibilité sur l’entrepôt de données AEM sous-jacent, ce qui permet de déboguer facilement l’environnement d’AEM as a Cloud Service. L’explorateur de référentiels prend en charge une vue en lecture seule des ressources et des propriétés d’AEM pour les services de production, d’évaluation et de développement, ainsi que pour les services de création, de publication et de prévisualisation.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

L’explorateur de référentiels est __UNIQUEMENT__ disponible sur les environnements AEM as a Cloud Service (utilisez [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) pour déboguer le SDK AEM local).

## Accéder à l’explorateur de référentiels

Pour accéder à l’explorateur de référentiels sur AEM as a Cloud Service :

1. Assurez-vous que votre utilisateur ou utilisatrice dispose de [l’accès requis](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=fr#access-prerequisites).
1. Connectez-vous à [Cloud Manager](https://my.cloudmanager.adobe.com).
1. Sélectionnez le programme contenant l’environnement AEM as a Cloud Service à déboguer.
1. Ouvrez la [Developer Console](./developer-console.md) correspondant à l’environnement AEM as a Cloud Service à déboguer.
1. Sélectionnez l’onglet __Explorateur de référentiels__.
1. Sélectionnez le niveau de service AEM à parcourir.
   + Toutes les personnes chargées de la création
   + Toutes les personnes chargées de la publication
   + Toutes les prévisualisations
1. Sélectionnez __Ouvrir l’explorateur de référentiel__.

L’explorateur de référentiels s’ouvre pour le niveau de service sélectionné (Création, Publication ou Prévisualisation) en mode lecture seule, affichant les ressources et les propriétés auxquelles votre utilisateur ou votre utilisatrice a accès.

## Accéder à la publication et à la prévisualisation

Par défaut, l’accès à la publication ou à la prévisualisation est limité, ce qui réduit les ressources disponibles dans l’explorateur de référentiels. [Pour afficher toutes les ressources sur l’instance de publication (ou de prévisualisation), ajoutez les utilisateurs et utilisatrices à un rôle d’administration de publication (ou prévisualisation).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=fr#navigate-the-hierarchy)
