---
title: Connecter AEM Sites à la propriété de balise à l’aide d’IMS
description: Découvrez comment connecter AEM Sites à la propriété de balise à l’aide de la configuration IMS dans AEM.
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
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 40%

---

# Connecter AEM Sites à la propriété de balise à l’aide d’IMS{#connect-aem-and-tag-property-using-ims}

Découvrez comment connecter l’AEM avec les balises Property à l’aide de la configuration IMS (système Identity Management) dans AEM. Cette configuration authentifie AEM avec l’API des balises et permet à AEM de communiquer via les API des balises pour accéder aux propriétés de balise.

## Créer ou réutiliser une configuration IMS

La configuration IMS qui utilise le projet Adobe Developer Console est requise pour intégrer AEM à la propriété de balise nouvellement créée. Cette configuration permet à AEM de communiquer avec l’application Balises à l’aide des API de balises et IMS gère l’aspect sécurité de cette intégration.

Chaque fois qu’un environnement AEM en tant qu’environnement Cloud Service est configuré, quelques configurations IMS telles que Asset compute, Adobe Analytics et les balises sont automatiquement créées. La création automatique **balises dans Adobe Experience Platform** La configuration IMS peut être utilisée ou une nouvelle configuration IMS doit être créée si vous utilisez AEM environnement 6.X.

Vérification créée automatiquement **balises dans Adobe Experience Platform** Configuration IMS à l’aide des étapes suivantes.

1. Dans AEM Auteur, ouvrez le **Outils** menu
1. Dans la section Sécurité, sélectionnez Configurations IMS Adobe.
1. Sélectionnez la carte **Adobe Launch** et cliquez sur **Propriétés**. Passez en revue les détails des onglets **Certificat** et **Compte**. Cliquez ensuite sur **Annuler** pour renvoyer sans modifier les détails créés automatiquement.
1. Sélectionnez la carte **Adobe Launch** et cette fois-ci, cliquez sur **Contrôle de l’intégrité**. Vous devriez voir apparaître le message **Succès** comme ci-dessous.

   ![Configuration IMS des balises saines](assets/adobe-launch-healthy-ims-config.png)

## Étapes suivantes

[Création d’une configuration de Cloud Service de balises dans AEM](create-aem-launch-cloud-service.md)
