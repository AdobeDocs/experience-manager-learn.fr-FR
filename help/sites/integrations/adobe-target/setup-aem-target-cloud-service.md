---
title: Créer un compte Adobe Target Cloud Service dans AEM
description: Intégrez Adobe Experience Manager as a Cloud Service à Adobe Target à l’aide de Cloud Service et de l’authentification Adobe IMS.
jira: KT-6044
thumbnail: 41244.jpg
topic: Integrations
feature: Integrations
role: Admin
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: dd6c17ae-8e08-4db3-95f9-081cc7dbd86e
duration: 316
source-git-commit: 8ebaba01d4b470a1ca7c1630f55b756796f3640d
workflow-type: ht
source-wordcount: '191'
ht-degree: 100%

---

# Créer un compte Adobe Target as a Cloud Service {#adobe-target-cloud-service}

La vidéo suivante explique comment connecter AEM as a Cloud Service à Adobe Target.

Cette intégration permet au service de création AEM de communiquer directement avec Adobe Target et de transmettre les fragments d’expérience d’AEM à Target sous forme d’offres.  Cette intégration n’ajoute *pas* le code JavaScript Adobe Target (AT.js) aux pages web AEM Sites. Pour ce faire, intégrez [AEM et des balises à l’aide de l’extension Target](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md).

>[!WARNING]
>
>La vidéo présente une méthode d’authentification JWT obsolète permettant de connecter AEM à Adobe Target. Toutefois, il est recommandé d’utiliser la méthode d’authentification de serveur à serveur OAuth. Pour plus d’informations, consultez [Migration des informations d’identification JWT vers OAuth pour AEM](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration). Nous travaillons à la mise à jour de la vidéo pour refléter ce changement.


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Il existe un problème connu avec la configuration Adobe Target Cloud Services définie dans la vidéo. Dans l’attente d’une résolution, suivez les mêmes étapes de la vidéo, mais utilisez la [configuration Adobe Target Cloud Services héritée](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html?lang=fr).
