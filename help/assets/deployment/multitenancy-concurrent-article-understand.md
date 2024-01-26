---
title: Présentation de l’architecture multi-client et du développement simultané
description: Découvrez les avantages, les défis et les techniques liés à la gestion d’une implémentation multi-client avec Adobe Experience Manager Assets.
feature: Connected Assets
version: 6.5
topic: Development
role: Developer
level: Intermediate
doc-type: Article
exl-id: c9ee29d4-a8a5-4e61-bc99-498674887da5
duration: 524
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2022'
ht-degree: 100%

---

# Présentation de l’architecture multi-client et du développement simultané {#understanding-multitenancy-and-concurrent-development}

## Présentation {#introduction}

Lorsque plusieurs équipes déploient leur code dans les mêmes environnements AEM, elles doivent suivre certaines pratiques pour s’assurer que les équipes peuvent travailler aussi indépendamment que possible, sans marcher sur les plates-bandes des autres équipes. Bien qu’elles ne puissent jamais être entièrement éliminées, ces techniques minimiseront les dépendances entre équipes. Pour qu’un modèle de développement simultané réussisse, une bonne communication entre les équipes de développement est essentielle.

En outre, lorsque plusieurs équipes de développement travaillent sur le même environnement AEM, il y a probablement un aspect multi-client impliqué. Beaucoup de choses ont été écrites sur les considérations pratiques à prendre en charge l’architecture multi-client dans un environnement AEM, en particulier sur les défis à relever dans la gestion de la gouvernance, des opérations et du développement. Cet article explore certains des défis techniques liés à l’implémentation d’AEM dans un environnement multi-client, mais bon nombre de ces recommandations s’appliquent à toute organisation comptant plusieurs équipes de développement.

Il est important de noter que, bien qu’AEM puisse prendre en charge plusieurs sites et même plusieurs marques déployées sur un seul environnement, elle ne propose pas de véritable architecture multi-client. Certaines configurations d’environnement et ressources système seront toujours partagées sur tous les sites déployés sur un environnement. Le présent article fournit des conseils pour minimiser l’impact de ces ressources partagées et offre des suggestions pour rationaliser la communication et la collaboration dans ces domaines.

## Avantages et défis {#benefits-and-challenges}

L’implémentation d’un environnement multi-client présente de nombreux défis.

Ces informations comprennent les éléments suivants :

* Complexité technique supplémentaire
* Augmentation des frais de développement
* Dépendances inter-organisations sur les ressources partagées
* Complexité opérationnelle accrue

Malgré les difficultés rencontrées, l’exécution d’une application multi-client présente des avantages, tels que les éléments suivants :

* Réduction des coûts matériels
* Réduction du temps de mise sur le marché pour les futurs sites
* Réduction des coûts d’implémentation pour les futurs clients
* Architecture standard et pratiques de développement dans l’entreprise
* Une base de code commune

Si l’entreprise nécessite une vraie architecture multi-client, sans connaissance préalable des autres clients et sans code ni contenu partagé, sans auteurs et autrices en commun, les instances de création séparées sont la seule option viable. L’augmentation globale de l’effort de développement doit être comparée aux économies réalisées en termes d’infrastructures et de coûts de licence afin de déterminer si cette approche est la plus adaptée.

## Techniques de développement {#development-techniques}

### Gérer les dépendances {#managing-dependencies}

Lors de la gestion des dépendances de projet Maven, il est important que toutes les équipes utilisent la même version d’un lot OSGi donné sur le serveur. Pour illustrer ce qui peut mal se passer lorsque les projets Maven sont mal gérés, nous présentons un exemple :

Le projet A dépend de la version 1.0 de la bibliothèque foo ; la version foo 1.0 est incorporée dans leur déploiement sur le serveur. Le projet B dépend de la version 1.1 de la bibliothèque foo ; la version foo 1.1 est incorporée dans leur déploiement.

En outre, supposons qu’une API ait changé dans cette bibliothèque entre les versions 1.0 et 1.1. À ce stade, l’un de ces deux projets ne fonctionnera plus correctement.

Pour résoudre ce problème, nous vous recommandons de créer tous les projets Maven enfants d’un seul projet de réacteur parent. Ce projet de réacteur a deux objectifs : il permet de créer et de déployer tous les projets ensemble si nécessaire, et il contient les déclarations de dépendance pour tous les projets enfants. Le projet parent définit les dépendances et leurs versions, tandis que les projets enfants ne déclarent que les dépendances dont ils ont besoin, héritant de la version du projet parent.

Dans ce scénario, si l’équipe qui travaille sur le projet B nécessite des fonctionnalités dans la version 1.1 de foo, il devient rapidement évident dans l’environnement de développement que cette modification va perturber le projet A. Les équipes peuvent discuter de cette modification et rendre le projet A compatible avec la nouvelle version ou rechercher une autre solution pour le projet B.

Notez que cela n’élimine pas la nécessité pour ces équipes de partager cette dépendance. Cela met en évidence les problèmes rapidement afin que les équipes puissent discuter des risques éventuels et s’entendre sur une solution.

### Prévenir la duplication du code {#preventing-code-duplication-nbsp-br}

Lorsque vous travaillez sur plusieurs projets, il est important de s’assurer que le code n’est pas dupliqué. La duplication de code augmente la probabilité d’occurrences de défauts, le coût des modifications du système et la rigidité globale dans la base de code. Pour éviter la duplication, refactorisez la logique commune en bibliothèques réutilisables pouvant être utilisées dans plusieurs projets.

Pour répondre à ce besoin, nous recommandons le développement et la maintenance d’un projet principal dont toutes les équipes peuvent dépendre et auquel elles peuvent contribuer. Pour ce faire, il est important de s’assurer que ce projet principal ne dépend à son tour d’aucun des projets de chaque équipe, ce qui permet un déploiement indépendant tout en encourageant la réutilisation du code.

Voici quelques exemples de code qui se trouvent généralement dans un module principal :

* Configurations à l’échelle du système, telles que :
   * Configurations OSGi
   * Filtres de servlet
   * Mappages ResourceResolver
   * Pipelines Sling Transformer
   * Gestionnaires d’erreurs (ou utilisez ACS AEM Commons Error Page Handler1)
   * Servlets d’autorisation pour la mise en cache sensible aux autorisations
* Classes d’utilitaires
* Logique commerciale principale
* Logique d’intégration tierce
* Recouvrements de l’interface utilisateur de création
* Autres personnalisations obligatoires pour la création, telles que les widgets personnalisés
* Lanceurs de workflow
* Éléments de conception courants utilisés sur plusieurs sites

*Architecture de projet modulaire*

Cela n’élimine pas la nécessité pour plusieurs équipes de dépendre et potentiellement de s’occuper de la mise à jour du même ensemble de code. En créant un projet principal, nous avons réduit la taille de la base de code qui est partagée entre les équipes et ainsi diminué, mais sans l’éliminer, le besoin de ressources partagées.

Pour garantir que les modifications apportées à ce package principal ne perturbent pas les fonctionnalités du système, nous recommandons qu’un développeur ou une développeuse senior ou une équipe de développement soit chargée de la supervision. Vous pouvez disposer d’une équipe unique qui gère toutes les modifications apportées à ce package ou demander aux équipes d’envoyer des demandes d’extraction qui sont examinées et fusionnées par ces ressources. Il est important qu’un modèle de gouvernance soit conçu et accepté par les équipes et que les développeurs et développeuses le suivent.

## Gérer la portée du déploiement {#managing-deployment-scope}

Lorsque différentes équipes déploient leur code dans le même référentiel, il est important que l’une ne remplace pas les modifications d’une autre. AEM dispose d’un mécanisme pour contrôler cela lors du déploiement des packages de contenu : le fichier xml de filtre. Il est important qu’il n’y ait pas de chevauchement entre les fichiers. xml de filtre, sinon le déploiement d’une équipe risque d’effacer le déploiement précédent d’une autre équipe. Pour illustrer ce point, voici des exemples de fichiers de filtre bien conçus, et d’autres problématiques :

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Si chaque équipe configure explicitement son fichier de filtre en fonction du ou des sites sur lesquels elle travaille, chacune d’elles peut déployer ses composants, ses bibliothèques clientes et ses conceptions de site indépendamment sans effacer les modifications de l’autre.

Puisqu’il s’agit d’un chemin d’accès global au système et qu’il n’est pas spécifique à un site, le servlet suivant doit être inclus dans le projet principal, car les modifications apportées ici peuvent avoir un impact potentiel sur n’importe quelle équipe :

/apps/sling/servlet/errorhandler

### Recouvrements {#overlays}

Les recouvrements sont fréquemment utilisés pour étendre ou remplacer la fonctionnalité d’AEM prête à l’emploi, mais l’utilisation d’un recouvrement affecte l’ensemble de l’application AEM (c’est-à-dire que toute modification des fonctionnalités recouvertes est disponible pour tous les clients). Cela serait plus compliqué si les clients avaient des exigences différentes pour le recouvrement. Idéalement, les groupes organisationnels doivent collaborer pour s’entendre sur la fonctionnalité et l’apparence des consoles administratives AEM.

Si un accord ne peut être trouvé entre les différentes unités organisationnelles, une solution possible serait simplement de ne pas utiliser de recouvrements. Créez plutôt une copie personnalisée de la fonctionnalité et exposez-la via un chemin différent pour chaque client. Cela permet à chaque client d’avoir une expérience client complètement différente, mais cette approche augmente le coût de mise en œuvre et les efforts de mise à niveau ultérieurs.

### Lanceurs de workflow {#workflow-launchers}

AEM utilise des lanceurs de workflow pour déclencher automatiquement l’exécution du workflow lorsque des modifications spécifiées sont apportées au référentiel. AEM offre plusieurs lanceurs prêts à l’emploi, par exemple pour exécuter des processus de génération de rendu et d’extraction de métadonnées sur des ressources nouvelles et mises à jour. Bien qu’il soit possible de laisser ces lanceurs tels quels, dans un environnement multi-client, si les clients ont des exigences de lanceur et/ou de modèle de workflow différentes, il est probable que des lanceurs individuels devront être créés et gérés pour chaque client. Ces lanceurs devront être configurés pour s’exécuter sur les mises à jour de leur client tout en laissant intact le contenu d’autres clients. Pour réaliser facilement cette opération, appliquez des lanceurs aux chemins de référentiel spécifiés spécifiques au client.

### URL de redirection {#vanity-urls}

AEM fournit une fonctionnalité d’URL de redirection pouvant être définie par page. Dans un scénario multi-client, cette approche pose problème : AEM ne garantit pas l’unicité entre les URL de redirection configurées de cette manière. Si deux personnes différentes configurent le même chemin de redirection pour différentes pages, un comportement inattendu peut être rencontré. Pour cette raison, nous vous recommandons d’utiliser les règles mod_rewrite dans les instances de Dispatcher Apache. Cela permet ainsi de disposer d’un point central de configuration, en plus des règles de résolveur de ressource de sortie uniquement.

### Groupes de composants {#component-groups}

Lors du développement de composants et de modèles pour plusieurs groupes de création, il est important d’utiliser efficacement les propriétés componentGroup et allowedPaths. En les exploitant efficacement avec les conceptions de site, nous pouvons nous assurer que les auteurs et autrices de la marque A ne voient que les composants et les modèles créés pour leur site, tandis que les auteurs et autrices de la marque B ne voient que les leurs.

### Tests {#testing}

Bien qu’une bonne architecture et des canaux de communication ouverts puissent contribuer à empêcher l’introduction de défauts dans des zones inattendues du site, ces approches ne sont pas infaillibles. C’est pourquoi il est important de tester entièrement ce qui est déployé sur la plateforme avant de publier quoi que ce soit en production. Cela nécessite une coordination entre les équipes sur leurs cycles de publication et renforce la nécessité d’une suite de tests automatisés couvrant autant de fonctionnalités que possible. En outre, comme un système est partagé par plusieurs équipes, les performances, la sécurité et les tests de charge deviennent plus importants que jamais.

## Remarques opérationnelles {#operational-considerations}

### Ressources partagées {#shared-resources}

AEM s’exécute dans un seul JVM ; toutes les applications AEM déployées partagent intrinsèquement des ressources les unes avec les autres, en plus des ressources déjà consommées lors de l’exécution normale d’AEM. Dans l’espace JVM lui-même, il n’existe aucune séparation logique des threads, et les ressources limitées disponibles pour AEM, telles que la mémoire, le processeur et l’E/S du disque, sont également partagées. Tout client qui consomme des ressources affectera inévitablement d’autres clients du système.

### Performance {#performance}

Si vous ne suivez pas les bonnes pratiques AEM, il est possible de développer des applications qui consomment des ressources au-delà de ce qui est considéré comme normal. Par exemple, cela peut déclencher de nombreuses opérations de workflow lourdes (telles que la mise à jour de ressources dans la gestion des ressources numériques), l’utilisation d’opérations Push-on-modify MSM sur de nombreux nœuds ou l’utilisation de requêtes JCR coûteuses pour le rendu du contenu en temps réel. Cela aura inévitablement un impact sur les performances d’autres applications de clients.

### Journalisation {#logging}

AEM fournit des interfaces prêtes à l’emploi pour une configuration de journalisation robuste qui peut être utilisée à notre avantage dans les scénarios de développement partagé. En spécifiant des enregistreurs distincts pour chaque marque, par nom de package, nous pouvons obtenir un certain degré de séparation des journaux. Bien que les opérations à l’échelle du système telles que la réplication et l’authentification soient toujours consignées à un emplacement central, le code personnalisé non partagé peut être consigné séparément, ce qui facilite la surveillance et le débogage pour l’équipe technique de chaque marque.

### Sauvegarde et restauration {#backup-and-restore}

En raison de la nature du référentiel JCR, les sauvegardes traditionnelles fonctionnent sur l’ensemble du référentiel, plutôt que sur un chemin de contenu individuel. Il n’est donc pas possible de séparer facilement les sauvegardes par client. À l’inverse, la restauration à partir d’une sauvegarde permet de restaurer le contenu et les nœuds du référentiel pour tous les clients du système. Bien qu’il soit possible d’effectuer des sauvegardes de contenu ciblées à l’aide d’outils tels que VLT ou de sélectionner du contenu à restaurer en créant un package dans un environnement distinct, ces\
approches ne prennent pas facilement en compte les paramètres de configuration ou la logique de l’application et peuvent être fastidieuses à gérer.

## Remarques relatives à la sécurité {#security-considerations}

### ACL {#acls}

Il est bien sûr possible d’utiliser des listes de contrôle d’accès (ACL) pour contrôler qui a accès à l’affichage, la création et la suppression de contenu en fonction des chemins de contenu, ce qui nécessite la création et la gestion des groupes d’utilisateurs et d’utilisatrices. La difficulté à gérer les listes de contrôle d’accès et les groupes dépend de l’importance accordée au fait que chaque client n’a aucune connaissance des autres et du fait que les applications déployées s’appuient sur des ressources partagées. Pour assurer l’efficacité de l’administration des listes de contrôle d’accès, des utilisateurs et utilisatrices et des groupes, nous vous recommandons de disposer d’un groupe centralisé avec la supervision nécessaire pour vous assurer que ces contrôles d’accès et entités principales se chevauchent (ou ne se chevauchent pas) d’une manière qui favorise l’efficacité et la sécurité.
