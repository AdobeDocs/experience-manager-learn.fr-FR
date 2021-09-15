---
title: Autres outils de débogage AEM SDK
description: Divers autres outils peuvent vous aider à déboguer le démarrage rapide local du SDK AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 12%

---

# Autres outils de débogage AEM SDK

Divers autres outils peuvent vous aider à déboguer votre application sur le démarrage rapide local du SDK AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite est une interface web permettant d’interagir avec le référentiel de données JCR AEM. CRXDE Lite offre une visibilité totale sur le JCR, y compris les noeuds, les propriétés, les valeurs de propriété et les autorisations.

CRXDE Lite se trouve à l’adresse :

+ Outils > Général > CRXDE Lite
+ ou directement à [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Explication de la requête

![Expliquer la requête](./assets/other-tools/explain-query.png)

Explique l’outil Web Query dans le fichier quickstart local du SDK AEM, qui fournit des informations clés sur la manière dont AEM interprète et exécute les requêtes, ainsi qu’un outil inestimable pour s’assurer que les requêtes sont exécutées de manière performante par l’.

Expliquer la requête se trouve à l’adresse :

+ Outils > Diagnostic > Performances des requêtes > Onglet Expliquer la requête
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)  > Onglet Expliquer la requête

## Débogueur QueryBuilder

![Débogueur QueryBuilder](./assets/other-tools/query-debugger.png)

Le débogueur QueryBuilder est un outil Web qui vous aide à déboguer et à comprendre les requêtes de recherche à l’aide de la syntaxe AEM [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

QueryBuilder Debugger se trouve à l’emplacement suivant :

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
