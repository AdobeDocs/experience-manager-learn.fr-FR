---
title: Déclencher un workflow AEM lors de l’introduction de l’envoi de formulaire HTML5
description: Déclencher un workflow AEM lors de l’envoi du formulaire mobile
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ce51b25f-6069-4799-9a61-98c0a672e821
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '195'
ht-degree: 100%

---

# Déclencher un workflow AEM lors de l’envoi du formulaire mobile

Un cas d’utilisation courant consiste à avoir la possibilité d’effectuer le rendu du fichier XDP en tant que HTML pour les activités de capture de données. Lors de l’envoi de ce formulaire, il peut être nécessaire de déclencher un workflow AEM. Dans le workflow AEM, vous pouvez fusionner les données avec le modèle XDP et présenter le PDF généré pour révision et approbation. Le formulaire est rendu sur une instance de publication et le workflow est déclenché sur une instance de traitement AEM.

Les étapes suivantes sont impliquées dans le cas d’utilisation.

* La personne remplit et envoie un formulaire HTML5 (rendu HTML5 de XDP).
* L’envoi est géré par un servlet dans l’instance de publication.
* Le servlet stocke les données dans un dossier prédéfini dans le référentiel de l’instance de traitement AEM.
* Le lanceur de workflow est configuré pour déclencher un workflow AEM chaque fois qu’un nouveau fichier est ajouté sous un dossier particulier.

Ce tutoriel décrit les étapes relatives au cas d’utilisation ci-dessus. Des exemples de code et de ressources associés à ce tutoriel sont [disponibles ici.](./deploy-assets.md)


## Étapes suivantes

[Gestion de l’envoi du formulaire](./handle-form-submission.md)
