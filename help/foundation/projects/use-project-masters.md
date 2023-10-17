---
title: Utilisation des projets principaux dans AEM
description: Les projets principaux simplifient considérablement la gestion des projets AEM par les utilisateurs, les utilisatrices et les équipes.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '368'
ht-degree: 100%

---

# Utiliser des Project Masters

Les projets principaux simplifient considérablement la gestion de [!DNL AEM Projects] par les utilisateurs, les utilisatrices et les équipes.

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

Les administrateurs et les administrarices peuvent désormais créer un **[!DNL Master Project]** et affecter des utilisateurs et utilisatrices à des rôles/autorisations dans le cadre d’une équipe de projet. Les projets peuvent être créés à partir d’un projet principal et héritent automatiquement de l’appartenance à l’équipe. Ceci offre plusieurs avantages :

* Réutiliser les équipes existantes sur plusieurs projets ;
* Accélérer la création du projet, car les équipes n’ont pas à être recréées à la main ;
* Gérer l’appartenance à l’équipe depuis un emplacement central et les mises à jour apportées aux équipes sont automatiquement héritées par les projets ;
* Éviter la création de listes de contrôle d’accès en double, ce qui peut entraîner des problèmes de performances.

Les [!DNL Master Projects] peuvent être créés sous le dossier [!UICONTROL Principaux] sous [!UICONTROL Projets AEM]. Une fois qu’un projet principal est créé, il s’affiche en tant qu’option à côté des modèles disponibles dans l’assistant lors de la création de projets.

URL [!DNL Project Masters] (instance de création AEM locale) : [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Supprimer [!DNL Project Masters]

La suppression d’un projet principal rend les projets dérivés inutilisables.

Avant de supprimer un projet principal, assurez-vous que tous les projets dérivés sont terminés et supprimés d’AEM. Veillez à enregistrer les données de projet requises avant de supprimer les projets dérivés. Une fois que tous les projets dérivés sont supprimés d’AEM, le projet principal peut être supprimé en toute sécurité.

## Marquer les [!DNL Project Masters] comme inactifs

Si vous définissez le statut d’un projet principal sur inactif dans les propriétés du projet, les projets principaux inactifs disparaissent de la liste des projets principaux.

Pour afficher les projets principaux inactifs, appuyez sur le bouton de filtre « Afficher les projets actifs » dans la barre supérieure (en regard du bouton d’affichage de liste). Pour que le projet inactif soit à nouveau actif, sélectionnez simplement le projet principal inactif, modifiez les propriétés du projet et redéfinissez-le sur actif.

## Comprendre les [!DNL Project Masters]

![Vue technique des projets principaux](assets/use-project-masters/project-masters-architecture.png)

Les [!DNL Project Masters] fonctionnent en définissant un ensemble de groupes d’utilisateurs et d’utilisatrices AEM (propriétaires, éditeurs et éditrices et observateurs et observatrices) et en autorisant les projets dérivés à référencer et à réutiliser ces groupes d’utilisateurs et d’utilisatrices définis de manière centralisée.

Cela réduit le nombre total de groupes d’utilisateurs et d’utilisatrices requis dans AEM. Avant les [!DNL Project Masters], chaque projet créait 3 groupes d’utilisateurs avec les listes de contrôle d’accès correspondantes pour appliquer l’autorisation, de sorte que 100 projets généraient 300 groupes d’utilisateurs. Les projets principaux permettent à n’importe quel nombre de projets de réutiliser les mêmes trois groupes, en supposant que l’appartenance partagée s’aligne sur les besoins professionnels dans l’ensemble du projet.
