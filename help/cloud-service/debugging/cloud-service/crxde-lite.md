---
title: CRXDE Lite
description: CRXDE Lite est un outil classique mais puissant pour déboguer les environnements de développement AEM as a Cloud Service. CRXDE Lite offre une suite de fonctionnalités qui facilitent le débogage en inspectant toutes les ressources et propriétés, en manipulant les parties modifiables du JCR et en recherchant les autorisations.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 168
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 100%

---

# Déboguer AEM as a Cloud Service avec CRXDE Lite

CRXDE Lite est __UNIQUEMENT__ disponible sur les environnements de développement AEM as a Cloud Service (ainsi que sur le SDK AEM local).

## Accéder à CRXDE Lite sur l’instance de création AEM

CRXDE Lite est __uniquement__ accessible sur les environnements de développement AEM as a Cloud Service et n’est __pas__ disponible dans les environnements d’évaluation ou de production.

Pour accéder à CRXDE Lite sur l’instance de création AEM :

1. Connectez-vous au service de création AEM as a Cloud Service.
1. Accédez à Outils > Général > CRXDE Lite.

CRXDE Lite s’ouvre alors à l’aide des informations d’identification et des autorisations utilisées pour se connecter à l’instance de création AEM.

## Déboguer le contenu

CRXDE Lite permet d’accéder directement au JCR. Le contenu visible via CRXDE Lite est limité par les autorisations accordées à votre utilisateur ou utilisatrice, ce qui signifie que vous ne pourrez peut-être pas voir ni modifier tout ce qui se trouve dans le JCR en fonction de votre accès.

Notez que `/apps`, `/libs` et `/oak:index` ne sont pas modifiables, et ne peuvent donc être changés au moment de l’exécution par personne. Ces emplacements dans le JCR ne peuvent être modifiés que par le biais de déploiements de code.

+ Pour parcourir et modifier la structure JCR, utilisez le volet de navigation de gauche.
+ La sélection d’un nœud dans le volet de navigation de gauche expose la propriété du nœud dans le volet inférieur.
   + Les propriétés peuvent être ajoutées, supprimées ou modifiées dans le volet.
+ Double-cliquez sur un nœud de fichier dans le volet de navigation de gauche pour ouvrir le contenu du fichier dans le volet supérieur droit.
+ Appuyez sur le bouton Enregistrer tout en haut à gauche pour conserver la modification ou sur la flèche vers le bas en regard de l’option Enregistrer tout pour Annuler les modifications non enregistrées.

![CRXDE Lite - Débogage de contenu.](./assets/crxde-lite/debugging-content.png)

Les modifications apportées au contenu modifiable au moment de l’exécution dans les environnements de développement AEM as a Cloud Service via CRXDE Lite doivent être effectuées avec précaution.
Toute modification apportée directement à AEM par le biais de CRXDE Lite peut s’avérer difficile à suivre et à gérer. Le cas échéant, assurez-vous que les modifications effectuées via CRXDE Lite reviennent aux packages de contenu modifiable du projet AEM (`ui.content`) et sont validées dans Git, afin de garantir que le problème est résolu. Idéalement, toutes les modifications de contenu d’application proviennent de la base de code et sont transmises à AEM via des déploiements, plutôt que d’apporter des modifications directement à AEM via CRXDE Lite.

### Déboguer les contrôles d’accès

CRXDE Lite permet de tester et d’évaluer le contrôle d’accès sur un nœud spécifique pour un utilisateur ou une utilisatrice ou un groupe spécifique (c’est-à-dire l’entité principale).

Pour accéder à la console de test du contrôle d’accès dans CRXDE Lite, accédez à :

+ CRXDE Lite > Outils > Tester le contrôle d’accès...

![CRXDE Lite - Test du contrôle d’accès.](./assets/crxde-lite/permissions__test-access-control.png)

1. À l’aide du champ Chemin, sélectionnez un chemin JCR à évaluer.
1. À l’aide du champ Entité principale, sélectionnez l’utilisateur ou l’utilisatrice ou le groupe pour en évaluer le chemin.
1. Appuyez sur le bouton Tester.

Les résultats s’affichent ci-dessous :

+ __Chemin__ répète le chemin qui a été évalué.
+ __Entité principale__ réitère l’utilisateur ou l’utilisatrice ou le groupe pour lequel le chemin a été évalué.
+ __Entités principales__ répertorie toutes les entités principales dont fait partie l’entité principale sélectionnée.
   + Cela s’avère utile pour comprendre les appartenances de groupe transitoires qui peuvent fournir des autorisations via l’héritage.
+ __Privilèges au chemin__ répertorie toutes les autorisations JCR dont dispose l’entité principale sélectionnée sur le chemin évalué.

### Activités de débogage non prises en charge

Vous trouverez ci-dessous les activités de débogage ne pouvant __pas__ être exécutées sur CRXDE Lite.

### Déboguer des configurations OSGi

Les configurations OSGi déployées ne peuvent pas être examinées via CRXDE Lite. Les configurations OSGi sont conservées dans le package de code `ui.apps` du projet AEM sur `/apps/example/config.xxx`. Toutefois, lors d’un déploiement dans les environnements AEM as a Cloud Service, les ressources de configuration OSGi ne sont pas conservées dans le JCR. Elles ne sont donc pas visibles via CRXDE Lite.

Utilisez plutôt [Developer Console > Configurations](./developer-console.md#configurations) pour examiner les configurations OSGi déployées.
