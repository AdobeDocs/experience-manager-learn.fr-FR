---
title: Présentation de l’authentification IMS Adobe avec AEM sur Adobe Managed Services
description: Adobe Experience Manager propose une prise en charge Admin Console des instances AEM et une authentification basée sur l’Adobe IMS (Identity Management System) pour AEM sur Managed Services.   Cette intégration permet aux clients d'AEM Managed Services de gérer tous les utilisateurs Experience Cloud dans une seule console Web unifiée. Les utilisateurs et les groupes peuvent être affectés à des profils de produits associés à des instances AEM, ce qui permet d’accorder un accès géré de manière centralisée aux instances AEM spécifiques.
version: 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 4%

---


# Présentation de l’authentification IMS Adobe avec AEM sur Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager propose une prise en charge Admin Console des instances AEM et une authentification basée sur l’Adobe IMS (Identity Management System) pour AEM sur Managed Services.   Cette intégration permet aux clients d&#39;AEM Managed Services de gérer tous les utilisateurs Experience Cloud dans une seule console Web unifiée. Les utilisateurs et les groupes peuvent être affectés à des profils de produits associés à des instances AEM, ce qui permet d’accorder un accès géré de manière centralisée aux instances AEM spécifiques.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La prise en charge de l’authentification Adobe Experience Manager IMS s’applique uniquement aux utilisateurs &quot;internes&quot; (auteurs, réviseurs, administrateurs, développeurs, etc.) et non aux utilisateurs finaux externes tels que les visiteurs de site Web.
* [Admin Console](https://adminconsole.adobe.com/) représentera les clients Managed Services en tant qu’organisations IMS et les instances AEM en tant que Contextes de produit. Les administrateurs système Admin Console et produits peuvent définir et gérer.
* aem Managed Services synchronise votre topologie avec le Admin Console, en créant un mappage 1-1 entre un contexte de produit et une instance AEM.
* Le Profil de produit dans le Admin Console détermine les instances AEM auxquelles un utilisateur peut accéder.
* La prise en charge de l’authentification inclut les IDP conformes SAML2 des clients pour l’authentification unique.
* Seuls les Enterprise ID ou Federated ID (pour l’authentification unique du client) sont pris en charge (les Personal Adobe ID ne sont pas pris en charge).

** Cette fonctionnalité est prise en charge pour AEM 6.4 SP3 et versions ultérieures pour les clients Adobe Managed Services.*

## Bonnes pratiques {#best-practices}

### Application des autorisations dans le Admin Console

L’application des autorisations et l’accès au niveau utilisateur doivent être évités dans le Admin Console et dans Adobe Experience Manager.

Dans le Admin Console, l’accès doit être accordé aux utilisateurs via des groupes d’utilisateurs au niveau du contexte du produit. Les groupes d’utilisateurs sont généralement mieux exprimés par le rôle logique au sein de l’organisation afin de promouvoir la réutilisation des groupes dans les produits Adobe Experience Cloud.

>[!NOTE]
>
> Si vous utilisez AEM comme Cloud Service, affectez des utilisateurs Admin Console directement aux Profils de produits. Les autorisations transitoires entre les utilisateurs Admin Console pour les Profils de produits via des groupes d’utilisateurs Admin Console ne sont pas prises en charge pour AEM en tant que Cloud Service.

### Application des autorisations dans Adobe Experience Manager

Dans Adobe Experience Manager, les groupes d’utilisateurs synchronisés à partir de l’IMS Adobe doivent être ajoutés à terme aux groupes [d’utilisateurs fournis par](https://helpx.adobe.com/fr/experience-manager/6-4/sites/administering/using/security.html)AEM, qui sont préconfigurés avec les autorisations appropriées pour exécuter des ensembles spécifiques de tâches dans AEM. Les utilisateurs synchronisés à partir d&#39;un Adobe IMS ne doivent pas être directement ajoutés aux groupes [d&#39;utilisateurs](https://helpx.adobe.com/fr/experience-manager/6-4/sites/administering/using/security.html)AEM fournis.
