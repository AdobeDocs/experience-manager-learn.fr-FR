---
title: Configuration de l’accès à AEM en tant que Cloud Service
description: 'AEM en tant que Cloud Service est le moyen natif d’exploiter les applications AEM et, à ce titre, exploite l’Adobe IMS (Identity Management System) pour faciliter la connexion des utilisateurs, tant administrateurs que utilisateurs réguliers, au service AEM Author. Découvrez comment les utilisateurs, groupes d’utilisateurs et profils de produits Adobe IMS sont tous utilisés conjointement avec les groupes d’AEM et les autorisations afin de fournir un accès spécifique à l’auteur AEM.  '
feature: 'Utilisateurs et groupes '
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: Administration, sécurité
role: Administrator
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 5%

---


# Configuration de l’accès à AEM en tant que Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Présentation d’Adobe IMS"
>abstract="AEM as a Cloud Service exploite l’Adobe IMS (système Identity Management) pour faciliter la connexion de ses utilisateurs, tant administrateurs que utilisateurs réguliers, au service AEM Author. Découvrez comment les utilisateurs, groupes et profils de produits Adobe IMS sont utilisés de concert avec les groupes AEM et les autorisations afin de fournir un accès affiné au service AEM Author."

AEM en tant que Cloud Service est le moyen natif d’exploiter les applications AEM et, à ce titre, exploite l’Adobe IMS (Identity Management System) pour faciliter la connexion de ses utilisateurs, tant administrateurs que membres réguliers, au service AEM Author. Découvrez comment les utilisateurs, groupes et profils de produits Adobe IMS sont utilisés de concert avec les groupes AEM et les autorisations afin de fournir un accès affiné au service AEM Author.

## Adobe des utilisateurs IMS

Les utilisateurs nécessitant l’accès au service AEM Author sont gérés en tant que [Adobe des utilisateurs IMS](https://helpx.adobe.com/fr/enterprise/using/set-up-identity.html) dans [Admin Console](https://adminconsole.adobe.com) de l’Adobe. Découvrez ce que sont les utilisateurs IMS Adobes et comment ils sont accessibles et gérés dans Admin Console.

[En savoir plus sur les utilisateurs IMS d’Adobe](./adobe-ims-users.md)

## Adobe des groupes d’utilisateurs IMS

Les utilisateurs accédant au service AEM Author doivent être organisés en groupes logiques à l’aide de [groupes d’utilisateurs IMS d’Adobe](https://helpx.adobe.com/enterprise/using/user-groups.html) dans [Admin Console](https://adminconsole.adobe.com) de l’Adobe. Les groupes d’utilisateurs IMS d’Adobe ne fournissent pas d’autorisations directes ni d’accès à AEM (c’est la tâche de [Adobe des profils de produit IMS](#adobe-ims-product-profiles)). Toutefois, ils constituent un excellent moyen de définir des regroupements logiques d’utilisateurs qui peuvent à leur tour être traduits vers des niveaux d’accès spécifiques dans le service AEM Author, à l’aide de groupes AEM et d’autorisations.

[En savoir plus sur les groupes d’utilisateurs IMS d’Adobe](./adobe-ims-user-groups.md)

## Adobe de profils de produits IMS

[Les profils](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html) de produit IMS d’Adobe, gérés dans Admin Console [ d’](https://adminconsole.adobe.com)Adobe, sont le mécanisme qui permet aux  [utilisateurs IMS d’](#adobe-ims-users) Adobe d’accéder au service AEM Author avec un niveau d’accès de base.

+ Le profil de produit __AEM Utilisateurs__ permet aux utilisateurs d’accéder en lecture seule à AEM via l’appartenance au groupe des contributeurs d’AEM.
+ Le profil de produit __Administrateurs d’AEM__ permet aux utilisateurs d’accéder pleinement et de manière administrative à AEM.

[En savoir plus sur les profils de produit IMS d’Adobe](./adobe-ims-product-profiles.md)

## AEM groupes d’utilisateurs et autorisations

Adobe Experience Manager s’appuie sur les utilisateurs IMS, les groupes d’utilisateurs et les profils de produits Adobe pour fournir aux utilisateurs un accès personnalisable à AEM. Découvrez comment créer des groupes d’AEM et des autorisations, et comment ils fonctionnent de concert avec les abstractions IMS d’Adobe afin de fournir un accès transparent et personnalisable à AEM.

[En savoir plus sur AEM utilisateurs, les groupes et les autorisations](./aem-users-groups-and-permissions.md)

## Présentation de l’accès et des autorisations

Présentation rapide de la configuration des utilisateurs IMS, des groupes d’utilisateurs et des profils de produits d’Adobe dans Adobe Admin Console et de la manière d’exploiter ces abstractions IMS d’Adobe dans AEM Author pour définir et gérer des autorisations spécifiques basées sur des groupes.

[Présentation AEM de l’accès et des autorisations](./walk-through.md)

## Ressources Adobe Admin Console supplémentaires

La documentation suivante couvre les détails et les préoccupations spécifiques à [Adobe Admin Console](https://adminconsole.adobe.com) qui peuvent aider à une meilleure compréhension de Adobe Admin Console et l’utiliser pour gérer les utilisateurs et accéder à l’ensemble des produits Experience Cloud.

+ [Présentation de Adobe Admin Console Identity](https://helpx.adobe.com/fr/enterprise/using/identity.html)
+ [Rôles d’administrateur Adobe Admin Console](https://helpx.adobe.com/fr/enterprise/using/admin-roles.html)
+ [Rôles de développement Adobe Admin Console](https://helpx.adobe.com/fr/enterprise/using/manage-developers.html)