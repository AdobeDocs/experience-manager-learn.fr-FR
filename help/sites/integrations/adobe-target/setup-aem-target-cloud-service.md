---
title: Créer un compte Cloud Service Adobe Target
description: Procédure pas à pas pour intégrer Adobe Experience Manager en tant que Cloud Service à Adobe Target à l'aide de l'authentification IMS Cloud Service et Adobe
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---


# Créer un compte Cloud Service Adobe Target {#adobe-target-cloud-service}

Procédure pas à pas pour intégrer Adobe Experience Manager en tant que Cloud Service à Adobe Target à l&#39;aide de l&#39;authentification IMS Cloud Service et Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Un problème connu avec la configuration des Cloud Services Adobe Target s&#39;affiche dans la vidéo. Il est recommandé de suivre les mêmes étapes dans la vidéo, mais d’utiliser la configuration [Cloud Services Adobe Target](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)hérité.

## Problèmes courants

Lors de l’exportation d’un fragment d’expérience vers une cible d’Adobe, si votre intégration de Cible dans la console d’administration ne dispose pas des autorisations appropriées, vous pouvez recevoir une erreur comme illustré ci-dessous.

**Erreur**![de l&#39;interface utilisateur de l&#39;API de Cible](assets/error-target-offer.png)

**Erreur**![de journal de la console API de Cible](assets/target-console-error.png)


**Solution**

1. Accéder au [Admin Console](https://adminconsole.adobe.com/)
2. Sélectionnez Produits > Adobe Target > Profil de produits.
3. Sous l&#39;onglet Intégrations, sélectionnez votre intégration (projet d&#39;E/S d&#39;Adobe).
4. Affecter un rôle d’éditeur ou d’approbateur.

![Erreur de l&#39;API de cible](assets/target-permissions.png)

ajouter les autorisations appropriées à votre intégration de Cible devrait vous aider à résoudre le problème ci-dessus.