---
title: Comprendre la multiplicité et le développement simultané
description: Découvrez les avantages, les défis et les techniques liés à la gestion d’une mise en oeuvre multi-clients avec les ressources Adobe Experience Manager.
feature: Ressources connectées
version: 6.5
topic: Développement
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2024'
ht-degree: 1%

---


# Comprendre le facteur multiplicateur et le développement simultané {#understanding-multitenancy-and-concurrent-development}

## Présentation {#introduction}

Lorsque plusieurs équipes déploient leur code vers les mêmes environnements AEM, il existe des pratiques qu’elles devraient suivre pour s’assurer que les équipes peuvent travailler de manière aussi indépendante que possible, sans avoir à marcher sur les orteils des autres équipes. Bien qu&#39;elles ne puissent jamais être entièrement éliminées, ces techniques minimiseront les dépendances entre équipes. Pour qu&#39;un modèle de développement simultané réussisse, une bonne communication entre les équipes de développement est essentielle.

De plus, lorsque plusieurs équipes de développement travaillent sur le même environnement AEM, il y a probablement un certain degré de multitenacité en jeu. Beaucoup de choses ont été écrites sur les considérations pratiques de la tentative d&#39;aider plusieurs locataires dans un environnement AEM, en particulier sur les défis à relever dans la gestion de la gouvernance, des opérations et du développement. Le présent article examine certains des défis techniques liés à la mise en oeuvre de l&#39;AEM dans un environnement à clients multiples, mais bon nombre de ces recommandations s&#39;appliqueront à toute organisation comptant plusieurs équipes de développement.

Il est important de noter d&#39;emblée que si AEM peut prendre en charge plusieurs sites et même plusieurs marques déployées sur un seul environnement, cela n&#39;offre pas la véritable multi-bail. Certaines configurations d&#39;environnement et ressources système seront toujours partagées sur tous les sites déployés sur un environnement. Le présent document fournit des orientations pour réduire au minimum l&#39;impact de ces ressources partagées et des suggestions d&#39;offres afin de rationaliser la communication et la collaboration dans ces domaines.

## Avantages et défis {#benefits-and-challenges}

La mise en oeuvre d&#39;un environnement à clients multiples pose de nombreux défis.

Celles-ci comprennent :

* Complexité technique supplémentaire
* Augmentation des frais de développement
* Dépendances inter-organisations sur les ressources partagées
* Complexité opérationnelle accrue

En dépit des difficultés rencontrées, l’exécution d’une application multi-locataire présente des avantages, tels que :

* Réduction des coûts matériels
* Réduction du temps de mise sur le marché des sites futurs
* Réduction des coûts de mise en oeuvre pour les futurs locataires
* Architecture standard et pratiques de développement dans l&#39;ensemble de l&#39;entreprise
* Un code de base commun

Si l’entreprise a besoin d’un véritable multi-bail, sans aucune connaissance des autres locataires et sans code partagé, contenu ou auteurs communs, alors les instances d’auteur distinctes sont la seule option viable. L&#39;augmentation globale de l&#39;effort de développement doit être comparée aux économies réalisées dans les coûts d&#39;infrastructure et de licence pour déterminer si cette approche est la meilleure.

## Techniques de développement {#development-techniques}

### Gestion des dépendances {#managing-dependencies}

Lors de la gestion des dépendances de projet Maven, il est important que toutes les équipes utilisent la même version d&#39;un lot OSGi donné sur le serveur. Pour illustrer ce qui peut mal se passer lorsque les projets Maven sont mal gérés, nous présentons un exemple :

Le projet A dépend de la version 1.0 de la bibliothèque, foo ; foo version 1.0 est intégré dans leur déploiement sur le serveur. Le projet B dépend de la version 1.1 de la bibliothèque, foo ; foo version 1.1 est intégré dans leur déploiement.

En outre, supposons qu’une API ait changé dans cette bibliothèque entre les versions 1.0 et 1.1. À ce stade, l’un de ces deux projets ne fonctionnera plus correctement.

Pour répondre à cette préoccupation, nous recommandons de faire de tous les projets Maven des enfants d&#39;un seul projet de réacteur parent. Ce projet de réacteur a deux objectifs : il permet la construction et le déploiement de tous les projets ensemble si nécessaire, et il contient les déclarations de dépendance pour tous les projets enfants. Le projet parent définit les dépendances et leurs versions tandis que les projets enfants déclarent uniquement les dépendances dont ils ont besoin, héritant de la version du projet parent.

Dans ce scénario, si l&#39;équipe qui travaille sur le projet B a besoin de fonctionnalités dans la version 1.1 de foo, il deviendra rapidement évident dans l&#39;environnement de développement que ce changement va rompre le projet A. À ce stade, les équipes peuvent discuter de ce changement et soit rendre le projet A compatible avec la nouvelle version, soit chercher une autre solution pour le projet B.

Notez que cela n&#39;élimine pas la nécessité pour ces équipes de partager cette dépendance-il met simplement en évidence les problèmes rapidement et rapidement afin que les équipes puissent discuter des risques éventuels et s&#39;entendre sur une solution.

### Prévention de la duplication de code {#preventing-code-duplication-nbsp-br}

Lorsque vous travaillez sur plusieurs projets, il est important de s’assurer que le code n’est pas dupliqué. La duplication de code augmente la probabilité d&#39;occurrences de défaut, le coût des modifications apportées au système et la rigidité générale de la base de code. Pour éviter la duplication, refondez la logique commune en bibliothèques réutilisables qui peuvent être utilisées dans plusieurs projets.

Pour répondre à ce besoin, nous recommandons l&#39;élaboration et la maintenance d&#39;un projet de base dont toutes les équipes peuvent dépendre et y contribuer. Il est important de veiller à ce que ce projet de base ne soit pas, à son tour, tributaire de projets individuels des équipes ; cela permet un déploiement indépendant tout en continuant à promouvoir la réutilisation du code.

Voici quelques exemples de code couramment cités dans un module noyau :

* Configurations à l&#39;échelle du système, telles que :
   * Configurations OSGi
   * Filtres de servlet
   * Mappages ResourceResolver
   * Gazoducs Sling Transformer
   * Gestionnaires d&#39;erreurs (ou utilisez ACS AEM Commons Error Page Handler1)
   * Serlets d’autorisation pour la mise en cache sensible aux autorisations
* Classes d&#39;utilitaires
* Logique métier de base
* Logique d’intégration tierce
* Création d’incrustations d’interface utilisateur
* Autres personnalisations requises pour la création, telles que les widgets personnalisés
* Lancements de processus
* Éléments de conception courants utilisés sur plusieurs sites

*Architecture de projet modulaire*

Cela n’élimine pas la nécessité pour plusieurs équipes de dépendre et potentiellement de mettre à jour le même jeu de code. En créant un projet de base, nous avons réduit la taille du code de base qui est partagé entre les équipes, ce qui diminue mais n&#39;élimine pas le besoin de ressources partagées.

Pour s&#39;assurer que les modifications apportées à ce paquet principal ne perturbent pas la fonctionnalité du système, nous recommandons qu&#39;un développeur ou une équipe de développeurs de haut niveau maintiennent la surveillance. Une option consiste à disposer d&#39;une équipe unique qui gère toutes les modifications apportées à ce package ; une autre est de demander aux équipes de soumettre des demandes d&#39;appel qui sont examinées et fusionnées par ces ressources. Il est important qu&#39;un modèle de gouvernance soit conçu et accepté par les équipes et que les développeurs le suivent.

## Gestion de l&#39;étendue du déploiement&amp;nbsp {#managing-deployment-scope}

Comme différentes équipes déploient leur code dans le même référentiel, il est important qu’elles ne remplacent pas les modifications de l’autre. AEM dispose d’un mécanisme de contrôle lors du déploiement des packages de contenu, le filtre. fichier xml. Il est important qu’il n’y ait pas de chevauchement entre les filtres.  fichiers xml, sinon le déploiement d’une équipe risque d’effacer le déploiement précédent d’une autre équipe. Pour illustrer ce point, reportez-vous aux exemples suivants de fichiers de filtre bien conçus ou problématiques :

/apps/my-société vs. /apps/my-société/my-site

/etc/clientlibs/my-société vs. /etc/clientlibs/my-société/my-site

/etc/designs/my-société vs. /etc/designs/my-société/my-site

Si chaque équipe configure explicitement son fichier de filtrage sur le ou les sites sur lesquels elle travaille, elle peut déployer ses composants, ses bibliothèques clientes et ses conceptions de site indépendamment, sans effacer les modifications des autres.

Puisqu&#39;il s&#39;agit d&#39;un chemin d&#39;accès global au système et non spécifique à un site, la servlet suivante doit être incluse dans le projet principal, car les modifications apportées ici peuvent avoir un impact sur toute équipe :

/apps/sling/servlet/errorhandler

### Recouvrements {#overlays}

Les incrustations sont fréquemment utilisées pour étendre ou remplacer la fonctionnalité AEM hors de la zone, mais l’utilisation d’une incrustation affecte l’ensemble de l’application AEM (c’est-à-dire que tout changement de fonctionnalité sera disponible pour tous les locataires). Cela serait encore plus compliqué si les locataires avaient des exigences différentes pour le recouvrement. Idéalement, les groupes d’entreprises devraient travailler ensemble pour s’entendre sur la fonctionnalité et l’apparence des consoles administratives de AEM.

Si un consensus ne peut être atteint entre les différentes unités opérationnelles, une solution possible serait simplement de ne pas utiliser de superpositions. Au lieu de cela, créez une copie personnalisée de la fonctionnalité et exposez-la via un chemin différent pour chaque client. Cela permet à chaque client d’avoir une expérience utilisateur complètement différente, mais cette approche augmente également le coût de mise en oeuvre et les efforts de mise à niveau ultérieurs.

### Lanceurs de workflow {#workflow-launchers}

AEM utilise des lanceurs de processus pour déclencher automatiquement l&#39;exécution du processus lorsque des modifications spécifiées sont apportées dans le référentiel. AEM fournit plusieurs lanceurs prêts à l’emploi, par exemple, pour exécuter des processus de génération de rendu et d’extraction des métadonnées sur des ressources nouvelles et mises à jour. Bien qu&#39;il soit possible de laisser ces lanceurs en l&#39;état, dans un environnement à plusieurs locataires, si les locataires ont des exigences de lanceur et/ou de modèle de processus différentes, il est probable que des lanceurs individuels devront être créés et entretenus pour chaque locataire. Ces lanceurs devront être configurés pour s’exécuter sur les mises à jour de leurs locataires tout en ne modifiant pas le contenu des autres locataires. Pour ce faire, appliquez facilement des lanceurs à des chemins d&#39;accès de référentiel spécifiques au client.

### URL de redirection vers un microsite  {#vanity-urls}

AEM fournit des fonctionnalités d’URL de redirection qui peuvent être définies sur la base d’une page. Dans un scénario à clients multiples, cette approche soulève la préoccupation suivante : AEM ne garantit pas l’unicité entre les URL vanity configurées de cette manière. Si deux utilisateurs différents configurent le même chemin d’accès à la vanité pour des pages différentes, un comportement inattendu peut être rencontré. Pour cette raison, nous vous recommandons d’utiliser les règles mod_rewrite dans les instances du répartiteur Apache, qui permettent un point central de configuration en parallèle avec les règles de résolution de ressources sortantes uniquement.

### Groupes de composants {#component-groups}

Lors du développement de composants et de modèles pour plusieurs groupes de création, il est important d’utiliser efficacement les propriétés componentGroup et allowedPaths. En exploitant ces éléments de manière efficace avec les conceptions de site, nous pouvons nous assurer que les auteurs de la marque A ne voient que les composants et les modèles créés pour leur site, tandis que les auteurs de la marque B ne voient que les leurs.

### Tests {#testing}

Bien qu&#39;une bonne architecture et des canaux de communication ouverts puissent aider à prévenir l&#39;introduction de défauts dans des zones inattendues du site, ces approches ne sont pas infaillibles. Pour cette raison, il est important de tester entièrement ce qui est déployé sur la plate-forme avant de lancer quoi que ce soit dans la production. Cela nécessite une coordination entre les équipes sur leurs cycles de publication et renforce la nécessité d&#39;une suite de tests automatisés couvrant le plus de fonctionnalités possible. En outre, comme un système sera partagé par plusieurs équipes, les performances, la sécurité et les tests de charge deviennent plus importants que jamais.

## Considérations opérationnelles {#operational-considerations}

### Ressources partagées {#shared-resources}

AEM s’exécute dans une seule JVM ; toutes les applications AEM déployées partagent intrinsèquement des ressources les unes avec les autres, en plus des ressources déjà consommées dans le fonctionnement normal des AEM. Dans l’espace JVM lui-même, il n’y aura pas de séparation logique des threads et les ressources finies disponibles pour AEM, telles que la mémoire, l’UC et les entrées/sorties de disque, seront également partagées. Tout locataire qui consomme des ressources affectera inévitablement d&#39;autres locataires du système.

### Performances {#performance}

Si vous ne suivez pas AEM bonnes pratiques, il est possible de développer des applications qui consomment des ressources au-delà de ce qui est considéré comme normal. Par exemple, vous déclenchez de nombreuses opérations de flux de travaux intensifs (telles que DAM Update Asset), utilisez des opérations Push-on-modify MSM sur de nombreux noeuds ou utilisez des requêtes JCR coûteuses pour générer du contenu en temps réel. Elles auront inévitablement un impact sur les performances des autres applications locataires.

### Journalisation {#logging}

AEM fournit des interfaces prêtes à l&#39;emploi pour une configuration de journalisation robuste qui peut être utilisée à notre avantage dans les scénarios de développement partagés. En spécifiant des journaux distincts pour chaque marque, par nom de paquet, nous pouvons obtenir un certain degré de séparation des journaux. Bien que les opérations à l’échelle du système, telles que la réplication et l’authentification, soient toujours consignées à un emplacement central, le code personnalisé non partagé peut être consigné séparément, ce qui facilite la surveillance et le débogage de l’équipe technique de chaque marque.

### Sauvegarde et restauration {#backup-and-restore}

En raison de la nature du référentiel JCR, les sauvegardes traditionnelles fonctionnent sur l’ensemble du référentiel, plutôt que sur un chemin de contenu individuel. Il n&#39;est donc pas possible de séparer facilement les sauvegardes par client. Inversement, la restauration à partir d’une sauvegarde annulera le contenu et les noeuds de référentiel pour tous les locataires du système. Bien qu’il soit possible d’effectuer des sauvegardes de contenu ciblées, à l’aide d’outils tels que VLT, ou de sélectionner du contenu à restaurer en créant un package dans un environnement distinct, ces options\
les approches ne prennent pas facilement en compte les paramètres de configuration ou la logique d’application et peuvent être difficiles à gérer.

## Considérations relatives à la sécurité {#security-considerations}

### Listes ACL {#acls}

Il est bien sûr possible d&#39;utiliser des Listes de Contrôle d&#39;accès (ACL) pour contrôler qui a accès à la vue, créer et supprimer du contenu en fonction des chemins d&#39;accès au contenu, ce qui nécessite la création et la gestion de groupes d&#39;utilisateurs. La difficulté de maintenir les listes de contrôle d&#39;accès et les groupes dépend de l&#39;importance accordée à la garantie que chaque locataire n&#39;a aucune connaissance des autres et de la nécessité pour les applications déployées de compter sur des ressources partagées. Pour assurer une administration efficace des listes de contrôle d&#39;accès, des utilisateurs et des groupes, nous recommandons la mise en place d&#39;un groupe centralisé avec la supervision nécessaire pour nous assurer que ces contrôles d&#39;accès et entités se chevauchent (ou ne se chevauchent pas) d&#39;une manière qui favorise l&#39;efficacité et la sécurité.
