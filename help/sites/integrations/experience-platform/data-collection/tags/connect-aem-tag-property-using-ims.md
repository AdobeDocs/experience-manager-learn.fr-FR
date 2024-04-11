---
title: Connecter AEM Sites à la propriété de balise à l’aide d’IMS
description: Découvrez comment connecter AEM Sites à la propriété de balise à l’aide de la configuration IMS dans AEM.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 72
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: ht
source-wordcount: '263'
ht-degree: 100%

---

# Connecter AEM Sites à la propriété de balise à l’aide d’IMS{#connect-aem-and-tag-property-using-ims}

Découvrez comment connecter AEM à la propriété de balises à l’aide de la configuration IMS (Identity Management System) dans AEM. Cette configuration authentifie AEM avec l’API de balises et permet à AEM de communiquer via les API de balises pour accéder aux propriétés de balise.

## Créer ou réutiliser une configuration IMS

La configuration IMS qui utilise le projet Adobe Developer Console est requise pour intégrer AEM à la propriété de balise nouvellement créée. Cette configuration permet à AEM de communiquer avec l’application des balises à l’aide des API de balises et IMS gère l’aspect sécurité de cette intégration.

Chaque fois qu’un environnement AEM as a Cloud Service est configuré, des configurations IMS telles que Asset Compute, Adobe Analytics et des balises sont automatiquement créées. Vous pouvez utiliser la configuration IMS de **balises dans Adobe Experience Platform** créée automatiquement ou créer une nouvelle configuration IMS si vous utilisez un environnement AEM 6.X.

Passez en revue la configuration IMS de **balises dans Adobe Experience Platform** créée automatiquement à l’aide des étapes suivantes.

1. Dans l’instance de création AEM, ouvrez le menu **Outils**.
1. Dans la section Sécurité, sélectionnez Configurations IMS Adobe.
1. Sélectionnez la carte **Adobe Launch** et cliquez sur **Propriétés**. Passez en revue les détails des onglets **Certificat** et **Compte**. Cliquez ensuite sur **Annuler** pour renvoyer sans modifier les détails créés automatiquement.
1. Sélectionnez la carte **Adobe Launch** et cette fois-ci, cliquez sur **Contrôle de l’intégrité**. Vous devriez voir apparaître le message **Succès** comme ci-dessous.

   ![Configuration IMS saine de balises](assets/adobe-launch-healthy-ims-config.png)

## Étapes suivantes

[Créer une configuration du service cloud de balises dans AEM](create-aem-launch-cloud-service.md)
