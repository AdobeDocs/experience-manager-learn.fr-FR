---
title: Notification de sécurité AEM (novembre 2018)
seo-title: AEM Security Notification (November 2018)
description: Dispatcher de notification de sécurité AEM Experience Manager
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: ht
source-wordcount: '428'
ht-degree: 100%

---

# Notification de sécurité AEM (novembre 2018)

## Résumé

Cet article traite de quelques vulnérabilités, récentes et anciennes, récemment signalées dans AEM. Notez que la plupart des vulnérabilités identifiées étaient des problèmes connus pour le produit AEM et que la limitation a déjà été identifiée. Une nouvelle version du Dispatcher est disponible pour les nouvelles vulnérabilités. Adobe conseille également vivement aux clientes et clients de compléter la [liste de contrôle de sécurité d’AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/security-checklist.html) et de suivre les directives pertinentes.

## Action requise

* Les déploiements d’AEM doivent commencer à utiliser la dernière version du Dispatcher.
* Les règles de sécurité du Dispatcher doivent être appliquées conformément à la configuration recommandée.
* La [liste de contrôle de sécurité d’AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/security-checklist.html) doit être complétée pour les déploiements d’AEM.

## Vulnérabilités et résolutions

| Problème | Résolution | Liens |
|-------|------------|-------|
| Contournement des règles du Dispatcher d’AEM | Installez la dernière version du Dispatcher (4.3.1) et suivez la configuration recommandée pour le Dispatcher. | Voir [Notes de mise à jour d’AEM Dispatcher](https://helpx.adobe.com/fr/experience-manager/dispatcher/release-notes.html) et [Configuration du Dispatcher](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnérabilité de contournement du filtre d’URL pouvant être utilisée pour contourner les règles du Dispatcher - CVE-2016-0957 | Ce problème a été corrigé dans une ancienne version du Dispatcher, mais il est désormais recommandé d’installer la dernière version du Dispatcher (4.3.1) et de suivre la configuration recommandée pour le Dispatcher. | Voir [Notes de mise à jour d’AEM Dispatcher](https://helpx.adobe.com/fr/experience-manager/dispatcher/release-notes.html) et [Configuration du Dispatcher](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnérabilité XSS liée aux fichiers SWF stockés | Ce problème a été résolu avec les correctifs de sécurité publiés précédemment. | Consultez le [Bulletin de sécurité AEM APSB18-10](https://helpx.adobe.com/fr/security/products/experience-manager/apsb18-10.html). |
| Exploitations de mot de passe | Suivez la recommandation figurant sur la liste de contrôle de sécurité pour les mots de passe plus sécurisés. | Voir [Liste de contrôle de sécurité d’AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/security-checklist.html). |
| Exposition de l’utilisation du disque pour les utilisateurs et utilisatrices anonymes | Ce problème a été résolu pour AEM 6.1 et versions ultérieures. Pour AEM 6.0, les autorisations d’usine peuvent être modifiées pour être plus restrictives. | Voir les [notes de mise à jour](https://helpx.adobe.com/fr/experience-manager/aem-previous-versions.html) pour AEM 6.1 et les versions antérieures. |
| Exposition du proxy d’Open Social pour les utilisateurs et utilisatrices anonymes | Ce problème a été résolu à partir de la version 6.0 SP2. | Voir les [notes de mise à jour](https://helpx.adobe.com/fr/experience-manager/aem-previous-versions.html) pour AEM 6.1 et les versions antérieures. |
| Accès à l’explorateur CRX sur les instances de production | La gestion de l’accès à l’explorateur CRX est déjà traitée dans la liste de contrôle de sécurité, l’explorateur CRX doit être supprimé des instances de création et de publication en production, et le contrôle d’intégrité de la sécurité le signale s’il n’est pas supprimé. | Voir [Liste de contrôle de sécurité d’AEM](https://helpx.adobe.com/fr/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets est exposé | Ce problème a été résolu depuis AEM 6.2. | Voir [Notes de mise à jour d’AEM 6.2](https://helpx.adobe.com/fr/experience-manager/6-2/release-notes.html). |

>[!MORELIKETHIS]
>
>* [Guide d’utilisation d’AEM Dispatcher](https://helpx.adobe.com/fr/experience-manager/dispatcher/user-guide.html)
>* [Notes de mise à jour d’AEM Dispatcher](https://helpx.adobe.com/fr/experience-manager/dispatcher/release-notes.html)
>* [Bulletins de sécurité AEM](https://helpx.adobe.com/fr/security.html#experience-manager)

