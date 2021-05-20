---
title: AEM Forms avec Marketo (partie 1)
seo-title: AEM Forms avec Marketo (partie 1)
description: Tutoriel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
seo-description: Tutoriel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Forms adaptatif, modèle de données de formulaire
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 1%

---


# AEM Forms avec Marketo

Marketo, composant d’Adobe, fournit un logiciel d’automatisation du marketing axé sur le compte, notamment les e-mails, les appareils mobiles, les réseaux sociaux, les annonces numériques, la gestion du web et les analyses.

Grâce au modèle de données de formulaire d’AEM Forms, nous pouvons désormais intégrer facilement AEM Form à Marketo.

[En savoir plus sur le modèle de données de formulaire](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo expose une API REST qui permet l’exécution à distance de nombreuses fonctionnalités du système. De la création de programmes à l’importation de pistes en bloc, il existe de nombreuses options qui permettent un contrôle précis d’une instance Marketo. L’utilisation du modèle de données de formulaire est assez simple pour intégrer AEM Forms à Marketo.

Ce tutoriel vous guide tout au long des étapes nécessaires à l’intégration d’AEM Forms à Marketo à l’aide du modèle de données de formulaire. Une fois le tutoriel terminé, vous disposez d’un lot OSGi qui effectue l’authentification personnalisée par rapport à Marketo. Vous avez également configuré la source de données à l’aide du fichier swagger fourni.

Pour commencer, il est vivement recommandé de vous familiariser avec les rubriques suivantes répertoriées dans la section Conditions préalables .

## Condition préalable

1. [Serveur AEM avec le package AEM Forms Add on installé](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Environnement de développement AEM local
1. Se familiariser avec le modèle de données de formulaire
1. Connaissances de base des fichiers Swagger
1. Création d’un Forms adaptatif

**ID de secret client et clé secrète client**

La première étape de l’intégration de Marketo avec AEM Forms consiste à obtenir les informations d’identification d’API nécessaires pour effectuer les appels REST à l’aide de l’API. Vous aurez besoin des éléments suivants :

1. client_id
1. client_secret
1. identity_endpoint
1. URL d’authentification.

[Veuillez consulter la documentation officielle de Marketo pour obtenir les propriétés mentionnées ci-dessus.](https://developers.marketo.com/rest-api/) Vous pouvez également contacter l’administrateur de votre instance Marketo.

**Avant de commencer**

[Téléchargez et décompressez les ressources liées à cet article.](assets/aemformsandmarketo.zip) Le fichier zip contient les éléments suivants :

1. BlankTemplatePackage.zip : il s’agit du modèle de formulaire adaptatif. Importez-le à l’aide du gestionnaire de packages.
1. marketo.json : fichier swagger qui sera utilisé pour configurer la source de données.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Il s’agit du lot qui effectue l’authentification personnalisée. N’hésitez pas à l’utiliser si vous ne parvenez pas à terminer le tutoriel ou si votre lot ne fonctionne pas comme prévu.
