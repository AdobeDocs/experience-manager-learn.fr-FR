---
title: Guide de mise en œuvre de la recherche simple
description: La mise en œuvre de recherche simple est le thème du Summit Lab AEM Search Demystified 2017. Cette page contient les éléments de ce Lab. Pour une visite guidée du Lab, consultez le classeur du Lab dans la section Présentation de cette page.
version: 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 182
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '627'
ht-degree: 100%

---

# Guide de mise en œuvre de la recherche simple{#simple-search-implementation-guide}

La mise en œuvre de recherche simple est le thème du **Lab Adobe Summit AEM Search Demystified**. Cette page contient les éléments de ce Lab. Pour une visite guidée du Lab, consultez le classeur du Lab dans la section Présentation de cette page.

![Vue d’ensemble de l’architecture de recherche.](assets/l4080/simple-search-application.png)

## Documents de présentation {#bookmarks}

* [Classeur Lab](assets/l4080/l4080-lab-workbook.pdf)
* [Présentation](assets/l4080/l4080-presentation.pdf)

## Signets {#bookmarks-1}

### Outils {#tools}

* [Gestionnaire d’index](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Expliquer la requête](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [Gestionnaire de packages CRX](http://localhost:4502/crx/packmgr/index.jsp)
* [Débogueur QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Générateur de définitions d’index Oak](https://oakutils.appspot.com/generate/index)

### Chapitres {#chapters}

*Les liens de chapitre ci-dessous supposent que les [Packages initiaux](#initialpackages) sont installés sur l’instance de création d’AEM sur`http://localhost:4502`*.

* [Chapitre 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [Chapitre 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [Chapitre 3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [Chapitre 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [Chapitre 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [Chapitre 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [Chapitre 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [Chapitre 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [Chapitre 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## Packages {#packages}

### Packages initiaux {#initial-packages}

* [Balises](assets/l4080/summit-tags.zip)
* [Package d’application de recherche simple](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Chapitre Packages {#chapter-packages}

* [Chapitre 1 Solution](assets/l4080/l4080-chapter1.zip)
* [Chapitre 2 Solution](assets/l4080/l4080-chapter2.zip)
* [Chapitre 3 Solution](assets/l4080/l4080-chapter3.zip)
* [Chapitre 4 Solution](assets/l4080/l4080-chapter4.zip)
* [Chapitre 5 Configuration](assets/l4080/l4080-chapter5-setup.zip)
* [Chapitre 5 Solution](assets/l4080/l4080-chapter5-solution.zip)
* [Chapitre 6 Solution](assets/l4080/l4080-chapter6.zip)
* [Chapitre 9 Solution](assets/l4080/l4080-chapter9.zip)

## Documents référencés {#reference-materials}

* [Référentiel Github](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modèles Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Exporteur de modèles Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API QueryBuilder](https://experienceleague.adobe.com/docs/?lang=fr)
* [Module externe Chrome AEM](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([Page de documentation](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Corrections et suivi {#corrections-and-follow-up}

Corrections et clarifications des discussions sur le Lab et réponses aux questions de suivi des participantes et des participants.

1. **Comment arrêter la réindexation ?**

   La réindexation peut être arrêtée via le MBean IndexStats disponible sur la [Console web AEM > JMX](http://localhost:4502/system/console/jmx).

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Exécutez `abortAndPause()` pour abandonner la réindexation. L’index sera ainsi verrouillé pour effectuer une nouvelle réindexation jusqu’à ce que la fonction `resume()` soit appelée.
      * L’exécution de `resume()` redémarre le processus d’indexation.
   * Documentation : [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean).

2. **Comment les index Oak peuvent-ils prendre en charge plusieurs clients ?**

   Oak prend en charge le placement d’index dans l’arborescence de contenu, et ces index ne s’indexent que dans cette sous-arborescence. Par exemple, **`/content/site-a/oak:index/cqPageLucene`** peut être créé pour indexer le contenu uniquement sous **`/content/site-a`.**

   Une approche équivalente consiste à utiliser les propriétés **`includePaths`** et **`queryPaths`** sur un index sous **`/oak:index`**. Par exemple :

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Cette approche doit tenir compte des points suivants :

   * Les requêtes DOIVENT spécifier une restriction de chemin égale à la portée du chemin de requête de l’index, ou en être descendant.
   * Les index au périmètre large (par exemple, `/oak:index/cqPageLucene`) vont AUSSI indexer les données, ce qui entraîne l’ingestion en double et le coût d’utilisation du disque.
   * Cela peut nécessiter une gestion de la configuration en double (par exemple, l’ajout des mêmes indexRules sur plusieurs index clients s’ils doivent satisfaire les mêmes jeux de requêtes).
   * Cette approche est mieux adaptée au niveau de publication d’AEM pour une recherche de site personnalisée, car sur le niveau de création d’AEM, il est courant que les requêtes soient exécutées dans le haut de l’arborescence de contenu pour différents clients (par exemple, via OmniSearch). Différentes définitions d’index peuvent entraîner un comportement différent uniquement en fonction de la restriction de chemin d’accès.

3. **Où est la liste de tous les analyseurs disponibles ?**

   Oak expose un ensemble d’éléments de configuration de l’analyseur Lucene fourni pour une utilisation dans AEM.

   * [Documentation sur les analyseurs Apache Oak](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtres](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Comment rechercher des pages et des ressources dans la même requête ?**

   La nouvelle version d’AEM 6.3 inclut la possibilité de rechercher plusieurs types de nœuds dans la même requête fournie. Requête QueryBuilder suivante. Notez que chaque « sous-requête » peut se résoudre par son propre index. Par conséquent, dans cet exemple, la `cq:Page`sous-requête est résolue sur `/oak:index/cqPageLucene` et la sous-requête `dam:Asset` est résolue sur `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   Cela génère la requête et le plan de requête suivants :

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Explorez la requête et les résultats via le [Débogueur QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) et le [Module externe Chrome AEM](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=fr-FR).

5. **Comment effectuer une recherche sur plusieurs chemins dans la même requête ?**

   La nouvelle version d’AEM 6.3 inclut la possibilité d’effectuer des requêtes sur plusieurs chemins dans la même requête fournie. Requête QueryBuilder suivante. Notez que chaque « sous-requête » peut se résoudre par son propre index.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   Cela génère la requête et le plan de requête suivants :

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Explorez la requête et les résultats via le [Débogueur QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) et le [Module Chrome AEM](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=fr-FR).
