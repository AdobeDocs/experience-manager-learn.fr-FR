---
title: Guide de mise en oeuvre de recherche simple
description: La mise en œuvre de recherche simple est le matériel du Summit Lab AEM Search Demystified 2017. Cette page contient les matériaux de ce laboratoire. Pour une visite guidée du laboratoire, consultez le classeur du laboratoire dans la section Présentation de cette page.
version: 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 11%

---

# Guide de mise en oeuvre de recherche simple{#simple-search-implementation-guide}

L’implémentation de la recherche simple est le matériel de la **Adobe Summit de la recherche AEM démystifiée**. Cette page contient les matériaux de ce laboratoire. Pour une visite guidée du laboratoire, consultez le classeur du laboratoire dans la section Présentation de cette page.

![Présentation de l’architecture de recherche](assets/l4080/simple-search-application.png)

## Documents de présentation {#bookmarks}

* [classeur Lab](assets/l4080/l4080-lab-workbook.pdf)
* [Présentation](assets/l4080/l4080-presentation.pdf)

## Signets {#bookmarks-1}

### Outils {#tools}

* [Gestionnaire d’index](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Expliquer la requête](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [Gestionnaire de modules CRX](http://localhost:4502/crx/packmgr/index.jsp)
* [Débogueur QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Générateur de définitions d’index Oak](https://oakutils.appspot.com/generate/index)

### Chapitres {#chapters}

*Les liens de chapitre ci-dessous supposent que la variable [Packages initiaux](#initialpackages) sont installés sur AEM Author à l’adresse`http://localhost:4502`*

* [Chapitre 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [Chapitre 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [Chapitre 3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [Chapitre 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [Chapitre 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [Chapitre 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [Chapitre 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [Chapitre 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [Chapitre 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## Modules {#packages}

### Packages initiaux {#initial-packages}

* [Balises](assets/l4080/summit-tags.zip)
* [Package d’application de recherche simple](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Packages de chapitre {#chapter-packages}

* [Solution de chapitre 1](assets/l4080/l4080-chapter1.zip)
* [Solution de chapitre 2](assets/l4080/l4080-chapter2.zip)
* [Solution de chapitre 3](assets/l4080/l4080-chapter3.zip)
* [Solution de chapitre 4](assets/l4080/l4080-chapter4.zip)
* [Configuration du chapitre 5](assets/l4080/l4080-chapter5-setup.zip)
* [Solution de chapitre 5](assets/l4080/l4080-chapter5-solution.zip)
* [Solution de chapitre 6](assets/l4080/l4080-chapter6.zip)
* [Solution du chapitre 9](assets/l4080/l4080-chapter9.zip)

## Documents référencés {#reference-materials}

* [Référentiel Github](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modèles Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Exportateur de modèles Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API QueryBuilder](https://experienceleague.adobe.com/docs/?lang=fr)
* [Module externe Chrome AEM](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([Page de documentation](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Corrections et suivi {#corrections-and-follow-up}

Corrections et clarifications des discussions de laboratoire et réponses aux questions de suivi des participants.

1. **Comment arrêter la réindexation ?**

   La réindexation peut être arrêtée via le MBean IndexStats disponible via [Console web AEM > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Exécuter `abortAndPause()` pour abandonner la réindexation. L’index sera ainsi verrouillé pour effectuer une nouvelle réindexation jusqu’à ce que la fonction `resume()` est appelée.
      * Exécution `resume()` redémarre le processus d’indexation.
   * Documentation : [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Comment les index Oak peuvent-ils prendre en charge plusieurs clients ?**

   Oak prend en charge le placement d’index dans l’arborescence de contenu, et ces index ne s’indexent que dans cette sous-arborescence. Par exemple **`/content/site-a/oak:index/cqPageLucene`** peut être créé pour indexer le contenu uniquement sous **`/content/site-a`.**

   Une approche équivalente consiste à utiliser la méthode **`includePaths`** et **`queryPaths`** propriétés sur un index sous **`/oak:index`**. Par exemple :

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Cette approche doit tenir compte des points suivants :

   * Les requêtes DOIVENT spécifier une restriction de chemin égale à la portée du chemin de requête de l’index, ou y être descendant.
   * Index à portée large (par exemple `/oak:index/cqPageLucene`) va ÉGALEMENT indexer les données, ce qui entraîne l’ingestion en double et le coût d’utilisation du disque.
   * Peut nécessiter une gestion de la configuration en double (ex. l’ajout du même indexRules sur plusieurs index client s’ils doivent satisfaire les mêmes jeux de requêtes)
   * Cette approche est mieux servie sur le niveau Publication AEM pour une recherche de site personnalisée, car sur l’auteur AEM, il est courant que les requêtes soient exécutées en hauteur dans l’arborescence de contenu pour différents clients (par exemple, via OmniSearch). Différentes définitions d’index peuvent entraîner un comportement différent uniquement en fonction de la restriction de chemin d’accès.


3. **Où est la liste de tous les analyseurs disponibles ?**

   Oak expose un ensemble d’éléments de configuration de l’analyseur Lucene-fournit à utiliser dans AEM.

   * [Documentation Apache Oak Analytics](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtres](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Comment rechercher des pages et des ressources dans la même requête ?**

   La nouvelle version d’AEM 6.3 est la possibilité de rechercher plusieurs types de noeuds dans la même requête fournie. Requête QueryBuilder suivante. Notez que chaque &quot;sous-requête&quot; peut se résoudre par son propre index. Par conséquent, dans cet exemple, la variable `cq:Page` la sous-requête est résolue sur `/oak:index/cqPageLucene` et le `dam:Asset` la sous-requête est résolue sur `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   génère le plan de requête et de requête suivant :

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Explorer la requête et les résultats via [Débogueur QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) et [Module externe Chrome AEM](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **Comment effectuer une recherche sur plusieurs chemins dans la même requête ?**

   La nouvelle version d’AEM 6.3 est la possibilité d’effectuer des requêtes sur plusieurs chemins dans la même requête fournie. Requête QueryBuilder suivante. Notez que chaque &quot;sous-requête&quot; peut se résoudre par son propre index.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   entraîne le plan de requête et de requête suivant :

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Explorer la requête et les résultats via [Débogueur QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) et [Module externe Chrome AEM](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
