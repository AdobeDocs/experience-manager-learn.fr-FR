---
title: Génération de documents à l’aide de l’API de lot dans AEM Forms CS
description: Configuration et déclenchement d’opérations par lots pour générer des documents.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 24%

---

# Présentation

Une demande par lot est l’endroit où des dizaines, des centaines ou des milliers de documents similaires sont générés simultanément. Exemple : Une société financière peut générer des relevés de carte de crédit à envoyer à tous ses clients.
Les API Batch (API asynchrones) conviennent pour la génération planifiée à débit élevé de documents multiples. Ces API génèrent des documents par lots. Il peut s’agir, par exemple, de factures de téléphone, de relevés de carte de crédit et de relevés de prestations générés tous les mois.

Pour utiliser l’API d’opération par lots AEM Forms CS, les configurations suivantes sont nécessaires

1. Configuration du compte de stockage Azure
1. Création d’une configuration cloud hébergée par l’espace de stockage Azure
1. Création d’une configuration d’entrepôt de données par lots
1. Exécution de l’API Batch

Il est recommandé de se familiariser avec le [Documentation des API](https://experienceleague.corp.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) avant de continuer à utiliser ce tutoriel.




