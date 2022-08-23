---
title: Groupes d’utilisateurs fermés dans AEM Assets
description: Les groupes d’utilisateurs fermés (CUG) sont une fonctionnalité utilisée pour restreindre l’accès au contenu à un groupe d’utilisateurs sélectionnés sur un site publié. Cette vidéo montre comment les groupes d’utilisateurs fermés peuvent être utilisés avec Adobe Experience Manager Assets pour restreindre l’accès à un dossier spécifique de ressources.
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 1%

---

# Groupes d’utilisateurs fermés{#using-closed-user-groups-with-aem-assets}

Les groupes d’utilisateurs fermés (CUG) sont une fonctionnalité utilisée pour restreindre l’accès au contenu à un groupe d’utilisateurs sélectionnés sur un site publié. Cette vidéo montre comment les groupes d’utilisateurs fermés peuvent être utilisés avec Adobe Experience Manager Assets pour restreindre l’accès à un dossier spécifique de ressources. La prise en charge des groupes d’utilisateurs fermés avec AEM Assets a été introduite pour la première fois dans AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Groupe d’utilisateurs fermé (CUG) avec AEM Assets

* Conçu pour restreindre l’accès aux ressources sur une instance AEM Publish.
* Accorde l’accès en lecture à un ensemble d’utilisateurs/de groupes.
* Le CUG ne peut être configuré qu’au niveau du dossier. Les groupes d’utilisateurs fermés ne peuvent pas être définis sur des ressources individuelles.
* Les stratégies de CUG sont automatiquement héritées par tous les sous-dossiers et ressources appliquées.
* Les stratégies de CUG peuvent être remplacées par des sous-dossiers en définissant une nouvelle stratégie de CUG. Cette méthode doit être utilisée avec parcimonie et n’est pas considérée comme une bonne pratique.

## Groupes d’utilisateurs fermés par rapport aux listes de contrôle d’accès {#closed-user-groups-vs-access-control-lists}

Les groupes d’utilisateurs fermés (CUG) et les listes de contrôle d’accès (ACL) sont utilisés pour contrôler l’accès au contenu dans AEM et en fonction des utilisateurs et des groupes de sécurité d’AEM. Toutefois, l’application et la mise en oeuvre de ces fonctionnalités sont très différentes. Le tableau suivant résume les différences entre les deux fonctions.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Utilisation prévue | Configurer et appliquer des autorisations pour le contenu sur le **current** AEM instance. | Configuration des stratégies de CUG pour le contenu sur AEM **author** instance. Application de stratégies de CUG pour le contenu sur AEM **publier** instances. |
| Niveaux d’autorisation | Définit les autorisations accordées/refusées pour les utilisateurs/groupes à tous les niveaux : Lire, modifier, créer, supprimer, lire la liste de contrôle d’accès, modifier la liste de contrôle d’accès, répliquer. | Accorde l’accès en lecture à un ensemble d’utilisateurs/de groupes. Refusent l’accès en lecture à *tous les autres* utilisateurs/groupes. |
| Publication | Les listes ACL sont *not* publié avec du contenu. | Stratégies de CUG *are* publié avec du contenu. |

## Liens pris en charge {#supporting-links}

* [Gestion des ressources et des groupes d’utilisateurs fermés](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Création d’un groupe d’utilisateurs fermé](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentation du groupe d’utilisateurs fermé Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
