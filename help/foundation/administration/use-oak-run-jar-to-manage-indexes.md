---
title: Utiliser oak-run.jar pour gérer les index
description: La commande d’index oak-run.jar regroupe plusieurs fonctionnalités permettant de gérer les index Oak dans AEM, depuis la collecte des statistiques d’index, l’exécution des contrôles de cohérence d’index et la réindexation des index eux-mêmes.
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '449'
ht-degree: 100%

---

# Utiliser oak-run.jar pour gérer les index

La commande d’index [!DNL oak-run.jar] regroupe plusieurs fonctionnalités permettant de gérer 200 index [!DNL Oak] dans AEM, depuis la collecte des statistiques d’index, l’exécution des contrôles de cohérence d’index et la réindexation des index eux-mêmes.

>[!NOTE]
>
>Dans cet article et les vidéos, les termes indexation et réindexation sont interchangeables et considérés comme la même opération.

## Bases de la commande d’index [!DNL oak-run.jar]

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* La version de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilisée doit correspondre à la version d’Oak utilisée sur l’instance AEM.
* La gestion des index à l’aide de [!DNL oak-run.jar] tire parti de la commande **[!DNL index]** avec différents indicateurs pour prendre en charge différentes opérations.

   * `java -jar oak-run*.jar index ...`

## Statistiques d’index

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` vide toutes les définitions d’index, toutes les statistiques d’index importantes et tout le contenu d’index en vue d’une analyse hors ligne.
* La collecte des statistiques d’index peut s’exécuter en toute sécurité sur les instances AEM en cours d’utilisation.

## Contrôle de cohérence d’index

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` détermine rapidement si les index Lucene Oak sont corrompus.
* Il est recommandé de lancer l’exécution sur une instance AEM en cours d’utilisation pour les niveaux de contrôle de cohérence 1 et 2.

## Indexation en ligne TarMK avec [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* L’indexation en ligne de [!DNL TarMK] avec [!DNL oak-run.jar] est plus rapide que de définir `reindex=true` sur le nœud `oak:queryIndexDefinition`. Malgré cette augmentation des performances, l’indexation en ligne avec [!DNL oak-run.jar] nécessite toujours une fenêtre de maintenance pour effectuer l’indexation.

* L’indexation en ligne de [!DNL TarMK] avec [!DNL oak-run.jar] ne doit **pas** être exécutée sur des instances AEM en dehors de la fenêtre de maintenance des instances AEM.

## Indexation hors ligne TarMK avec oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* L’indexation hors ligne de [!DNL TarMK] avec [!DNL oak-run.jar] constitue la méthode d’indexation basée sur [!DNL oak-run.jar] la plus simple pour [!DNL TarMK], car elle nécessite une seule commande [!DNL oak-run.jar]. Toutefois, elle requiert l’arrêt de l’instance AEM.

## Indexation hors bande TarMK avec oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* L’indexation hors bande sur [!DNL TarMK] avec [!DNL oak-run.jar] réduit l’impact de l’indexation sur les instances AEM en cours d’utilisation.
* L’indexation hors bande est l’approche d’indexation recommandée pour les installations AEM où le temps de réindexation/indexation dépasse les fenêtres de maintenance disponibles.

## Indexation en ligne MongoMK avec oak-run.jar

* L’indexation en ligne avec [!DNL oak-run.jar] sur [!DNL MongoMK] et [!DNL RDBMK] est la méthode recommandée pour la réindexation/indexation des installations AEM [!DNL MongoMK] (et [!DNL RDBMK]). **Aucune autre méthode ne doit être utilisée pour [!DNL MongoMK] ou [!DNL RDBMK].**
* Cette indexation ne doit être exécutée que sur une seule instance AEM du cluster.
* L’indexation en ligne de [!DNL MongoMK] peut être exécutée en toute sécurité sur un cluster AEM en cours d’exécution, car la traversée du référentiel ne se produit que sur un seul nœud [!DNL MongoDB], ce qui permet aux autres nœuds de continuer à traiter des demandes sans affecter considérablement les performances.

La commande d’index [!DNL oak-run.jar] pour effectuer une indexation en ligne de [!DNL MongoMK] est [identique à l’indexation en ligne  [!DNL TarMK]  avec  [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) à la différence que le paramètre de magasin de segments pointe vers l’instance [!DNL MongoDB] qui contient le magasin de nœuds.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Documents annexes

* [Télécharger [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Assurez-vous que la version téléchargée correspond à la version d’Oak installée sur AEM, comme décrit ci-dessus.*
* [Documentation relative à la commande d’index Apache Jackrabbit Oak oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
