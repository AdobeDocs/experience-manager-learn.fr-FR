---
title: Intégration d’AEM Sites et d’Adobe Analytics au SDK Web de Platform
description: Intégrez AEM Sites et Adobe Analytics à l’aide de l’approche SDK Web Platform moderne.
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
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '1637'
ht-degree: 3%

---

# Intégration d’AEM Sites et d’Adobe Analytics au SDK Web de Platform

En savoir plus **approche moderne** sur la manière d’intégrer Adobe Experience Manager (AEM) et Adobe Analytics à l’aide du SDK Web Platform. Ce tutoriel complet vous guide tout au long du processus de collecte transparente [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) données pageview et CTA click. Obtenez des informations précieuses en visualisant les données collectées dans Adobe Analysis Workspace, où vous pouvez explorer diverses mesures et dimensions. Explorez également le jeu de données Platform pour vérifier et analyser les données. Rejoignez-nous sur ce parcours pour exploiter la puissance d’AEM et d’Adobe Analytics pour une prise de décision basée sur les données.

## Vue d’ensemble

L’obtention d’informations sur le comportement des utilisateurs est un objectif essentiel pour chaque équipe marketing. En comprenant comment les utilisateurs interagissent avec leur contenu, les équipes peuvent prendre des décisions éclairées, optimiser des stratégies et générer de meilleurs résultats. L’équipe marketing WKND, une entité fictive, a défini ses objectifs en mettant en oeuvre Adobe Analytics sur son site web pour atteindre cet objectif. L’objectif principal est de collecter des données sur deux mesures clés : pages vues et clics CTA (homepage call-to-action).

En suivant les pages vues, l’équipe peut analyser les pages qui reçoivent le plus d’attention de la part des utilisateurs. En outre, le suivi des clics CTA sur la page d’accueil fournit des informations précieuses sur l’efficacité des éléments d’appel à l’action de l’équipe. Ces données peuvent révéler quelles CTA intéressent les utilisateurs, lesquels doivent être ajustés, et potentiellement découvrir de nouvelles opportunités pour améliorer l’engagement des utilisateurs et générer des conversions.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Conditions préalables requises

Les éléments suivants sont requis lors de l’intégration d’Adobe Analytics à l’aide du SDK Web Platform.

Vous avez terminé les étapes de configuration de la **[Intégrer le SDK Web Experience Platform](./web-sdk.md)** tutoriel .

Dans **AEM en tant que Cloud Service**:

+ [Accès AEM administrateur à AEM environnement as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=fr)
+ Accès de Deployment Manager à Cloud Manager
+ Cloner et déployer le [WKND - exemple de projet Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) à votre environnement as a Cloud Service AEM.

Dans **Adobe Analytics**:

+ Accès à la création **Suite de rapports**
+ Accès à la création **Analysis Workspace**

Dans **Experience Platform**:

+ Accès à la production par défaut, **Prod** sandbox.
+ Accès à **Schémas** Sous Data Management
+ Accès à **Jeux de données** Sous Data Management
+ Accès à **Datastreams** Sous Collecte de données
+ Accès à **Balises** (anciennement Launch) sous Collecte de données

Si vous ne disposez pas des autorisations nécessaires, votre administrateur système utilisant [Adobe Admin Console](https://adminconsole.adobe.com/) peuvent accorder les autorisations nécessaires.

Avant de passer au processus d’intégration d’AEM et d’Analytics à l’aide du SDK Web Platform, nous vous invitons à _récapitulation des composants essentiels et des éléments clés_ qui ont été établies dans la variable [Intégrer le SDK Web Experience Platform](./web-sdk.md) tutoriel . Il fournit une base solide pour l’intégration.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Après la récapitulation du schéma XDM, du flux de données, du jeu de données, de la propriété Tag et, AEM et de la connexion à la propriété tag, nous allons nous lancer dans le parcours d’intégration.

## Définition du document de référence de conception de solution Analytics (SDR)

Dans le cadre du processus de mise en oeuvre, il est recommandé de créer un document de référence de conception de solution (SDR). Ce document joue un rôle essentiel en tant que plan directeur pour définir les besoins de l’entreprise et concevoir des stratégies de collecte de données efficaces.

Le document SDR offre un aperçu complet du plan de mise en oeuvre, en veillant à ce que toutes les parties prenantes soient alignées et comprennent les objectifs et la portée du projet.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Pour plus d’informations sur les concepts et les différents éléments qui doivent être inclus dans le document SDR, consultez la rubrique [Créer et gérer un document de référence de conception de solution (SDR)](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html). Vous pouvez également télécharger un exemple de modèle Excel, mais une version spécifique à WKND est également disponible. [here](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Configuration d’Analytics - suite de rapports, Analysis Workspace

La première étape consiste à configurer Adobe Analytics, en particulier une suite de rapports avec des variables de conversion (ou eVar) et des événements de succès. Les variables de conversion sont utilisées pour mesurer les causes et les effets. Les événements de succès servent à effectuer le suivi des actions.

Dans ce tutoriel,  `eVar5, eVar6, and eVar7` track  _Nom de page WKND, ID CTA WKND et nom CTA WKND_ respectivement, et `event7` est utilisé pour effectuer le suivi  _Événement de clic CTA WKND_.

Pour analyser, collecter des informations et partager ces informations avec d’autres à partir des données collectées, un projet Analysis Workspace est créé.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Pour en savoir plus sur la configuration et les concepts d’Analytics, nous vous recommandons vivement les ressources suivantes :

+ [Suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=fr)
+ [Variables de conversion](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [Événements de succès](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## Mise à jour du flux de données - Ajout d’un service Analytics

Un flux de données indique au réseau Platform Edge où envoyer les données collectées. Dans le [tutoriel précédent](./web-sdk.md), un flux de données est configuré pour envoyer les données à l’Experience Platform. Ce flux de données est mis à jour pour envoyer les données à la suite de rapports Analytics qui a été configurée dans la variable [above](#setup-analytics---report-suite-analysis-workspace) étape .

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## Créer un schéma XDM

Le schéma de modèle de données d’expérience (XDM) vous aide à normaliser les données collectées. Dans le [tutoriel précédent](./web-sdk.md), un schéma XDM avec `AEP Web SDK ExperienceEvent` un groupe de champs est créé. En outre, l’utilisation de ce schéma XDM crée un jeu de données pour stocker les données collectées dans l’Experience Platform.

Toutefois, ce schéma XDM ne dispose pas de groupes de champs spécifiques à Adobe Analytics pour envoyer l’eVar, les données d’événement. Un nouveau schéma XDM est créé au lieu de mettre à jour le schéma existant afin d’éviter de stocker les données d’eVar et d’événement dans la plateforme.

Le schéma XDM que vous venez de créer contient `AEP Web SDK ExperienceEvent` et `Adobe Analytics ExperienceEvent Full Extension` groupes de champs.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Mettre à jour la propriété Tag

Dans le [tutoriel précédent](./web-sdk.md), une propriété de balise est créée. Elle contient des éléments de données et une règle pour rassembler, mapper et envoyer les données de page vue. Il doit être amélioré pour les éléments suivants :

+ Mappage du nom de page à `eVar5`
+ Déclenchement de la variable **pageview** Appel Analytics (ou envoi de balise)
+ Collecte de données CTA à l’aide de la couche de données client Adobe
+ Mappage de l’ID et du nom du CTA à `eVar6` et `eVar7` respectivement. En outre, le nombre de clics CTA pour `event7`
+ Déclenchement de la variable **clic sur les liens** Appel Analytics (ou envoi de balise)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>L’élément de données et le code d’événement de règle affiché dans la vidéo sont disponibles à titre de référence, **développer l’élément d’accordéon ci-dessous ;**. Cependant, si vous n’utilisez PAS la couche de données client Adobe, vous devez modifier le code ci-dessous, mais le concept de définition des éléments de données et de leur utilisation dans la définition de règle s’applique toujours.

+++ Élément de données et code d’événement de règle

+ La variable `Component ID` Code d’élément de données.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ La variable `Component Name` Code d’élément de données.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ La variable `all pages - on load` **Rule-Condition** code

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ La variable `home page - cta click` **Rule-Event** code

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

+ La variable `home page - cta click` **Rule-Condition** code

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

Pour plus d’informations sur l’intégration des composants principaux AEM à la couche de données client Adobe, reportez-vous à la section [Utilisation du guide Adobe de la couche de données client avec AEM composants principaux](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr).


>[!INFO]
>
>Pour une compréhension complète de la variable **Carte des variables** Détails des propriétés de l’onglet dans le document Solution Design Reference (SDR) , accédez à la version complète spécifique à WKND à télécharger [here](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## Vérification de la propriété de balise mise à jour sur WKND

Pour vous assurer que la propriété de balise mise à jour est créée, publiée et fonctionne correctement sur les pages du site WKND. Utilisation du navigateur web Google Chrome [Extension Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob):

+ Pour vous assurer que la propriété de balise est la dernière version, vérifiez la date de création.

+ Pour vérifier les données d’événement XDM pour PageView et HomePage CTA Click, utilisez l’option de menu SDK Web Experience Platform dans l’extension.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Simulation du trafic web - Automatisation Selenium

Pour générer une quantité significative de trafic à des fins de test, un script d’automatisation Selenium est développé. Ce script personnalisé simule les interactions de l’utilisateur avec le site web WKND, comme la page vue et le fait de cliquer sur des CTA.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Vérification du jeu de données - Pageview WKND, données CTA

Le jeu de données est une structure de stockage et de gestion pour une collecte de données telle qu’une table de base de données qui suit un schéma. Le jeu de données créé dans la variable [tutoriel précédent](./web-sdk.md) est réutilisé pour vérifier que les données de page vue et de clic CTA sont ingérées dans le jeu de données Experience Platform. Dans l’interface utilisateur du jeu de données, divers détails tels que le nombre total d’enregistrements, la taille et les lots ingérés s’affichent avec un graphique à barres attrayant visuellement.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics - Pageview WKND, visualisation des données CTA

Analysis Workspace est un outil puissant d’Adobe Analytics qui permet d’explorer et de visualiser les données de manière flexible et interactive. Il offre une interface par glisser-déposer pour créer des rapports personnalisés, effectuer une segmentation avancée et appliquer diverses visualisations de données.

rouvrons le projet Analysis Workspace créé dans le [Configuration d’Analytics](#setup-analytics---report-suite-analysis-workspace) étape . Dans le **Pages principales** , examinez diverses mesures telles que les visites, les visiteurs uniques, les entrées, le taux de rebond, etc. Pour évaluer les performances des CTAS des pages WKND et de la page d’accueil, effectuez un glisser-déposer des dimensions spécifiques à WKND (nom de page WKND, nom CTA WKND) et des mesures (événement de clic CTA WKND). Ces informations s’avèrent utiles pour les marketeurs afin de déterminer les CTAS les plus efficaces et de prendre des décisions fondées sur les données, en fonction de leurs objectifs commerciaux.

Pour visualiser les parcours utilisateur, utilisez la visualisation Flux , en commençant par le **Nom de page WKND** et se développer en différents chemins.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Résumé

Très bon travail ! Vous avez terminé la configuration d’AEM et d’Adobe Analytics à l’aide du SDK Web de Platform pour collecter, analyser la page vue et les données de clic CTA.

La mise en oeuvre d’Adobe Analytics est essentielle pour que les équipes marketing puissent obtenir des informations sur le comportement des utilisateurs, prendre des décisions éclairées, leur permettant d’optimiser leur contenu et de prendre des décisions basées sur les données.

En implémentant les étapes recommandées et en utilisant les ressources fournies, telles que le document de référence pour la conception de solution (SDR) et en comprenant les concepts clés d’Analytics, les marketeurs peuvent collecter et analyser efficacement les données.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Si vous préférez la variable **vidéo de bout en bout** qui couvre l’ensemble du processus d’intégration au lieu des vidéos individuelles de l’étape de configuration, vous pouvez cliquer sur [here](https://video.tv.adobe.com/v/3419889/) pour y accéder.


## Ressources supplémentaires

+ [Intégrer le SDK Web Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [Utilisation de la couche de données client Adobe avec les composants principaux](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=fr)
+ [Intégration de balises de collecte de données Experience Platform et d’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=fr)
+ [SDK web Adobe Experience Platform et présentation du réseau Edge](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutoriels sur la collecte de données](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger - Aperçu](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
