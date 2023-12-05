---
title: Connecter AEM Sites à la propriété de balise à l’aide d’IMS
description: Découvrez comment connecter AEM Sites à la propriété de balise à l’aide de la configuration IMS dans AEM. Cette configuration authentifie AEM avec l’API Launch et permet à AEM de communiquer via les API Launch pour accéder aux propriétés de balise.
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
duration: 81
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 100%

---

# Connecter AEM Sites à la propriété de balise à l’aide d’IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>Le processus de changement de nom d’Adobe Experience Platform Launch en tant qu’ensemble de technologies de collecte de données est en cours de mise en œuvre dans l’interface utilisateur, le contenu et la documentation du produit AEM. Par conséquent, le terme Launch est toujours utilisé ici.

Découvrez comment connecter AEM avec la propriété de balise à l’aide de la configuration IMS (Identity Management System) dans AEM. Cette configuration authentifie AEM avec l’API Launch et permet à AEM de communiquer via les API Launch pour accéder aux propriétés de balise.

## Créer ou réutiliser une configuration IMS

La configuration IMS qui utilise le projet Adobe Developer Console est requise pour intégrer AEM à la propriété de balise nouvellement créée. Cette configuration permet à AEM de communiquer avec l’application des balises à l’aide des API Launch et IMS gère l’aspect sécurité de cette intégration.

Chaque fois qu’un environnement AEM as a Cloud Service est configuré, des configurations IMS telles que Asset Compute, Adobe Analytics et Adobe Launch sont automatiquement créées. Vous pouvez utiliser la configuration IMS **Adobe Launch** créée automatiquement ou créer une nouvelle configuration IMS si vous utilisez un environnement AEM 6.X.

Passez en revue la configuration IMS **Adobe Launch** créée automatiquement à l’aide des étapes suivantes.

1. Dans AEM, ouvrez le menu **Outils**.

1. Dans la section Sécurité, sélectionnez Configurations IMS Adobe.

1. Sélectionnez la carte **Adobe Launch** et cliquez sur **Propriétés**. Passez en revue les détails des onglets **Certificat** et **Compte**. Cliquez ensuite sur **Annuler** pour renvoyer sans modifier les détails créés automatiquement.

1. Sélectionnez la carte **Adobe Launch** et cette fois-ci, cliquez sur **Contrôle de l’intégrité**. Vous devriez voir apparaître le message **Succès** comme ci-dessous.

   ![Configuration IMS saine d’Adobe Launch](assets/adobe-launch-healthy-ims-config.png).


## Étapes suivantes

[Créer une configuration de service cloud Launch dans AEM](create-aem-launch-cloud-service.md)
