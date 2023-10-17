---
title: Intégrations d’AEM as a Cloud Service avec Adobe Experience Cloud
description: Découvrez les intégrations prises en charge par AEM as a Cloud Service avec d’autres produits Adobe Experience Cloud.
version: Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
kt: 10718
thumbnail: KT-10718.png
last-substantial-update: 2022-11-17T00:00:00Z
mini-toc-levels: 1
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="d’AEM as a Cloud Service" before-title="false"
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: ht
source-wordcount: '928'
ht-degree: 100%

---

# Intégrations d’AEM as a Cloud Service avec Adobe Experience Cloud

Découvrez les intégrations prises en charge par AEM as a Cloud Service avec d’autres produits Adobe Experience Cloud.
Cliquez sur le produit Experience Cloud pour obtenir de la documentation sur la configuration et l’utilisation des intégrations.

|                                                                   | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |           |            | ✔ |
| Advertising |           |            |          |
| [Analytics](#adobe-analytics) | ✔ | ✔ | ✔ |
| Audience Manager |           |            |          |
| Campaign Classic |           |            |          |
| Campaign Standard |           |            |          |
| [Commerce](#adobe-commerce) | ✔ | ✔ |          |
| Customer Journey Analytics |           |            |          |
| [Balises Experience Platform](#adobe-experience-platform-tags) | ✔ |            | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |           | ✔ |          |
| [Learning Manager](#adobe-learning-manager) | ✔ |            |          |
| Marketo Engage |           |            |          |
| Real-time CDP |           |            |          |
| [Sensei](#adobe-sensei) | ✔ | ✔ | ✔ |
| [Target](#adobe-target) | ✔ |            |          |
| [Workfront](#adobe-workfront) |           | ✔ |          |


## Adobe Acrobat Sign

Adobe Acrobat Sign (anciennement Acrobat Sign) active les processus de signature électronique pour les formulaires adaptatifs d’AEM Forms en améliorant les processus de traitement des documents dans les domaines juridique, commercial, de la paie, des ressources humaines et autres.

### AEM Forms

+ [Configuration de l’intégration Adobe Acrobat Sign](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html?lang=fr)
+ [Tutoriel sur AEM Forms et Adobe Acrobat Sign](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html?lang=fr)

## Adobe Analytics

L’intégration d’Adobe Analytics avec AEM as a Cloud Service vous permet de suivre l’activité de contenu et d’analyser les données à partir de n’importe quel emplacement du parcours client. De plus, vous bénéficiez de rapports polyvalents, de l’intelligence prédictive, etc.

### AEM Sites

+ [Configuration de l’intégration Adobe Analytics](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html?lang=fr)
+ [Tutoriel sur AEM Sites et Analytics](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html?lang=fr)
+ Couche de données client Adobe (ACDL)

   + [Étendre l’ACDL dans les composants de base du gestionnaire de contenu web AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html?lang=fr)
   + [Intégrer l’ACDL aux composants de base de la gestion de contenu web AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html?lang=fr)
   + [Gérer des données pilotée par les événements avec l’ACDL](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html?lang=fr)
   + [Tutoriel sur la couche de données client Adobe (ACDL)](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr)

### AEM Assets

+ [Vue d’ensemble des Insights d’Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html?lang=fr)
+ [Configuration des statistiques sur les ressources](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html?lang=fr#configure-asset-insights)
+ [Tutoriel sur les Insights d’Assets](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html?lang=fr)

### AEM Forms

+ [Configurer l’intégration d’Adobe Analytics](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html?lang=fr)

### AEM Sites

+ [Intégrer à Adobe Campaign Classic](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-campaign-classic.html?lang=fr#configure-user)
+ [Créer une newsletter Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/creating-newsletter.html?lang=fr)
+ [Documentation sur les composants principaux d’e-mail AEM](https://github.com/adobe/aem-core-email-components#aem-email-core-components)

## Adobe Commerce

L’intégration d’Adobe Commerce à AEM as a Cloud Service permet aux marques de se développer et d’innover comme jamais auparavant, en proposant des expériences commerciales uniques et en captant la croissance des dépenses en ligne. Lorsqu’AEM et Commerce agissent à l’unisson, les expériences immersives, omnicanales et personnalisées d’Experience Manager jouissent de la puissance des solutions de Commerce afin de proposer des expériences uniques, et ce à toutes les étapes du parcours d’achat. À la clé, un gain de temps précieux et un taux de conversion plus élevé.

### AEM Sites

+ [Guide d’utilisation d’AEM Content and Commerce](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html?lang=fr)


## Balises Adobe Experience Platform

Les balises Adobe Experience Platform (anciennement Adobe Launch, DTM) s’intègrent de manière transparente à AEM et constituent un moyen simple de déployer et de gérer les balises d’[analyse](#adobe-analytics), de [ciblage](#adobe-target), marketing et publicitaires indispensables à toute expérience client de grand cru.

### AEM Sites

+ [Guide d’utilisation des balises Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=fr)
+ [Tutoriel sur les balises Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=fr)

### AEM Forms

+ [Guide d’utilisation des balises Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=fr)
+ [Tutoriel sur les balises Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=fr)

## Adobe Journey Optimizer

Adobe Journey Optimizer vous permet de planifier des campagnes omnicanales et des moments personnalisés avec des millions de clientes et clients à partir d’une application unique. Le parcours entier est optimisé grâce à une prise de décision intelligente et l’apport d’Insights.

### AEM Assets

+ [Intégrer AEM Assets Essentials à Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/create-messages/create-email-content-with-the-message-editor.html?lang=fr)

## Adobe Learning Manager

Adobe Learning Manager (anciennement Adobe Captivate Prime) offre des formations sur mesure aux organisations et aux personnes employées.

### AEM Sites

+ [Intégrer AEM Sites à Adobe Learning Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-learning-manager.html?lang=fr)

## Adobe Sensei

Adobe Sensei fournit des technologies d’IA et d’apprentissage automatique pour transformer le processus de gestion de contenu grâce aux balises intelligentes, au recadrage intelligent, à la recherche visuelle et bien plus encore.

### AEM Sites

+ [Résumer le texte en fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html?lang=fr#summarizing-text)

### AEM Assets

+ [Balises intelligentes pour les images](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html?lang=fr)
+ [Balises intelligentes personnalisées pour les images](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html?lang=fr)
+ [Balises intelligentes pour les vidéos](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html?lang=fr)
+ [Recadrage intelligent](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html?lang=fr)
+ [Recherche visuelle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html?lang=fr)

### AEM Forms

+ [Service de conversion automatisée de formulaires](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html?lang=fr)


## Adobe Target

Adobe Target s’intègre à AEM as a Cloud Service afin de fournir une expérience web optimisée pour chaque personne finale, le tout optimisé par le contenu d’AEM.

### AEM Sites

+ [Configurer l’intégration d’Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=fr)
+ Fragments d’expérience vers Target

   + [Publier des fragments d’expérience sur Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=fr)
   + [Publier des fragments d’expérience au format JSON sur Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=fr)

+ [Utiliser AEM Context Hub avec Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html?lang=fr#creating-an-adobe-target-audience-using-the-audience-console)
+ [Tutoriel sur AEM Sites et Target](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html?lang=fr)

## Adobe Workfront

L’intégration d’Adobe Workfront à AEM as a Cloud Service simplifie le processus de création, de collaboration et de gestion du cycle de vie des ressources numériques.

### AEM Assets

+ [Configurer le connecteur amélioré Workfront](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html?lang=fr)
+ [Vidéos sur le connecteur amélioré Workfront](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html?lang=fr)
+ AEM Assets Essentials

   + [Guide d’utilisation d’Adobe Workfront pour Assets Essentials](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Vidéos Adobe Workfront et Assets Essentials](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html?lang=fr)
