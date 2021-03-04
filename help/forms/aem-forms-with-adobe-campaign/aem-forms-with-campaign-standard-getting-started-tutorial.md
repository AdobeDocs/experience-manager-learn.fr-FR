---
title: Prise en main de AEM Forms et Adobe Campaign Standard
seo-title: Prise en main de AEM Forms et Adobe Campaign Standard
description: Intégrez AEM Forms à Adobe Campaign Standard à l'aide du modèle de données AEM Forms Form pour récupérer les informations du profil de campagne ACS, etc.
seo-description: Intégrez AEM Forms à Adobe Campaign Standard à l'aide du modèle de données AEM Forms Form pour récupérer les informations du profil de campagne ACS, etc.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: '"Forms adaptatif, modèle de données de formulaire"'
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 3%

---


# Prise en main de AEM Forms et Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Ce tutoriel liste les différents cas d&#39;utilisation pour l&#39;intégration de AEM Forms à Adobe Campaign Standard(ACS).

ACS dispose d&#39;un riche ensemble d&#39;API qui permet à ACS d&#39;être relié à la technologie de notre choix. Dans ce tutoriel, nous allons nous concentrer sur l&#39;interface AEM Forms avec ACS.

Pour intégrer AEM Forms à ACS, vous devez suivre les étapes suivantes :

* [Configurez l&#39;accès à l&#39;API sur votre instance ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* Créez un jeton Web JSON.
* Exchange the JSON Web Token with Adobe Identity Management Service for an Jeton d&#39;accès.
* Incluez ce Jeton d&#39;accès dans l’en-tête HTTP d’autorisation, ainsi que la clé X-API dans chaque requête à une instance ACS.

Pour commencer, suivez les instructions suivantes :

* [Téléchargez et décompressez les ressources liées à ce didacticiel.](assets/aem-forms-and-acs-bundles.zip)
* Déployez les lots à l’aide de [la console Web Felix](http://localhost:4502/system/console/bundles)
* Fournissez les paramètres appropriés pour Adobe Campaign dans la configuration Felix OSGI.
* [Créez un utilisateur de service comme mentionné dans cet article](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Assurez-vous de déployer le lot OSGi associé à l’article.
* Stockez la clé privée ACS dans etc/key/campaign/private.key. Vous devez créer un dossier appelé campaign sous etc/key.
* [Fournissez un accès en lecture au dossier de campagne aux &quot;données&quot; de l’utilisateur du service.](http://localhost:4502/useradmin)
