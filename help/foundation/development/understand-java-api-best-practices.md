---
title: Comprendre les bonnes pratiques de l’API Java dans AEM
description: AEM est construit sur une riche pile de logiciels open-source qui expose de nombreuses API Java à des fins de développement. Cet article explore les principales API et explique quand et pourquoi elles doivent être utilisées.
version: 6.2, 6.3, 6.4, 6.5
sub-product: fondation, ressources, sites
feature: APIs
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 8%

---


# Comprendre les meilleures pratiques de l’API Java

Adobe Experience Manager (AEM) repose sur une riche pile de logiciels open source qui expose de nombreuses API Java à utiliser pendant le développement. Cet article explore les principales API et explique quand et pourquoi elles doivent être utilisées.

AEM est basé sur 4 jeux d&#39;API Java Principaux.

* **Adobe Experience Manager (AEM)**

   * abstractions de produits telles que pages, ressources, workflows, etc.

* **[!DNL Apache Sling]Web Framework**

   * abstractions REST et basées sur les ressources telles que ressources, mappages de valeurs et requêtes HTTP.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * Expressions de données et de contenu telles que noeud, propriétés et sessions.

* **[!DNL OSGi (Apache Felix)]**

   * Les abstractions du conteneur d’applications OSGi telles que les services et les composants (OSGi).

## API Java préférence &quot;règle de base&quot;

La règle générale consiste à préférer les API/abstractions dans l’ordre suivant :

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

Si une API est fournie par AEM, préférez-la à [!DNL Sling], JCR et OSGi. Si AEM ne fournit pas d’API, préférez [!DNL Sling] au JCR et OSGi.

Cet ordre est une règle générale, ce qui signifie qu&#39;il existe des exceptions. Les raisons acceptables pour rompre avec cette règle sont les suivantes :

* Exceptions bien connues, comme décrit ci-dessous.
* La fonctionnalité requise n&#39;est pas disponible dans une API de niveau supérieur.
* Fonctionner dans le contexte du code existant (code de produit personnalisé ou AEM) qui utilise lui-même une API moins prisée, et le coût de déplacement vers la nouvelle API est injustifiable.

   * Il est préférable d’utiliser systématiquement l’API de niveau inférieur plutôt que de créer un mélange.

## API AEM

* [**Documentation JavaDocs de l’API AEM**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

Les API AEM fournissent des abstractions et des fonctionnalités spécifiques aux cas d’utilisation produits.

Par exemple, les API AEM [PageManager](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) et [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) fournissent des abstractions pour les noeuds `cq:Page` dans AEM qui représentent des pages Web.

Bien que ces noeuds soient disponibles via les API [!DNL Sling] en tant que ressources et les API JCR en tant que noeuds, les API AEM fournissent des abstractions pour les cas d’utilisation courants. L’utilisation des API AEM permet d’assurer un comportement cohérent entre AEM le produit, ainsi que des personnalisations et des extensions pour les AEM.

### com.adobe.* par rapport à com.day.* API

AEM API ont une préférence intra-package, identifiée par les packages Java suivants, par ordre de préférence :

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` prend en charge les cas d’utilisation de produits, tandis que  `com.adobe.granite` prend en charge les cas d’utilisation de plateformes inter-produits, tels que les processus ou les tâches (qui sont utilisés entre les produits : AEM Assets, Sites, etc.).

`com.day.cq` contient des API &quot;d’origine&quot;. Ces API traitent des abstractions et fonctionnalités de base qui existaient avant et/ou autour de l&#39;acquisition par le Adobe de [!DNL Day CQ]. Ces API sont prises en charge et ne doivent pas être évitées, à moins que `com.adobe.cq` ou `com.adobe.granite` ne fournissent une alternative (plus récente).

De nouvelles abstractions telles que [!DNL Content Fragments] et [!DNL Experience Fragments] sont construites dans l&#39;espace `com.adobe.cq` plutôt que `com.day.cq` décrit ci-dessous.

### API de requête

AEM prend en charge plusieurs langues de requête. Les trois langues principales sont [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath et [AEM Requête Builder](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

Le plus important est de maintenir un langage de requête cohérent dans toute la base de code, afin de réduire la complexité et les coûts de compréhension.

Tous les langages de requête ont effectivement les mêmes profils de performance, car [!DNL Apache Oak] les transpile en JCR-SQL2 pour l&#39;exécution finale de la requête, et le temps de conversion en JCR-SQL2 est négligeable par rapport au temps de requête lui-même.

L’API préférée est [AEM Requête Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), qui est l’abstraction de niveau supérieur et fournit une API robuste pour construire, exécuter et récupérer les résultats pour les requêtes, et fournit les éléments suivants :

* Construction de requêtes simple et paramétrée (paramètres de requête modélisés en tant que carte)
* [API Java et API HTTP natives](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [Débogueur de Requête OOTB](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [Prévisions d&#39;OOTB ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) prenant en charge les besoins de requête communs

* API extensible, permettant le développement de prédicats de requête [personnalisés ](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* Les API JCR-SQL2 et XPath peuvent être exécutées directement par [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) et [JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html), renvoyant respectivement les résultats suivants : [[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) ou [Noeuds JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html).

>[!CAUTION]
>
>AEM l&#39;API QueryBuilder fuit un objet ResourceResolver. Pour atténuer cette fuite, suivez cet [exemple de code](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).


## [!DNL Sling]les API ;

* [**JavaDocs  [!DNL Sling] API Apache**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apacheis est le cadre web RESTful qui sous-tend AEM. [!DNL Sling] fournit un routage de requête HTTP, modélise les noeuds JCR en tant que ressources, fournit un contexte de sécurité, et bien plus encore.

[!DNL Sling] Les API ont l&#39;avantage supplémentaire d&#39;être créées pour l&#39;extension, ce qui signifie qu&#39;il est souvent plus facile et plus sûr d&#39;augmenter le comportement des applications créées à l&#39;aide d&#39; [!DNL Sling] API que les API JCR moins extensibles.

### Utilisations courantes des API [!DNL Sling]

* Accès aux noeuds JCR sous la forme [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) et accès à leurs données via [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fournir un contexte de sécurité via [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Création et suppression de ressources via les [méthodes de création/déplacement/copie/suppression de ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Mise à jour des propriétés via [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Création de blocs de création de traitement de demande

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtres de servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Blocs de création de traitement de travail asynchrones

   * [Gestionnaires de événements et de tâches](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planificateur](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modèles Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Utilisateurs du service](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## API JCR

* **[JavaDocs JCR 2.0](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Les API [JCR (Java Content Repository) 2.0](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) font partie d’une spécification pour les implémentations JCR (dans le cas d’AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Toutes les implémentations JCR doivent se conformer à ces API et les implémenter. Il s’agit donc de l’API de niveau le plus bas pour interagir avec le contenu AEM.

Le JCR lui-même est un AEM de banque de données NoSQL hiérarchisé/basé sur une arborescence utilisé comme référentiel de contenu. Le JCR dispose d’une vaste gamme d’API prises en charge, allant du contenu CRUD à l’interrogation de contenu. Malgré cette API robuste, il est rare qu&#39;elles soient préférées à l&#39;AEM de niveau supérieur et aux abstractions [!DNL Sling].

Préférez toujours les API JCR plutôt que les API Apache Jackrabbit Oak. Les API JCR sont destinées à ***interagir*** avec un référentiel JCR, tandis que les API Oak sont destinées à ***implémenter*** un référentiel JCR.

### Les idées reçues sur les API JCR

Bien que le JCR soit AEM référentiel de contenu, ses API ne sont PAS la méthode préférée pour interagir avec le contenu. Préférez plutôt les API AEM (Page, Ressources, Balise, etc.) ou les API de ressources Sling car elles fournissent de meilleures abstractions.

>[!CAUTION]
>
>L’utilisation généralisée des interfaces Session et Noeud des API JCR dans une application AEM est à l’odorat du code. Assurez-vous que les API [!DNL Sling] ne doivent pas être utilisées à la place.

### Utilisations courantes des API JCR

* [Gestion des contrôles d&#39;accès](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [Gestion autorisée (utilisateurs/groupes)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observation JCR (écoute des événements JCR)
* Création de structures de noeud profondes

   * Bien que les API Sling prennent en charge la création de ressources, les API JCR disposent de méthodes pratiques dans [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) et [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) qui accélèrent la création de structures profondes.

## API OSGi

* [**JavaDocs OSGi R6**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[Services déclaratifs OSGi 1.2 Annotations de composants JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Annotations de type de métadonnées JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**Documentation JavaDocs de la structure OSGi**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Il y a peu de chevauchement entre les API OSGi et les API de niveau supérieur (AEM, [!DNL Sling] et JCR), et la nécessité d&#39;utiliser les API OSGi est rare et nécessite un haut niveau d&#39;expertise en développement AEM.

### OSGi et API Apache Felix

OSGi définit une spécification que tous les conteneurs OSGi doivent implémenter et respecter. AEM mise en oeuvre OSGi, Apache Felix, fournit également plusieurs de ses propres API.

* Préférez les API OSGi (`org.osgi`) sur les API Apache Felix (`org.apache.felix`).

### Utilisations courantes des API OSGi

* Annotations OSGi pour la déclaration des services et composants OSGi.

   * Préférer [OSGi Declarative Services (DS) 1.2 Annotations](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) sur [Felix SCR Annotations](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) pour la déclaration des services et composants OSGi

* API OSGi pour les [un/l&#39;enregistrement dynamique des services/composants OSGi](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html) dans le code.

   * Préférez l’utilisation d’annotations OSGi DS 1.2 lorsque la gestion conditionnelle du service/composant OSGi n’est pas nécessaire (ce qui est le plus souvent le cas).

## Exceptions à la règle

Voici quelques exceptions courantes aux règles définies ci-dessus.

### API AEM Asset

* Préférez [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) par [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Bien que les `com.day.cq` API Ressources offrent davantage d&#39;outils complémentaires aux cas d&#39;utilisation de la gestion des ressources AEM.
   * Les API Granite Assets prennent en charge les cas d’utilisation de la gestion des ressources de bas niveau (version, relations).

### API de requête

* AEM QueryBuilder ne prend pas en charge certaines fonctions de requête telles que [suggestions](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), les indicateurs d&#39;orthographe et d&#39;index parmi d&#39;autres fonctions moins courantes. Pour requête avec ces fonctions, JCR-SQL2 est préférable.

### [!DNL Sling] Enregistrement du servlet  {#sling-servlet-registration}

* [!DNL Sling] enregistrement de servlet, préférez les annotations  [OSGi DS 1.2 avec @](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover  `@SlingServlet`

### [!DNL Sling] Enregistrement du filtre  {#sling-filter-registration}

* [!DNL Sling] enregistrement de filtre, préférez les annotations  [OSGi DS 1.2 avec @](https://sling.apache.org/documentation/the-sling-engine/filters.html) SlingServletFilterover  `@SlingFilter`

## Extraits de code utiles

Les extraits de code Java suivants illustrent les meilleures pratiques relatives aux cas d’utilisation courants à l’aide des API décrites. Ces extraits illustrent également comment passer d’API moins préférées à des API plus préférées.

### Session JCR à [!DNL Sling] ResourceResolver

#### Résolveur de ressources Sling à fermeture automatique

Depuis AEM 6.2, le ResourceResolver [!DNL Sling] est `AutoClosable` dans une instruction [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). En utilisant cette syntaxe, un appel explicite à `resourceResolver .close()` n&#39;est pas nécessaire.

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

ResourceResolvers peut être fermé manuellement dans un bloc `finally` si la technique de fermeture automatique décrite ci-dessus ne peut pas être utilisée.

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

### Chemin d&#39;accès JCR à [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Noeud JCR à [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] à AEM ressource

#### Approche recommandée

`DamUtil.resolveToAsset(..)`résout toute ressource sous l&#39;objet  `dam:Asset` d&#39;actif en montant l&#39;arborescence si nécessaire.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Approche alternative

L&#39;adaptation d&#39;une ressource à un actif requiert que la ressource elle-même soit le noeud `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Page Ressource vers AEM

#### Approche recommandée

`pageManager.getContainingPage(..)` résout toute ressource sous l&#39;objet Page  `cq:Page` en montant l&#39;arborescence si nécessaire.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Autre approche {#alternative-approach-1}

L&#39;adaptation d&#39;une ressource à une page requiert que la ressource elle-même soit le noeud `cq:Page`.

```java
Page page = resource.adaptTo(Page.class);
```

### Propriétés de la page AEM lus

Utilisez les méthodes getters de l&#39;objet Page pour obtenir des propriétés bien connues (`getTitle()`, `getDescription()`, etc.) et `page.getProperties()` pour obtenir le `[cq:Page]/jcr:content` ValueMap pour récupérer d&#39;autres propriétés.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lecture des propriétés de métadonnées AEM fichier

L’API Asset fournit des méthodes pratiques pour lire les propriétés à partir du noeud `[dam:Asset]/jcr:content/metadata`. Notez qu’il ne s’agit pas d’un ValueMap, le 2ème paramètre (valeur par défaut et casting de type automatique) n’est pas pris en charge.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### [!DNL Sling] [!DNL Resource] propriétés {#read-sling-resource-properties}

Lorsque des propriétés sont stockées dans des emplacements (propriétés ou ressources relatives) auxquels les API AEM (Page, Asset) ne peuvent pas accéder directement, les ressources [!DNL Sling] et les zones ValueMaps peuvent être utilisées pour obtenir les données.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

Dans ce cas, l&#39;objet AEM peut être converti en [!DNL Sling] [!DNL Resource] pour localiser efficacement la propriété ou la sous-ressource souhaitée.

#### AEM page à [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM actif à [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Écrire des propriétés à l&#39;aide de [!DNL Sling]&#39;s ModifiableValueMap

Utilisez [!DNL Sling] de [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) pour écrire des propriétés sur les noeuds. Ceci ne peut écrire que sur le noeud immédiat (les chemins de propriété relatifs ne sont pas pris en charge).

Notez que l&#39;appel à `.adaptTo(ModifiableValueMap.class)` nécessite des autorisations d&#39;écriture sur la ressource, sinon il retournera null.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Création d’une page AEM

Utilisez toujours PageManager pour créer des pages au fur et à mesure qu&#39;il utilise un modèle de page, est nécessaire pour définir et initialiser correctement les pages dans AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Créer une ressource [!DNL Sling]

ResourceResolver prend en charge les opérations de base pour la création de ressources. Lors de la création d’abstractions de niveau supérieur (AEM pages, ressources, balises, etc.) utiliser les méthodes fournies par leurs gestionnaires respectifs.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Supprimer une ressource [!DNL Sling]

ResourceResolver prend en charge la suppression d&#39;une ressource. Lors de la création d’abstractions de niveau supérieur (AEM pages, ressources, balises, etc.) utiliser les méthodes fournies par leurs gestionnaires respectifs.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
