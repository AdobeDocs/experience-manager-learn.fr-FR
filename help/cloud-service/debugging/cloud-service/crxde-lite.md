---
title: CRXDE Lite
description: 'CRXDE Lite est un outil classique, mais puissant, pour le débogage d’AEM en tant qu’environnements de développement de Cloud Service. CRXDE Lite fournit une suite de fonctionnalités qui aide le débogage à ne pas inspecter toutes les ressources et propriétés, à manipuler les parties modifiables du JCR et à rechercher les autorisations. '
feature: Outils de développement
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# Débogage d’AEM en tant que Cloud Service avec CRXDE Lite

CRXDE Lite est __UNIQUEMENT__ disponible sur AEM en tant qu’environnements de développement de Cloud Service (ainsi que le SDK AEM local).

## Accès au CRXDE Lite sur l’auteur AEM

CRXDE Lite est __accessible uniquement__ sur AEM en tant qu’environnements de développement de Cloud Service et __n’est pas__ disponible dans les environnements d’évaluation ou de production.

Pour accéder au CRXDE Lite sur AEM Author :

1. Connectez-vous à l’AEM en tant que service AEM Author Cloud Service.
1. Accédez à Outils > Général > CRXDE Lite .

Le CRXDE Lite s’ouvre alors à l’aide des informations d’identification et des autorisations utilisées pour se connecter à l’auteur AEM.

## Débogage du contenu

CRXDE Lite permet d’accéder directement au JCR. Le contenu visible via CRXDE Lite est limité par les autorisations accordées à votre utilisateur, ce qui signifie que vous ne pourrez peut-être pas voir ni modifier tout ce qui se trouve dans le JCR en fonction de votre accès.

Notez que `/apps`, `/libs` et `/oak:index` sont immuables, ce qui signifie qu’ils ne peuvent pas être modifiés au moment de l’exécution par un utilisateur. Ces emplacements dans le JCR ne peuvent être modifiés que par le biais de déploiements de code.

+ La structure JCR est parcourue et manipulée à l’aide du volet de navigation de gauche.
+ La sélection d’un noeud dans le volet de navigation de gauche expose la propriété du noeud dans le volet inférieur.
   + Les propriétés peuvent être ajoutées, supprimées ou modifiées dans le volet
+ Double-cliquez sur un noeud de fichier dans le volet de navigation de gauche pour ouvrir le contenu du fichier dans le volet supérieur droit.
+ Appuyez sur le bouton Enregistrer tout en haut à gauche pour conserver la modification ou sur la flèche vers le bas en regard de l’option Enregistrer tout pour annuler les modifications non enregistrées.

![CRXDE Lite - Débogage de contenu](./assets/crxde-lite/debugging-content.png)

Les modifications apportées au contenu modifiable au moment de l’exécution dans les environnements de développement d’AEM as a Cloud Service via CRXDE Lite doivent être effectuées avec précaution.
Toute modification apportée directement à AEM par le biais de CRXDE Lite peut être difficile à suivre et à gérer. Le cas échéant, assurez-vous que les modifications effectuées via CRXDE Lite reviennent aux modules de contenu modifiable du projet AEM (`ui.content`) et sont validées dans Git, afin de vous assurer que le problème est résolu. Idéalement, toutes les modifications de contenu d’application proviennent de la base de code et sont transmises à AEM via des déploiements, plutôt que d’apporter des modifications directement à l’AEM via CRXDE Lite.

### Débogage des contrôles d’accès

CRXDE Lite permet de tester et d’évaluer le contrôle d’accès sur un noeud spécifique pour un utilisateur ou un groupe spécifique (c’est-à-dire l’entité principale).

Pour accéder à la console de contrôle d’accès Test dans CRXDE Lite, accédez à :

+ CRXDE Lite > Outils > Tester le contrôle d’accès ...

![CRXDE Lite - Test du contrôle d’accès](./assets/crxde-lite/permissions__test-access-control.png)

1. À l’aide du champ Chemin d’accès , sélectionnez un chemin JCR à évaluer.
1. À l’aide du champ Entité de sécurité, sélectionnez l’utilisateur ou le groupe pour lequel évaluer le chemin d’accès.
1. Appuyez sur le bouton Test .

Les résultats s’affichent ci-dessous :

+ ____ Path réitère le chemin qui a été évalué.
+ ____ Principal réitère à l’utilisateur ou au groupe que le chemin a été évalué pour
+ ____ Principalsrépertorie toutes les entités dont fait partie l’entité sélectionnée.
   + Cela s’avère utile pour comprendre les appartenances de groupe transitoire qui peuvent fournir des autorisations via l’héritage.
+ __Privilèges au chemin__ Répertorie toutes les autorisations JCR dont dispose l’entité sélectionnée sur le chemin évalué.

### Activités de débogage non prises en charge

Vous trouverez ci-dessous des activités de débogage qui peuvent __ne pas__ être exécutées en CRXDE Lite.

### Débogage des configurations OSGi

Les configurations OSGi déployées ne peuvent pas être examinées via CRXDE Lite. Les configurations OSGi sont conservées dans le package de code `ui.apps` du projet AEM à `/apps/example/config.xxx`. Toutefois, lors du déploiement d’AEM en tant qu’environnements de Cloud Service, les ressources de configuration OSGi ne sont pas conservées dans le JCR, donc elles ne sont pas visibles par CRXDE Lite.

Utilisez plutôt [Developer Console > Configurations](./developer-console.md#configurations) pour passer en revue les configurations OSGi déployées.
