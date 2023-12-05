---
title: API Java™ de diffusion d’images optimisées pour le web
description: Découvrez comment utiliser les API Java™ de diffusion d’images optimisées pour le web d’AEM as a Cloud Service pour développer des expériences web hautement performantes.
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
exl-id: c6bb9d6d-aef0-42d5-a189-f904bbbd7694
duration: 527
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 100%

---

# API Java™ de diffusion d’images optimisées pour le web

Découvrez comment utiliser les API Java™ de diffusion d’images optimisées pour le web d’AEM as a Cloud Service pour développer des expériences web hautement performantes.

AEM as a Cloud Service prend en charge la [diffusion d’images optimisées pour le web](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=fr) qui génère automatiquement des rendus d’images de ressources optimisées pour le web. La diffusion d’images optimisées pour le web peut être utilisée de trois manières princiales :

1. [Utiliser les composants principaux de gestion de contenu web d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
2. Créer un composant personnalisé qui [étend le composant d’image du composant de gestion de contenu web principal d’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=fr#tackling-the-image-problem)
3. Créez un composant personnalisé qui utilise l’API Java™ AssetDelivery pour générer des URL d’image optimisées pour le web.

Cet article explore l’utilisation des API Java™ d’image optimisées pour le web dans un composant personnalisé, d’une manière qui permet à la base de code de fonctionner à la fois sur AEM as a Cloud Service et sur le SDK AEM.

## API Java™

L’[API AssetDelivery](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) est un service OSGi qui génère des URL de diffusion optimisées pour le web pour les ressources d’image. Les options autorisées d’`AssetDelivery.getDeliveryURL(...)` sont [documentées ici](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=fr#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

Le service OSGi `AssetDelivery` n’est satisfait que lors de l’exécution dans AEM as a Cloud Service. Sur le SDK AEM, les références au service OSGi `AssetDelivery` renvoient `null`. Il est préférable d’utiliser de manière conditionnelle l’URL optimisée pour le web lors de l’exécution sur AEM as a Cloud Service et d’utiliser une URL d’image de secours sur le SDK AEM. En règle générale, le rendu web de la ressource est une solution de secours suffisante.


### Utiliser des API dans le service OSGi

Marquez la référence `AssetDelivery` comme facultative dans les services OSGi personnalisés afin que le service OSGi personnalisé reste disponible sur le SDK AEM.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### Utiliser des API dans le modèle Sling

Marquez la référence `AssetDelivery` comme facultative dans les modèles Sling personnalisés afin que le modèle Sling personnalisé reste disponible sur le SDK AEM.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### Utiliser des API de manière conditionnelle

Renvoie de manière conditionnelle l’URL d’image optimisée pour le web ou l’URL de secours en fonction de la disponibilité du service OSGi `AssetDelivery`. L’utilisation conditionnelle permet au code de fonctionner lors de l’exécution du code sur le SDK AEM.

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## Exemple de code

Le code suivant crée un exemple de composant qui affiche une liste de ressources d’image à l’aide d’URL d’images optimisées pour le web.

Lorsque le code s’exécute sur AEM as a Cloud Service, les rendus d’images WebP optimisées pour le web sont utilisés dans le composant personnalisé.

![Images optimisées pour le web sur AEM as a Cloud Service.](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEM as a Cloud Service prend en charge l’API AssetDelivery qui permet d’utiliser le rendu WebP optimisé pour le web._

Lorsque le code s’exécute sur le SDK AEM, les rendus web statiques moins optimaux sont utilisés, ce qui permet au composant de fonctionner pendant le développement local.

![Images de secours optimisées pour le web sur le SDK AEM.](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_Le SDK AEM ne prend pas en charge l’API AssetDelivery. Le rendu web statique de secours (PNG ou JPEG) est donc utilisé._

L’implémentation est divisée en trois parties logiques :

1. Le service OSGi `WebOptimizedImage` agit comme un « proxy intelligent » pour le service OSGi `AssetDelivery` fourni par AEM pouvant gérer l’exécution dans le SDK AEM et dans AEM as a Cloud Service.
2. Le modèle Sling `ExampleWebOptimizedImages` fournit une logique commerciale de collecte de la liste des ressources d’image et de leurs URL optimisées pour le web à afficher.
3. Le composant AEM `example-web-optimized-images` met en œuvre le code HTL pour afficher la liste des images optimisées pour le web.

L’exemple de code ci-dessous peut être copié dans votre base de code et mis à jour si nécessaire.

### Service OSGi

Le service OSGi `WebOptimizedImage` est divisé en une interface publique adressable (`WebOptimizedImage`) et une implémentation interne (`WebOptimizedImageImpl`). `WebOptimizedImageImpl` renvoie une URL d’image optimisée pour le web lors de l’exécution sur AEM as a Cloud Service, ainsi qu’une URL de rendu web statique sur le SDK AEM, ce qui permet au composant de rester fonctionnel sur le SDK AEM.

#### Interface

L’interface définit le contrat de service OSGi avec lequel d’autres codes tels que les modèles Sling peuvent interagir.

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### Implémentation

La mise en œuvre du service OSGi comprend une référence facultative au service OSGi `AssetDelivery` d’AEM et une logique de secours pour sélectionner une URL d’image appropriée lorsque `AssetDelivery` est `null` sur le SDK AEM. La logique de secours peut être mise à jour en fonction des exigences.

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### Modèle Sling

Le modèle Sling `ExampleWebOptimizedImages` est divisé en une interface publique adressable (`ExampleWebOptimizedImages`) et une implémentation interne (`ExampleWebOptimizedImagesImpl`) ;

Le modèle Sling `ExampleWebOptimizedImagesImpl` collecte la liste des ressources d’image à afficher et appelle le service OSGi personnalisé `WebOptimizedImage` pour obtenir l’URL de l’image optimisée pour le web. Puisque ce modèle Sling représente un composant AEM, il possède les méthodes habituelles, telles que `isEmpty()`, `getId()` et `getData()` ; toutefois, ces méthodes ne sont pas directement pertinentes pour l’utilisation d’images optimisées pour le web.

#### Interface

L’interface définit le contrat du modèle Sling avec lequel d’autres codes, tels que HTL, peuvent interagir.

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### Implémentation

Le modèle Sling utilise le service OSGi `WebOptimizeImage` personnalisé permettant de collecter les URL d’image optimisées pour le web pour les ressources d’image affichées par son composant.

Dans cet exemple, une requête simple est utilisée pour collecter des ressources d’image.

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### Composant AEM

Un composant AEM est lié au type de ressource Sling de l’implémentation du modèle Sling `WebOptimizedImagesImpl` et est responsable de l’affichage de la liste des images.



Le composant reçoit une liste d’objets `Img` via `getImages()` qui incluent les images WebP optimisées pour le web lors de l’exécution sur AEM as a Cloud Service. Le composant reçoit une liste d’objets `Img` via `getImages()` qui incluent des images web JPEG/PNG statiques lors de l’exécution sur le SDK AEM.

#### HTL

Le code HTL utilise le modèle Sling `WebOptimizedImages` et affiche la liste des objets `Img` renvoyés par `getImages()`.

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```
