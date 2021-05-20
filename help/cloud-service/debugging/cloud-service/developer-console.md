---
title: Developer Console
description: AEM as a Cloud Service fournit une console de développement pour chaque environnement qui expose divers détails du service AEM en cours d’exécution qui sont utiles pour le débogage.
feature: Outils de développement
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: 048a37a9813e7b61ff069c4606b8d23cc6b6844f
workflow-type: tm+mt
source-wordcount: '1351'
ht-degree: 2%

---


# Débogage d’AEM en tant que Cloud Service avec Developer Console

AEM as a Cloud Service fournit une console de développement pour chaque environnement qui expose divers détails du service AEM en cours d’exécution qui sont utiles pour le débogage.

Chaque AEM en tant qu’environnement de Cloud Service possède sa propre console de développement.

## Accès à Developer Console

Pour accéder à Developer Console et l’utiliser, les autorisations suivantes doivent être accordées à Adobe ID du développeur via [Admin Console de l’Adobe](https://adminconsole.adobe.com).

1. Assurez-vous que l’organisation d’Adobe qui a affecté Cloud Manager et AEM en tant que produits de Cloud Service est principale dans le sélecteur d’organisation d’Adobe.
1. Le développeur doit être membre du __Profil de produit Développeur - Cloud Service__ du produit Cloud Manager.
   + Si cet abonnement n’existe pas, le développeur ne pourra pas se connecter à Developer Console.
1. Le développeur doit être membre du __profil de produit AEM utilisateurs__ ou __AEM administrateurs__ sur AEM Author et/ou Publish.
   + Si cette appartenance n’existe pas, les vidages [status](#status) expirent avec une erreur 401 Non autorisé.

### Dépannage de l’accès à Developer Console

#### 401 Erreur non autorisée lors du statut de dumping

![Developer Console - 401 Non autorisé](./assets/developer-console/troubleshooting__401-unauthorized.png)

Si une erreur 401 Non autorisé est signalée lors du rejet d’un statut, cela signifie que votre utilisateur n’existe pas encore avec les autorisations nécessaires dans AEM en tant que Cloud Service ou que les jetons de connexion utilisés sont invalides ou ont expiré.

Pour résoudre le problème 401 Non autorisé :

1. Assurez-vous que votre utilisateur est membre du profil de produit IMS d’Adobe approprié (administrateurs AEM ou utilisateurs AEM) pour l’instance de produit associée à Developer Console en tant qu’instance de produit Cloud Service.
   + N’oubliez pas que Developer Console accède à 2 instances de produit IMS Adobes ; AEM en tant qu’instances de produit de création et de publication Cloud Service, afin de vous assurer que les profils de produit corrects sont utilisés en fonction du niveau de service auquel l’accès est requis via Developer Console.
1. Connectez-vous à l’AEM en tant que Cloud Service (auteur ou publication) et assurez-vous que vos utilisateurs et groupes sont correctement synchronisés dans AEM.
   + Developer Console exige que vos enregistrements d’utilisateur soient créés au niveau de service AEM correspondant pour qu’il s’authentifie à ce niveau de service.
1. Effacez vos cookies de navigateur ainsi que l’état de l’application (stockage local) et connectez-vous à nouveau à Developer Console, en vous assurant que le jeton d’accès utilisé par Developer Console est correct et non expiré.

## Capsule

Les services d’auteur et de publication d’AEM en tant que Cloud Service sont constitués de plusieurs instances respectivement afin de gérer la variabilité du trafic et les mises à jour en continu sans temps d’arrêt. Ces instances sont appelées capsules. La sélection de capsule dans Developer Console définit la portée des données qui seront exposées par le biais d’autres contrôles.

![Developer Console - Capsule](./assets/developer-console/pod.png)

+ Une capsule est une instance distincte qui fait partie d’un service AEM (auteur ou publication).
+ Les capsules sont transitoires, ce qui signifie qu’AEM en tant que Cloud Service les crée et les détruit selon les besoins.
+ Seules les capsules qui font partie de l’AEM associé en tant qu’environnement de Cloud Service sont répertoriées dans le sélecteur de capsule de Developer Console de l’environnement.
+ Au bas du sélecteur de capsule, les options pratiques permettent de sélectionner des capsules par type de service :
   + Tous les auteurs
   + Tous les éditeurs
   + Toutes les instances

## État

Status (État) fournit des options permettant de générer un état d’exécution AEM spécifique dans la sortie de texte ou JSON. Developer Console fournit des informations similaires à celles de la console web OSGi du SDK AEM, avec une différence marquée que Developer Console est en lecture seule.

![Developer Console - Status](./assets/developer-console/status.png)

### Lots

Les lots répertorient tous les lots OSGi dans AEM. Cette fonctionnalité est similaire aux [Bundles OSGi du SDK d’AEM local quickstart](http://localhost:4502/system/console/bundles) à `/system/console/bundles`.

Les lots aident au débogage en :

+ Liste de tous les lots OSGi déployés vers AEM as a Service
+ Répertorier l’état de chaque lot OSGi ; y compris s’ils sont principaux ou non
+ Ajout de détails sur les dépendances non résolues qui entraînent la principale des lots OSGi

### Composants

La section Composants répertorie tous les composants OSGi d’AEM. Cette fonctionnalité est similaire à [AEM Composants OSGi du SDK local de démarrage rapide ](http://localhost:4502/system/console/components) à `/system/console/components`.

Les composants aident au débogage en :

+ Liste de tous les composants OSGi déployés sur AEM en tant que Cloud Service
+ indiquer l’état de chaque composant OSGi ; y compris s’ils sont principaux ou insatisfaits
+ Le fait de fournir des détails sur des références de service insatisfaites peut entraîner la principale des composants OSGi.
+ Liste des propriétés OSGi et de leurs valeurs liées au composant OSGi

### Configurations

Configurations répertorie toutes les configurations du composant OSGi (propriétés et valeurs OSGi). Cette fonctionnalité est similaire à [AEM Configuration Manager OSGi du SDK de démarrage rapide local ](http://localhost:4502/system/console/configMgr) à `/system/console/configMgr`.

Les configurations aident au débogage en procédant comme suit :

+ Liste des propriétés OSGi et de leurs valeurs par composant OSGi
+ Localisation et identification des propriétés mal configurées

### Index Oak

Les index Oak fournissent un vidage des noeuds définis sous `/oak:index`. Gardez à l’esprit que cela n’affiche pas les index fusionnés qui se produisent lorsqu’un index d’AEM est modifié.

Les index Oak aident au débogage en :

+ Liste de toutes les définitions d’index Oak fournissant des informations sur la manière dont les requêtes de recherche sont exécutées dans AEM. Gardez à l’esprit que les index modifiés en index AEM ne sont pas reflétés ici. Cette vue n’est utile que pour les index qui sont uniquement fournis par AEM ou uniquement fournis par le code personnalisé.

### Services OSGi

La section Composants répertorie tous les services OSGi. Cette fonctionnalité est similaire aux [Services OSGi du SDK d’AEM local quickstart ](http://localhost:4502/system/console/services) à `/system/console/services`.

Aide des services OSGi pour le débogage :

+ Répertorier tous les services OSGi d’AEM, ainsi que le bundle OSGi fourni et tous les bundles OSGi qui les utilisent

### Tâches Sling

Tâches Sling répertorie toutes les files d’attente de tâches Sling. Cette fonctionnalité est similaire aux [Tâches du démarrage rapide local du SDK AEM](http://localhost:4502/system/console/slingevent) à l’adresse `/system/console/slingevent`.

Aide sur les tâches Sling dans le débogage en :

+ Liste des files d’attente de tâches Sling et de leurs configurations
+ Fournissant des informations sur le nombre de tâches Sling principales, en file d’attente et traitées, ce qui s’avère utile pour le débogage des problèmes liés aux workflows, aux workflows transitoires et aux autres tâches effectuées par les tâches Sling dans AEM.

## Modules Java

Les packages Java permettent de vérifier si un package Java, ainsi qu’une version, sont disponibles dans AEM en tant que Cloud Service. Cette fonctionnalité est identique à [AEM l’ outil de recherche de dépendance du démarrage local du SDK d’à `/system/console/depfinder`.](http://localhost:4502/system/console/depfinder)

![Developer Console - Packages Java](./assets/developer-console/java-packages.png)

Les packages Java sont utilisés pour empêcher le démarrage des lots en raison d’importations non résolues ou de classes non résolues dans les scripts (HTL, JSP, etc.). Si les packages Java signalent qu’aucun lot n’exporte un package Java (ou que la version ne correspond pas à celle importée par un lot OSGi) :

+ Assurez-vous que la version de la dépendance Maven de l’API AEM de votre projet correspond à la version AEM de publication de l’environnement (et, si possible, mettez tout à jour vers la dernière version).
+ Si des dépendances Maven supplémentaires sont utilisées dans le projet Maven
   + Déterminez si une autre API fournie par la dépendance d’API du SDK AEM peut être utilisée à la place.
   + Si la dépendance supplémentaire est requise, assurez-vous qu’elle est fournie sous la forme d’un lot OSGi (plutôt qu’un lot Jar simple) et qu’elle est incorporée dans le module de code de votre projet (`ui.apps`), comme le lot OSGi principal est incorporé dans le module `ui.apps`.

## Servlets

Servlets permet de savoir comment AEM résout une URL en un servlet ou un script Java (HTL, JSP) qui gère finalement la requête. Cette fonctionnalité est identique à [AEM au résolveur de servlet Sling du SDK local du démarrage rapide ](http://localhost:4502/system/console/servletresolver) à l’adresse `/system/console/servletresolver`.

![Developer Console - Servlets](./assets/developer-console/servlets.png)

Les servlets permettent de déterminer :

+ Comment une URL est décomposée en parties adressables (ressource, sélecteur, extension).
+ Servlet ou script auquel une URL correspond, ce qui permet d’identifier les URL mal formées ou les servlets/scripts mal enregistrés.

## Requêtes

Les requêtes permettent d’obtenir des informations sur les requêtes de recherche exécutées sur AEM. Cette fonctionnalité est identique à la console [AEM de démarrage rapide locale du SDK Outils > Performances des requêtes ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html).

Les requêtes ne fonctionnent que lorsqu’une capsule spécifique est sélectionnée, car elle ouvre la console web Performances des requêtes de cette capsule, ce qui nécessite que le développeur ait accès au service AEM.

![Developer Console - Requêtes - Expliquer la requête](./assets/developer-console/queries__explain-query.png)

Les requêtes permettent de déboguer en :

+ Expliquer comment les requêtes sont interprétées, analysées et exécutées par Oak. Ceci est très important lors du suivi des raisons pour lesquelles une requête est lente et de la compréhension de la manière dont elle peut être accélérée.
+ Liste des requêtes les plus populaires s’exécutant dans AEM, avec la possibilité de les expliquer.
+ Liste des requêtes les plus lentes s’exécutant dans AEM, avec la possibilité de les expliquer.
