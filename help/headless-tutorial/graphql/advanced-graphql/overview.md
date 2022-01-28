---
title: Concepts avancés d’AEM sans affichage - GraphQL
description: Tutoriel complet illustrant des concepts avancés d’API GraphQL Adobe Experience Manager (AEM).
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '926'
ht-degree: 2%

---

# Concepts avancés d’AEM sans affichage

Ce tutoriel complet poursuit la [tutoriel de base](../multi-step/overview.md) qui couvrait les principes de base d’Adobe Experience Manager (AEM) Headless et GraphQL. Le tutoriel avancé illustre des aspects détaillés de l’utilisation des modèles de fragment de contenu, des fragments de contenu et de l’API GraphQL d’AEM, y compris l’utilisation d’AEM GraphQL dans une application cliente.

## Prérequis

Procédez comme suit : [configuration rapide pour AEM as a Cloud Service](../quick-setup/cloud-service.md) pour configurer votre environnement.

Il est vivement recommandé d’effectuer la [tutoriel de base](../multi-step/overview.md) et [série vidéo](../video-series/modeling-basics.md) tutoriels avant de poursuivre ce tutoriel avancé. Bien que vous puissiez suivre le tutoriel à l’aide d’un environnement d’AEM local, ce tutoriel ne couvre que le processus pour AEM as a Cloud Service.

## Objectifs

Ce tutoriel couvre les rubriques suivantes :

* Créez des modèles de fragment de contenu à l’aide de règles de validation et de types de données plus avancés tels que des espaces réservés d’onglet, des références de fragments imbriqués, des objets JSON et des types de données Date et heure.
* Créez des fragments de contenu lorsque vous utilisez du contenu imbriqué et des références à des fragments, et configurez des stratégies de dossier pour la gouvernance de création de fragments de contenu.
* Explorez AEM fonctionnalités de l’API GraphQL à l’aide de requêtes GraphQL avec des variables et des directives.
* Conserver les requêtes GraphQL avec des paramètres dans AEM et apprendre à utiliser les paramètres de contrôle du cache avec des requêtes persistantes.
* Intégrez les requêtes de requêtes persistantes dans l’exemple d’application WKND GraphQL React à l’aide du SDK JavaScript sans affichage AEM.

## Présentation des concepts avancés d’AEM sans affichage

La vidéo suivante présente un aperçu général des concepts abordés dans ce tutoriel. Le tutoriel comprend la définition de modèles de fragment de contenu avec des types de données plus avancés, l’imbrication de fragments de contenu et la persistance de requêtes GraphQL dans AEM.

>[!VIDEO](https://video.tv.adobe.com/v/340035/?quality=12&learn=on)

## Configuration du projet

Le projet de site WKND comporte toutes les configurations nécessaires pour que vous puissiez commencer le tutoriel juste après avoir terminé la [configuration rapide](../quick-setup/cloud-service.md). Cette section ne met en évidence que certaines étapes importantes que vous pouvez utiliser lors de la création de votre propre projet AEM sans affichage.

### Création d’une configuration

La première étape pour démarrer un nouveau projet dans AEM consiste à créer sa configuration en tant qu’espace de travail et à créer des points d’entrée API GraphQL. Pour afficher ou créer une configuration, accédez à **Outils** > **Général** > **Explorateur de configuration**.

![Accédez à l’explorateur de configurations](assets/overview/create-configuration.png)

Notez que la configuration du site WKND a déjà été créée pour le tutoriel. Pour créer une configuration pour votre propre projet, sélectionnez **Créer** dans le coin supérieur droit et remplissez le formulaire dans le modal Créer une configuration qui s’affiche.

### Création de points d’entrée d’API GraphQL

Ensuite, vous devez configurer les points d’entrée API pour envoyer des requêtes GraphQL à . Pour passer en revue les points de fin existants ou en créer un, accédez à **Outils** > **Ressources** > **GraphQL**.

![Configuration des points de fin](assets/overview/endpoints.png)

Notez que les points d’entrée globaux et WKND ont déjà été créés. Pour créer un point de fin pour votre projet, sélectionnez **Créer** dans le coin supérieur droit et suivez le processus.

>[!NOTE]
>
> Après avoir enregistré le point de terminaison, un modal s’affiche pour vous rendre dans la console de sécurité, ce qui vous permet d’ajuster les paramètres de sécurité si vous souhaitez configurer l’accès au point de terminaison. Toutefois, les autorisations de sécurité elles-mêmes ne font pas partie de ce tutoriel. Pour plus d’informations, reportez-vous à la section [Documentation AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=fr).

### Création d’un dossier racine de langue pour votre projet

Un dossier racine de langue est un dossier dont le nom contient un code de langue ISO, tel que EN ou FR. Le système de gestion des traductions AEM utilise ces dossiers pour définir la Principale langue de votre contenu et les langues de traduction du contenu.

Accédez à **Navigation** > **Ressources** > **Fichiers**.

![Accès aux fichiers](assets/overview/files.png)

Accédez au **Site WKND** dossier. Observez le dossier avec le titre &quot;Anglais&quot; et le nom &quot;EN&quot;. Ce dossier est le dossier racine de langue du projet WKND Site.

![Dossier anglais](assets/overview/english.png)

Pour votre propre projet, créez un dossier racine de langue dans votre configuration. Voir la section sur [création de dossiers](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) pour plus d’informations.

### Attribuer une configuration au dossier imbriqué

Enfin, vous devez affecter la configuration de votre projet au dossier racine de langue. Cette affectation permet la création de fragments de contenu en fonction de modèles de fragments de contenu définis dans la configuration de votre projet.

Pour attribuer le dossier racine de langue à la configuration, sélectionnez le dossier, puis sélectionnez **Propriétés** dans la barre de navigation supérieure.

![Sélectionnez Propriétés](assets/overview/properties.png)

Ensuite, accédez au **Cloud Services** et sélectionnez l’icône de dossier dans la **Configuration du cloud** champ .

![Configuration du cloud](assets/overview/cloud-conf.png)

Dans le modal qui s’affiche, sélectionnez la configuration précédemment créée pour lui affecter le dossier racine de langue.

### Bonnes pratiques

Voici les bonnes pratiques à appliquer lors de la création de votre propre projet dans AEM :

* La hiérarchie de dossiers doit être modélisée en tenant compte de la localisation et de la traduction. En d’autres termes, les dossiers de langue doivent être imbriqués dans des dossiers de configuration, ce qui permet une traduction facile du contenu dans ces dossiers de configuration.
* La hiérarchie des dossiers doit être conservée à plat et simple. Évitez de déplacer ou de renommer des dossiers et des fragments ultérieurement, en particulier après publication pour une utilisation en direct, car cela modifie les chemins d’accès pouvant affecter les références aux fragments et les requêtes GraphQL.

## Packages de démarrage et de solution

Deux AEM **packages** sont disponibles et peuvent être installées via [Gestionnaire de modules](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip) est utilisé ultérieurement dans le tutoriel et contient des exemples d’images et de dossiers.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip) contient la solution terminée pour les chapitres 1 à 4, y compris les nouveaux modèles de fragment de contenu, les fragments de contenu et les requêtes GraphQL persistantes. Utile pour ceux qui souhaitent passer directement dans le [Intégration d’applications client](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) chapitre.

Deux projets React JS sont disponibles pour tester des requêtes. [à partir d’une application cliente sans interface](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

* [aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip) - application cliente de démarrage terminée dans [Chapitre 5 - Intégration de l’application client](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).
* [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) : application client terminée qui utilise **persistant** requêtes.


## Prise en main

Pour commencer à utiliser ce tutoriel avancé, procédez comme suit :

1. Configuration d’un environnement de développement à l’aide de [AEM as a Cloud Service](../quick-setup/cloud-service.md).
1. Démarrez le chapitre du tutoriel sur [création de modèles de fragment de contenu](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
