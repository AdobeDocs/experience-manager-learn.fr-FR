---
title: Configuration de l’accès à AEM as a Cloud Service
description: AEM as a Cloud Service est la méthode native du cloud qui permet d’exploiter les applications AEM et, en tant que telle, utilise Adobe IMS (système Identity Management) pour faciliter la connexion des utilisateurs, administrateurs et utilisateurs réguliers, au service AEM Author. Découvrez comment les utilisateurs, groupes d’utilisateurs et profils de produits Adobe IMS sont tous utilisés conjointement avec les groupes d’AEM et les autorisations afin de fournir un accès spécifique à l’auteur AEM.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 8%

---

# Configuration de l’accès à AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Présentation d’Adobe IMS"
>abstract="AEM as a Cloud Service exploite Adobe IMS (système Identity Management) pour faciliter la connexion de ses utilisateurs, administrateurs et utilisateurs réguliers, au service AEM Author. Découvrez comment les utilisateurs, groupes et profils de produits Adobe IMS sont utilisés de concert avec les groupes AEM et les autorisations afin de fournir un accès affiné au service AEM Author."

AEM as a Cloud Service est la méthode native du cloud qui permet d’exploiter les applications AEM et, à ce titre, d’utiliser Adobe IMS (système Identity Management) pour faciliter la connexion de ses utilisateurs, tant administrateurs qu’utilisateurs réguliers, au service AEM Author.

![Adobe Admin Console](./assets/hero.png)

Découvrez comment les utilisateurs, groupes et profils de produits Adobe IMS sont utilisés de concert avec les groupes AEM et les autorisations afin de fournir un accès affiné au service AEM Author.

## Utilisateurs Adobe IMS

Les utilisateurs nécessitant l’accès au service AEM Author sont gérés en tant que [Utilisateurs Adobe IMS](https://helpx.adobe.com/fr/enterprise/using/set-up-identity.html) in [Admin Console de l’Adobe](https://adminconsole.adobe.com). Découvrez ce que sont les utilisateurs Adobe IMS et comment ils sont accessibles et gérés en Admin Console.

[En savoir plus sur les utilisateurs Adobe IMS](./adobe-ims-users.md)

## Groupes d’utilisateurs Adobe IMS

Les utilisateurs accédant au service AEM Author doivent être organisés en groupes logiques à l’aide de [Groupes d’utilisateurs Adobe IMS](https://helpx.adobe.com/fr/enterprise/using/user-groups.html) in [Admin Console de l’Adobe](https://adminconsole.adobe.com). Les groupes d’utilisateurs Adobe IMS ne fournissent pas d’autorisations directes ni d’accès à AEM (c’est la tâche de [Profils de produit Adobe IMS](#adobe-ims-product-profiles)), cependant, il s’agit d’un excellent moyen de définir des regroupements logiques d’utilisateurs qui peuvent à leur tour être traduits en niveaux d’accès spécifiques dans le service AEM Author, à l’aide de groupes d’AEM et d’autorisations.

[En savoir plus sur les groupes d’utilisateurs Adobe IMS](./adobe-ims-user-groups.md)

## Profils de produit Adobe IMS

[Profils de produit Adobe IMS](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), géré dans [Admin Console de l’Adobe](https://adminconsole.adobe.com), sont le mécanisme qui fournit [Utilisateurs Adobe IMS](#adobe-ims-users) accès pour se connecter au service AEM Author avec un niveau d’accès de base.

+ Le __Utilisateurs AEM__ Le profil de produit permet aux utilisateurs d’accéder aux AEM en lecture seule via l’appartenance au groupe AEM contributeurs.
+ Le __Administrateurs AEM__ Le profil de produit permet aux utilisateurs d’accéder pleinement à l’AEM d’administration.

[En savoir plus sur les profils de produit Adobe IMS](./adobe-ims-product-profiles.md)

## AEM groupes d’utilisateurs et autorisations

Adobe Experience Manager s’appuie sur les utilisateurs, groupes d’utilisateurs et profils de produits Adobe IMS pour offrir aux utilisateurs un accès personnalisable à AEM. Découvrez comment créer des groupes d’AEM et des autorisations, et comment ils fonctionnent de concert avec les abstractions Adobe IMS afin de fournir un accès transparent et personnalisable à AEM.

[En savoir plus sur AEM utilisateurs, les groupes et les autorisations](./aem-users-groups-and-permissions.md)

## Présentation de l’accès et des autorisations

Présentation rapide de la configuration des utilisateurs, groupes d’utilisateurs et profils de produits Adobe IMS dans Admin Console Adobe et de la manière d’exploiter ces abstractions Adobe IMS dans l’auteur AEM pour définir et gérer des autorisations spécifiques basées sur des groupes.

[Présentation AEM de l’accès et des autorisations](./walk-through.md)

## Ressources Adobe Admin Console supplémentaires

La documentation suivante couvre [Adobe Admin Console](https://adminconsole.adobe.com)- des détails et des préoccupations spécifiques qui peuvent aider à une meilleure compréhension de Adobe Admin Console et à l’utiliser pour gérer les utilisateurs et accéder à l’ensemble des produits Experience Cloud.

+ [Présentation de Adobe Admin Console Identity](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Rôles d’administrateur Adobe Admin Console](https://helpx.adobe.com/fr/enterprise/using/admin-roles.html)
+ [Rôles de développement Adobe Admin Console](https://helpx.adobe.com/fr/enterprise/using/manage-developers.html)
