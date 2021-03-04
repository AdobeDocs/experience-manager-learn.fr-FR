---
title: Configuration de l’accès à AEM en tant que Cloud Service
description: 'L’AEM en tant que Cloud Service est le moyen natif du cloud d’exploiter les applications AEM et, en tant que tel, utilise l’Adobe IMS (Identity Management System) pour faciliter la connexion des utilisateurs, tant administrateurs que utilisateurs réguliers, au service AEM Author. Découvrez comment les utilisateurs IMS d’Adobe, les groupes d’utilisateurs et les profils de produits sont tous utilisés conjointement avec les groupes d’AEM et les autorisations pour fournir un accès spécifique à AEM Author.  '
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
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 5%

---


# Configuration de l’accès à AEM en tant que Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introduction à l&#39;Adobe IMS"
>abstract="AEM en tant que Cloud Service utilise l’Adobe IMS (Identity Management System) pour faciliter la connexion de ses utilisateurs, tant administrateurs que utilisateurs réguliers, au service AEM Author. Découvrez comment les utilisateurs, les groupes et les profils de produits Adobes IMS sont utilisés de concert avec les groupes AEM et les autorisations afin de fournir un accès affiné au service Auteur AEM."

L’AEM en tant que Cloud Service est le moyen natif du cloud d’exploiter les applications AEM et, en tant que tel, utilise l’Adobe IMS (Identity Management System) pour faciliter la connexion de ses utilisateurs, tant administrateurs que utilisateurs réguliers, au service Auteur AEM. Découvrez comment les utilisateurs, les groupes et les profils de produits Adobes IMS sont utilisés de concert avec les groupes AEM et les autorisations afin de fournir un accès affiné au service Auteur AEM.

## Adobes utilisateurs IMS

Les utilisateurs nécessitant l’accès au service Auteur AEM sont gérés en tant qu’[utilisateurs IMS d’Adobe](https://helpx.adobe.com/fr/enterprise/using/set-up-identity.html) dans [Console d’administration d’Adobe](https://adminconsole.adobe.com). Découvrez ce que sont les utilisateurs Adobes du SGI et comment ils sont accédés et gérés en Admin Console.

[En savoir plus sur les utilisateurs Adobe IMS](./adobe-ims-users.md)

## Groupes d’utilisateurs IMS Adobe

Les utilisateurs qui accèdent au service Auteur AEM doivent être organisés en groupes logiques à l’aide de [groupes d’utilisateurs IMS d’Adobe](https://helpx.adobe.com/enterprise/using/user-groups.html) dans [Console d’administration d’Adobe](https://adminconsole.adobe.com). Les groupes d’utilisateurs IMS d’Adobe ne fournissent pas d’autorisations directes ou d’accès à l’AEM (il s’agit du travail [Adobe de profils de produits IMS](#adobe-ims-product-profiles)), mais ils constituent un excellent moyen de définir des regroupements logiques d’utilisateurs qui peuvent à leur tour être traduits en niveaux d’accès spécifiques dans le service Auteur AEM, en utilisant des groupes AEM et des autorisations.

[En savoir plus sur les groupes d&#39;utilisateurs IMS d&#39;Adobe](./adobe-ims-user-groups.md)

## Profils de produits IMS Adobe

[Les profils](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html) de produits IMS d’Adobe, gérés dans la  [console d’administration](https://adminconsole.adobe.com) d’Adobe, sont le mécanisme qui permet aux  [utilisateurs de l’](#adobe-ims-users) Adobe IMS d’accéder au service Auteur AEM avec un niveau d’accès de base.

+ Le profil de produits __AEM Utilisateurs__ permet aux utilisateurs d’accéder en lecture seule aux AEM via l’adhésion au groupe des contributeurs d’AEM.
+ Le profil de produits __AEM Administrateurs__ permet aux utilisateurs d’accéder à l’AEM de manière administrative et complète.

[En savoir plus sur les profils de produits IMS Adobe](./adobe-ims-product-profiles.md)

## AEM groupes d’utilisateurs et autorisations

Adobe Experience Manager s’appuie sur les utilisateurs d’Adobe IMS, les groupes d’utilisateurs et les profils de produits afin de fournir aux utilisateurs un accès personnalisable à l’AEM. Découvrez comment créer des groupes AEM et des autorisations et comment ils fonctionnent de concert avec les abstractions IMS Adobes afin de fournir un accès transparent et personnalisable à AEM.

[En savoir plus sur AEM utilisateurs, groupes et autorisations](./aem-users-groups-and-permissions.md)

## Accès et autorisations sans fil

Présentation abrégée de la configuration des utilisateurs IMS d’Adobe, des groupes d’utilisateurs et des profils de produits dans la Console d’administration d’Adobe et de la manière d’exploiter ces abstractions IMS d’Adobe dans AEM Author pour définir et gérer des autorisations spécifiques basées sur des groupes.

[Accès AEM et autorisations sans fil](./walk-through.md)

## Ressources Adobe Admin Console supplémentaires

La documentation suivante couvre les détails et les préoccupations spécifiques à [Adobe Admin Console](https://adminconsole.adobe.com) qui peuvent aider à mieux comprendre le Adobe Admin Console et à l&#39;utiliser pour gérer les utilisateurs et l&#39;accès entre les produits Experience Cloud.

+ [Présentation de l’identité Adobe Admin Console](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Rôles d’administrateur Adobe Admin Console](https://helpx.adobe.com/fr/enterprise/using/admin-roles.html)
+ [Rôles de développement Adobe Admin Console](https://helpx.adobe.com/fr/enterprise/using/manage-developers.html)