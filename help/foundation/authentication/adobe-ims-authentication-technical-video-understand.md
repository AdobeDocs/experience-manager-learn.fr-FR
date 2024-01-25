---
title: Comprendre l’authentification Adobe IMS avec AEM sur Adobe Managed Services
description: Adobe Experience Manager offre une prise en charge des instances AEM avec Admin Console ainsi qu’une authentification basée sur Adobe IMS (Identity Management System) pour AEM sur Managed Services.   Cette intégration permet aux clientes et clients Managed Services d’AEM de gérer tous les utilisateurs et utilisatrices d’Experience Cloud dans une seule console web unifiée. Les utilisateurs, les utilisatrices et les groupes peuvent être affectés aux profils de produit associés aux instances AEM, ce qui permet d’accorder un accès géré de manière centralisée aux instances AEM spécifiques.
version: 6.4, 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 420
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 100%

---

# Comprendre l’authentification Adobe IMS avec AEM sur Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager offre une prise en charge des instances AEM avec Admin Console ainsi qu’une authentification basée sur Adobe IMS (Identity Management System) pour AEM sur Managed Services.   Cette intégration permet aux clientes et clients Managed Services d’AEM de gérer tous les utilisateurs et utilisatrices d’Experience Cloud dans une seule console web unifiée. Les utilisateurs, les utilisatrices et les groupes peuvent être affectés aux profils de produit associés aux instances AEM, ce qui permet d’accorder un accès géré de manière centralisée aux instances AEM spécifiques.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La prise en charge de l’authentification IMS par Adobe Experience Manager est réservée aux utilisateurs et utilisatrices « internes » (auteurs, autrices, réviseurs, réviseuses, administrateurs, administratrices, développeurs, développeuses, etc.) et non aux personnes finales externes qui visitent le site web.
* L’[Admin Console](https://adminconsole.adobe.com/) représente les clientes et clients d’AEM Managed Services en tant qu’organisations IMS et les instances AEM en tant que contextes de produit. Les administrateurs et administratrices système et produit de l’Admin Console peuvent définir et gérer les différents éléments.
* AEM Managed Services synchronise votre topologie avec l’Admin Console, créant ainsi un mappage 1-1 entre un contexte de produit et une instance AEM.
* Le profil de produit dans l’Admin Console détermine les instances AEM auxquelles un utilisateur ou une utilisatrice peut accéder.
* La prise en charge de l’authentification inclut les IDP compatibles SAML2 des clientes et clients pour l’authentification unique.
* Seuls les ID Enterprise ou Federated (pour l’authentification unique du client ou de la cliente) sont pris en charge (contrairement aux ID Adobe).

*&#42;Cette fonctionnalité est prise en charge pour AEM 6.4 SP3 et versions ultérieures pour les clientes et clients Adobe Managed Services.*

## Bonnes pratiques {#best-practices}

### Appliquer les autorisations dans l’Admin Console

Il est recommandé d’éviter d’appliquer des autorisations et des accès au niveau de l’utilisateur ou de l’utilisatrice dans l’Admin Console et dans Adobe Experience Manager.

Les utilisateurs et les utilisatrices de l’Admin Console doivent se voir accorder l’accès via des groupes d’utilisateurs et d’utilisatrices au niveau du contexte du produit. Les groupes d’utilisateurs et d’utilisatrices sont généralement mieux exprimés par rôle logique au sein de l’organisation afin de promouvoir la réutilisation des groupes dans les produits Adobe Experience Cloud.

>[!NOTE]
>
> Si vous utilisez AEM as a Cloud Service, affectez des utilisateurs et des utilisatrices de l’Admin Console directement aux profils de produit. Les autorisations transitoires entre les utilisateurs et les utilisatrices Admin Console et les profils de produit via les groupes Admin Console ne sont pas prises en charge pour AEM as a Cloud Service.

### Appliquer des autorisations dans Adobe Experience Manager

Dans Adobe Experience Manager, les groupes d’utilisateurs et d’utilisatrices synchronisés à partir d’Adobe IMS doivent être ajoutés dans les [groupes AEM fournis](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=fr), qui sont préconfigurés avec les autorisations appropriées pour exécuter des ensembles de tâches spécifiques dans AEM. Les utilisateurs et les utilisatrices synchronisés depuis Adobe IMS ne doivent pas être directement ajoutés aux [groupes AEM fournis](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=fr).
