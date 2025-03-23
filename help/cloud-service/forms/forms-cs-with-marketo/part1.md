---
title: Intégration d’AEM Forms as a Cloud Service et de Marketo
description: Découvrez comment intégrer AEM Forms et Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Form Data Model,Integration
version: Experience Manager as a Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: c3145149-bfa4-4dcb-acde-c359e9348f99
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '342'
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

1. Accès à l’instance AEM Forms as a Cloud Service
1. Se familiariser avec le modèle de données de formulaire
1. Connaissances de base des fichiers Swagger
1. Créer des formulaires adaptatifs

**ID secret client et clé secrète client**

La première étape de l’intégration de Marketo avec AEM Forms consiste à obtenir les informations d’identification d’API nécessaires pour effectuer les appels REST à l’aide de l’API. Vous aurez besoin des éléments suivants :

1. client_id
1. client_secret
1. identity_endpoint

[Veuillez consulter la documentation officielle de Marketo pour obtenir les propriétés mentionnées ci-dessus.](https://developers.marketo.com/rest-api/) Vous pouvez également contacter l’administrateur ou l’administratrice de votre instance Marketo.

**Avant de commencer**

* [Téléchargement et décompression des ressources liées à ce tutoriel](assets/marketo.zip)

Le fichier .zip contient les fichiers suivants :

1. marketo.json : fichier Swagger utilisé pour configurer la source de données.
1. Veillez à modifier la propriété hôte dans le fichier marketo.json pour qu’il pointe vers votre instance Marketo.

## Étapes suivantes

[Création d’une source de données](./part2.md)
