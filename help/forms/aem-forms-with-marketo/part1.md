---
title: aem forms avec Marketo (partie 1)
seo-title: aem forms avec Marketo (partie 1)
description: Didacticiel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
seo-description: Didacticiel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 1%

---


# aem forms avec Marketo

Marketo, qui fait partie de l’Adobe, fournit un logiciel d’automatisation du marketing axé sur le marketing basé sur les comptes, notamment les e-mails, les dispositifs portables, les réseaux sociaux, les annonces numériques, la gestion Web et les analyses.

Grâce au modèle de données de formulaire d’AEM Forms, nous pouvons désormais intégrer AEM Form à Marketo de manière transparente.

[En savoir plus sur le modèle de données de formulaire](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo expose une API REST qui permet l&#39;exécution à distance de nombreuses fonctionnalités du système. De la création de programmes à l&#39;importation en vrac de plomb, il existe de nombreuses options qui permettent un contrôle affiné d&#39;une instance de marché. En utilisant le modèle de données de formulaire, il est assez simple d’intégrer AEM Forms à Marketo.

Ce didacticiel décrit les étapes à suivre pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire. Une fois le didacticiel terminé, vous disposez d’un lot OSGi qui effectue l’authentification personnalisée par rapport à Marketo. Vous aurez également configuré la source de données à l’aide du fichier swagger fourni.

Pour commencer, il est vivement recommandé de se familiariser avec les rubriques suivantes répertoriées dans la section Condition préalable.

## Condition requise

1. [Serveur AEM avec Ajoute AEM Forms installée sur le package](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Environnement de développement des AEM locales
1. Familier avec le modèle de données de formulaire
1. Connaissance de base des fichiers Swagger
1. Création de Forms adaptatif

**ID de secret client et clé secrète client**

La première étape de l&#39;intégration de Marketo avec AEM Forms consiste à obtenir les informations d&#39;identification d&#39;API nécessaires pour effectuer les appels REST à l&#39;aide de l&#39;API. Vous aurez besoin des éléments suivants :

1. client_id
1. client_secret
1. identity_endpoint
1. URL d’authentification.

[Veuillez suivre la documentation officielle de Marketo pour obtenir les propriétés mentionnées ci-dessus.](https://developers.marketo.com/rest-api/) Vous pouvez également contacter l’administrateur de votre instance de marché.

**Avant de commencer**

[Téléchargez et décompressez les ressources liées à cet article.](assets/aemformsandmarketo.zip) Le fichier zip contient les éléments suivants :

1. BlankTemplatePackage.zip : modèle de formulaire adaptatif. Importez ceci à l’aide du gestionnaire de packages.
1. marketo.json - Il s’agit du fichier swagger qui sera utilisé pour configurer la source de données.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Il s’agit du lot qui effectue l’authentification personnalisée. N’hésitez pas à l’utiliser si vous ne pouvez pas terminer le didacticiel ou si votre offre groupée ne fonctionne pas comme prévu.
