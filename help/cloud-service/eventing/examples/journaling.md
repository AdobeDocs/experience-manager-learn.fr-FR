---
title: Journalisation et événements AEM
description: Découvrez comment récupérer le jeu initial d’événements AEM dans le journal et explorer les détails de chaque événement.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 280
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
exl-id: 33eb0757-f0ed-4c2d-b8b9-fa6648e87640
source-git-commit: e01eb7ff050321a70d84f8a613705799017dbf5d
workflow-type: ht
source-wordcount: '579'
ht-degree: 100%

---

# Journalisation et événements AEM

Découvrez comment récupérer le jeu initial d’événements AEM dans le journal et explorer les détails de chaque événement.

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

La journalisation est une méthode d’extraction permettant d’utiliser des événements AEM, et un journal est une liste d’événements ordonnés. À l’aide de l’API de journalisation des événements Adobe I/O, vous pouvez récupérer les événements AEM du journal et les traiter dans votre application. Cette approche vous permet de gérer les événements selon une cadence spécifiée et de les traiter efficacement en bloc. Voir la section [Journalisation](https://developer.adobe.com/events/docs/guides/journaling_intro/?lang=fr) pour obtenir des informations détaillées, notamment sur des points essentiels tels que les périodes de conservation, la pagination, etc.

Dans le projet Adobe Developer Console, chaque enregistrement d’événement est automatiquement activé pour la journalisation, ce qui permet une intégration transparente.

>[!IMPORTANT]
>
>Les points d’entrée de la démonstration en direct dans ce tutoriel étaient auparavant hébergés sur [Glitch](https://glitch.com/). Depuis juillet 2025, Glitch a mis fin à son service d’hébergement et les points d’entrée ne sont plus accessibles.
>>Nous travaillons activement à la migration des démonstrations vers une autre plateforme. Le contenu du tutoriel reste exact et des liens mis à jour seront bientôt fournis.
>>Merci pour votre compréhension et pour votre patience.

Utilisez votre propre application jusqu’à ce que les points d’entrée de démonstration en direct soient à nouveau disponibles.

## Prérequis

Les éléments suivants sont requis afin de terminer ce tutoriel :

- Environnement AEM as a Cloud Service avec [AEM Eventing activé](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Projet Adobe Developer Console configuré pour des événements AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Accéder à une application web

Pour accéder à l’application web fournie par Adobe, procédez comme suit :

- Vérifiez que vous pouvez accéder à l’[application web hébergée par Glitch](https://indigo-speckle-antler.glitch.me/) dans un nouvel onglet du navigateur.

  ![Application web hébergée sur Glitch](../assets/examples/journaling/glitch-hosted-web-application.png)

## Collecter des détails du projet Adobe Developer Console

Pour récupérer les événements AEM à partir du journal, des informations d’identification telles que l’_identifiant de l’organisation IMS_, l’_identifiant du client ou de la cliente_ et le _Jeton d’accès_ sont obligatoires. Pour collecter ces informations d’identification, procédez comme suit :

- Dans [Adobe Developer Console](https://developer.adobe.com), accédez à votre projet et cliquez pour l’ouvrir.

- Sous la section **Informations d’identification**, cliquez sur le lien **OAuth serveur à serveur** pour ouvrir l’onglet **Détails des informations d’identification**.

- Cliquez sur le bouton **Générer un jeton d’accès** pour générer le jeton d’accès.

  ![Jeton d’accès généré par le projet Adobe Developer Console](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Copiez le **jeton d’accès généré**, l’**identifiant du client ou de la cliente** et l’**identifiant de l’organisation**. Vous en aurez besoin plus loin dans ce tutoriel.

  ![Informations d’identification copiées du projet Adobe Developer Console](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Chaque enregistrement d’événement est automatiquement activé pour la journalisation. Pour obtenir le _point d’entrée de l’API de journalisation unique_ de l’enregistrement de votre événement, cliquez sur la carte d’événement qui est abonnée aux événements AEM. Dans l’onglet **Détails de l’enregistrement**, copiez le **point d’entrée de l’API unique de journalisation**.

  ![Carte des événements du projet Adobe Developer Console](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## Charger le journal des événements AEM

Pour simplifier les choses, cette application web hébergée récupère uniquement le premier lot d’événements AEM à partir du journal. Il s’agit des événements disponibles les plus anciens du journal. Pour plus d’informations, consultez le [premier lot d’événements](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- Dans l’[application web hébergée sur Glitch](https://indigo-speckle-antler.glitch.me/), saisissez l’**identifiant de l’organisation IMS**, l’**identifiant du client ou de la cliente** et le **jeton d’accès** que vous avez préalablement copiés à partir du projet Adobe Developer Console, puis cliquez sur **Envoyer**.

- En cas de succès, le composant de tableau affiche les données du journal des événements AEM.

  ![Données du journal des événements AEM](../assets/examples/journaling/load-journal.png)

- Pour afficher la payload d’événement complète, double-cliquez sur la ligne. Vous constatez que les détails de l’événement AEM disposent de toutes les informations nécessaires pour traiter l’événement dans le webhook. Par exemple, le type d’événement (`type`), la source de l’événement (`source`), l’identifiant de l’événement (`event_id`), l’heure de l’événement (`time`) et les données de l’événement (`data`).

  ![Achèvement de la payload d’événement AEM](../assets/examples/journaling/complete-journal-data.png)

## Ressources supplémentaires

- L’[API de journalisation d’Adobe I/O Events](https://developer.adobe.com/events/docs/guides/api/journaling_api/) fournit des informations détaillées sur l’API, telles que le premier lot d’événements, le suivant et le dernier, la pagination, etc.
