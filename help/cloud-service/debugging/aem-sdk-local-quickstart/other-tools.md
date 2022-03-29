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
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 5%

---

# Autres outils de débogage AEM SDK

Divers autres outils peuvent vous aider à déboguer votre application sur le démarrage rapide local du SDK AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite est une interface web permettant d’interagir avec le référentiel de données JCR AEM. CRXDE Lite offre une visibilité totale sur le JCR, y compris les noeuds, les propriétés, les valeurs de propriété et les autorisations.

CRXDE Lite se trouve à l’adresse :

+ Outils > Général > CRXDE Lite
+ ou directement à [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Débogage du contenu

CRXDE Lite permet d’accéder directement au JCR. Le contenu visible via CRXDE Lite est limité par les autorisations accordées à votre utilisateur, ce qui signifie que vous ne pourrez peut-être pas voir ni modifier tout ce qui se trouve dans le JCR en fonction de votre accès.

+ La structure JCR est parcourue et manipulée à l’aide du volet de navigation de gauche.
+ La sélection d’un noeud dans le volet de navigation de gauche expose la propriété du noeud dans le volet inférieur.
   + Les propriétés peuvent être ajoutées, supprimées ou modifiées dans le volet
+ Double-cliquez sur un noeud de fichier dans le volet de navigation de gauche pour ouvrir le contenu du fichier dans le volet supérieur droit.
+ Appuyez sur le bouton Enregistrer tout en haut à gauche pour conserver la modification ou sur la flèche vers le bas en regard de l’option Enregistrer tout pour annuler les modifications non enregistrées.

![CRXDE Lite - Débogage de contenu](./assets/other-tools/crxde-lite__debugging-content.png)

Toute modification apportée directement au SDK AEM via CRXDE Lite peut être difficile à suivre et à gérer. Le cas échéant, assurez-vous que les modifications effectuées via CRXDE Lite reviennent aux modules de contenu modifiable du projet AEM (`ui.content`) et sont engagés dans Git. Idéalement, toutes les modifications de contenu d’application proviennent de la base de code et sont transmises au SDK AEM par le biais de déploiements, plutôt que d’apporter des modifications directement au SDK AEM via CRXDE Lite.

### Débogage des contrôles d’accès

CRXDE Lite permet de tester et d’évaluer le contrôle d’accès sur un noeud spécifique pour un utilisateur ou un groupe spécifique (c’est-à-dire l’entité principale).

Pour accéder à la console de contrôle d’accès Test dans CRXDE Lite, accédez à :

+ CRXDE Lite > Outils > Tester le contrôle d’accès ...

![CRXDE Lite - Test du contrôle d’accès](./assets/other-tools/crxde-lite__test-access-control.png)

1. À l’aide du champ Chemin d’accès , sélectionnez un chemin JCR à évaluer.
1. À l’aide du champ Entité de sécurité, sélectionnez l’utilisateur ou le groupe pour lequel évaluer le chemin d’accès.
1. Appuyez sur le bouton Test .

Les résultats s’affichent ci-dessous :

+ __Chemin__ répète le chemin qui a été évalué
+ __Principal__ réitère l’utilisateur ou le groupe pour lequel le chemin a été évalué.
+ __Principaux__ répertorie toutes les entités dont fait partie l’entité sélectionnée.
   + Cela s’avère utile pour comprendre les appartenances de groupe transitoire qui peuvent fournir des autorisations via l’héritage.
+ __Privilèges au chemin__ répertorie toutes les autorisations JCR dont dispose l’entité sélectionnée sur le chemin évalué.

## Explication de la requête

![Expliquer la requête](./assets/other-tools/explain-query.png)

Explique l’outil Web Query dans le fichier quickstart local du SDK AEM, qui fournit des informations clés sur la manière dont AEM interprète et exécute les requêtes, ainsi qu’un outil inestimable pour s’assurer que les requêtes sont exécutées de manière performante par l’.

Expliquer la requête se trouve à l’adresse :

+ Outils > Diagnostic > Performances des requêtes > Onglet Expliquer la requête
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Onglet Expliquer la requête

## Débogueur QueryBuilder

![Débogueur QueryBuilder](./assets/other-tools/query-debugger.png)

Le débogueur QueryBuilder est un outil Web qui vous aide à déboguer et à comprendre les requêtes de recherche à l’aide d’AEM [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) syntaxe.

QueryBuilder Debugger se trouve à l’emplacement suivant :

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
