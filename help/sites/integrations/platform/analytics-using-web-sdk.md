---
title: Intégrer AEM Sites et Adobe Analytics au SDK web Platform
description: Intégrez AEM Sites et Adobe Analytics à l’aide de l’approche moderne du SDK web de Platform.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
duration: 2330
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '1533'
ht-degree: 100%

---

# Intégrer AEM Sites et Adobe Analytics au SDK web Platform

Découvrez l’**approche moderne** de l’intégration d’Adobe Experience Manager (AEM) et d’Adobe Analytics à l’aide du SDK web Platform. Ce tutoriel complet vous guide tout au long du processus de collecte transparente des données pageView et clics CTA de [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project). Obtenez des informations précieuses en visualisant les données collectées dans Adobe Analysis Workspace, où vous pouvez explorer diverses mesures et dimensions. Explorez également le jeu de données Platform pour vérifier et analyser les données. Rejoignez-nous pour exploiter la puissance d’AEM et d’Adobe Analytics pour une prise de décision basée sur les données.

## Vue d’ensemble

L’obtention d’informations sur le comportement des utilisateurs et utilisatrices est un objectif essentiel pour chaque équipe marketing. En comprenant comment les utilisateurs et utilisatrices interagissent avec leur contenu, les équipes peuvent prendre des décisions éclairées, optimiser des stratégies et générer de meilleurs résultats. L’équipe marketing WKND, une entité fictive, a défini ses objectifs en implémentant Adobe Analytics sur son site web pour atteindre cet objectif. L’objectif principal est de collecter des données sur deux mesures clés : pages vues et clics CTA sur la page d’accueil.

En suivant les pages vues, l’équipe peut analyser les pages qui reçoivent le plus d’attention de la part des utilisateurs et utilisatrices. En outre, le suivi des clics CTA sur la page d’accueil fournit des informations précieuses sur l’efficacité des éléments d’appel à l’action de l’équipe. Ces données peuvent révéler les CTA qui intéressent les utilisateurs et utilisatrices, ceux qui doivent être ajustés, et potentiellement découvrir de nouvelles opportunités pour améliorer l’interaction client et générer des conversions.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Conditions préalables

Les éléments suivants sont requis lors de l’intégration d’Adobe Analytics à l’aide du SDK web Platform.

Vous avez terminé les étapes de configuration du tutoriel **[Intégrer le SDK web Experience Platform](./web-sdk.md)**.

Dans **AEM as a Cloud Service** :

+ [Accès d’administration AEM à l’environnement AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=fr)
+ Un accès responsable de déploiement à Cloud Manager.
+ Clonez et déployez [WKND - exemple de projet Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) sur votre environnement AEM as a Cloud Service.

Dans **Adobe Analytics** :

+ Accès à la création d’une **suite de rapports**
+ Accès à la création d’**Analysis Workspace**

Dans **Experience Platform** :

+ Accès à la sandbox **Prod** de production par défaut.
+ Accès à **Schémas** sous Gestion des données.
+ Accès à **Jeux de données** sous Gestion des données.
+ Accès à **Trains de données** sous Collecte de données.
+ Accès à **Balises** (anciennement Launch) sous Collecte de données.

Si vous ne disposez pas des autorisations nécessaires, votre administrateur ou administratrice système peut vous les accorder en utilisant l’[Adobe Admin Console](https://adminconsole.adobe.com/).

Avant de passer au processus d’intégration d’AEM et d’Analytics à l’aide du SDK web Platform, nous vous invitons à _récapituler les composants essentiels et les éléments clés_ qui ont été établis dans le tutoriel [Intégrer le SDK web Experience Platform](./web-sdk.md). Cela fournit une base solide pour l’intégration.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Après la récapitulation du schéma XDM, du train de données, du jeu de données, de la propriété Balises, et de la connexion de la propriété Balises et d’AEM, nous allons nous lancer dans le parcours d’intégration.

## Définir le document de référence de conception de solution d’analyse (SDR)

Dans le cadre du processus d’implémentation, il est recommandé de créer un document de référence de conception de solution (SDR). Ce document joue un rôle essentiel en tant que plan directeur pour définir les besoins de l’entreprise et concevoir des stratégies de collecte de données efficaces.

Le document SDR offre une vue d’ensemble complète du plan d’implémentation, en veillant à ce que toutes les parties prenantes soient alignées et comprennent les objectifs et la portée du projet.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Pour plus d’informations sur les concepts et les différents éléments qui doivent être inclus dans le document SDR, consultez la rubrique [Créer et gérer un document de référence de conception de solution (SDR)](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html?lang=fr). Vous pouvez également télécharger un exemple de modèle Excel, mais une version spécifique à WKND est également disponible [ici](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Configurer Analytics - Suite de rapports, Analysis Workspace

La première étape consiste à configurer Adobe Analytics, en particulier une suite de rapports avec des variables de conversion (ou eVar) et des événements de succès. Les variables de conversion sont utilisées pour mesurer les causes et les effets. Les événements de succès servent à suivre les actions.

Dans ce tutoriel, `eVar5, eVar6, and eVar7` suivent respectivement le _nom de page WKND, l’ID CTA WKND et le nom CTA WKND_. `event7` est utilisé pour suivre l’_événement de clic CTA WKND_.

Pour analyser, collecter des informations et partager ces informations avec d’autres à partir des données collectées, un projet Analysis Workspace est créé.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Pour en savoir plus sur la configuration et les concepts d’Analytics, nous vous recommandons vivement les ressources suivantes :

+ [Suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=fr)
+ [Variables de conversion](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=fr)
+ [Événements de succès](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html?lang=fr)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=fr)

## Mettre à jour un train de données - Ajouter un service Analytics

Un train de données indique au réseau Platform Edge où envoyer les données collectées. Dans le [tutoriel précédent](./web-sdk.md), un train de données est configuré pour envoyer les données à l’Experience Platform. Ce train de données est mis à jour pour envoyer les données à la suite de rapports Analytics qui a été configurée à l’étape [ci-dessus](#setup-analytics---report-suite-analysis-workspace).

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## Créer un schéma XDM

Le schéma de modèle de données d’expérience (XDM) vous aide à normaliser les données collectées. Dans le [tutoriel précédent](./web-sdk.md), un schéma XDM avec un groupe de champs `AEP Web SDK ExperienceEvent` est créé. En outre, l’utilisation de ce schéma XDM crée un jeu de données pour stocker les données collectées dans Experience Platform.

Toutefois, ce schéma XDM ne dispose pas de groupes de champs spécifiques à Adobe Analytics pour envoyer des données d’événement à l’eVar. Un nouveau schéma XDM est créé au lieu de mettre à jour le schéma existant afin d’éviter de stocker les données d’eVar et d’événement dans la plateforme.

Le schéma XDM que vous venez de créer contient des groupes de champs `AEP Web SDK ExperienceEvent` et `Adobe Analytics ExperienceEvent Full Extension`.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Mettre à jour la propriété de balise

Dans le [tutoriel précédent](./web-sdk.md), une propriété de balise est créée. Elle contient des éléments de données et une règle pour rassembler, mapper et envoyer les données pageView. Elle doit être améliorée pour :

+ Mapper le nom de la page à `eVar5`.
+ Déclencher l’appel Analytics **pageView** (ou envoi de balise).
+ Collecter des données CTA à l’aide de la couche de données client Adobe.
+ Mapper respectivement l’ID et le nom du CTA à `eVar6` et `eVar7`. En outre, le nombre de clics CTA pour `event7`.
+ Déclencher l’appel Analytics de **clic sur les liens** (ou envoi de balise).


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>Le code de l’élément de données et de l’événement de règle affiché dans la vidéo est disponible à titre de référence, **développez l’élément accordéon ci-dessous**. Cependant, si vous n’utilisez PAS la couche de données cliente Adobe, vous devez modifier le code ci-dessous, mais le concept de définition des éléments de données et de leur utilisation dans la définition de règle s’applique toujours.

+++ Code d’élément de données et d’événement de règle

+ Le code d’élément de données `Component ID`.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ Code d’élément de données `Component Name`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ Code de **condition de règle** `all pages - on load`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ Code d’**événement de règle** `home page - cta click`.

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
      // i.e `event.component['someKey']`
      trigger(event);
  }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  //push the event listener for cmp:click into the data layer
  window.adobeDataLayer.push(function (dl) {
  //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
  dl.addEventListener("cmp:click", componentClickedHandler);
  });    
  ```

+ Code de **condition de règle** `home page - cta click`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

Pour plus d’informations sur l’intégration des composants principaux AEM à la couche de données client Adobe, consultez le [Guide d’utilisation de la couche de données client Adobe avec les composants principaux AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr).


>[!INFO]
>
>Pour une meilleure compréhension des détails des propriétés de l’onglet **Mappage des variables** dans le document SDR (Solution Design Reference), accédez à la version complète spécifique à WKND à télécharger [ici](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## Vérifier la propriété de balise mise à jour sur WKND

Vous devez vous assurer que la propriété de balise mise à jour a bien été créée et publiée, et qu’elle fonctionne correctement sur les pages du site WKND. Utilisez l’[extension Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) du navigateur web Google Chrome :

+ Pour vous assurer que vous disposez de la dernière version de la propriété de balise, vérifiez sa date de création.

+ Pour vérifier les données d’événement XDM pour les pages vues et les clics sur le bouton CTA de la page d’accueil, utilisez l’option de menu SDK web Experience Platform dans l’extension.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Simuler du trafic web : automatisation Selenium

Pour générer une quantité significative de trafic à des fins de test, un script d’automatisation Selenium doit être développé. Ce script personnalisé simule les interactions de l’utilisateur ou de l’utilisatrice avec le site web WKND, comme les pages vues et les clics sur les boutons CTA.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Vérifier le jeu de données : pages vues WKND, données CTA

Le jeu de données est une structure de stockage et de gestion pour une collecte de données telle qu’un tableau de base de données qui suit un schéma. Le jeu de données créé dans le [tutoriel précédent](./web-sdk.md) est réutilisé pour vérifier que les données relatives aux pages vues et aux clics sur le bouton CTA sont ingérées dans le jeu de données Experience Platform. Dans l’interface utilisateur du jeu de données, divers détails tels que le nombre total d’enregistrements, la taille et les lots ingérés s’affichent avec un graphique en barres visuellement attrayant.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics : pages vues WKND, visualisation des données CTA

Analysis Workspace est un outil puissant d’Adobe Analytics qui permet d’explorer et de visualiser les données de manière flexible et interactive. Elle offre une interface glisser-déposer pour créer des rapports personnalisés, effectuer une segmentation avancée et appliquer diverses visualisations de données.

Ouvrons à nouveau le projet Analysis Workspace créé à l’étape de [Configuration d’Analytics](#setup-analytics---report-suite-analysis-workspace). Dans la section **Pages principales**, examinez diverses mesures telles que les visites, les visiteurs et visiteuses uniques, les entrées, le taux de rebond, etc. Pour évaluer les performances des pages WKND et des boutons CTA sur la page d’accueil, glissez et déposez des dimensions spécifiques à WKND (nom de page WKND, nom du bouton CTA WKND) et des mesures (événement de clic sur le bouton CTA WKND). Ces informations s’avèrent utiles pour les personnes responsables du marketing afin de déterminer les boutons CTA les plus efficaces et de prendre des décisions fondées sur les données, en fonction de leurs objectifs commerciaux.

Pour visualiser les parcours utilisateur, utilisez la visualisation Flux, en commençant par le **Nom de page WKND** et en développant différents chemins.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Résumé

Très bon travail. Vous avez terminé la configuration d’AEM et d’Adobe Analytics à l’aide du SDK web Platform pour collecter et analyser les données sur les pages vues et les clics sur les boutons CTA.

La mise en œuvre d’Adobe Analytics est essentielle pour que les équipes marketing puissent obtenir des informations sur le comportement des utilisateurs et des utilisatrices et prendre des décisions éclairées. Elles peuvent ainsi optimiser leur contenu et prendre des décisions fondées sur les données.

En implémentant les étapes recommandées et en utilisant les ressources fournies, telles que le document SDR (Solution Design Reference) et en comprenant les concepts clés d’Analytics, les personnes responsables du marketing peuvent collecter et analyser les données de manière efficace.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Si vous préférez regarder la **vidéo de bout en bout** qui couvre l’ensemble du processus d’intégration (au lieu de vidéos d’étapes de configuration individuelles), cliquez [ici](https://video.tv.adobe.com/v/3419889/) pour y accéder.


## Ressources supplémentaires

+ [Intégrer le SDK web Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html?lang=fr)
+ [Utilisation de la couche de données de la clientèle Adobe avec les composants principaux](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr)
+ [Intégration de balises de collecte de données Experience Platform et AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=fr)
+ [Vue d’ensemble du SDK web Adobe Experience Platform et du réseau Edge](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html?lang=fr)
+ [Tutoriels sur la collecte de données](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html?lang=fr)
+ [Vue d’ensemble d’Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=fr)
