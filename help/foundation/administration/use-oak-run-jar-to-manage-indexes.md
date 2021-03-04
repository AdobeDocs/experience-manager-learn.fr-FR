---
title: Utiliser le fichier oak-run.jar pour gérer les index
description: la commande index de oak-run.jar consolide un certain nombre de fonctionnalités pour gérer les index Oak dans AEM, depuis la collecte des statistiques d'index, l'exécution de vérifications de cohérence d'index et la ré-indexation des index eux-mêmes.
version: 6.4, 6.5
feature: chêne
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 3%

---


# Utiliser le fichier oak-run.jar pour gérer les index

[!DNL oak-run.jar]La commande index de consolide un certain nombre de fonctionnalités pour gérer  [!DNL Oak]200 index dans AEM, depuis la collecte des statistiques d&#39;index, l&#39;exécution de vérifications de cohérence d&#39;index et la réindexation des index eux-mêmes.

>[!NOTE]
>
>Dans cet article et les vidéos, les termes indexation et réindexation sont interchangeables et considérés comme la même opération.

## [!DNL oak-run.jar] base des commandes d&#39;index

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* La version de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilisée doit correspondre à la version de Oak utilisée sur l’instance AEM.
* La gestion des index à l&#39;aide de [!DNL oak-run.jar] utilise la commande **[!DNL index]** avec divers indicateurs pour prendre en charge différentes opérations.

   * `java -jar oak-run*.jar index ...`

## Statistiques sur les index

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` vide toutes les définitions d’index, toutes les statistiques d’index importantes et tout le contenu d’index en vue d’une analyse hors ligne.
* La collecte des statistiques d’index peut s’exécuter en toute sécurité sur les instances AEM en cours d’utilisation.

## Vérification de la cohérence de l&#39;index

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` détermine rapidement si les index lucene Oak sont corrompus.
* La vérification de cohérence peut être exécutée sur l’instance AEM en cours d’utilisation pour les niveaux de contrôle de cohérence 1 et 2.

## Indexation en ligne de TarMK avec [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* L&#39;indexation en ligne de [!DNL TarMK] en utilisant [!DNL oak-run.jar] est plus rapide que la définition de `reindex=true` sur le noeud `oak:queryIndexDefinition`. Malgré cette augmentation des performances, l&#39;indexation en ligne à l&#39;aide de [!DNL oak-run.jar] nécessite toujours une fenêtre de maintenance pour effectuer l&#39;indexation.

* L&#39;indexation en ligne de [!DNL TarMK] à l&#39;aide de [!DNL oak-run.jar] doit **ne pas** être exécutée par rapport aux instances AEM en dehors de la fenêtre de maintenance des instances AEM.

## TarMK Indexation hors ligne avec oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* L&#39;indexation hors ligne de [!DNL TarMK] à l&#39;aide de [!DNL oak-run.jar] est l&#39;approche d&#39;indexation basée sur [!DNL oak-run.jar] la plus simple pour [!DNL TarMK] car elle requiert une seule commande [!DNL oak-run.jar], mais elle nécessite l&#39;arrêt de l&#39;instance AEM.

## Indexation hors bande TarMK avec oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* L&#39;indexation hors bande sur [!DNL TarMK] en utilisant [!DNL oak-run.jar] réduit l&#39;impact de l&#39;indexation sur les instances AEM en cours d&#39;utilisation.
* L&#39;indexation hors bande est la méthode d&#39;indexation recommandée pour les installations AEM où le temps de réindexation dépasse les fenêtres de maintenance disponibles.

## MongoMK Indexation en ligne avec oak-run.jar

* L&#39;index en ligne avec [!DNL oak-run.jar] sur [!DNL MongoMK] et [!DNL RDBMK] est la méthode recommandée pour réindexer/indexer [!DNL MongoMK] (et [!DNL RDBMK]) les installations AEM. **Aucune autre méthode ne doit être utilisée pour  [!DNL MongoMK] ou  [!DNL RDBMK].**
* Cette indexation ne doit être exécutée que sur une seule instance AEM de la grappe.
* L&#39;indexation en ligne de [!DNL MongoMK] peut s&#39;exécuter en toute sécurité sur un cluster AEM en cours d&#39;exécution, car la traversée du référentiel se fera sur un seul noeud [!DNL MongoDB], ce qui permettra aux autres utilisateurs de continuer à traiter les requêtes sans impact significatif sur les performances.

La commande d&#39;index [!DNL oak-run.jar] permettant d&#39;indexer en ligne [!DNL MongoMK] est la [même que l&#39; [!DNL TarMK] indexation en ligne avec  [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar), à la différence que le paramètre de magasin de segments pointe vers l&#39;instance [!DNL MongoDB] qui contient le magasin de noeuds.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Documents de support

* [Télécharger [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Assurez-vous que la version téléchargée correspond à la version d’Oak installée sur AEM, comme décrit ci-dessus.*
* [Documentation de la commande Apache Jackrabbit Oak oak-run.jar Index](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
