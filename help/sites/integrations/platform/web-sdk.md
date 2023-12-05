---
title: Intégrer AEM Sites et le SDK web Experience Platform
description: Découvrez comment intégrer AEM Sites as a Cloud Service au SDK web Experience Platform. Cette étape est essentielle pour intégrer des produits Adobe Experience Cloud, tels qu’Adobe Analytics, Target, ou des produits innovants récents tels que Real-time Customer Data Platform, Customer Journey Analytics et Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service" before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
duration: 1398
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1235'
ht-degree: 100%

---

# Intégrer AEM Sites et le SDK web Experience Platform

Découvrez comment intégrer AEM as a Cloud Service au [SDK web](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=fr) Experience Platform. Cette étape est essentielle pour intégrer des produits Adobe Experience Cloud, tels qu’Adobe Analytics, Target, ou des produits innovants récents tels que Real-time Customer Data Platform, Customer Journey Analytics et Journey Optimizer.

Vous apprenez également à collecter et à envoyer les données pageview [WKND - exemple de projet Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) dans [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=fr).

Après avoir terminé cette configuration, vous disposerez d’une base solide. En outre, vous êtes en mesure de faire progresser la mise en œuvre d’Experience Platform à l’aide d’applications telles que [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=fr), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html?lang=fr), et [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=fr). La mise en œuvre avancée permet d’augmenter l’engagement des clientes et clients en normalisant les données web et clientes.

## Conditions préalables

Les éléments suivants sont requis lors de l’intégration du SDK web Experience Platform.

Dans **AEM as a Cloud Service** :

+ Un accès administratif AEM à l’environnement AEM as a Cloud Service.
+ Un accès responsable de déploiement à Cloud Manager.
+ Clonez et déployez [WKND - exemple de projet Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) sur votre environnement AEM as a Cloud Service.

Dans **Experience Platform** :

+ Accès à la sandbox **Prod** de production par défaut.
+ Accès à **Schémas** sous Gestion des données.
+ Accès à **Jeux de données** sous Gestion des données.
+ Accès à **Trains de données** sous Collecte de données.
+ Accès à **Balises** (anciennement Launch) sous Collecte de données.

Si vous ne disposez pas des autorisations nécessaires, votre administrateur ou administratrice système peut vous les accorder en utilisant l’[Adobe Admin Console](https://adminconsole.adobe.com/).

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Créer un schéma XDM - Experience Platform

Le schéma de modèle de données d’expérience (XDM) vous aide à normaliser les données d’expérience client. Pour collecter les données **pageView WKND**, créez un schéma XDM et utilisez les groupes de champs `AEP Web SDK ExperienceEvent` fournis par Adobe pour la collecte de données web.

Il existe des modèles de données génériques et spécifiques à certains secteurs, par exemple la vente au détail, les services financiers, les soins de santé et bien d’autres encore. Voir [Vue d’ensemble des modèles de données de secteur](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html?lang=fr) pour plus d’informations.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Découvrez le schéma XDM et les concepts associés tels que les groupes de champs, les types, les classes et les types de données dans la [vue d’ensemble du système XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=fr).

La [vue d’ensemble du système XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=fr) est une excellente ressource pour en savoir plus sur le schéma XDM et les concepts connexes tels que les groupes de champs, les types, les classes et les types de données. Elle offre une compréhension complète du modèle de données XDM et explique comment créer et gérer les schémas XDM pour normaliser les données dans l’entreprise. Explorez-la pour mieux comprendre le schéma XDM et les avantages qu’il peut présenter à vos processus de collecte et de gestion des données.

## Créer un train de données - Experience Platform

Un train de données indique au réseau Platform Edge où envoyer les données collectées. Par exemple, elles peuvent être envoyées à Experience Platform, Analytics ou Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Familiarisez-vous avec le concept des trains de données et les sujets connexes, tels que la gouvernance et la configuration des données, en consultant la page [vue d’ensemble des trains de données](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html?lang=fr).

## Créer une propriété de balise - Experience Platform

Découvrez comment créer une propriété de balise (anciennement connue sous le nom de Launch) dans Experience Platform pour ajouter la bibliothèque JavaScript du SDK web au site web WKND. La propriété de balise nouvellement définie comporte les ressources suivantes :

+ Extensions de balise : [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) et [SDK web Adobe Experience Platform](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Éléments de données : éléments de données de type de code personnalisé qui extraient page-name, site-section et host-name à l’aide de la couche de données cliente Adobe du site WKND. En outre, l’élément de données de type objet XDM conforme au schéma XDM WKND nouvellement créé lors de l’étape précédente [Créer un schéma XDM](#create-xdm-schema---experience-platform).
+ Règle : envoyer des données au réseau Platform Edge chaque fois qu’une page web WKND est visitée à l’aide de l’événement `cmp:show` déclenché par la couche de données cliente Adobe.

Lors de la création et de la publication de la bibliothèque de balises à l’aide du **Flux de publication**, vous pouvez utiliser le bouton **Ajouter toutes les ressources modifiées**. Pour sélectionner toutes les ressources telles qu’un Élément de données, une Règle et des Extensions de balises au lieu d’identifier et de sélectionner une ressource individuelle. En outre, pendant la phase de développement, vous pouvez publier la bibliothèque uniquement sur l’environnement de _Développement_, puis la vérifier et la promouvoir dans l’environnement d’_évaluation_ ou de _Production_.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>Le code de l’élément de données et de l’événement de règle affiché dans la vidéo est disponible à titre de référence, **développez l’élément accordéon ci-dessous**. Cependant, si vous n’utilisez PAS la couche de données cliente Adobe, vous devez modifier le code ci-dessous, mais le concept de définition des éléments de données et de leur utilisation dans la définition de règle s’applique toujours.


+++ Code d’élément de données et d’événement de règle

+ Le code d’élément de données `Page Name`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ Le code d’élément de données `Site Section`.

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

+ Le code d’élément de données `Host Name`.

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ Le code d’événement de règle `all pages - on load`.

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


La [vue d’ensemble des balises](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=fr) fournit des connaissances approfondies sur des concepts importants tels que les éléments de données, les règles et les extensions.

Pour plus d’informations sur l’intégration des composants principaux AEM à la couche de données cliente Adobe, reportez-vous au [guide Utilisation de la couche de données cliente Adobe avec les composants principaux AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr).

## Connecter la propriété de balise à AEM

Découvrez comment lier la propriété de balise récemment créée à AEM via Adobe IMS et la configuration d’Adobe Launch dans AEM. Lorsqu’un environnement AEM as a Cloud Service est établi, plusieurs configurations de compte technique Adobe IMS sont automatiquement générées, y compris Adobe Launch. Toutefois, pour la version AEM 6.5, vous devez en configurer une manuellement.

Après avoir lié la propriété de balise, le site WKND peut charger la bibliothèque JavaScript de la propriété de balise sur les pages web à l’aide de la configuration du service cloud Adobe Launch.

### Vérifier le chargement de la propriété de balise sur WKND

À l’aide de l’extension [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) ou [Firefox](https://addons.mozilla.org/fr/firefox/addon/adobe-experience-platform-dbg/) de l’Adobe Experience Platform Debugger, vérifiez si la propriété de balise se charge sur les pages WKND. Vous pouvez vérifier :

+ Les détails de propriété de balise tels que l’extension, la version, le nom, etc.
+ La version de la bibliothèque SDK web de Platform, l’ID de train de données.
+ L’objet XDM en tant qu’attribut `events` dans le SDK web Experience Platform.

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Créer un jeu de données - Experience Platform

Les données pageView collectées à l’aide du SDK web sont stockées dans le lac de données Experience Platform sous la forme de jeux de données. Le jeu de données est une structure de stockage et de gestion pour une collecte de données telle qu’un tableau de base de données qui suit un schéma. Découvrez comment créer un jeu de données et configurer le train de données créé précédemment pour envoyer des données vers Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

La [vue d’ensemble des jeux de données](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html?lang=fr) fournit des informations supplémentaires sur les concepts, les configurations et d’autres fonctionnalités d’ingestion.


## Données WKND pageView dans Experience Platform

Après la configuration du SDK web avec AEM, en particulier sur le site WKND, il est temps de générer du trafic en parcourant les pages du site. Confirmez ensuite que les données de pageView sont ingérées dans le jeu de données Experience Platform. Dans l’interface utilisateur du jeu de données, divers détails tels que le nombre total d’enregistrements, la taille et les lots ingérés s’affichent avec un graphique en barres visuellement attrayant.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Résumé

Très bon travail. Vous avez terminé la configuration d’AEM avec le SDK web Experience Platform permettant de collecter et d’ingérer des données à partir d’un site web. Grâce à ces bases établies, vous pouvez désormais explorer d’autres possibilités d’amélioration et d’intégration de produits, parmi lesquels Analytics, Target et Customer Journey Analytics (CJA), afin de créer des expériences riches et personnalisées pour votre clientèle. Continuez à apprendre et à explorer pour exploiter tout le potentiel d’Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Si vous préférez consulter une **vidéo de bout en bout** qui couvre l’ensemble du processus d’intégration plutôt que des vidéos individuelles de chaque étape de configuration, cliquez [ici](https://video.tv.adobe.com/v/3418905/).

## Ressources supplémentaires

+ [Utilisation de la couche de données de la clientèle Adobe avec les composants principaux](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr)
+ [Intégration de balises de collecte de données Experience Platform et AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=fr)
+ [Vue d’ensemble du SDK web Adobe Experience Platform et du réseau Edge](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html?lang=fr)
+ [Tutoriels sur la collecte de données](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html?lang=fr)
+ [Vue d’ensemble d’Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=fr)
