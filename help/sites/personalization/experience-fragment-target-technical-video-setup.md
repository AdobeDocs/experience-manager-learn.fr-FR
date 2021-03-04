---
title: Configuration de fragments d’expérience et d’une intégration Adobe Target dans AEM
seo-title: Configuration de fragments d’expérience et d’une intégration Adobe Target dans AEM
description: Adobe Experience Manager 6.4 réimagine le processus de personnalisation entre l’AEM et la Cible. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’Offres HTML. Il permet aux spécialistes du marketing de tester et de personnaliser en toute transparence le contenu sur différents canaux.
seo-description: Adobe Experience Manager 6.4 réimagine le processus de personnalisation entre l’AEM et la Cible. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’Offres HTML. Il permet aux spécialistes du marketing de tester et de personnaliser en toute transparence le contenu sur différents canaux.
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
role: Administrateur, développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 3%

---


# Configuration des fragments d’expérience et de l’intégration Adobe Target{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 réimagine le processus de personnalisation entre l’AEM et la Cible. Les expériences créées dans AEM peuvent désormais être diffusées directement à Adobe Target en tant qu’Offres HTML. Il permet aux spécialistes du marketing de tester et de personnaliser en toute transparence le contenu sur différents canaux.

>[!VIDEO](https://video.tv.adobe.com/v/22380/?quality=9&learn=on)

>[!NOTE]
>
>Il est recommandé d’utiliser la bibliothèque cliente at.js et la meilleure pratique consiste à utiliser des solutions de gestion des balises telles que la gestion dynamique des balises Launch by Adobe, Adobe ou toute solution de gestion des balises tierce pour ajouter des bibliothèques de cible aux pages de votre site.

* La configuration du service cible Cloud appliquée au dossier de fragments d’expérience hérite de tous les fragments d’expérience créés directement sous le dossier parent. Le dossier enfant n’hérite pas de la configuration des services cloud parents.
* Vous pouvez obtenir le code du client cible à partir de Adobe Experience Cloud > Lancer la Cible > Sous Onglet Configurer > Implémentation > Modifier les paramètres at.js.
* Vous pouvez obtenir le nom d’utilisateur et le mot de passe de l’API de cible en envoyant un ticket au service à la clientèle avec une demande d’activation de la fonctionnalité d’intégration de Cible de fragments d’expérience.

## Ressources supplémentaires {#additional-resources}

* [Documentation des fragments d’expérience](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Utilisation des fragments d’expérience](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
