---
title: Intégration de balises de collecte de données Experience Platform (Launch) et d’AEM
description: Les balises dans la collecte de données Experience Platform sont la solution de gestion des balises de nouvelle génération de l’Adobe et le meilleur moyen de déployer Adobe Analytics, Target, l’Audience Manager et de nombreuses autres solutions. Obtenez un aperçu des balises (anciennement appelées Launch) et de l’intégration recommandée avec Adobe Experience Manager.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 8%

---

# Intégration de balises de collecte de données Experience Platform et d’AEM {#overview}

Découvrez comment intégrer l’Experience Platform _Balises de collecte de données_ (anciennement Launch) avec Adobe Experience Manager.

>[!NOTE]
>
>Adobe Experience Platform Launch a été rebaptisé en tant que suite de technologies de collecte de données dans Adobe Experience Platform. Plusieurs modifications terminologiques ont par conséquent été apportées à la documentation du produit. Reportez-vous aux [document](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) pour une référence consolidée des modifications terminologiques.


Les balises sont Adobe Experience Platform qui  la nouvelle génération de la technologie de gestion des balises. Les balises constituent le moyen le plus simple de déployer Adobe Analytics, Target, l’Audience Manager et de nombreuses autres solutions. Obtenez un aperçu des balises et de l’intégration recommandée avec Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## Conditions préalables

Les éléments suivants sont requis lors de l’intégration de balises de collecte de données Experience Platform.

+ AEM accès administrateur à AEM environnement as a Cloud Service
+ Un site de référence comme [WKND](https://github.com/adobe/aem-guides-wknd) déployé sur celle-ci.
+ Accès à la solution de collecte de données Adobe Experience Platform
+ Accès administrateur système à [Console Adobe Developer](https://developer.adobe.com/developer-console/)


## Étapes de haut niveau

+ Dans la collecte de données Adobe Experience Platform, créez une propriété Tag et modifiez-la en _Ajouter une règle_. Alors _Ajouter une bibliothèque_, sélectionnez la règle nouvellement ajoutée, approuvez-la et publiez-la.
+ Connexion des AEM et des balises à l’aide d’une configuration IMS existante (ou nouvelle)
+ Dans AEM, créez une configuration de services cloud Launch, puis appliquez-la à un site existant et vérifiez enfin la propriété Balises et ses bibliothèques sont chargées sur le site Publié ou Auteur.

## Étapes suivantes

[Création d’une propriété de balise](create-tag-property.md)

## Ressources supplémentaires {#additional-resources}

+ [Intégrations Experience Platform à des applications Experience Cloud.](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=fr)
+ [Présentation des balises](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=fr)
+ [Mise en oeuvre de l’Experience Cloud dans les sites web avec des balises](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
