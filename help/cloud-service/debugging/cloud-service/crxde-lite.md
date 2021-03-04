---
title: CRXDE Lite
description: 'CRXDE Lite est un outil classique mais puissant pour déboguer les AEM en tant qu''environnements de développement Cloud Service. CRXDE Lite fournit une suite de fonctionnalités qui aide le débogage à éviter d’inspecter toutes les ressources et propriétés, de manipuler les parties mutables du JCR et d’étudier les autorisations. '
feature: Outils de développement
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Développement
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# Débogage des AEM en tant que Cloud Service avec CRXDE Lite

Le CRXDE Lite est __UNIQUEMENT__ disponible sur AEM en tant qu&#39;environnements de développement Cloud Service (ainsi que le SDK AEM local).

## Accès au CRXDE Lite sur l’auteur AEM

Le CRXDE Lite est __accessible uniquement__ sur l&#39;AEM en tant qu&#39;environnements de développement Cloud Service et __n&#39;est pas__ disponible sur l&#39;étape ou les environnements de production.

Pour accéder au CRXDE Lite sur AEM Author :

1. Connectez-vous à l’AEM en tant que service Auteur AEM Cloud Service.
1. Accédez à Outils > Général > CRXDE Lite.

Le CRXDE Lite s’ouvre alors à l’aide des informations d’identification et des autorisations utilisées pour se connecter à AEM Author.

## Débogage du contenu

CRXDE Lite fournit un accès direct au JCR. Le contenu visible par l’intermédiaire du CRXDE Lite est limité par les autorisations accordées à votre utilisateur, ce qui signifie que vous ne pourrez peut-être pas voir ou modifier tout ce qui se trouve dans le JCR en fonction de votre accès.

Notez que `/apps`, `/libs` et `/oak:index` sont immuables, ce qui signifie qu’ils ne peuvent pas être modifiés au moment de l’exécution par un utilisateur. Ces emplacements dans le JCR ne peuvent être modifiés que par le biais de déploiements de code.

+ La structure JCR est parcourue et manipulée à l’aide du volet de navigation de gauche.
+ La sélection d’un noeud dans le volet de navigation de gauche expose les propriétés de noeud dans le volet inférieur.
   + Les propriétés peuvent être ajoutées, supprimées ou modifiées à partir du volet
+ Doublon-clic sur un noeud de fichier dans le volet de navigation de gauche, ouvre le contenu du fichier dans le volet supérieur droit
+ Appuyez sur le bouton Enregistrer tout en haut à gauche pour conserver la modification ou sur la flèche vers le bas en regard de l’option Enregistrer tout pour annuler les modifications non enregistrées.

![CRXDE Lite - Débogage du contenu](./assets/crxde-lite/debugging-content.png)

Les modifications apportées au contenu modifiable au moment de l’exécution en AEM en tant qu’environnements de développement Cloud Service via CRXDE Lite doivent être effectuées avec précaution.
Tout changement apporté directement à AEM par l&#39;intermédiaire d&#39;un CRXDE Lite peut être difficile à suivre et à gouverner. Le cas échéant, assurez-vous que les modifications effectuées par l&#39;intermédiaire du CRXDE Lite reviennent aux packages de contenu modifiable du projet AEM (`ui.content`) et qu&#39;elles sont appliquées à Git, afin de vous assurer que le problème est résolu. Idéalement, toutes les modifications de contenu d’application proviennent de la base de code et sont acheminées vers l’AEM par le biais de déploiements, plutôt que d’apporter des modifications directement à l’AEM par l’intermédiaire du CRXDE Lite.

### Débogage des contrôles d&#39;accès

CRXDE Lite permet de tester et d’évaluer le contrôle d&#39;accès sur un noeud spécifique pour un utilisateur ou un groupe spécifique (c’est-à-dire l’entité principale).

Pour accéder à la console de Contrôle d&#39;accès de test dans le CRXDE Lite, accédez à :

+ CRXDE Lite > Outils > Contrôle d&#39;accès de test...

![CRXDE Lite - Contrôle d&#39;accès de test](./assets/crxde-lite/permissions__test-access-control.png)

1. A l’aide du champ Chemin, sélectionnez un chemin JCR à évaluer.
1. A l’aide du champ Entité principale, sélectionnez l’utilisateur ou le groupe pour évaluer le chemin par rapport à
1. Appuyez sur le bouton Tester

Les résultats s’affichent ci-dessous :

+ ____ Pathre réitère le chemin qui a été évalué
+ ____ Principal rappelle à l’utilisateur ou au groupe que le chemin d’accès a été évalué pour
+ ____ Principales répertorie toutes les entités dont fait partie l&#39;entité de sécurité sélectionnée.
   + Ceci s’avère utile pour comprendre les appartenances de groupe transitoires qui peuvent fournir des autorisations via l’héritage.
+ __Privilèges chez__ Path répertorie toutes les autorisations JCR dont dispose l&#39;entité de sécurité sélectionnée sur le chemin d&#39;accès évalué.

### Activités de débogage non prises en charge

Les activités de débogage suivantes peuvent __ne pas__ être exécutées dans le CRXDE Lite.

### Débogage des configurations OSGi

Les configurations OSGi déployées ne peuvent pas être examinées par CRXDE Lite. Les configurations OSGi sont conservées dans le package de code `ui.apps` du projet AEM à `/apps/example/config.xxx`. Toutefois, lors du déploiement vers AEM en tant qu&#39;environnements Cloud Service, les ressources de configuration OSGi ne sont pas conservées dans le JCR, donc ne sont pas visibles par CRXDE Lite.

Utilisez plutôt [Console développeur > Configurations](./developer-console.md#configurations) pour passer en revue les configurations OSGi déployées.
