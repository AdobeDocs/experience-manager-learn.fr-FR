---
title: Notification de sécurité AEM (novembre 2018)
seo-title: Notification de sécurité AEM (novembre 2018)
description: Répartiteur de notifications de sécurité Experience Manager
seo-description: Répartiteur de notifications de sécurité Experience Manager
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Sécurité
role: Architecte
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 18%

---


# Notification de sécurité AEM (novembre 2018)

## Résumé

Cet article traite de quelques vulnérabilités récentes et anciennes qui ont été récemment rapportées dans AEM. Notez que la plupart des vulnérabilités identifiées étaient des problèmes connus pour le produit AEM et l&#39;atténuation ont été identifiés précédemment, une nouvelle version du répartiteur est disponible pour les nouvelles vulnérabilités. L&#39;Adobe exhorte également les clients à remplir la [Liste de vérification de sécurité AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/security-checklist.html) et à suivre les directives pertinentes.

## Action requise

* AEM déploiements doivent début avec la dernière version du répartiteur.
* Les règles de sécurité du répartiteur doivent être appliquées conformément à la configuration recommandée.
* La [liste de contrôle de sécurité AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) doit être complétée pour les déploiements AEM.

## Vulnérabilités et résolutions

| Problème | Résolution | Liens |
|-------|------------|-------|
| Contournement des règles du répartiteur AEM | Installez la dernière version de Dispatcher(4.3.1) et suivez la configuration recommandée pour le répartiteur. | Voir [AEM Notes de mise à jour du répartiteur](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) et [Configuration du répartiteur](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnérabilité de contournement du filtre d’URL pouvant être utilisée pour contourner les règles du répartiteur - CVE-2016-0957 | Ce problème a été corrigé dans une ancienne version de Dispatcher, mais il est maintenant recommandé d&#39;installer la dernière version de Dispatcher (4.3.1) et de suivre la configuration recommandée de Dispatcher. | Voir [AEM Notes de mise à jour du répartiteur](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) et [Configuration du répartiteur](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnérabilité XSS liée aux fichiers SWF stockés | Ceci a été résolu avec des correctifs de sécurité publiés précédemment. | Consultez [AEM Bulletin de sécurité APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Exploits liés au mot de passe | Suivez les recommandations de la liste de contrôle de sécurité pour obtenir des mots de passe plus difficiles à deviner. | Voir [AEM liste de contrôle de sécurité](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Exposition sur l&#39;utilisation du disque pour les utilisateurs anonymes | Ce problème a été résolu pour AEM 6.1 et les versions ultérieures. Pour AEM 6.0, les autorisations prêtes à l’emploi peuvent être modifiées pour être plus restrictives. | Voir [notes de mise à jour](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=fr#previous-updates)pour AEM version 6.1 et ultérieure. |
| Exposition du proxy social ouvert pour les utilisateurs anonymes | Ceci a été résolu dans les versions à partir de la version 6.0 SP2. | Voir [notes de mise à jour](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) pour AEM version 6.1 et ultérieure. |
| CRX Explorer Access sur les instances de production | La gestion de l’accès à CRX Explorer est déjà couverte dans la liste de contrôle de sécurité, CRX Explorer doit être supprimé de l’auteur et de la publication en production et le contrôle d’intégrité de la sécurité le signale s’il n’est pas supprimé. | Voir [AEM liste de contrôle de sécurité](https://helpx.adobe.com/fr/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets est exposé | Cela a été résolu depuis AEM 6.2. | Voir [Notes de mise à jour AEM 6.2](https://helpx.adobe.com/fr/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guide de l&#39;utilisateur du répartiteur AEM](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Notes de mise à jour d’AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Bulletins de sécurité AEM](https://helpx.adobe.com/security.html#experience-manager)

