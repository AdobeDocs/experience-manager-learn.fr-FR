---
title: Intégrer AEM Forms et Marketo
description: Découvrez comment intégrer AEM Forms et Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 94
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 100%

---

# Intégrer AEM Forms et Marketo

Marketo, qui fait partie d’Adobe, fournit un logiciel d’automatisation du marketing axé sur le compte (Account-Based Marketing), notamment via les e-mails, les dispositifs mobiles, les réseaux sociaux, les publicités numériques, la gestion du web et les analyses.

Grâce au modèle de données de formulaire d’AEM Forms, nous pouvons désormais intégrer facilement AEM Forms avec Marketo.

[En savoir plus sur le modèle de données de formulaire](https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/data-integration.html).

Marketo fournit une API REST qui permet l’exécution à distance de nombreuses fonctionnalités du système. De la création de programmes à l’import de Leads en bloc, de nombreuses options permettent un contrôle précis d’une instance Marketo. L’utilisation du modèle de données de formulaire est assez simple pour intégrer AEM Forms avec Marketo.

Ce tutoriel vous indiquera les étapes nécessaires à l’intégration d’AEM Forms avec Marketo à l’aide du modèle de données de formulaire. Une fois le tutoriel terminé, vous disposerez d’un lot OSGi qui effectuera l’authentification personnalisée par rapport à Marketo. Vous avez également configuré la source de données à l’aide du fichier Swagger fourni.

Pour commencer, il est vivement recommandé de vous familiariser avec les rubriques suivantes répertoriées dans la section Conditions préalables.

## Prérequis

1. [Serveur AEM avec package de modules complémentaires AEM Forms installé](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Environnement de développement local AEM
1. Se familiariser avec le modèle de données de formulaire
1. Connaissances de base des fichiers Swagger
1. Créer des formulaires adaptatifs

**ID secret client et clé secrète client**

La première étape de l’intégration de Marketo avec AEM Forms consiste à obtenir les informations d’identification d’API nécessaires pour effectuer les appels REST à l’aide de l’API. Vous aurez besoin des éléments suivants :

1. client_id
1. client_secret
1. identity_endpoint
1. URL d’authentification

[Veuillez consulter la documentation officielle de Marketo pour obtenir les propriétés mentionnées ci-dessus.](https://developers.marketo.com/rest-api/) Vous pouvez également contacter l’administrateur ou l’administratrice de votre instance Marketo.

**Avant de commencer**

[Téléchargez et décompressez les ressources décrites dans cet article.](assets/aemformsandmarketo.zip) Le fichier .zip contient les fichiers suivants :

1. BlankTemplatePackage.zip : il s’agit du modèle de formulaire adaptatif. Importez-le à l’aide du gestionnaire de packages.
1. marketo.json : fichier Swagger utilisé pour configurer la source de données.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar : il s’agit du lot qui effectue l’authentification personnalisée. N’hésitez pas à l’utiliser si vous ne parvenez pas à terminer le tutoriel ou si votre lot ne fonctionne pas comme prévu.

## Étapes suivantes

[Créer une authentification personnalisée](./part2.md)
