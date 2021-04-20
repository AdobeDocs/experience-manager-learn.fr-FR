---
title: Groupes d’utilisateurs fermés en AEM Assets
description: Les groupes d’utilisateurs fermés (CUG) sont une fonction utilisée pour restreindre l’accès au contenu à un groupe d’utilisateurs sélectionnés sur un site publié. Cette vidéo montre comment les groupes d’utilisateurs fermés peuvent être utilisés avec les ressources Adobe Experience Manager pour restreindre l’accès à un dossier spécifique de ressources.
version: 6.3, 6.4, 6.5, cloud-service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 2%

---


# Groupes d’utilisateurs fermés{#using-closed-user-groups-with-aem-assets}

Les groupes d’utilisateurs fermés (CUG) sont une fonction utilisée pour restreindre l’accès au contenu à un groupe d’utilisateurs sélectionnés sur un site publié. Cette vidéo montre comment les groupes d’utilisateurs fermés peuvent être utilisés avec les ressources Adobe Experience Manager pour restreindre l’accès à un dossier spécifique de ressources. La prise en charge des groupes d’utilisateurs fermés avec AEM Assets a été introduite pour la première fois dans AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Groupe d’utilisateurs fermé (CUG) avec AEM Assets

* Conçu pour restreindre l’accès aux ressources sur une instance de publication AEM.
* Accorde l’accès en lecture à un ensemble d’utilisateurs/de groupes.
* CUG ne peut être configuré qu&#39;au niveau du dossier. CUG ne peut pas être défini sur des ressources individuelles.
* Les stratégies CUG sont automatiquement héritées de tous les sous-dossiers et ressources appliquées.
* Les stratégies CUG peuvent être remplacées par des sous-dossiers en définissant une nouvelle stratégie CUG. Cette méthode doit être utilisée avec parcimonie et n&#39;est pas considérée comme une bonne pratique.

## Groupes d’utilisateurs fermés par rapport aux Listes de Contrôle d&#39;accès {#closed-user-groups-vs-access-control-lists}

Les groupes d’utilisateurs fermés (CUG) et les Listes de Contrôle d&#39;accès (ACL) sont utilisés pour contrôler l’accès au contenu dans AEM et en fonction des utilisateurs et groupes AEM Security. Cependant, l&#39;application et la mise en oeuvre de ces fonctionnalités sont très différentes. Le tableau suivant résume les différences entre les deux caractéristiques.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Utilisation prévue | Configurez et appliquez des autorisations pour le contenu sur l&#39;instance d&#39;AEM **active**. | Configurez les stratégies CUG pour le contenu sur l&#39;instance d&#39;AEM **auteur**. Appliquez des stratégies CUG pour le contenu sur AEM instance **publish**. |
| Niveaux d’autorisation | Définit les autorisations accordées/refusées pour les utilisateurs/groupes pour tous les niveaux : Lisez, modifiez, créez, supprimez, lisez ACL, modifiez ACL, répliquez. | Accorde l’accès en lecture à un ensemble d’utilisateurs/de groupes. Refusent l’accès en lecture à *tous les autres utilisateurs/groupes*. |
| Publication | Les listes ACL *ne sont pas* publiées avec le contenu. | Les stratégies CUG *sont* publiées avec le contenu. |

## Liens de prise en charge {#supporting-links}

* [Gestion des ressources et des groupes d’utilisateurs fermés](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Création d’un groupe d’utilisateurs fermé](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentation du groupe d’utilisateurs fermé en chêne](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
