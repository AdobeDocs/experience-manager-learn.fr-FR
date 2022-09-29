---
title: Présentation de l’authentification Adobe IMS avec AEM sur Adobe Managed Services
description: Adobe Experience Manager offre une prise en charge Admin Console des instances d’AEM et une authentification basée sur Adobe IMS (système Identity Management) pour AEM sur Managed Services.   Cette intégration permet aux clients Managed Services d’AEM de gérer tous les utilisateurs Experience Cloud dans une seule console web unifiée. Les utilisateurs et les groupes peuvent être affectés aux profils de produit associés aux instances AEM, ce qui permet d’accorder un accès géré de manière centralisée aux instances AEM spécifiques.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 4%

---

# Présentation de l’authentification Adobe IMS avec AEM sur Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager offre une prise en charge Admin Console des instances d’AEM et une authentification basée sur Adobe IMS (système Identity Management) pour AEM sur Managed Services.   Cette intégration permet aux clients Managed Services d’AEM de gérer tous les utilisateurs Experience Cloud dans une seule console web unifiée. Les utilisateurs et les groupes peuvent être affectés aux profils de produit associés aux instances AEM, ce qui permet d’accorder un accès géré de manière centralisée aux instances AEM spécifiques.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La prise en charge de l’authentification IMS Adobe Experience Manager est réservée aux utilisateurs &quot;internes&quot; (auteurs, réviseurs, administrateurs, développeurs, etc.) et non aux utilisateurs finaux externes tels que les visiteurs du site web.
* [Admin Console](https://adminconsole.adobe.com/) représente AEM clients Managed Services en tant qu’organisations IMS et les instances AEM en tant que contextes de produit. Les administrateurs système et produit de Admin Console peuvent définir et gérer.
* AEM Managed Services synchronise votre topologie avec Admin Console, créant ainsi un mappage 1-1 entre un contexte de produit et une instance AEM.
* Le profil de produit dans Admin Console détermine les instances AEM auxquelles un utilisateur peut accéder.
* La prise en charge de l’authentification inclut les IDP compatibles SAML2 des clients pour SSO.
* Seuls les Enterprise ID ou les Federated ID (pour l’authentification unique du client) sont pris en charge (les Personal Adobe ID ne sont pas pris en charge).

*&#42;Cette fonctionnalité est prise en charge pour AEM 6.4 SP3 et versions ultérieures pour les clients Adobe Managed Services.*

## Bonnes pratiques {#best-practices}

### Application des autorisations dans Admin Console

L’application des autorisations et de l’accès au niveau de l’utilisateur doit être évitée dans Admin Console et dans Adobe Experience Manager.

Dans Admin Console, l’accès doit être accordé aux utilisateurs via des groupes d’utilisateurs au niveau du contexte du produit. Les groupes d’utilisateurs sont généralement mieux exprimés par rôle logique au sein de l’organisation afin de promouvoir la réutilisation des groupes dans les produits Adobe Experience Cloud.

>[!NOTE]
>
> Si vous utilisez AEM as a Cloud Service, affectez des utilisateurs Admin Console directement aux profils de produit. Les autorisations transitoires entre les utilisateurs Admin Console et les profils de produit via les groupes d’utilisateurs Admin Console ne sont pas prises en charge pour AEM as a Cloud Service.

### Application des autorisations dans Adobe Experience Manager

Dans Adobe Experience Manager, les groupes d’utilisateurs synchronisés à partir d’Adobe IMS doivent être ajoutés dans le terme à [Groupes d’utilisateurs AEM fournis](https://helpx.adobe.com/fr/experience-manager/6-4/sites/administering/using/security.html), qui sont préconfigurés avec les autorisations appropriées pour exécuter des ensembles de tâches spécifiques dans AEM. Les utilisateurs synchronisés depuis Adobe IMS ne doivent pas être directement ajoutés à [Groupes d’utilisateurs AEM fournis](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
