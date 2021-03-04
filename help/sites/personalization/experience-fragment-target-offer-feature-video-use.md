---
title: Utilisation d’Offres de fragments d’expérience AEM dans Adobe Target
seo-title: Utilisation d’Offres de fragments d’expérience AEM dans Adobe Target
description: Adobe Experience Manager 6.4 réimagine le processus de personnalisation entre l’AEM et la Cible. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’Offres HTML. Il permet aux spécialistes du marketing de tester et de personnaliser en toute transparence le contenu sur différents canaux.
seo-description: Adobe Experience Manager 6.4 réimagine le processus de personnalisation entre l’AEM et la Cible. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’Offres HTML. Il permet aux spécialistes du marketing de tester et de personnaliser en toute transparence le contenu sur différents canaux.
sub-product: content-services
feature: Fragments d’expérience
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: Personnalisation
role: Professionnel
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 11%

---


# Utilisation des Offres de fragments d’expérience dans Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4 réimagine le processus de personnalisation entre l’AEM et la Cible. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’Offres HTML. Il permet aux spécialistes du marketing de tester et de personnaliser en toute transparence le contenu sur différents canaux.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Il est recommandé d’utiliser la bibliothèque cliente at.js et la meilleure pratique consiste à utiliser des solutions de gestion des balises telles que la gestion dynamique des balises Launch by Adobe, Adobe ou toute solution de gestion des balises tierce pour ajouter des bibliothèques de cible aux pages de votre site.

>[!NOTE]
>
>Les Offres AEM Experience Fragment au sein de Adobe Target sont également disponibles en tant que Feature Pack pour les utilisateurs AEM 6.3. Reportez-vous à la section ci-dessous pour les Feature Packs et les dépendances.


* Le fait d&#39;intégrer des mécanismes de création de contenu simples d&#39;utilisation et puissants, de même que l&#39;intelligence artificielle (IA) Adobe Target et l&#39;apprentissage automatique, aide les auteurs de contenu à créer et à gérer du contenu pour tous les canaux dans un emplacement centralisé. Avec la possibilité d’exporter des fragments d’expérience en Adobe Target en tant qu’offres HTML, les spécialistes du marketing disposent désormais d’une plus grande flexibilité pour créer une expérience plus personnalisée à l’aide de ces offres et peuvent désormais tester et mettre à l’échelle chaque expérience qu’ils créent.
* La principale différence entre les offres HTML et les offres de fragments d’expérience réside dans le fait que la modification pour la version ultérieure ne peut être effectuée qu’en AEM puis synchronisée avec Adobe Target.
* La configuration du service cible Cloud appliquée au dossier de fragments d’expérience hérite de tous les fragments d’expérience créés directement sous le dossier parent. Le dossier enfant n’hérite pas de la configuration des services cloud parents.
* Pour créer une offre personnalisée, nous pouvons désormais exploiter facilement le contenu stocké dans AEM.
* Vous pouvez créer des types d’activités d’Cible, y compris les activités optimisées par les utilisateurs comme l’affectation automatique, la Cible automatique et l’Automated Personalization.

## AEM 6.3 Feature Packs et dépendances {#aem-feature-packs-and-dependencies}

| AEM 6.3 Feature Pack | Dépendances |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## Ressources supplémentaires {#additional-resources}

* [Documentation des fragments d’expérience](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Utilisation des fragments d’expérience](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
