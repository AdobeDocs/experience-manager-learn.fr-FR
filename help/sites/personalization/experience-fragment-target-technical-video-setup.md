---
title: Configuration des fragments d’expérience et de l’intégration Adobe Target dans AEM
seo-title: Configuration des fragments d’expérience et de l’intégration Adobe Target dans AEM
description: Adobe Experience Manager 6.4 réimagine le workflow de personnalisation entre AEM et Target. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’offres HTML. Il permet aux marketeurs de tester et de personnaliser facilement le contenu sur différents canaux.
seo-description: Adobe Experience Manager 6.4 réimagine le workflow de personnalisation entre AEM et Target. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’offres HTML. Il permet aux marketeurs de tester et de personnaliser facilement le contenu sur différents canaux.
sub-product: content-services
feature: Fragments d’expérience
topics: integrations
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.4, 6.5
uuid: 05fd477d-0c1a-42c0-ab92-2bca86602e2e
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: Personnalisation
role: Administrator, Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 2%

---


# Configuration des fragments d’expérience et intégration Adobe Target{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 réimagine le workflow de personnalisation entre AEM et Target. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’offres HTML. Il permet aux marketeurs de tester et de personnaliser facilement le contenu sur différents canaux.

>[!VIDEO](https://video.tv.adobe.com/v/22380/?quality=9&learn=on)

>[!NOTE]
>
>Il est recommandé d’utiliser la bibliothèque cliente at.js. Il est recommandé d’utiliser des solutions de gestion des balises telles que Launch by Adobe, Adobe DTM ou toute solution tierce de gestion des balises pour ajouter des bibliothèques cibles à vos pages de site.

* La configuration du service Cloud Target appliquée au dossier de fragments d’expérience hérite de tous les fragments d’expérience créés directement sous le dossier parent. Le dossier enfant n’hérite pas de la configuration des services cloud parents.
* Vous pouvez obtenir le code client Target à partir de Adobe Experience Cloud > Cible de lancement > Sous Onglet de configuration > Implémentation > Modifier les paramètres at.js .
* Vous pouvez obtenir le nom d’utilisateur et le mot de passe de l’API Target en envoyant un ticket au service à la clientèle avec une demande d’activation de la fonctionnalité d’intégration de Target du fragment d’expérience.

## Ressources supplémentaires {#additional-resources}

* [Documentation sur les fragments d’expérience](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Utilisation de fragments d’expérience](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
