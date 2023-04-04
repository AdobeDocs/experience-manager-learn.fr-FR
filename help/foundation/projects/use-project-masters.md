---
title: Utilisation des Principal de projet dans AEM
description: Les Principal de projet simplifient considérablement la gestion des utilisateurs et des équipes avec AEM Projets.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# Utilisation de Principal de projet

Les Principal de projet simplifient considérablement la gestion des utilisateurs et des équipes avec [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

Les administrateurs peuvent désormais créer un **[!DNL Master Project]** et affecter des utilisateurs à des rôles/autorisations dans le cadre d’une équipe de projet. Les projets peuvent être créés à partir d’un projet Principal et héritent automatiquement de l’appartenance à l’équipe. Cela offre plusieurs avantages :

* Réutilisation d’équipes existantes sur plusieurs projets
* accélère la création du projet, car les équipes n’ont pas à être recréées à la main ;
* Gérer l’appartenance à l’équipe depuis un emplacement central et toutes les mises à jour apportées aux équipes sont automatiquement héritées par les projets.
* évite la création de listes de contrôle d’accès en double, ce qui peut entraîner des problèmes de performances.

[!DNL Master Projects] peut être créé sous le [!UICONTROL Principal] Dossier sous [!UICONTROL Projets AEM]. Une fois qu’un projet Principal est créé, il s’affiche en tant qu’option en même temps que les modèles disponibles dans l’assistant lors de la création de projets.

[!DNL Project Masters] URL (instance d’auteur AEM locale) : [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Supprimer [!DNL Project Masters]

La suppression d’un projet maître entraîne des projets dérivés inutilisables.

Avant de supprimer un projet maître, assurez-vous que tous les projets dérivés sont terminés et supprimés d’AEM. Veillez à enregistrer les données de projet requises avant de supprimer les projets dérivés. Une fois que tous les projets dérivés sont supprimés d’AEM, le projet maître peut être supprimé en toute sécurité.

## Marquer [!DNL Project Masters] comme inactif

En définissant l’état du projet maître sur inactif dans les propriétés du projet, les projets maîtres inactifs disparaissent de la liste des projets maîtres.

Pour afficher les projets maîtres inactifs, activez le bouton de filtre &quot;Afficher principal&quot; dans la barre supérieure (en regard du bouton d’affichage de liste). Pour que le projet inactif soit à nouveau principal, sélectionnez simplement le projet maître inactif, modifiez les propriétés du projet et redéfinissez-le sur principal.

## Comprendre [!DNL Project Masters]

![Vue technique des chefs de projet](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] définissez un ensemble de groupes d’utilisateurs AEM (propriétaires, éditeurs et observateurs) et autorisez les projets dérivés à référencer et à réutiliser ces groupes d’utilisateurs définis de manière centralisée.

Cela réduit le nombre total de groupes d’utilisateurs requis dans AEM. Avant [!DNL Project Masters], chaque projet a créé 3 groupes d’utilisateurs avec les listes de contrôle d’accès associées pour appliquer l’autorisation, de sorte que 100 projets ont généré 300 groupes d’utilisateurs. Les Principal de projet permettent à n’importe quel nombre de projets de réutiliser les mêmes trois groupes, en supposant que l’appartenance partagée s’aligne sur les besoins professionnels dans l’ensemble du projet.
