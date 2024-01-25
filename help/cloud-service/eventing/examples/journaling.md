---
title: Journalisation et événements AEM
description: Découvrez comment récupérer le jeu initial d’événements AEM dans le journal et explorez les détails de chaque événement.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 144
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# Journalisation et événements AEM

Découvrez comment récupérer le jeu initial d’événements AEM dans le journal et explorez les détails de chaque événement.

La journalisation est une méthode d’extraction permettant d’utiliser AEM événements, et un journal est une liste d’événements ordonnés. À l’aide de l’API de journalisation des événements d’Adobe I/O, vous pouvez récupérer les événements d’AEM du journal et les traiter dans votre application. Cette approche vous permet de gérer les événements selon une cadence spécifiée et de les traiter efficacement en bloc. Voir [Journalisation](https://developer.adobe.com/events/docs/guides/journaling_intro/) pour obtenir des informations détaillées, notamment des considérations essentielles telles que les périodes de rétention, la pagination, etc.

Dans le projet de console Adobe Developer, chaque enregistrement d’événement est automatiquement activé pour la journalisation, ce qui permet une intégration transparente.

Dans cet exemple, en utilisant un Adobe fourni _application web hébergée_ vous permet de récupérer le premier lot d’événements AEM du journal sans avoir à configurer votre application. Cette application web fournie par Adobe est hébergée sur [Glitch](https://glitch.com/), une plateforme connue pour proposer un environnement web propice à la création et au déploiement d’applications web. Cependant, l’option permettant d’utiliser votre propre application est également disponible si vous préférez.

## Conditions préalables

Pour suivre ce tutoriel, vous devez :

- AEM environnement as a Cloud Service avec [AEM Eventing activé](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Projet de console Adobe Developer configuré pour les événements AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Accès à une application web

Pour accéder à l’application web fournie par l’Adobe, procédez comme suit :

- Vérifiez que vous pouvez accéder au [Glitch - application web hébergée](https://indigo-speckle-antler.glitch.me/) dans un nouvel onglet du navigateur.

  ![Glitch - application web hébergée](../assets/examples/journaling/glitch-hosted-web-application.png)

## Collecte des détails du projet de console Adobe Developer

Pour récupérer les événements AEM du journal, des informations d’identification telles que _Identifiant de l’organisation IMS_, _ID client_, et _Jeton d’accès_ sont obligatoires. Pour collecter ces informations d’identification, procédez comme suit :

- Dans le [Console Adobe Developer](https://developer.adobe.com), accédez à votre projet et cliquez pour l’ouvrir.

- Sous , **Informations d’identification** , cliquez sur le bouton **OAuth serveur à serveur** pour ouvrir le lien **Informations d’identification** .

- Cliquez sur le bouton **Générer un jeton d’accès** pour générer le jeton d’accès.

  ![Jeton d’accès généré par le projet de la console Adobe Developer](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Copiez le **Jeton d’accès généré**, **ID CLIENT**, et **ID D’ORGANISATION**. Vous en aurez besoin plus loin dans ce tutoriel.

  ![Informations d’identification de copie de projet de la console Adobe Developer](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Chaque enregistrement d’événement est automatiquement activé pour la journalisation. Pour obtenir le _point d’entrée de l’API de journalisation unique_ Lors de l’enregistrement de votre événement, cliquez sur la carte d’événement qui est abonnée aux événements AEM. Dans la **Détails de l’enregistrement** , copiez la variable **JOURNALISATION D’UN POINT DE TERMINAISON D’API UNIQUE**.

  ![Carte des événements de projet de la console Adobe Developer](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## Chargement du journal des événements AEM

Pour simplifier les choses, cette application web hébergée récupère uniquement le premier lot d’événements AEM du journal. Il s’agit des événements disponibles les plus anciens du journal. Pour plus d’informations, voir [premier lot d’événements](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- Dans le [Glitch - application web hébergée](https://indigo-speckle-antler.glitch.me/), saisissez la variable **Identifiant de l’organisation IMS**, **ID client**, et **Jeton d’accès** Vous avez copié précédemment à partir du projet de console Adobe Developer et cliquez sur **Envoyer**.

- En cas de succès, le composant de tableau affiche les données du journal des événements AEM.

  ![AEM données du journal des événements](../assets/examples/journaling/load-journal.png)

- Pour afficher la charge utile d’événement complète, double-cliquez sur la ligne. Vous pouvez constater que les détails de l’événement AEM disposent de toutes les informations nécessaires pour traiter l’événement dans le webhook. Par exemple, le type d’événement (`type`), source d’événement (`source`), identifiant d’événement (`event_id`), heure de l’événement (`time`) et les données d’événement (`data`).

  ![Compléter AEM charge utile d’événement](../assets/examples/journaling/complete-journal-data.png)

## Ressources supplémentaires

- [Glitter le code source webhook](https://glitch.com/edit/#!/indigo-speckle-antler) est disponible à titre de référence. Il s’agit d’une application React simple qui utilise [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) composants pour effectuer le rendu de l’interface utilisateur.

- [API de journalisation des événements Adobe I/O](https://developer.adobe.com/events/docs/guides/api/journaling_api/) fournit des informations détaillées sur l’API, telles que le premier, le suivant et le dernier lot d’événements, la pagination, etc.
