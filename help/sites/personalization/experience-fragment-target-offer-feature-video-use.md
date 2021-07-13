---
title: Utilisation d’offres de fragments d’expérience AEM dans Adobe Target
seo-title: Utilisation d’offres de fragments d’expérience AEM dans Adobe Target
description: Adobe Experience Manager 6.4 réimagine le workflow de personnalisation entre AEM et Target. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’offres HTML. Il permet aux marketeurs de tester et de personnaliser facilement le contenu sur différents canaux.
seo-description: Adobe Experience Manager 6.4 réimagine le workflow de personnalisation entre AEM et Target. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’offres HTML. Il permet aux marketeurs de tester et de personnaliser facilement le contenu sur différents canaux.
sub-product: content-services
feature: Fragments d’expérience
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: 'Personnalisation '
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 11%

---


# Utilisation d’offres de fragments d’expérience dans Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4 réimagine le workflow de personnalisation entre AEM et Target. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’offres HTML. Il permet aux marketeurs de tester et de personnaliser facilement le contenu sur différents canaux.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Il est recommandé d’utiliser la bibliothèque cliente at.js. Il est recommandé d’utiliser des solutions de gestion des balises telles que Launch by Adobe, Adobe DTM ou toute solution tierce de gestion des balises pour ajouter des bibliothèques cibles à vos pages de site.

>[!NOTE]
>
>Les offres de fragments d’expérience AEM dans Adobe Target sont également disponibles en tant que Feature Pack pour les utilisateurs d’AEM 6.3. Reportez-vous à la section ci-dessous pour les Feature Packs et les dépendances.


* Adobe Experience Manager un mécanisme de création de contenu convivial et puissant, ainsi qu’une intelligence artificielle (AI) Adobe Target et un apprentissage automatique, aide les auteurs de contenu à créer et gérer du contenu pour tous les canaux à un emplacement centralisé. Avec la possibilité d’exporter des fragments d’expérience dans Adobe Target en tant qu’offres HTML, les marketeurs disposent désormais d’une plus grande flexibilité pour créer une expérience plus personnalisée à l’aide de ces offres et peuvent désormais tester et mettre à l’échelle chaque expérience qu’ils créent.
* La principale différence entre les offres HTML et les offres de fragments d’expérience réside dans le fait que la modification de la version ultérieure ne peut être effectuée que dans AEM, puis synchronisée avec Adobe Target.
* La configuration du service Cloud Target appliquée au dossier de fragments d’expérience hérite de tous les fragments d’expérience créés directement sous le dossier parent. Le dossier enfant n’hérite pas de la configuration des services cloud parents.
* Pour créer une offre personnalisée, nous pouvons désormais facilement exploiter le contenu stocké dans AEM.
* Vous pouvez créer des types d’activités Target, y compris les activités Sensei telles que l’affectation automatique, le ciblage automatique et Automated Personalization.

## AEM 6.3 Feature Packs et dépendances {#aem-feature-packs-and-dependencies}

| AEM 6.3 Feature Pack | Dépendances |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## Ressources supplémentaires {#additional-resources}

* [Documentation sur les fragments d’expérience](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Utilisation de fragments d’expérience](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
