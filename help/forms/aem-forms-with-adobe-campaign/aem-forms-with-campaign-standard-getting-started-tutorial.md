---
title: Intégration d’AEM Forms et d’Adobe Campaign Standard
description: Intégrez AEM Forms à Adobe Campaign Standard à l’aide du modèle de données de formulaire AEM Forms pour récupérer les informations de profil de campagne ACS, etc.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 3%

---

# Intégration d’AEM Forms et d’Adobe Campaign Standard

![formsandcampaign](assets/helpx-cards-forms.png)

Découvrez comment intégrer AEM Forms à Adobe Campaign Standard (ACS).

ACS dispose d’un large éventail d’API, ce qui permet à ACS d’être interfacée avec la technologie de votre choix. Dans ce tutoriel, nous allons nous concentrer sur l’interface d’AEM Forms avec ACS.

Pour intégrer AEM Forms à ACS, procédez comme suit :

* [Configurez l’accès aux API sur votre instance ACS.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* Créez un jeton Web JSON.
* Échangez le jeton Web JSON avec Adobe Identity Management Service pour obtenir un jeton d’accès.
* Incluez ce jeton d’accès dans l’en-tête HTTP d’autorisation, ainsi que la clé X-API dans chaque requête à une instance ACS.

Pour commencer, suivez les instructions suivantes :

* [Téléchargez et décompressez les ressources liées à ce tutoriel.](assets/aem-forms-and-acs-bundles.zip)
* Déployez les lots à l’aide de [Console web Felix](http://localhost:4502/system/console/bundles)
* Indiquez les paramètres appropriés pour Adobe Campaign dans la configuration Felix OSGI.
* [Créer un utilisateur de service comme mentionné dans cet article](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Veillez à déployer le lot OSGi associé à l’article.
* Stockez la clé privée ACS dans etc/key/campaign/private.key. Vous devrez créer un dossier appelé campaign sous etc/key.
* [Fournissez un accès en lecture au dossier de campagne aux &quot;données&quot; de l’utilisateur du service.](http://localhost:4502/useradmin)

## Étapes suivantes

[Génération de JWT et du jeton d’accès](partone.md)
