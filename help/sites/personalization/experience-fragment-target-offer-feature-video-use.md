---
title: Utilisation d’offres de fragments d’expérience AEM dans Adobe Target
description: Adobe Experience Manager 6.4 réimagine le workflow de personnalisation entre AEM et Target. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’offres HTML. Il permet aux marketeurs de tester et de personnaliser facilement le contenu sur différents canaux.
feature: Experience Fragments
version: 6.4, 6.5
topic: Personalization
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 1%

---


# Utilisation d’offres de fragments d’expérience dans Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager ré-imagine le workflow de personnalisation entre AEM et Target. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’offres HTML. Il permet aux marketeurs de tester et de personnaliser facilement le contenu sur différents canaux.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Il est recommandé d’utiliser la bibliothèque cliente `at.js`. Il est recommandé d’utiliser des solutions de gestion des balises telles que Launch by Adobe, Adobe DTM ou toute solution tierce de gestion des balises pour ajouter des bibliothèques cibles à vos pages de site.


* Adobe Experience Manager un mécanisme de création de contenu convivial et puissant, ainsi qu’une intelligence artificielle (AI) Adobe Target et un apprentissage automatique, aide les auteurs de contenu à créer et gérer du contenu pour tous les canaux à un emplacement centralisé. Avec la possibilité d’exporter des fragments d’expérience dans Adobe Target en tant qu’offres HTML, les marketeurs disposent désormais d’une plus grande flexibilité pour créer une expérience plus personnalisée à l’aide de ces offres et peuvent désormais tester et mettre à l’échelle chaque expérience qu’ils créent.
* La principale différence entre les offres HTML et les offres de fragments d’expérience réside dans le fait que la modification de la version ultérieure ne peut être effectuée que dans AEM, puis synchronisée avec Adobe Target.
* La configuration du service Cloud Target appliquée au dossier de fragments d’expérience hérite de tous les fragments d’expérience créés directement sous le dossier parent. Le dossier enfant n’hérite pas de la configuration des services cloud parents.
* Pour créer une offre personnalisée, nous pouvons désormais facilement exploiter le contenu stocké dans AEM.
* Vous pouvez créer des types d’activités Target, y compris les activités Sensei telles que l’affectation automatique, le ciblage automatique et Automated Personalization.

## Ressources supplémentaires {#additional-resources}

* [Documentation sur les fragments d’expérience](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html)
* [Utilisation de fragments d’expérience](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
