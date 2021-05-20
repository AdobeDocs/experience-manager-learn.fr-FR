---
title: Utilisation de oak-run.jar pour gérer les index
description: La commande d’index oak-run.jar regroupe plusieurs fonctionnalités permettant de gérer les index Oak dans AEM, depuis la collecte des statistiques d’index, l’exécution des contrôles de cohérence d’index et la réindexation des index eux-mêmes.
version: 6.4, 6.5
feature: Rechercher
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performances
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 3%

---


# Utilisation de oak-run.jar pour gérer les index

[!DNL oak-run.jar]La commande d’index de  [!DNL Oak] regroupe un certain nombre de fonctionnalités permettant de gérer200 index dans AEM, depuis la collecte des statistiques d’index, l’exécution des contrôles de cohérence d’index et la réindexation des index eux-mêmes.

>[!NOTE]
>
>Dans cet article et les vidéos, les termes indexation et réindexation sont interchangeables et considérés comme la même opération.

## [!DNL oak-run.jar] base des commandes d’index

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* La version de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilisée doit correspondre à la version d’Oak utilisée sur l’instance AEM.
* La gestion des index à l’aide de [!DNL oak-run.jar] utilise la commande **[!DNL index]** avec différents indicateurs pour prendre en charge différentes opérations.

   * `java -jar oak-run*.jar index ...`

## Statistiques sur les index

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` vide toutes les définitions d’index, toutes les statistiques d’index importantes et tout le contenu d’index en vue d’une analyse hors ligne.
* La collecte des statistiques d’index peut s’exécuter en toute sécurité sur les instances AEM en cours d’utilisation.

## Contrôle de cohérence de l’index

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` détermine rapidement si les index Lucene Oak sont corrompus.
* La vérification de cohérence peut être exécutée en toute sécurité sur l’instance AEM en cours d’utilisation pour les niveaux de contrôle de cohérence 1 et 2.

## Indexation TarMK en ligne avec [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* L’indexation en ligne de [!DNL TarMK] à l’aide de [!DNL oak-run.jar] est plus rapide que la définition de `reindex=true` sur le noeud `oak:queryIndexDefinition`. Malgré cette augmentation des performances, l’indexation en ligne à l’aide de [!DNL oak-run.jar] nécessite toujours une fenêtre de maintenance pour effectuer l’indexation.

* L’indexation en ligne de [!DNL TarMK] à l’aide de [!DNL oak-run.jar] doit **ne pas** être exécutée par rapport aux instances AEM en dehors de la fenêtre de maintenance des instances AEM.

## Indexation hors ligne TarMK avec oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* L’indexation hors ligne de [!DNL TarMK] à l’aide de [!DNL oak-run.jar] est l’approche d’indexation la plus simple [!DNL oak-run.jar] pour [!DNL TarMK], car elle nécessite une seule commande [!DNL oak-run.jar], mais elle nécessite l’arrêt de l’instance d’AEM.

## Indexation hors bande TarMK avec oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* L’indexation hors-bande sur [!DNL TarMK] à l’aide de [!DNL oak-run.jar] réduit l’impact de l’indexation sur les instances d’AEM en cours d’utilisation.
* L’indexation hors-bande est l’approche d’indexation recommandée pour les installations AEM où le temps de réindexation/indexation dépasse les fenêtres de maintenance disponibles.

## Indexation MongoMK Online avec oak-run.jar

* L’index en ligne avec [!DNL oak-run.jar] sur [!DNL MongoMK] et [!DNL RDBMK] est la méthode recommandée pour réindexer les installations [!DNL MongoMK] (et [!DNL RDBMK]) AEM. **Aucune autre méthode ne doit être utilisée pour  [!DNL MongoMK] ou  [!DNL RDBMK].**
* Cette indexation ne doit être exécutée que sur une seule instance AEM de la grappe.
* L’indexation en ligne de [!DNL MongoMK] peut s’exécuter en toute sécurité sur un cluster AEM en cours d’exécution, car la traversée du référentiel ne se produit que sur un seul noeud [!DNL MongoDB], ce qui permet aux autres de continuer à traiter des requêtes sans impact significatif sur les performances.

La commande d’index [!DNL oak-run.jar] pour effectuer une indexation en ligne de [!DNL MongoMK] est la [identique à l’indexation en ligne  [!DNL TarMK] avec [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar), à la différence que le paramètre de magasin de segments pointe vers l’instance [!DNL MongoDB] qui contient le magasin de noeuds.

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
* [Documentation de la commande d’index Apache Jackrabbit Oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
