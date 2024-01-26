---
title: Générer des documents à l’aide de l’API Batch dans AEM Forms CS
description: Configurer et déclencher des opérations par lot spour générer des documents.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
duration: 32
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 100%

---

# Présentation

Lors d’une requête par lots, des dizaines, des centaines voire des milliers de documents similaires sont générés simultanément. Exemple : un organisme bancaire peut générer des relevés de carte de crédit et les envoyer à l’ensemble de leurs clientes et clients.
Les API Batch (API asynchrones) conviennent pour la génération planifiée à débit élevé de documents multiples. Ces API génèrent des documents par lots. Il peut s’agir, par exemple, de factures de téléphone, de relevés de carte de crédit et de relevés de prestations générés tous les mois.

Pour utiliser l’API d’opération par lots d’AEM Forms CS, vous devez effectuer les configurations suivantes :

1. configurer le compte de stockage Azure ;
1. créer une configuration cloud dans l’espace de stockage Azure ;
1. créer une configuration d’entrepôt de données par lots ;
1. lancer lʼAPI Batch.

Il est recommandé de consulter attentivement la [documentation de l’API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=fr) avant de commencer ce tutoriel.
