---
title: Groupes d’utilisateurs fermés en AEM Assets
description: 'Les groupes d’utilisateurs fermés (CUG) sont une fonction utilisée pour restreindre l’accès au contenu à un groupe d’utilisateurs sélectionnés sur un site publié. Cette vidéo montre comment les groupes d’utilisateurs fermés peuvent être utilisés avec les ressources Adobe Experience Manager pour restreindre l’accès à un dossier spécifique de ressources. La prise en charge des groupes d’utilisateurs fermés avec AEM Assets a été introduite pour la première fois dans AEM 6.4. '
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 1%

---


# Groupes d’utilisateurs fermés{#using-closed-user-groups-with-aem-assets}

Les groupes d’utilisateurs fermés (CUG) sont une fonction utilisée pour restreindre l’accès au contenu à un groupe d’utilisateurs sélectionnés sur un site publié. Cette vidéo montre comment les groupes d’utilisateurs fermés peuvent être utilisés avec les ressources Adobe Experience Manager pour restreindre l’accès à un dossier spécifique de ressources. La prise en charge des groupes d’utilisateurs fermés avec AEM Assets a été introduite pour la première fois dans AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=9&learn=on)

## Groupe d’utilisateurs fermé (CUG) avec AEM Assets

* Conçu pour restreindre l’accès aux ressources sur une instance de publication AEM.
* Accorde l’accès en lecture à un ensemble d’utilisateurs/de groupes.
* CUG ne peut être configuré qu&#39;au niveau du dossier. CUG ne peut pas être défini sur des ressources individuelles.
* Les stratégies CUG sont automatiquement héritées de tous les sous-dossiers et ressources appliquées.
* Les stratégies CUG peuvent être remplacées par des sous-dossiers en définissant une nouvelle stratégie CUG. Cette méthode doit être utilisée avec parcimonie et n&#39;est pas considérée comme une bonne pratique.

## Représentation CUG dans le JCR {#cug-representation-in-the-jcr}

![Représentation CUG dans le JCR](assets/closed-user-groups/folder-properties-closed-user-groups.png)

Le groupe Membres We.Retail a été ajouté en tant que groupe d’utilisateurs fermé au dossier : /content/dam/we-commerce/fr/beta-products

Un mixin de **rep:CugMixin** est appliqué au dossier **/content/dam/we-commerce/en/beta-products** . Un noeud de **rep:cugPolicy** est ajouté sous le dossier et nous-commerçants-membres est spécifié comme entité de sécurité. Un autre mixin de **granite:AuthenticationRequired** est appliqué au dossier bêta-products et la propriété** granite:loginPath** spécifie la page de connexion à utiliser si un utilisateur n’est pas authentifié et tente de demander une ressource sous le dossier **bêta-products** .

Description JCR ci-dessous :

```xml
/beta-products
    - jcr:primaryType = sling:Folder
    - jcr:mixinTypes = rep:CugMixin, granite:AuthenticationRequired
    - granite:loginPath = /content/we-retail/us/en/community/signin
    + rep:cugPolicy
         - jcr:primaryType = rep:CugPolicy
         - rep:principalNames = we-retail-members
```

## Groupes d’utilisateurs fermés par rapport aux Listes de Contrôle d&#39;accès {#closed-user-groups-vs-access-control-lists}

Les groupes d’utilisateurs fermés (CUG) et les Listes de Contrôle d&#39;accès (ACL) sont utilisés pour contrôler l’accès au contenu dans AEM et en fonction des utilisateurs et groupes AEM Security. Cependant, l&#39;application et la mise en oeuvre de ces fonctionnalités sont très différentes. Le tableau suivant résume les différences entre les deux caractéristiques.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Utilisation prévue | Configurez et appliquez des autorisations pour le contenu sur l’instance **AEM actuelle** . | Configurez les stratégies CUG pour le contenu sur AEM instance **d’auteur** . Appliquez des stratégies CUG pour le contenu sur AEM instance de **publication** . |
| Niveaux d’autorisation | Définit les autorisations accordées/refusées pour les utilisateurs/groupes pour tous les niveaux : Lisez, modifiez, créez, supprimez, lisez ACL, modifiez ACL, répliquez. | Accorde l’accès en lecture à un ensemble d’utilisateurs/de groupes. Refusent l’accès en lecture à tous les autres utilisateurs/groupes. |
| Réplication | Les listes ACL ne sont pas répliquées avec le contenu. | Les stratégies CUG sont répliquées avec du contenu. |

## Liens pris en charge {#supporting-links}

* [Gestion des ressources et des groupes d’utilisateurs fermés](https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html#ClosedUserGroup)
* [Création d’un groupe d’utilisateurs fermé](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/cug.html)
* [Documentation du groupe d’utilisateurs fermé en chêne](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
