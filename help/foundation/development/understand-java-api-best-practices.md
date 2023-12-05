---
title: Bonnes pratiques relatives aux API Java™ dans AEM
description: AEM repose sur une riche pile logicielle open source qui expose de nombreuses API Java™ à utiliser pendant le développement. Cet article couvre les principales API et explique quand et pourquoi elles doivent être utilisées.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 564
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 100%

---

# Bonnes pratiques relatives aux API Java™

Adobe Experience Manager (AEM) repose sur une riche pile logicielle open source qui expose de nombreuses API Java™ à utiliser pendant le développement. Cet article couvre les principales API et explique quand et pourquoi elles doivent être utilisées.

AEM repose sur quatre ensembles principaux d’API Java™.

* **Adobe Experience Manager (AEM)**

   * Abstractions de produits telles que des pages, des ressources, des workflows, etc.

* **Framework web Apache Sling**

   * REST et abstractions basées sur des ressources telles que des ressources, des cartes de valeurs et des requêtes HTTP.

* **JCR (Apache Jackrabbit Oak)**

   * Abstractions de données et de contenu telles que des nœuds, propriétés et sessions.

* **OSGi (Apache Felix)**

   * Abstractions du conteneur d’application OSGi telles que les services et les composants (OSGi).

## Règle de base de préférence d’API Java™

La règle générale est de préférer les API/abstractions dans l’ordre suivant :

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

Si une API est fournie par AEM, préférez-la à [!DNL Sling], JCR et OSGi. Si AEM ne fournit pas d’API, préférez [!DNL Sling] à JCR et OSGi.

Cet ordre est une règle générale, ce qui signifie qu’il existe des exceptions. Les raisons possibles de dérogation à cette règle sont les suivantes :

* Exceptions connues, comme décrit ci-dessous.
* La fonctionnalité requise n’est pas disponible dans une API de niveau supérieur.
* Le fonctionnement dans le contexte du code existant (code de produit personnalisé ou AEM) qui utilise lui-même une API située plus bas dans la liste de préférence, et le coût de déplacement vers la nouvelle API est injustifiable.

   * Il est préférable d’utiliser systématiquement l’API de niveau inférieur plutôt que de créer un mélange.

## API d’AEM

* [**JavaDocs d’API AEM**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

Les API d’AEM fournissent des abstractions et des fonctionnalités spécifiques aux cas d’utilisation productifs.

Par exemple, les API AEM [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) et [Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) fournissent des abstractions pour les nœuds `cq:Page` dans AEM représentant les pages web.

Alors que ces nœuds sont disponibles via les API [!DNL Sling] en tant que ressources et via les API JCR en tant que nœuds, les API AEM fournissent des abstractions pour les cas d’utilisation courants. L’utilisation des API d’AEM permet d’assurer un comportement cohérent entre AEM, le produit, ainsi que les personnalisations et les extensions d’AEM.

### com.adobe.&#42; et com.day.&#42; API

Les API d’AEM ont une préférence intra-package, identifiée par les packages Java™ suivants, par ordre de préférence :

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

Le package `com.adobe.cq` prend en charge les cas d’utilisation de produits, alors que `com.adobe.granite` prend en charge les cas d’utilisation de plateformes inter-produits, tels que les workflows et les tâches (qui sont utilisés dans plusieurs produits : AEM Assets, Sites, etc.).

Le package `com.day.cq` contient des API « originales ». Ces API traitent des abstractions et fonctionnalités de base qui existaient avant et/ou autour de l’acquisition par Adobe de [!DNL Day CQ]. Ces API sont prises en charge et doivent être évitées, sauf si les packages `com.adobe.cq` ou `com.adobe.granite` ne fournissent PAS d’alternative (plus récente).

De nouvelles abstractions telles que [!DNL Content Fragments] et [!DNL Experience Fragments] sont conçues dans l’espace `com.adobe.cq` plutôt que celui `com.day.cq` décrit ci-dessous.

### API de requête

AEM prend en charge plusieurs langages de requête. Les trois langages principaux sont : [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath et [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=fr).

La préoccupation la plus importante est de conserver un langage de requête cohérent dans l’ensemble de la base de code, afin de réduire la complexité et les coûts de compréhension.

Tous les langages de requête possèdent effectivement les mêmes profils de performance, comme [!DNL Apache Oak] les transpile vers JCR-SQL2 pour l’exécution de la requête finale, et le temps de conversion vers JCR-SQL2 est négligeable par rapport au temps de requête lui-même.

L’API préférée est [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=fr), qui constitue le niveau d’abstraction le plus élevé et fournit une API robuste pour la création, l’exécution et la récupération des résultats des requêtes, et fournit les éléments suivants :

* Une construction de requêtes simple et paramétrée (paramètres de requête modélisés en tant que carte).
* Une [API Java™ native et des API HTTP](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=fr).
* Un [débogueur de requêtes AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=fr).
* Des [prédicats AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html?lang=fr) prenant en charge des exigences de requête courantes.

* Une API extensible, permettant le développement de [prédicats de requête](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=fr) personnalisés.
* JCR-SQL2 et XPath peuvent être exécutés directement via [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) et les [API JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), renvoyant des résultats de [[!DNL Sling] ressources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) ou de [nœuds JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), respectivement.

>[!CAUTION]
>
>L’API QueryBuilder AEM produit une fuite d’un objet ResourceResolver. Pour atténuer cette fuite, suivez cet [exemple de code](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## API [!DNL Sling]

* [**JavaDocs de l’API Apache [!DNL Sling]**](https://sling.apache.org/apidocs/sling10/)

[Apache  [!DNL Sling]](https://sling.apache.org/) est le framework web RESTful qui sous-tend AEM. [!DNL Sling] fournit le routage des requêtes HTTP, modélise les nœuds JCR en tant que ressources, fournit un contexte de sécurité, etc.

Les API [!DNL Sling] ont l’avantage supplémentaire d’être créées pour l’extension, ce qui signifie qu’il est souvent plus facile et plus sûr d’augmenter le comportement des applications créées à l’aide des API [!DNL Sling] plutôt que des API JCR moins extensibles.

### Utilisations courantes des API [!DNL Sling]

* Accéder aux nœuds JCR en tant que [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) et accéder à leurs données via [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fournir un contexte de sécurité via le [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Créer et supprimer des ressources via les [méthodes créer/déplacer/copier/supprimer](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html) de ResourceResolver.
* Mettre à jour des propriétés via [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Créer des blocs de création de traitement des demandes.

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtres de servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Blocs de création de traitement de travail asynchrone.

   * [Gestionnaires d’événements et de tâches](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planificateur](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modèles Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Utilisateurs et utilisatrices de service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=fr)

## API JCR

* **[JavaDocs JCR 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Les [API JCR (Java™ Content Repository) 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) font partie d’une spécification pour les implémentations JCR (dans le cas d’AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). Toute implémentation JCR doit se conformer à ces API et les mettre en œuvre. Il s’agit donc de l’API de niveau le plus bas pour interagir avec le contenu AEM.

Le JCR lui-même est un magasin de données NoSQL hiérarchique/basé sur une arborescence qu’AEM utilise comme référentiel de contenu. Le JCR dispose d’une vaste gamme d’API prises en charge, allant des opérations CRUD de contenu à l’interrogation de contenu. Malgré la robustesse de ces API, il est rare qu’elles soient privilégiées par rapport aux abstractions [!DNL Sling] et AEM de niveau supérieur.

Privilégiez toujours les API JCR plutôt que les API Apache Jackrabbit Oak. Les API JCR sont destinées à l’***interaction*** avec un référentiel JCR, tandis que les API Oak sont destinées à l’***implémentation*** d’un référentiel JCR.

### Erreurs courantes à propos des API JCR

Bien que JCR soit le référentiel de contenu d’AEM, ses API ne constituent PAS la méthode privilégiée pour interagir avec le contenu. Privilégiez plutôt les API AEM (Page, Ressources, Balise, etc.) ou les API de ressource Sling, car elles fournissent de meilleures abstractions.

>[!CAUTION]
>
>L’utilisation généralisée des interfaces Session et Node des API JCR dans une application AEM constitue une mauvaise pratique. Assurez-vous que les API [!DNL Sling] sont utilisées à la place.

### Utilisations courantes des API JCR

* [Gestion du contrôle d’accès](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=fr)
* [Gestion autorisable (utilisateurs et utilisatrices/groupes)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observation JCR (écoute des événements JCR)
* Créer des structures de nœuds profondes

   * Bien que les API Sling prennent en charge la création de ressources, les API JCR disposent de méthodes pratiques dans [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) et [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) qui facilitent la création de structures profondes.

## API OSGi

* [**JavaDocs OSGi R6**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs sur les annotations des composants OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs sur les annotations de métatype OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs sur le framework OSGi**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Les API OSGi et les API de niveau supérieur (AEM, [!DNL Sling], et JCR) se chevauchent peu. Il est rare d’avoir à utiliser les API OSGi et cela nécessite un haut niveau d’expertise en développement AEM.

### API OSGi vs Apache Felix

OSGi définit une spécification que tous les conteneurs OSGi doivent implémenter et respecter. L’implémentation OSGi d’AEM, Apache Felix, fournit également plusieurs de ses propres API.

* Privilégiez les API OSGi (`org.osgi`) par rapport aux API Apache Felix (`org.apache.felix`).

### Utilisations courantes des API OSGi

* Annotations OSGi pour la déclaration des services et composants OSGi.

   * Privilégiez les [annotations OSGi Declarative Services (DS) 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) par rapport aux [annotations SCR Felix](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) pour la déclaration des services et composants OSGi.

* API OSGi pour [l’annulation/l’enregistrement dynamique dans le code des services/composants OSGi](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Privilégiez l’utilisation des annotations OSGi DS 1.2 lorsque la gestion conditionnelle des services/composants OSGi n’est pas nécessaire (ce qui est le plus souvent le cas).

## Exceptions à la règle

Vous trouverez ci-dessous des exceptions courantes aux règles définies ci-dessus.

### API OSGi

Lorsque vous traitez des abstractions OSGi de bas niveau, telles que la définition ou la lecture dans les propriétés de composant OSGi, les abstractions les plus récentes fournies par `org.osgi` sont privilégiées par rapport aux abstractions Sling de niveau supérieur. Les abstractions Sling concurrentes n’ont pas été marquées comme `@Deprecated` et suggèrent l’alternative `org.osgi`.

Notez également que la définition de nœud de configuration OSGi préfère le format `cfg.json` au format `sling:OsgiConfig`.

### API Assets AEM

* Privilégiez [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) par rapport à [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Les API Assets `com.day.cq` fournissent des outils plus complets pour le cas d’utilisation de la gestion des ressources AEM.
   * Les API Assets Granite prennent en charge la gestion des ressources de bas niveau (version, relations).

### API de requête

* AEM QueryBuilder ne prend pas en charge certaines fonctions de requête, telles que les [suggestions](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), la vérification orthographique et les indices d’index parmi d’autres fonctions moins courantes. Il est préférable d’effectuer des requêtes avec ces fonctions à l’aide de JCR-SQL2.

### Enregistrement de servlet [!DNL Sling] {#sling-servlet-registration}

* Pour l’enregistrement du servlet [!DNL Sling], privilégiez les [annotations OSGi DS 1.2 avec @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) par rapport à `@SlingServlet`.

### Enregistrement de filtre [!DNL Sling] {#sling-filter-registration}

* Pour l’enregistrement de filtre [!DNL Sling], privilégiez les [annotations OSGi DS 1.2 avec @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) par rapport à `@SlingFilter`

## Extraits de code utiles

Vous trouverez ci-dessous des extraits de code Java™ qui illustrent les bonnes pratiques relatives aux cas d’utilisation courants des API mentionnées. Ces extraits illustrent également comment passer d’API au niveau de préférence inférieur à des API dont le niveau de préférence est supérieur.

### Session JCR à ResourceResolver [!DNL Sling]

#### Fermer automatiquement ResourceResolver Sling

Depuis AEM 6.2, le ResourceResolver [!DNL Sling] est `AutoClosable` dans une instruction [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). Avec cette syntaxe, un appel explicite à `resourceResolver .close()` n’est pas nécessaire.

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

#### Fermer manuellement ResourceResolver Sling

ResourceResolver peut être fermé manuellement dans un bloc `finally`, si la technique de fermeture automatique illustrée ci-dessus ne peut pas être utilisée.

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

### Chemin JCR vers [!DNL Resource] [!DNL Sling]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Nœud JCR vers [!DNL Resource] [!DNL Sling]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Resource] [!DNL Sling] vers AEM Assets

#### Approche recommandée

La fonction `DamUtil.resolveToAsset(..)` résout n’importe quelle ressource sous `dam:Asset` vers l’objet Asset en montant l’arborescence selon les besoins.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Approche alternative

L’adaptation d’une ressource à un objet Asset nécessite que la ressource elle-même soit le nœud `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### Ressource [!DNL Sling] vers la page AEM

#### Approche recommandée

`pageManager.getContainingPage(..)` résout toute ressource sous `cq:Page` vers l’objet Page en montant l’arborescence selon les besoins.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Approche alternative {#alternative-approach-1}

L’adaptation d’une ressource à un objet Page requiert que la ressource elle-même soit le nœud `cq:Page`.

```java
Page page = resource.adaptTo(Page.class);
```

### Lire les propriétés de page AEM

Utilisez les getters de l’objet Page pour obtenir des propriétés connues (`getTitle()`, `getDescription()`, etc.) et `page.getProperties()` pour obtenir la ValueMap `[cq:Page]/jcr:content` pour récupérer d’autres propriétés.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lire des propriétés de métadonnées Asset AEM

L’API Asset fournit des méthodes pratiques pour lire les propriétés à partir du nœud `[dam:Asset]/jcr:content/metadata`. Il ne s’agit pas d’une ValueMap, le deuxième paramètre (valeur par défaut et conversion de type automatique) n’est pas pris en charge.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lire des propriétés [!DNL Resource] [!DNL Sling] {#read-sling-resource-properties}

Lorsque les propriétés sont stockées dans des emplacements (propriétés ou ressources relatives) auxquels les API AEM (Page, Asset) ne peuvent pas accéder directement, les ressources [!DNL Sling] et les ValueMaps peuvent être utilisées pour obtenir les données.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

Dans ce cas, l’objet AEM peut devoir être converti en [!DNL Resource] [!DNL Sling] pour localiser efficacement la propriété ou la sous-ressource souhaitée.

#### Page AEM vers [!DNL Resource] [!DNL Sling]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Asset AEM vers [!DNL Resource] [!DNL Sling]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Écrire des propriétés à l’aide de la ModifiableValueMap de [!DNL Sling]

Utiliser la [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) de [!DNL Sling] pour écrire des propriétés sur des nœuds. Cela permet uniquement d’écrire sur le nœud immédiat (les chemins de propriété relatifs ne sont pas pris en charge).

Notez que l’appel à `.adaptTo(ModifiableValueMap.class)` nécessite des autorisations d’écriture sur la ressource, sinon elle renvoie null.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Créer une page AEM

Utilisez toujours PageManager pour créer des pages, car un modèle de page est nécessaire pour définir et initialiser correctement les pages dans AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Créer une ressource [!DNL Sling]

ResourceResolver prend en charge les opérations de base pour la création de ressources. Lors de la création d’abstractions de niveau supérieur (Pages, Ressources, Balises AEM, etc.), utilisez les méthodes fournies par leurs gestionnaires respectifs.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Supprimer une ressource [!DNL Sling]

ResourceResolver prend en charge la suppression d’une ressource. Lors de la création d’abstractions de niveau supérieur (Pages, Ressources, Balises AEM, etc.), utilisez les méthodes fournies par leurs gestionnaires respectifs.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
