---
title: Bonnes pratiques de l’API Java dans AEM
description: AEM repose sur une riche pile de logiciels open source qui expose de nombreuses API Java à utiliser lors du développement. Cet article explore les principales API et explique quand et pourquoi elles doivent être utilisées.
version: 6.2, 6.3, 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
source-git-commit: 967bcf3c4046a17303eb2fe70d7156267a7cbed7
workflow-type: tm+mt
source-wordcount: '2074'
ht-degree: 6%

---

# Bonnes pratiques relatives aux API Java

Adobe Experience Manager (AEM) repose sur une riche pile de logiciels open source qui expose de nombreuses API Java à utiliser pendant le développement. Cet article explore les principales API et explique quand et pourquoi elles doivent être utilisées.

AEM est basé sur 4 ensembles d’API Java Principaux.

* **Adobe Experience Manager (AEM)**

   * abstractions de produits telles que pages, ressources, workflows, etc.

* **Structure web Apache Sling**

   * abstractions REST et basées sur des ressources telles que les ressources, les mappages de valeurs et les requêtes HTTP.

* **JCR (Apache Jackrabbit Oak)**

   * Extraits de données et de contenu tels que le noeud, les propriétés et les sessions.

* **OSGi (Apache Felix)**

   * abstractions du conteneur d’applications OSGi telles que les composants de services et (OSGi).

## Préférence API Java &quot;règle de base&quot;

La règle générale est de préférer les API/abstractions dans l’ordre suivant :

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

Si une API est fournie par AEM, préférez-la à [!DNL Sling], JCR et OSGi. Si AEM ne fournit pas d’API, préférez [!DNL Sling] sur JCR et OSGi.

Cet ordre est une règle générale, ce qui signifie qu&#39;il existe des exceptions. Les raisons possibles de rompre avec cette règle sont les suivantes :

* Exceptions connues, comme décrit ci-dessous.
* La fonctionnalité requise n’est pas disponible dans une API de niveau supérieur.
* Fonctionner dans le contexte du code existant (code de produit personnalisé ou AEM) qui utilise lui-même une API moins prisée, et le coût de déplacement vers la nouvelle API est injustifiable.

   * Il est préférable d’utiliser systématiquement l’API de niveau inférieur plutôt que de créer un mélange.

## API AEM

* [**AEM JavaDocs de l’API**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

Les API d’AEM fournissent des abstractions et des fonctionnalités spécifiques aux cas d’utilisation productifs.

Par exemple, AEM [PageManager](https://www.adobe.io/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) et [Page](https://www.adobe.io/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) Les API fournissent des abstractions pour `cq:Page` noeuds dans AEM représentant les pages web.

Bien que ces noeuds soient disponibles via [!DNL Sling] Les API en tant que ressources et les API JCR en tant que noeuds, les API AEM fournissent des abstractions pour les cas d’utilisation courants. L’utilisation des API d’AEM permet d’assurer un comportement cohérent entre AEM le produit, ainsi que des personnalisations et des extensions à .

### com.adobe.* par rapport à com.day.* API

AEM API ont une préférence intra-package, identifiée par les packages Java suivants, par ordre de préférence :

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` prend en charge les cas d’utilisation de produits, tandis que `com.adobe.granite` prend en charge les cas d’utilisation de plateformes inter-produits, tels que les workflows ou les tâches (qui sont utilisés dans plusieurs produits : AEM Assets, Sites, etc.).

`com.day.cq` contient des API &quot;originales&quot;. Ces API traitent des abstractions et fonctionnalités de base qui existaient avant et/ou autour de l’acquisition par l’Adobe de [!DNL Day CQ]. Ces API sont prises en charge et ne doivent pas être évitées, sauf si `com.adobe.cq` ou `com.adobe.granite` fournissez une alternative (plus récente).

Nouvelles abstractions telles que [!DNL Content Fragments] et [!DNL Experience Fragments] sont conçus dans la variable `com.adobe.cq` espace plutôt que `com.day.cq` décrit ci-dessous.

### API de requête

AEM prend en charge plusieurs langages de requête. Les 3 langues principales sont les suivantes : [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath et [AEM Query Builder](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

La préoccupation la plus importante est de conserver un langage de requête cohérent dans l’ensemble de la base de code, afin de réduire la complexité et les coûts à comprendre.

Tous les langages de requête possèdent effectivement les mêmes profils de performance, comme [!DNL Apache Oak] Les transpilotent vers JCR-SQL2 pour l’exécution de la requête finale, et le temps de conversion vers JCR-SQL2 est négligeable par rapport au temps de requête lui-même.

L’API préférée est : [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), qui constitue l’abstraction de niveau supérieur et fournit une API robuste pour construire, exécuter et récupérer les résultats des requêtes, et fournit les éléments suivants :

* Construction de requêtes simple et paramétrée (paramètres de requête modélisés en tant que carte)
* Native [API Java et API HTTP](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [AEM Query Debugger](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [AEM prédicats](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) prise en charge des exigences de requête courantes

* API extensible, permettant le développement de [prédicats de requête](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 et XPath peuvent être exécutés directement via [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) et [API JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html), renvoi des résultats a [[!DNL Sling] Ressources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) ou [Noeuds JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html), respectivement.

>[!CAUTION]
>
>AEM l’API QueryBuilder fuit un objet ResourceResolver. Pour atténuer cette fuite, procédez comme suit : [exemple de code](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).

## [!DNL Sling]les API ;

* [**Apache [!DNL Sling] Documentation JavaDocs de l’API**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) est la structure web RESTful qui sous-tend AEM. [!DNL Sling] fournit le routage des requêtes HTTP, modélise les noeuds JCR en tant que ressources, fournit un contexte de sécurité, etc.

[!DNL Sling] Les API ont l’avantage supplémentaire d’être créées pour l’extension, ce qui signifie qu’il est souvent plus facile et plus sûr d’augmenter le comportement des applications créées à l’aide de [!DNL Sling] API plutôt que les API JCR moins extensibles.

### Utilisations courantes de [!DNL Sling] API

* Accès aux noeuds JCR en tant que [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) et accéder à leurs données via [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fournir un contexte de sécurité via [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Création et suppression de ressources via ResourceResolver [méthodes create/move/copy/delete](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Mise à jour des propriétés via le [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Création de blocs de création de traitement des demandes

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtres de servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Blocs de création de traitement de travail asynchrone

   * [Gestionnaires d’événements et de tâches](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planificateur](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modèles Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Utilisateurs du service](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## API JCR

* **[JavaDocs JCR 2.0](https://www.adobe.io/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Le [API JCR (Java Content Repository) 2.0](https://www.adobe.io/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) fait partie d’une spécification pour les implémentations JCR (dans le cas d’AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Toute implémentation JCR doit se conformer à ces API et les mettre en oeuvre. Il s’agit donc de l’API de niveau le plus bas pour interagir avec le contenu AEM.

Le JCR lui-même est une AEM de banque de données NoSQL hiérarchique/basée sur une arborescence qui utilise comme référentiel de contenu. Le JCR dispose d’une vaste gamme d’API prises en charge, allant du CRUD de contenu à l’interrogation de contenu. Malgré cette API robuste, il est rare qu’elles soient préférées aux AEM de niveau supérieur et [!DNL Sling] abstractions.

Préférez toujours les API JCR aux API Apache Jackrabbit Oak. Les API JCR sont destinées à ***interaction*** avec un référentiel JCR, alors que les API Oak sont pour ***implémentation*** un référentiel JCR.

### Erreurs courantes à propos des API JCR

Bien que JCR soit AEM référentiel de contenu, ses API ne sont PAS la méthode préférée pour interagir avec le contenu. À la place, préférez les API AEM (Page, Ressources, Balise, etc.) ou des API de ressources Sling, car elles fournissent de meilleures abstractions.

>[!CAUTION]
>
>L’utilisation généralisée des interfaces Session et Node des API JCR dans une application AEM est une utilisation puissante du code. Assurez-vous que [!DNL Sling] Les API ne doivent pas être utilisées à la place.

### Utilisations courantes des API JCR

* [Gestion du contrôle d&#39;accès](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Gestion autorisable (utilisateurs/groupes)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observation JCR (écoute des événements JCR)
* Création de structures de noeuds profonds

   * Bien que les API Sling prennent en charge la création de ressources, les API JCR disposent de méthodes pratiques dans [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) et [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) qui accélèrent la création de structures profondes.

## API OSGi

* [**JavaDocs OSGi R6**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[Documentation JavaDocs sur les annotations des composants OSGi Declarative Services 1.2](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[Documentation JavaDocs sur les annotations de métadonnées OSGi Declarative Services 1.2](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**Documents Java sur la structure OSGi**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Il existe peu de chevauchement entre les API OSGi et les API de niveau supérieur (AEM, [!DNL Sling], et JCR), et la nécessité d’utiliser les API OSGi est rare et nécessite une expertise de développement AEM de haut niveau.

### API OSGi et Apache Felix

OSGi définit une spécification que tous les conteneurs OSGi doivent implémenter et respecter. AEM mise en oeuvre OSGi, Apache Felix, fournit également plusieurs de ses propres API.

* Préférence des API OSGi (`org.osgi`) sur les API Apache Felix (`org.apache.felix`).

### Utilisations courantes des API OSGi

* Annotations OSGi pour la déclaration des services et composants OSGi.

   * Préférence [Annotations OSGi Declarative Services (DS) 1.2](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Annotations SCR Felix](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) pour la déclaration des services et composants OSGi

* API OSGi pour code dynamique [d’annulation/d’enregistrement des services/composants OSGi ;](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Préférez l’utilisation des annotations OSGi DS 1.2 lorsque la gestion conditionnelle des services/composants OSGi n’est pas nécessaire (ce qui est le plus souvent le cas).

## Exceptions à la règle

Vous trouverez ci-dessous des exceptions courantes aux règles définies ci-dessus.

### API OSGi

Lorsque vous traitez d’abstractions OSGi de bas niveau, telles que la définition ou la lecture dans les propriétés de composant OSGi, les abstractions les plus récentes fournies par `org.osgi` sont préférées aux abstractions Sling de niveau supérieur. Les abstractions Sling concurrentes n’ont pas été marquées comme `@Deprecated` et suggérez le `org.osgi` alternative.

Notez également que la définition de noeud de configuration OSGi préfère `cfg.json` via `sling:OsgiConfig` format.

### AEM API Asset

* Préférence [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) over [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Lorsque la variable `com.day.cq` Les API Assets offrent des outils plus complets pour AEM cas d’utilisation de la gestion des ressources.
   * Les API de ressources Granite prennent en charge les cas d’utilisation de la gestion des ressources de bas niveau (version, relations).

### API de requête

* AEM QueryBuilder ne prend pas en charge certaines fonctions de requête, telles que [suggestions](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), des indices de vérification d’orthographe et d’index parmi d’autres fonctions moins courantes. Il est préférable d’effectuer des requêtes avec ces fonctions JCR-SQL2.

### [!DNL Sling] Enregistrement de servlet {#sling-servlet-registration}

* [!DNL Sling] enregistrement du servlet, préférer [Annotations OSGi DS 1.2 avec @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) over `@SlingServlet`

### [!DNL Sling] Enregistrement de filtre {#sling-filter-registration}

* [!DNL Sling] enregistrement de filtre, préférez [Annotations OSGi DS 1.2 avec @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) over `@SlingFilter`

## Fragments de code utiles

Vous trouverez ci-dessous des fragments de code Java utiles qui illustrent les bonnes pratiques pour les cas d’utilisation courants à l’aide des API discutées. Ces fragments illustrent également comment passer d’API moins prisées à des API plus préférées.

### Session JCR à [!DNL Sling] ResourceResolver

#### Sling ResourceResolver à fermeture automatique

Depuis AEM 6.2, la variable [!DNL Sling] ResourceResolver est `AutoClosable` dans [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) . En utilisant cette syntaxe, un appel explicite à `resourceResolver .close()` n’est pas nécessaire.

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

ResourceResolvers peut être fermé manuellement dans une `finally` block, si la technique de fermeture automatique illustrée ci-dessus ne peut pas être utilisée.

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

### Chemin JCR vers [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Noeud JCR vers [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] pour AEM la ressource

#### Approche recommandée

`DamUtil.resolveToAsset(..)`résout toute ressource sous la propriété `dam:Asset` à l’objet Asset en montant l’arborescence selon les besoins.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Approche alternative

L’adaptation d’une ressource à une ressource nécessite que la ressource elle-même soit la `dam:Asset` noeud .

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Ressource vers AEM page

#### Approche recommandée

`pageManager.getContainingPage(..)` résout toute ressource sous la propriété `cq:Page` à l’objet Page en montant l’arborescence selon les besoins.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Approche alternative {#alternative-approach-1}

L’adaptation d’une ressource à une page requiert que la ressource elle-même soit la `cq:Page` noeud .

```java
Page page = resource.adaptTo(Page.class);
```

### Lire les propriétés AEM page

Utilisez les getters de l’objet Page pour obtenir des propriétés bien connues (`getTitle()`, `getDescription()`, etc.) et `page.getProperties()` pour obtenir la variable `[cq:Page]/jcr:content` ValueMap pour récupérer d’autres propriétés.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lecture des propriétés AEM métadonnées des ressources

L’API Asset fournit des méthodes pratiques pour lire les propriétés à partir du `[dam:Asset]/jcr:content/metadata` noeud . Notez qu’il ne s’agit pas d’une ValueMap, le 2e paramètre (valeur par défaut et diffusion de type automatique) n’est pas pris en charge.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lecture [!DNL Sling] [!DNL Resource] properties {#read-sling-resource-properties}

Lorsque les propriétés sont stockées dans des emplacements (propriétés ou ressources relatives) auxquels les API AEM (Page, Ressource) ne peuvent pas accéder directement, la variable [!DNL Sling] Les ressources et les ValueMaps peuvent être utilisés pour obtenir les données.

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

#### AEM la ressource à [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Ecrire des propriétés à l’aide de [!DNL Sling]&#39;s ModifiableValueMap

Utilisation [!DNL Sling]&#39;s [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) pour écrire des propriétés sur des noeuds. Cela peut uniquement écrire sur le noeud immédiat (les chemins de propriété relatifs ne sont pas pris en charge).

Notez l’appel à `.adaptTo(ModifiableValueMap.class)` nécessite des autorisations d’écriture sur la ressource, sinon elle renverra une valeur nulle.

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

### Créez un [!DNL Sling] Ressource

ResourceResolver prend en charge les opérations de base pour la création de ressources. Lors de la création d’abstractions de niveau supérieur (AEM Pages, Ressources, Balises, etc.) utiliser les méthodes fournies par leurs responsables respectifs.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Suppression d’une [!DNL Sling] Ressource

ResourceResolver prend en charge la suppression d’une ressource. Lors de la création d’abstractions de niveau supérieur (AEM Pages, Ressources, Balises, etc.) utiliser les méthodes fournies par leurs responsables respectifs.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
