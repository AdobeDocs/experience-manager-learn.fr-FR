---
title: Débogage d’AEM avec l’explorateur de référentiels
description: Repository Browser est un outil puissant qui offre une visibilité sur AEM entrepôt de données sous-jacent, ce qui permet de déboguer facilement AEM environnement as a Cloud Service.
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
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---


# Débogage AEM as a Cloud Service avec l’explorateur de référentiel

Repository Browser est un outil puissant qui offre une visibilité sur AEM entrepôt de données sous-jacent, ce qui permet de déboguer facilement AEM environnement as a Cloud Service. Repository Browser prend en charge une vue en lecture seule des ressources et des propriétés d’AEM dans les services de production, d’évaluation et de développement, ainsi que dans les services de création, de publication et d’aperçu.

>[!VIDEO](https://video.tv.adobe.com/v/341464/?quality=12&learn=on)

Repository Browser est __UNIQUEMENT__ disponible sur AEM environnements as a Cloud Service (utilisez [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) pour déboguer le SDK d’AEM local).

## Accès à l’explorateur de référentiels

Pour accéder à l’explorateur de référentiels sur AEM as a Cloud Service :

1. Assurez-vous que votre utilisateur a [l’accès requis](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Connectez-vous à [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Sélectionnez le Programme contenant l’environnement as a Cloud Service AEM à déboguer.
1. Ouvrez le [Developer Console](./developer-console.md) correspondant à l’environnement as a Cloud Service AEM à déboguer.
1. Sélectionnez la __Explorateur de référentiels__ tab
1. Sélectionnez le niveau de service AEM à parcourir.
   + Tous les auteurs
   + Tous les éditeurs
   + Tous les aperçus
1. Sélectionner __Ouvrir l’explorateur de référentiels__

L’explorateur de référentiels s’ouvre pour le niveau de service sélectionné (Auteur, Publier ou Aperçu) en mode lecture seule, affichant les ressources et les propriétés auxquelles votre utilisateur a accès.

## Accès à la publication et à la prévisualisation

Par défaut, l’accès à Publier ou Aperçu est limité, ce qui réduit les ressources disponibles dans l’explorateur de référentiels. [Pour afficher toutes les ressources sur Publier (ou Aperçu), ajoutez des utilisateurs à un rôle Administrateurs de publication (ou Aperçu).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)

