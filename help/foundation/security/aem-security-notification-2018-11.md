---
title: Notification de sécurité AEM (novembre 2018)
seo-title: Notification de sécurité AEM (novembre 2018)
description: Dispatcher de notification de sécurité des Experience Manager
seo-description: Dispatcher de notification de sécurité des Experience Manager
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Sécurité
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 17%

---


# Notification de sécurité AEM (novembre 2018)

## Résumé

Cet article traite de quelques vulnérabilités anciennes et récentes récemment signalées dans AEM. Notez que la plupart des vulnérabilités identifiées étaient des problèmes connus pour le produit AEM et la limitation ont été précédemment identifiés. Une nouvelle version de Dispatcher est disponible pour les nouvelles vulnérabilités. Adobe demande également aux clients de compléter la [Liste de contrôle de sécurité AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/security-checklist.html) et de suivre les directives appropriées.

## Action requise

* AEM déploiements doivent commencer à utiliser la dernière version de Dispatcher.
* Les règles de sécurité du Dispatcher doivent être appliquées conformément à la configuration recommandée.
* La [Liste de contrôle de sécurité AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) doit être renseignée pour les déploiements AEM.

## Vulnérabilités et résolutions

| Problème | Résolution | Liens |
|-------|------------|-------|
| Contournement des règles du Dispatcher AEM | Installez la dernière version de Dispatcher (4.3.1) et suivez la configuration recommandée pour Dispatcher. | Voir [AEM Notes de mise à jour de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) et [Configuration de Dispatcher](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnérabilité de contournement du filtre d’URL pouvant être utilisée pour contourner les règles du Dispatcher - CVE-2016-0957 | Ce problème a été corrigé dans une ancienne version de Dispatcher, mais il est désormais recommandé d’installer la dernière version de Dispatcher (4.3.1) et de suivre la configuration recommandée pour Dispatcher. | Voir [AEM Notes de mise à jour de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) et [Configuration de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnérabilité XSS liée aux fichiers SWF stockés | Ce problème a été résolu avec les correctifs de sécurité publiés précédemment. | Voir [Bulletin de sécurité AEM APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Explosions liées au mot de passe | Suivez la recommandation figurant sur la liste de contrôle de sécurité pour les mots de passe plus difficiles à deviner. | Voir [Liste de contrôle AEM sécurité](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Exposition de l’utilisation du disque pour les utilisateurs anonymes | Ce problème a été résolu pour AEM 6.1 et versions ultérieures. Pour AEM 6.0, les autorisations d’usine peuvent être modifiées pour être plus restrictives. | Voir les [notes de mise à jour](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=fr#previous-updates)pour AEM 6.1 et versions antérieures. |
| Exposition d’Open Social Proxy pour les utilisateurs anonymes | Ce problème a été résolu dans les versions à partir de la version 6.0 SP2. | Voir les [notes de mise à jour](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) pour AEM 6.1 et versions antérieures. |
| Accès à l’explorateur CRX sur les instances de production | La gestion de l’accès à l’Explorateur CRX est déjà traitée dans la liste de contrôle de sécurité, l’Explorateur CRX doit être supprimé de l’auteur et de la publication en production et le contrôle d’intégrité de la sécurité le signale s’il n’est pas supprimé. | Voir [Liste de contrôle AEM sécurité](https://helpx.adobe.com/fr/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets est exposé | Ce problème a été résolu depuis AEM 6.2. | Voir [Notes de mise à jour AEM 6.2](https://helpx.adobe.com/fr/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guide de l’utilisateur de Dispatcher AEM](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Notes de mise à jour d’AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Bulletins de sécurité AEM](https://helpx.adobe.com/security.html#experience-manager)

