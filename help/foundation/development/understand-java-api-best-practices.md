---
title: Présentation des bonnes pratiques relatives à l’API Java dans AEM
description: AEM repose sur une riche pile de logiciels open source qui expose de nombreuses API Java à utiliser lors du développement. Cet article explore les principales API et explique quand et pourquoi elles doivent être utilisées.
version: 6.2, 6.3, 6.4, 6.5
sub-product: foundation, ressources, sites
feature: les API ;
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2025'
ht-degree: 8%

---


# Présentation des bonnes pratiques relatives à l’API Java

Adobe Experience Manager (AEM) repose sur une riche pile de logiciels open source qui expose de nombreuses API Java à utiliser pendant le développement. Cet article explore les principales API et explique quand et pourquoi elles doivent être utilisées.

AEM est basé sur 4 ensembles d’API Java Principaux.

* **Adobe Experience Manager (AEM)**

   * abstractions de produits telles que pages, ressources, workflows, etc.

* **[!DNL Apache Sling]Structure web**

   * abstractions REST et basées sur des ressources telles que les ressources, les mappages de valeurs et les requêtes HTTP.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * Extraits de données et de contenu tels que le noeud, les propriétés et les sessions.

* **[!DNL OSGi (Apache Felix)]**

   * abstractions du conteneur d’applications OSGi telles que les composants de services et (OSGi).

## Préférence API Java &quot;règle de base&quot;

La règle générale est de préférer les API/abstractions dans l’ordre suivant :

1. **AEM **
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

Si une API est fournie par AEM, préférez-la à [!DNL Sling], JCR et OSGi. Si AEM ne fournit pas d’API, préférez [!DNL Sling] à JCR et OSGi.

Cet ordre est une règle générale, ce qui signifie qu&#39;il existe des exceptions. Les raisons possibles de rompre avec cette règle sont les suivantes :

* Exceptions connues, comme décrit ci-dessous.
* La fonctionnalité requise n’est pas disponible dans une API de niveau supérieur.
* Fonctionner dans le contexte du code existant (code de produit personnalisé ou AEM) qui utilise lui-même une API moins prisée, et le coût de déplacement vers la nouvelle API est injustifiable.

   * Il est préférable d’utiliser systématiquement l’API de niveau inférieur plutôt que de créer un mélange.

## API AEM

* [**AEM JavaDocs de l’API**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

Les API d’AEM fournissent des abstractions et des fonctionnalités spécifiques aux cas d’utilisation productifs.

Par exemple, AEM API [PageManager](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) et [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) fournissent des abstractions pour les noeuds `cq:Page` dans AEM qui représentent des pages web.

Bien que ces noeuds soient disponibles via les API [!DNL Sling] en tant que ressources et les API JCR en tant que noeuds, les API AEM fournissent des abstractions pour les cas d’utilisation courants. L’utilisation des API d’AEM permet d’assurer un comportement cohérent entre AEM le produit, ainsi que des personnalisations et des extensions à .

### com.adobe.* par rapport à com.day.* API

AEM API ont une préférence intra-package, identifiée par les packages Java suivants, par ordre de préférence :

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` prend en charge les cas d’utilisation de produits, tandis que  `com.adobe.granite` prend en charge les cas d’utilisation de plateformes inter-produits, tels que les workflows ou les tâches (qui sont utilisés dans plusieurs produits : AEM Assets, Sites, etc.).

`com.day.cq` contient des API &quot;originales&quot;. Ces API traitent des abstractions et fonctionnalités de base qui existaient avant et/ou autour de l’acquisition de [!DNL Day CQ] par l’Adobe. Ces API sont prises en charge et ne doivent pas être évitées, sauf si `com.adobe.cq` ou `com.adobe.granite` fournissent une alternative (plus récente).

De nouvelles abstractions telles que [!DNL Content Fragments] et [!DNL Experience Fragments] sont construites dans l’espace `com.adobe.cq` au lieu de `com.day.cq` décrit ci-dessous.

### API de requête

AEM prend en charge plusieurs langages de requête. Les 3 langues principales sont [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath et [AEM Query Builder](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

La préoccupation la plus importante est de conserver un langage de requête cohérent dans l’ensemble de la base de code, afin de réduire la complexité et les coûts à comprendre.

Tous les langages de requête ont effectivement les mêmes profils de performances, car [!DNL Apache Oak] les transpiles en JCR-SQL2 pour l’exécution de la requête finale, et le temps de conversion en JCR-SQL2 est négligeable par rapport au temps de requête lui-même.

L’API préférée est [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), qui est l’abstraction de niveau supérieur et fournit une API robuste pour créer, exécuter et récupérer les résultats des requêtes. Elle fournit les éléments suivants :

* Construction de requêtes simple et paramétrée (paramètres de requête modélisés en tant que carte)
* [API Java et API HTTP natives](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [Débogueur de requêtes OOTB](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) Prédicats prêts à l’emploi prenant en charge les exigences de requête courantes

* API extensible, permettant le développement de prédicats de requête [personnalisés ](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* Les API JCR-SQL2 et XPath peuvent être exécutées directement via [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) et [JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html), renvoyant respectivement les résultats des noeuds [[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) ou [JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html).

>[!CAUTION]
>
>AEM l’API QueryBuilder fuit un objet ResourceResolver. Pour atténuer cette fuite, suivez cet [exemple de code](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).


## [!DNL Sling]les API ;

* [**Documentation Java sur  [!DNL Sling] l’API Apache**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apache est la structure web RESTful qui sous-tend AEM. [!DNL Sling] fournit le routage des requêtes HTTP, modélise les noeuds JCR en tant que ressources, fournit un contexte de sécurité, etc.

[!DNL Sling] Les API ont l’avantage supplémentaire d’être créées pour l’extension, ce qui signifie qu’il est souvent plus facile et plus sûr d’augmenter le comportement des applications créées à l’aide d’ [!DNL Sling] API que les API JCR moins extensibles.

### Utilisations courantes des API [!DNL Sling]

* Accès aux noeuds JCR en tant que [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) et accès à leurs données via [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fournir un contexte de sécurité via [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Création et suppression de ressources via les [méthodes de création/déplacement/copie/suppression de ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Mise à jour des propriétés via [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Création de blocs de création de traitement des demandes

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtres de servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Blocs de création de traitement de travail asynchrone

   * [Gestionnaires d’événements et de tâches](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planificateur](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modèles Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Utilisateurs du service](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## API JCR

* **[JavaDocs JCR 2.0](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Les API [JCR (Java Content Repository) 2.0](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) font partie d’une spécification pour les implémentations JCR (dans le cas d’AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Toute implémentation JCR doit se conformer à ces API et les mettre en oeuvre. Il s’agit donc de l’API de niveau le plus bas pour interagir avec le contenu AEM.

Le JCR lui-même est une AEM de banque de données NoSQL hiérarchique/basée sur une arborescence qui utilise comme référentiel de contenu. Le JCR dispose d’une vaste gamme d’API prises en charge, allant du CRUD de contenu à l’interrogation de contenu. Malgré cette API robuste, il est rare qu’elles soient préférées aux AEM de niveau supérieur et aux abstractions [!DNL Sling].

Préférez toujours les API JCR aux API Apache Jackrabbit Oak. Les API JCR servent à ***interagir*** avec un référentiel JCR, tandis que les API Oak servent à ***implémenter*** un référentiel JCR.

### Erreurs courantes à propos des API JCR

Bien que JCR soit AEM référentiel de contenu, ses API ne sont PAS la méthode préférée pour interagir avec le contenu. À la place, préférez les API AEM (Page, Ressources, Balise, etc.) ou des API de ressources Sling, car elles fournissent de meilleures abstractions.

>[!CAUTION]
>
>L’utilisation étendue des interfaces Session et Node des API JCR dans une application AEM est une utilisation inodore du code. Assurez-vous que les API [!DNL Sling] ne doivent pas être utilisées à la place.

### Utilisations courantes des API JCR

* [Gestion du contrôle d&#39;accès](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [Gestion autorisable (utilisateurs/groupes)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observation JCR (écoute des événements JCR)
* Création de structures de noeuds profonds

   * Bien que les API Sling prennent en charge la création de ressources, les API JCR disposent de méthodes pratiques dans [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) et [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) qui accélèrent la création de structures profondes.

## API OSGi

* [**JavaDocs OSGi R6**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[Documentation JavaDocs sur les annotations des composants OSGi Declarative Services 1.2](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[Documentation JavaDocs sur les annotations de métadonnées OSGi Declarative Services 1.2](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**Documents Java sur la structure OSGi**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Il existe peu de chevauchement entre les API OSGi et les API de niveau supérieur (AEM, [!DNL Sling] et JCR), et la nécessité d’utiliser les API OSGi est rare et nécessite un haut niveau d’AEM expertise en développement.

### API OSGi et Apache Felix

OSGi définit une spécification que tous les conteneurs OSGi doivent implémenter et respecter. AEM mise en oeuvre OSGi, Apache Felix, fournit également plusieurs de ses propres API.

* Préférez les API OSGi (`org.osgi`) par rapport aux API Apache Felix (`org.apache.felix`).

### Utilisations courantes des API OSGi

* Annotations OSGi pour la déclaration des services et composants OSGi.

   * Préférez les [Annotations OSGi Declarative Services (DS) 1.2](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) par [Annotations Felix SCR](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) pour la déclaration des services et composants OSGi.

* API OSGi pour l’annulation/l’enregistrement dynamique des services/composants OSGi dans le code [OSGi](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Préférez l’utilisation des annotations OSGi DS 1.2 lorsque la gestion conditionnelle des services/composants OSGi n’est pas nécessaire (ce qui est le plus souvent le cas).

## Exceptions à la règle

Vous trouverez ci-dessous des exceptions courantes aux règles définies ci-dessus.

### AEM API Asset

* Préférez [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) par [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Bien que les API Assets `com.day.cq` fournissent des outils supplémentaires pour les cas d’utilisation de la gestion des ressources AEM.
   * Les API de ressources Granite prennent en charge les cas d’utilisation de la gestion des ressources de bas niveau (version, relations).

### API de requête

* AEM QueryBuilder ne prend pas en charge certaines fonctions de requête telles que [suggestions](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), les indices de vérification orthographique et d’index parmi d’autres fonctions moins courantes. Il est préférable d’effectuer des requêtes avec ces fonctions JCR-SQL2.

### [!DNL Sling] Enregistrement de servlet  {#sling-servlet-registration}

* [!DNL Sling] enregistrement de servlet, préférez les annotations  [OSGi DS 1.2 avec @](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover  `@SlingServlet`

### [!DNL Sling] Enregistrement de filtre  {#sling-filter-registration}

* [!DNL Sling] enregistrement de filtre, préférez les annotations  [OSGi DS 1.2 avec @](https://sling.apache.org/documentation/the-sling-engine/filters.html) SlingServletFilterover  `@SlingFilter`

## Fragments de code utiles

Vous trouverez ci-dessous des fragments de code Java utiles qui illustrent les bonnes pratiques pour les cas d’utilisation courants à l’aide des API discutées. Ces fragments illustrent également comment passer d’API moins prisées à des API plus préférées.

### Session JCR à [!DNL Sling] ResourceResolver

#### Sling ResourceResolver à fermeture automatique

Depuis AEM 6.2, le ResourceResolver est `AutoClosable` dans une instruction [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). [!DNL Sling] En utilisant cette syntaxe, un appel explicite à `resourceResolver .close()` n’est pas nécessaire.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### Sling ResourceResolver fermé manuellement

ResourceResolvers peut être fermé manuellement dans un bloc `finally` si la technique de fermeture automatique illustrée ci-dessus ne peut pas être utilisée.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### Chemin d’accès JCR à [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Noeud JCR à [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] pour AEM la ressource

#### Approche recommandée

`DamUtil.resolveToAsset(..)`résout toute ressource sous l’objet  `dam:Asset` de ressource en montant l’arborescence selon les besoins.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Approche alternative

L’adaptation d’une ressource à une ressource nécessite que la ressource elle-même soit le noeud `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Ressource vers AEM page

#### Approche recommandée

`pageManager.getContainingPage(..)` résout toute ressource sous l’objet  `cq:Page` de page en montant l’arborescence selon les besoins.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Approche alternative {#alternative-approach-1}

Pour adapter une ressource à une page, la ressource elle-même doit être le noeud `cq:Page`.

```java
Page page = resource.adaptTo(Page.class);
```

### Lire les propriétés AEM page

Utilisez les getters de l’objet Page pour obtenir des propriétés bien connues (`getTitle()`, `getDescription()`, etc.) et `page.getProperties()` pour obtenir la `[cq:Page]/jcr:content` ValueMap afin de récupérer d’autres propriétés.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lecture des propriétés AEM métadonnées des ressources

L’API Asset fournit des méthodes pratiques pour lire les propriétés à partir du noeud `[dam:Asset]/jcr:content/metadata`. Notez qu’il ne s’agit pas d’une ValueMap, le 2e paramètre (valeur par défaut et diffusion de type automatique) n’est pas pris en charge.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lire [!DNL Sling] [!DNL Resource] propriétés {#read-sling-resource-properties}

Lorsque les propriétés sont stockées dans des emplacements (propriétés ou ressources relatives) auxquels les API AEM (Page, Asset) ne peuvent pas accéder directement, les ressources [!DNL Sling] et les ValueMaps peuvent être utilisées pour obtenir les données.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

Dans ce cas, il se peut que l’objet AEM doive être converti en [!DNL Sling] [!DNL Resource] pour localiser efficacement la propriété ou la sous-ressource souhaitée.

#### AEM Page à [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Ressource à [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Écrire des propriétés à l’aide de [!DNL Sling]&#39;s ModifiableValueMap

Utilisez [!DNL Sling] de [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) pour écrire des propriétés sur des noeuds. Cela peut uniquement écrire sur le noeud immédiat (les chemins de propriété relatifs ne sont pas pris en charge).

Notez que l’appel à `.adaptTo(ModifiableValueMap.class)` nécessite des autorisations d’écriture sur la ressource, sinon il renverra null.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Création d’une page AEM

Utilisez toujours PageManager pour créer des pages, car un modèle de page est nécessaire pour définir et initialiser correctement les pages dans AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Créer une ressource [!DNL Sling]

ResourceResolver prend en charge les opérations de base pour la création de ressources. Lors de la création d’abstractions de niveau supérieur (AEM Pages, Ressources, Balises, etc.) utiliser les méthodes fournies par leurs responsables respectifs.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Suppression d’une ressource [!DNL Sling]

ResourceResolver prend en charge la suppression d’une ressource. Lors de la création d’abstractions de niveau supérieur (AEM Pages, Ressources, Balises, etc.) utiliser les méthodes fournies par leurs responsables respectifs.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
