---
title: Déploiement d’une extension de console de fragments de contenu AEM
description: Découvrez comment déployer une extension de console de fragments de contenu AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '804'
ht-degree: 0%

---


# Déploiement d’une extension

Pour une utilisation dans AEM environnements as a Cloud Service, l’application App Builder d’extension doit être déployée et approuvée.

Plusieurs points doivent être pris en compte lors du déploiement des applications App Builder d’extension :

+ Les extensions sont déployées dans l’espace de travail du projet de la console Adobe Developer. Les espaces de travail par défaut sont les suivants :
   + __Production__ workspace contient des déploiements d’extension disponibles dans tous les AEM as a Cloud Service.
   + __Évaluation__ workspace fait office d’espace de travail développeur. Les extensions déployées dans l’espace de travail Stage ne sont pas disponibles dans AEM as a Cloud Service.
Les espaces de travail de la console Adobe Developer ne comportent aucune corrélation directe avec les types d’environnements as a Cloud Service AEM.
+ Une extension déployée dans l’espace de travail de production s’affiche dans tous les environnements as a Cloud Service AEM dans l’organisation Adobe dans laquelle l’extension existe.
Une extension ne peut pas être limitée aux environnements dans lesquels elle est enregistrée en ajoutant [logique conditionnelle qui vérifie le nom d’hôte as a Cloud Service AEM](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ Plusieurs extensions peuvent être utilisées sur AEM as a Cloud Service. Adobe recommande que chaque application App Builder d’extension soit utilisée pour résoudre un seul objectif commercial. Cela dit, une seule application App Builder d’extension peut mettre en oeuvre plusieurs points d’extension qui prennent en charge un objectif commercial commun.

## Déploiement initial

Pour qu’une extension soit disponible dans AEM environnements as a Cloud Service, elle doit être déployée sur la console Adobe Developer.

Le processus de déploiement se divise en deux étapes logiques :

1. Déploiement de l’application App Builder d’extension vers la console Adobe Developer par un développeur.
1. Validation de l’extension par un responsable de déploiement ou un propriétaire d’entreprise.

### Déploiement de l’extension de l’application App Builder

Déployez l’extension dans l’espace de travail Production. Les extensions déployées dans l’espace de travail de production sont automatiquement ajoutées à tous les services d’auteur as a Cloud Service AEM dans l’organisation Adobe vers laquelle l’extension est déployée.

1. Ouvrez une ligne de commande à la racine de l’extension mise à jour de l’application App Builder.
1. Assurez-vous que l’espace de travail Production est principal

   ```shell
   $ aio app use -w Production
   ```

   Fusionner toutes les modifications dans `.env` et `.aio`.

1. Déployez l’extension mise à jour de l’application App Builder.

   ```shell
   $ aio app deploy
   ```

#### Demande d’approbation de déploiement

![Soumettre l’extension à validation](./assets/deploy/submit-for-approval.png){align="center"}

1. Connectez-vous à [Console Adobe Developer](https://developer.adobe.com)
1. Sélectionner __Console__
1. Accédez à __Projets__
1. Sélectionner le projet associé à l’extension
1. Sélectionnez la __Production__ workspace
1. Sélectionner __Soumettre à validation__
1. Remplissez et envoyez le formulaire, en mettant à jour les champs selon vos besoins.

Notez qu’une icône est requise. Si vous ne disposez pas d’une icône, vous pouvez utiliser [cette icône](./assets/deploy/icon.png).

### Approuver la requête de déploiement

![Validation des extensions](./assets/deploy/adobe-exchange.png){align="center"}

1. Connectez-vous à [Adobe Exchange](https://exchange.adobe.com/)
1. Accédez à __Gérer__ > __Applications en attente de révision__
1. __Réviser__ l’application App Builder d’extension
1. Si les modifications d’extension sont acceptables __Accepter__ la révision. Cette opération injecte immédiatement l’extension sur tous les services de création as a Cloud Service AEM dans l’organisation Adobe.

Une fois la demande d’extension approuvée, l’extension devient immédiatement principale dans les services d’auteur as a Cloud Service AEM.

## Mettre à jour une extension

La mise à jour et l’extension de l’application App Builder suivent le même processus que le [déploiement initial](#initial-deployment), avec l’écart selon lequel le déploiement d’extension existant doit d’abord être révoqué.

### Révoquer l’extension

Pour déployer une nouvelle version d’une extension, celle-ci doit d’abord être révoquée (ou supprimée). Bien que l’extension soit Révoquée, elle n’est pas disponible dans AEM consoles.

1. Connectez-vous à [Adobe Exchange](https://exchange.adobe.com/)
1. Accédez à __Gérer__ > __Applications de créateur d’applications__
1. __Révoquer__ l’extension à mettre à jour

### Déployer l’extension

Déployez l’extension dans l’espace de travail Production. Les extensions déployées dans l’espace de travail de production sont automatiquement ajoutées à tous les services d’auteur as a Cloud Service AEM dans l’organisation Adobe vers laquelle l’extension est déployée.

1. Ouvrez une ligne de commande à la racine de l’extension mise à jour de l’application App Builder.
1. Assurez-vous que l’espace de travail Production est principal

   ```shell
   $ aio app use -w Production
   ```

   Fusionner toutes les modifications dans `.env` et `.aio`.

1. Déployez l’extension mise à jour de l’application App Builder.

   ```shell
   $ aio app deploy
   ```

#### Demande d’approbation de déploiement

![Soumettre l’extension à validation](./assets/deploy/submit-for-approval.png){align="center"}

1. Connectez-vous à [Console Adobe Developer](https://developer.adobe.com)
1. Sélectionner __Console__
1. Accédez à __Projets__
1. Sélectionner le projet associé à l’extension
1. Sélectionnez la __Production__ workspace
1. Sélectionner __Soumettre à validation__
1. Remplissez et envoyez le formulaire, en mettant à jour les champs selon vos besoins.

#### Approuver la requête de déploiement

![Validation des extensions](./assets/deploy/adobe-exchange.png){align="center"}

1. Connectez-vous à [Adobe Exchange](https://exchange.adobe.com/)
1. Accédez à __Gérer__ > __Applications en attente de révision__
1. __Réviser__ l’application App Builder d’extension
1. Si les modifications d’extension sont acceptables __Accepter__ la révision. Cette opération injecte immédiatement l’extension sur tous les services de création as a Cloud Service AEM dans l’organisation Adobe.

Une fois la demande d’extension approuvée, l’extension devient immédiatement principale dans les services d’auteur as a Cloud Service AEM.

## Suppression d’une extension

![Suppression d’une extension](./assets/deploy/revoke.png)

Pour supprimer une extension, révoquez-la (ou supprimez-la) d’Adobe Exchange. Lorsque l’extension est révoquée, elle est supprimée de tous les services de création as a Cloud Service AEM.

1. Connectez-vous à [Adobe Exchange](https://exchange.adobe.com/)
1. Accédez à __Gérer__ > __Applications de créateur d’applications__
1. __Révoquer__ l’extension à supprimer
