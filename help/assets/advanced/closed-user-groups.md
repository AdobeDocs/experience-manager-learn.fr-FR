---
title: Groupes d’utilisateurs et utilisatrices fermés dans AEM Assets
description: Les groupes d’utilisateurs et utilisatrices fermés (CUG) sont une fonctionnalité utilisée pour restreindre l’accès au contenu à un groupe d’utilisateurs et utilisatrices sélectionnés sur un site publié. Cette vidéo montre comment les groupes d’utilisateurs et utilisatrices fermés peuvent être utilisés avec Adobe Experience Manager Assets pour restreindre l’accès à un dossier spécifique de ressources.
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 100%

---

# Groupes d’utilisateurs et d’utilisatrices fermés{#using-closed-user-groups-with-aem-assets}

Les groupes d’utilisateurs et utilisatrices fermés (CUG) sont une fonctionnalité utilisée pour restreindre l’accès au contenu à un groupe d’utilisateurs et utilisatrices sélectionnés sur un site publié. Cette vidéo montre comment les groupes d’utilisateurs et utilisatrices fermés peuvent être utilisés avec Adobe Experience Manager Assets pour restreindre l’accès à un dossier spécifique de ressources. La prise en charge des groupes d’utilisateurs et utilisatrices fermés avec AEM Assets a été introduite pour la première fois dans AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Groupe d’utilisateurs et utilisatrices fermé avec AEM Assets

* Conçu pour restreindre l’accès aux ressources sur une instance de publication AEM.
* Accorde l’accès en lecture à un ensemble d’utilisateurs et utilisatrices/de groupes.
* Le CUG ne peut être configuré qu’au niveau du dossier. Le CUG ne peut pas être défini sur des ressources individuelles.
* Les politiques de CUG sont automatiquement héritées par tous les sous-dossiers et ressources appliquées.
* Les politiques de CUG peuvent être remplacées par des sous-dossiers en définissant une nouvelle politique de CUG. Cette méthode doit être utilisée avec parcimonie et n’est pas considérée comme une bonne pratique.

## Groupes d’utilisateurs et utilisatrices fermés et listes de contrôle d’accès {#closed-user-groups-vs-access-control-lists}

Les groupes d’utilisateurs et utilisatrices fermés (CUG) et les listes de contrôle d’accès (ACL) sont utilisés pour contrôler l’accès au contenu dans AEM et en fonction des utilisateurs, des utilisatrices et des groupes de sécurité d’AEM. Toutefois, l’application et la mise en œuvre de ces fonctionnalités sont très différentes. Le tableau suivant résume les différences entre les deux fonctions.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Utilisation prévue | Configurer et appliquer des autorisations pour le contenu sur l’instance AEM **actuelle**. | Configurer des politiques de CUG pour le contenu sur l’instance de **création** AEM. Appliquer de politiques de CUG pour le contenu sur la ou les instances de **publication** AEM. |
| Niveaux d’autorisation | Définit les autorisations accordées/refusées pour les utilisateurs et utilisatrices/groupes à tous les niveaux : Lire, Modifier, Créer, Supprimer, Lire une liste de contrôle d’accès, Modifier une liste de contrôle d’accès, Répliquer. | Accorde l’accès en lecture à un ensemble d’utilisateurs et utilisatrices/de groupes. Refuse l’accès en lecture à *tous les autres* utilisateurs et utilisatrices/groupes. |
| Publication | Les ACL ne sont *pas* publiées avec du contenu. | Les politiques de CUG *sont* publiées avec du contenu. |

## Liens connexes {#supporting-links}

* [Gérer des ressources et des groupes d’utilisateurs et utilisatrices fermés](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=fr#closed-user-group)
* [Créer un groupe d’utilisateurs et d’utilisatrices fermé](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html?lang=fr)
* [Documentation du groupe d’utilisateurs et utilisatrices fermé Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
