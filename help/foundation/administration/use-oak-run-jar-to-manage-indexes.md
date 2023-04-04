---
title: Utilisation de oak-run.jar pour gérer les index
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
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 4%

---

# Utilisation de oak-run.jar pour gérer les index

[!DNL oak-run.jar]La commande d’index de &quot;regroupe un certain nombre de fonctionnalités à gérer [!DNL Oak]200 index dans AEM, depuis la collecte des statistiques d’index, l’exécution des contrôles de cohérence d’index et la réindexation des index eux-mêmes.

>[!NOTE]
>
>Dans cet article et les vidéos, les termes indexation et réindexation sont interchangeables et considérés comme la même opération.

## [!DNL oak-run.jar] base des commandes d’index

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* La version de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilisé doit correspondre à la version d’Oak utilisée sur l’instance AEM.
* Gestion des index à l’aide de [!DNL oak-run.jar] tire parti de **[!DNL index]** avec différents indicateurs pour prendre en charge différentes opérations.

   * `java -jar oak-run*.jar index ...`

## Statistiques d’index

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` vide toutes les définitions d’index, toutes les statistiques d’index importantes et tout le contenu d’index en vue d’une analyse hors ligne.
* La collecte des statistiques d’index peut s’exécuter en toute sécurité sur les instances AEM en cours d’utilisation.

## Contrôle de cohérence de l’index

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` détermine rapidement si les index Lucene Oak sont corrompus.
* La vérification de cohérence peut être exécutée en toute sécurité sur l’instance AEM en cours d’utilisation pour les niveaux de contrôle de cohérence 1 et 2.

## Indexation TarMK en ligne avec [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Indexation en ligne de [!DNL TarMK] using [!DNL oak-run.jar] est plus rapide que le paramètre `reindex=true` sur le `oak:queryIndexDefinition` noeud . Malgré cette augmentation des performances, l’indexation en ligne à l’aide de [!DNL oak-run.jar] nécessite toujours une fenêtre de maintenance pour effectuer l’indexation.

* Indexation en ligne de [!DNL TarMK] using [!DNL oak-run.jar] should **not** être exécutée par rapport aux instances AEM en dehors de la fenêtre de maintenance des instances AEM.

## Indexation hors ligne TarMK avec oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Indexation hors ligne de [!DNL TarMK] using [!DNL oak-run.jar] est le plus simple [!DNL oak-run.jar] méthode d’indexation basée sur pour [!DNL TarMK] car il nécessite une seule [!DNL oak-run.jar] , mais il nécessite l’arrêt de l’instance AEM.

## Indexation hors bande TarMK avec oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* Indexation hors bande activée [!DNL TarMK] using [!DNL oak-run.jar] réduit l’impact de l’indexation sur les instances d’AEM en cours d’utilisation.
* L’indexation hors-bande est l’approche d’indexation recommandée pour les installations AEM où le temps de réindexation/indexation dépasse les fenêtres de maintenance disponibles.

## Indexation MongoMK Online avec oak-run.jar

* Index en ligne avec [!DNL oak-run.jar] on [!DNL MongoMK] et [!DNL RDBMK] est la méthode recommandée pour la réindexation/l’indexation. [!DNL MongoMK] (et [!DNL RDBMK]) AEM les installations. **Aucune autre méthode ne doit être utilisée pour [!DNL MongoMK] ou [!DNL RDBMK].**
* Cette indexation ne doit être exécutée que sur une seule instance AEM de la grappe.
* Indexation en ligne de [!DNL MongoMK] est sécurisé pour s’exécuter sur un cluster AEM en cours d’exécution, car la traversée du référentiel ne se produit que sur une seule [!DNL MongoDB] , ce qui permet aux autres utilisateurs de continuer à traiter des demandes sans affecter considérablement les performances.

Le [!DNL oak-run.jar] commande d’index pour effectuer une indexation en ligne de [!DNL MongoMK] est la valeur [identique au [!DNL TarMK] Indexation en ligne avec [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) à la différence que le paramètre de magasin de segments pointe vers la variable [!DNL MongoDB] qui contient le magasin de noeuds.

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
