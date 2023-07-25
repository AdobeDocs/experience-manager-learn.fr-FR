---
title: Workflows à démarrage automatique
description: Les workflows de démarrage automatique étendent le traitement des ressources en appelant automatiquement le workflow personnalisé lors du chargement ou du retraitement.
feature: Asset Compute Microservices, Workflow
version: Cloud Service
kt: 4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
source-git-commit: 861b171b8ebbcf9565bdc94fb84a043ecb99c00a
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 2%

---

# Workflows à démarrage automatique

Les workflows de démarrage automatique étendent le traitement des ressources dans AEM as a Cloud Service en appelant automatiquement le workflow personnalisé lors du téléchargement ou du retraitement une fois le traitement des ressources terminé.

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)
> `Notice`: Utilisez des workflows de démarrage automatique pour personnaliser le post-traitement des ressources plutôt que d’utiliser des lanceurs de workflow. Les workflows de démarrage automatique sont _only_ appelé une fois qu’une ressource est en cours de traitement au lieu de lanceurs, qui peuvent être appelés plusieurs fois pendant le traitement de la ressource.

## Personnalisation du workflow de post-traitement

Pour personnaliser le workflow de post-traitement, copiez le post-traitement cloud de ressources par défaut. [modèle de workflow](../../foundation/workflow/use-the-workflow-editor.md).

1. Démarrez à l’écran Modèles de processus en accédant à _Outils_ > _Workflow_ > _Modèles_
2. Recherchez et sélectionnez la variable _Post-traitement d’Assets Cloud_ modèle de workflow<br/>
   ![Sélection du modèle de workflow de post-traitement d’Assets Cloud](assets/auto-start-workflow-select-workflow.png)
3. Sélectionnez la _Copier_ pour créer votre workflow personnalisé
4. Sélectionnez votre modèle de workflow actuel (qui sera appelé _Post-traitement d’Assets Cloud1_), puis cliquez sur le bouton _Modifier_ pour éditer le workflow
5. Dans les propriétés du workflow, attribuez un nom significatif à votre workflow de post-traitement personnalisé.<br/>
   ![Modification du nom](assets/auto-start-workflow-change-name.png)
6. Ajoutez les étapes nécessaires pour répondre aux besoins de votre entreprise, en ajoutant dans ce cas une tâche lorsque le traitement des ressources est terminé. Assurez-vous que la dernière étape du workflow est toujours la _Workflow terminé_ step<br/>
   ![Ajouter des étapes de processus](assets/auto-start-workflow-customize-steps.png)
   > `Note`: Les workflows de démarrage automatique s’exécutent à chaque chargement ou retraitement de ressources, de sorte à prendre en compte l’implication de mise à l’échelle des étapes de workflow, en particulier pour les opérations en masse telles que [Imports en bloc](../../cloud-service/migration/bulk-import.md) ou des migrations.
7. Sélectionnez la _Synchronisation_ pour enregistrer vos modifications et synchroniser le modèle de processus

## Utilisation d’un workflow de post-traitement personnalisé

Les post-traitements personnalisés sont configurés sur des dossiers. Pour configurer un workflow de post-traitement personnalisé sur un dossier :

1. Sélectionnez le dossier pour lequel vous souhaitez configurer le workflow et modifiez les propriétés du dossier.
2. Basculez vers le _Traitement des ressources_ tab
3. Sélectionnez votre workflow de post-traitement personnalisé dans le _Processus de démarrage automatique_ zone de sélection<br/>
   ![Définition du workflow de post-traitement](assets/auto-start-workflow-set-workflow.png)
4. Enregistrez vos modifications

Désormais, votre workflow de post-traitement personnalisé sera exécuté pour toutes les ressources chargées ou retraitées dans ce dossier.