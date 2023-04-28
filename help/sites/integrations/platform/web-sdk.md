---
title: Intégrer le SDK Web Experience Platform
description: Découvrez comment intégrer AEM as a Cloud Service avec le SDK Web Experience Platform. Cette étape essentielle est essentielle pour intégrer des produits Adobe Experience Cloud, tels qu’Adobe Analytics, Target, ou des produits innovants récents tels que Real-time Customer Data Platform, Customer Journey Analytics et Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
exl-id: b5182d35-ec38-4ffd-ae5a-ade2dd3f856d
source-git-commit: 63afa03de70d6f8f695d552018344d53a5cec6f5
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 5%

---

# Intégrer le SDK Web Experience Platform

Découvrez comment intégrer AEM as a Cloud Service à Experience Platform [SDK Web](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Cette étape essentielle est essentielle pour intégrer des produits Adobe Experience Cloud, tels qu’Adobe Analytics, Target, ou des produits innovants récents tels que Real-time Customer Data Platform, Customer Journey Analytics et Journey Optimizer.

Vous apprenez également à collecter et à envoyer [WKND - exemple de projet Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) données pageview dans la variable [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=fr).

Après avoir terminé cette configuration, vous avez mis en oeuvre une base solide. En outre, vous êtes prêt à faire avancer la mise en oeuvre de l’Experience Platform à l’aide d’applications telles que [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), et [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=fr). L’implémentation avancée permet d’augmenter l’engagement des clients en normalisant les données web et clients.

## Prérequis

Les éléments suivants sont requis lors de l’intégration du SDK Web Experience Platform.

Dans **AEM en tant que Cloud Service**:

+ Accès AEM administrateur à AEM environnement as a Cloud Service
+ Accès de Deployment Manager à Cloud Manager
+ Cloner et déployer le [WKND - exemple de projet Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) à votre environnement as a Cloud Service AEM.

Dans **Experience Platform**:

+ Accès à la production par défaut, **Prod** sandbox.
+ Accès à **Schémas** Sous Data Management
+ Accès à **Jeux de données** Sous Data Management
+ Accès à **Datastreams** Sous Collecte de données
+ Accès à **Balises** (anciennement Launch) sous Collecte de données

Si vous ne disposez pas des autorisations nécessaires, votre administrateur système utilisant [Adobe Admin Console](https://adminconsole.adobe.com/) peuvent accorder les autorisations nécessaires.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Création d’un schéma XDM - Experience Platform

Le schéma de modèle de données d’expérience (XDM) vous aide à normaliser les données d’expérience client. Pour collecter le **Pageview WKND** données, créer un schéma XDM et utiliser les groupes de champs fournis par l’Adobe `AEP Web SDK ExperienceEvent` pour la collecte de données web.

Il existe des secteurs génériques et spécifiques, par exemple le commerce de détail, les services financiers, les soins de santé, etc., une suite de modèles de données de référence, voir [Présentation des modèles de données du secteur](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) pour plus d’informations.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Découvrez le schéma XDM et les concepts associés, tels que les groupes de champs, les types, les classes et les types de données, issus de [Présentation du système XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

Le [Présentation du système XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) est une excellente ressource pour en savoir plus sur le schéma XDM et les concepts connexes tels que les groupes de champs, les types, les classes et les types de données. Il offre une compréhension complète du modèle de données XDM et de la création et de la gestion de schémas XDM pour normaliser les données dans l’entreprise. Explorez-le pour mieux comprendre le schéma XDM et les avantages qu’il peut présenter à vos processus de collecte et de gestion des données.

## Créer un flux de données - Experience Platform

Un flux de données indique au réseau Platform Edge où envoyer les données collectées. Par exemple, il peut être envoyé à Experience Platform, Analytics ou Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Familiarisez-vous avec le concept des flux de données et les sujets connexes, tels que la gouvernance et la configuration des données, en consultant la section [Présentation des flux de données](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html?lang=fr) page.

## Créer une propriété de balise - Experience Platform

Découvrez comment créer une propriété de balise (anciennement connue sous le nom de Launch) dans Experience Platform pour ajouter la bibliothèque JavaScript du SDK Web au site Web WKND. La propriété de balise nouvellement définie comporte les ressources suivantes :

+ Extensions de balise : [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) et [SDK Web Adobe Experience Platform](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Éléments de données : Éléments de données de type de code personnalisé qui extraient page-name, site-section et nom-hôte à l’aide de la couche de données client Adobe du site WKND. En outre, l’élément de données de type objet XDM conforme à la version de schéma XDM WKND que vous venez de créer. [Création d’un schéma XDM](#create-xdm-schema---experience-platform) étape .
+ Règle : Envoyez des données à Platform Edge Network chaque fois qu’une page web WKND est visitée à l’aide de la couche de données client d’Adobe déclenchée. `cmp:show` .

Lors de la création et de la publication de la bibliothèque de balises à l’aide de la variable **Flux de publication**, vous pouvez utiliser la variable **Ajouter toutes les ressources modifiées** bouton . Pour sélectionner toutes les ressources telles que Data Element, Rule et Tag Extensions au lieu d’identifier et de sélectionner une ressource individuelle. En outre, pendant la phase de développement, vous pouvez publier la bibliothèque uniquement sur le _Développement_ , puis vérifiez et validez sur la page _Évaluation_ ou _Production_ environnement.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>L’élément de données et le code d’événement de règle affiché dans la vidéo sont disponibles à titre de référence, **développer l’élément d’accordéon ci-dessous ;**. Cependant, si vous n’utilisez PAS la couche de données client Adobe, vous devez modifier le code ci-dessous, mais le concept de définition des éléments de données et de leur utilisation dans la définition de règle s’applique toujours.


+++ Élément de données et code d’événement de règle

+ Le `Page Name` Code d’élément de données.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ Le `Site Section` Code d’élément de données.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('repo:path')) {
   let pagePath = event.component['repo:path'];
   
   let siteSection = '';
   
   //Check of html String in URL.
   if (pagePath.indexOf('.html') > -1) { 
    siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
   
    //replace slash with colon
    siteSection = siteSection.replaceAll('/', ':');
   
    //remove `:content`
    siteSection = siteSection.replaceAll(':content:','');
   }
   
       return siteSection 
   }
   ```

+ Le `Host Name` Code d’élément de données.

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ Le `all pages - on load` Code d’événement de règle

   ```javascript
   var pageShownEventHandler = function(evt) {
   // defensive coding to avoid a null pointer exception
   if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
       //trigger Launch Rule and pass event
       console.debug("cmp:show event: " + evt.eventInfo.path);
       var event = {
           //include the path of the component that triggered the event
           path: evt.eventInfo.path,
           //get the state of the component that triggered the event
           component: window.adobeDataLayer.getState(evt.eventInfo.path)
       };
   
       //Trigger the Launch Rule, passing in the new 'event' object
       // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
       // i.e 'event.component['someKey']'
       trigger(event);
       }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
       //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
       dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

+++


Le [Présentation des balises](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=fr) fournit des connaissances approfondies sur des concepts importants tels que les éléments de données, les règles et les extensions.

Pour plus d’informations sur l’intégration des composants principaux AEM à la couche de données client Adobe, reportez-vous à la section [Utilisation du guide Adobe de la couche de données client avec AEM composants principaux](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr).

## Connecter la propriété Tag à AEM

Découvrez comment lier la propriété de balise récemment créée à AEM via Adobe IMS et Adobe Launch Configuration dans AEM. Lorsqu’un environnement as a Cloud Service AEM est établi, plusieurs configurations de compte technique Adobe IMS sont automatiquement générées, y compris Adobe Launch. Toutefois, pour AEM version 6.5, vous devez en configurer une manuellement.

Après avoir lié la propriété de balise, le site WKND peut charger la bibliothèque JavaScript de la propriété de balise sur les pages web à l’aide de la configuration du service cloud Adobe Launch.

### Vérification du chargement de la propriété de balise sur WKND

Utilisation du débogueur Adobe Experience Platform [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) ou [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) , vérifiez si la propriété de balise se charge sur les pages WKND. Vous pouvez vérifier,

+ Les détails de propriété de balise tels que l’extension, la version, le nom, etc.
+ Version de la bibliothèque SDK Web de Platform, ID de flux de données
+ Objet XDM intégré `events` dans le SDK Web Experience Platform

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Créer un jeu de données - Experience Platform

Les données pageview collectées à l’aide du SDK Web sont stockées dans le lac de données Experience Platform sous la forme de jeux de données. Le jeu de données est une structure de stockage et de gestion pour une collecte de données telle qu’une table de base de données qui suit un schéma. Découvrez comment créer un jeu de données et configurer le flux de données créé précédemment pour envoyer des données à l’Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

Le [Présentation des jeux de données](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html?lang=fr) fournit des informations supplémentaires sur les concepts, les configurations et d’autres fonctionnalités d’ingestion.


## Données WKND pageview dans Experience Platform

Après la configuration du SDK Web avec AEM, en particulier sur le site WKND, il est temps de générer du trafic en parcourant les pages du site. Vérifiez ensuite que les données de page vue sont ingérées dans le jeu de données Experience Platform. Dans l’interface utilisateur du jeu de données, divers détails tels que le nombre total d’enregistrements, la taille et les lots ingérés s’affichent avec un graphique à barres attrayant visuellement.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Résumé

Très bon travail ! Vous avez terminé la configuration d’AEM avec le SDK web Experience Platform pour collecter et ingérer des données à partir d’un site web. Grâce à cette fondation, vous pouvez désormais explorer d’autres possibilités d’amélioration et d’intégration de produits tels qu’Analytics, Target, Customer Journey Analytics (CJA), et de nombreux autres afin de créer des expériences riches et personnalisées pour vos clients. Continuez à apprendre et à explorer pour exploiter tout le potentiel de Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)

## Ressources supplémentaires

+ [Utilisation de la couche de données client Adobe avec les composants principaux](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr)
+ [Intégration de balises de collecte de données Experience Platform et d’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [SDK web Adobe Experience Platform et présentation du réseau Edge](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutoriels sur la collecte de données](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Présentation de Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
