---
title: Déclencher le processus AEM sur l’introduction de l’envoi de formulaire HTML5
description: Déclenchement d’un processus AEM lors de l’envoi d’un formulaire mobile
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 14%

---

# Déclencher AEM processus sur une soumission de formulaire mobile

Un cas d’utilisation courant consiste à avoir la possibilité de rendre le XDP comme HTML pour les activités de capture de données. Lors de l’envoi de ce formulaire, il peut être nécessaire de déclencher un processus d’AEM. Dans le workflow AEM, nous pouvons fusionner les données avec le modèle xdp et présenter le PDF généré pour révision et approbation. Le formulaire sera rendu sur une instance de publication et le workflow sera déclenché sur une instance de traitement AEM.
Les étapes suivantes sont impliquées dans le cas pratique

* L’utilisateur remplit et envoie un formulaire HTML5 (rendu HTML5 de XDP).
* L’envoi est géré par une servlet dans l’instance de publication.
* Le servlet stocke les données dans un dossier prédéfini dans le référentiel de l’instance de traitement AEM.
* Le lanceur de workflow est configuré pour déclencher un workflow AEM chaque fois qu’un nouveau fichier est ajouté sous un dossier particulier.

Ce tutoriel décrit les étapes relatives au cas d’utilisation ci-dessus. Des exemples de code et de ressources associés à ce tutoriel sont [disponibles ici.](./deploy-assets.md)


## Étapes suivantes

[Gérer l’envoi de formulaire](./handle-form-submission.md)