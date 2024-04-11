---
title: Intégrer des balises dans Adobe Experience Platform et AEM
description: Les balises pour la collecte de données Experience Platform constituent la solution de nouvelle génération d’Adobe pour la gestion de balises et constituent aussi le meilleur moyen de déployer Adobe Analytics, Target, Audience Manager et de nombreuses autres solutions. Découvrez une vue d’ensemble des balises dans Adobe Experience Platform et l’intégration recommandée avec Adobe Experience Manager.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 256
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Intégrer Balises pour la collecte de données Experience Platform et AEM {#overview}

Découvrez comment intégrer les balises dans Adobe Experience Platform avec Adobe Experience Manager.

Balises représente la nouvelle génération de technologie de gestion des balises d’Adobe Experience Platform. Balises constitue le moyen le plus simple de déployer Adobe Analytics, Target, Audience Manager et de nombreuses autres solutions. Découvrez Balises et l’intégration recommandée avec Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## Conditions préalables

Les éléments suivants sont requis lors de l’intégration de Balises pour la collecte de données Experience Platform.

+ Un accès administratif AEM à l’environnement AEM as a Cloud Service.
+ Un site de référence comme [WKND](https://github.com/adobe/aem-guides-wknd) déployé sur celui-ci.
+ Un accès à la solution de collecte de données Adobe Experience Platform
+ Un accès d’administration système à [Adobe Developer Console](https://developer.adobe.com/developer-console/)


## Étapes avancées

+ Dans la collecte de données Adobe Experience Platform, créez une propriété Tag et modifiez-la en _Ajoutant une règle_. Puis _Ajoutez une bibliothèque_, sélectionnez la règle nouvellement ajoutée, approuvez-la et publiez-la.
+ Connecter AEM et Balises à l’aide d’une configuration IMS existante (ou nouvelle)
+ Dans AEM, créez une configuration de services cloud de balises, puis appliquez-la à un site existant et vérifiez que la propriété Balises et ses bibliothèques sont chargées sur le site de publication ou de création.

## Étapes suivantes

[Créer une propriété de balise](create-tag-property.md)

## Ressources supplémentaires {#additional-resources}

+ [Intégrations Experience Platform à des applications Experience Cloud.](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=fr)
+ [Vue d’ensemble de Balises](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=fr)
+ [Implémenter Experience Cloud dans les sites web avec Balises](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html?lang=fr)
