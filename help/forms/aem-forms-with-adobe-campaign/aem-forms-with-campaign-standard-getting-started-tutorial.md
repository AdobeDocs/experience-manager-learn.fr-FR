---
title: Intégrer AEM Forms et Adobe Campaign Standard
description: Intégrez AEM Forms à Adobe Campaign Standard à l’aide du modèle de données de formulaire AEM Forms pour récupérer les informations de profil de campagne ACS, etc.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 56
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '238'
ht-degree: 100%

---

# Intégrer AEM Forms et Adobe Campaign Standard

![formsandcampaign](assets/helpx-cards-forms.png)

Découvrez comment intégrer AEM Forms à Adobe Campaign Standard (ACS).

ACS dispose d’un large éventail d’API, ce qui permet à ACS de disposer d’une interface avec la technologie de votre choix. Dans ce tutoriel, nous allons nous concentrer sur l’interface d’AEM Forms avec ACS.

Pour intégrer AEM Forms à ACS, procédez comme suit :

* [Configurez l’accès aux API sur votre instance ACS.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=fr)
* Créez un jeton web JSON.
* Échangez le jeton web JSON avec le service Adobe Identity Management pour obtenir un jeton d’accès.
* Incluez ce jeton d’accès dans l’en-tête HTTP d’autorisation, ainsi que la X-API-Key dans chaque requête à une instance ACS.

Pour commencer, suivez les instructions suivantes :

* [Téléchargez et décompressez les ressources liées à ce tutoriel.](assets/aem-forms-and-acs-bundles.zip)
* Déployez les lots à l’aide de la [console web Felix](http://localhost:4502/system/console/bundles).
* Indiquez les paramètres appropriés pour Adobe Campaign dans la configuration OSGI Felix.
* [Créez un profil utilisateur de service comme mentionné dans cet article](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Veillez à déployer le lot OSGi associé à l’article.
* Stockez la clé privée ACS dans etc/key/campaign/private.key. Vous devrez créer un dossier appelé campaign sous etc/key.
* [Fournissez un accès en lecture au dossier campaign aux « données » de l’utilisateur ou de l’utilisatrice du service.](http://localhost:4502/useradmin)

## Étapes suivantes

[Générer le JWT et le jeton d’accès](partone.md)
