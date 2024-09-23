---
title: Activation de la mise en cache du réseau CDN
description: Découvrez comment activer la mise en cache des réponses HTTP dans le réseau CDN d’AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '637'
ht-degree: 100%

---

# Activation de la mise en cache du réseau CDN

Découvrez comment activer la mise en cache des réponses HTTP dans le réseau CDN d’AEM as a Cloud Service. La mise en cache des réponses est contrôlée par par les en-têtes de cache de réponse HTTP `Cache-Control`, `Surrogate-Control` ou `Expires`.

Ces en-têtes de cache sont généralement définis dans les configurations vhost du Dispatcher d’AEM à l’aide de `mod_headers`, mais peuvent également être définis dans du code Java™ personnalisé s’exécutant dans l’instance de publication AEM directement.

## Comportement de mise en cache par défaut

Lorsque des configurations personnalisées NE sont PAS présentes, les valeurs par défaut sont utilisées. Dans la copie d’écran suivante, vous pouvez voir le comportement de mise en cache par défaut pour les instances de publication et de création AEM lors du déploiement d’un projet AEM `mynewsite` basé sur l’[archétype de projet AEM](https://github.com/adobe/aem-project-archetype).

![Comportement de mise en cache par défaut](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Pour plus d’informations, consultez [Instance de publication AEM – Durée de vie du cache par défaut](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html?lang=fr#cdn-cache-life) et [Instance de création AEM – Durée de vie du cache par défaut](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?lang=fr#default-cache-life).

En résumé, AEM as a Cloud Service met en cache la plupart des types de contenu (HTML, JSON, JS, CSS et Assets) dans l’instance de publication AEM et quelques types de contenu (JS, CSS) dans l’instance de création AEM.

## Activer la mise en cache

Pour modifier le comportement de mise en cache par défaut, vous pouvez mettre à jour les en-têtes de cache de deux façons.

1. **Configuration vhost de Dispatcher :** disponible uniquement pour l’instance de publication AEM.
1. **Code Java™ personnalisé :** disponible pour les instances de publication et de création AEM.

Examinons chacune de ces options.

### Configuration vhost du Dispatcher

Cette option est l’approche recommandée pour activer la mise en cache, mais elle n’est disponible que pour l’instance de publication AEM. Pour mettre à jour les en-têtes de cache, utilisez le module `mod_headers` et la directive `<LocationMatch>` dans le fichier vhost du serveur Apache HTTP. La syntaxe générale se présente comme suit :

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

Le tableau suivant résume l’objectif de chaque **en-tête** et des **attributs** applicables pour l’en-tête.

|                     | Navigateur web | Réseau de diffusion de contenu (CDN) | Description |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | Cet en-tête contrôle la durée de vie du navigateur web et du cache du réseau CDN. |
| Surrogate-Control | ✘ | ✔ | Cet en-tête contrôle la durée de vie du cache du réseau CDN. |
| Expiration | ✔ | ✔ | Cet en-tête contrôle la durée de vie du navigateur web et du cache du réseau CDN. |


- **max-age** : cet attribut contrôle la « durée de vie » (TTL, Time to live) du contenu de la réponse en secondes.
- **stale-while-revalidate** : cet attribut contrôle le traitement de l’_état obsolète_ du contenu de la réponse sur la couche de réseau CDN lorsque la requête reçue se trouve dans la période spécifiée en secondes. L’_état obsolète_ est la période qui suit l’expiration du TTL et avant que la réponse ne soit revalidée.
- **stale-if-error** : cet attribut contrôle le traitement d’_état obsolète_ du contenu de la réponse sur la couche de réseau CDN lorsque le serveur d’origine n’est pas disponible et que la requête reçue est comprise dans la période spécifiée en secondes.

Pour plus d’informations, consultez les détails de [l’obsolescence et de la revalidation](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/).

#### Exemple

Pour augmenter la durée de vie du navigateur web et du cache du réseau CDN du **type de contenu HTML** à _10 minutes_ sans traitement d’état obsolète, procédez comme suit :

1. Dans votre projet AEM, recherchez le fichier vhost souhaité à partir du répertoire `dispatcher/src/conf.d/available_vhosts`.
1. Mettez à jour le fichier vhost (par exemple, `wknd.vhost`) comme suit :

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   Les fichiers vhost dans le répertoire `dispatcher/src/conf.d/enabled_vhosts` sont des **liens symboliques** vers des fichiers dans le répertoire `dispatcher/src/conf.d/available_vhosts`, veillez donc à créer des liens symboliques s’ils ne sont pas présents.
1. Déployez les modifications vhost dans l’environnement AEM as a Cloud Service souhaité à l’aide de [Cloud Manager – Pipeline de configuration de niveau web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=fr#web-tier-config-pipelines) ou des [commandes du RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=fr#deploy-apache-or-dispatcher-configuration).

Cependant, pour avoir des valeurs différentes pour la durée de vie du navigateur web et du cache du réseau CDN, vous pouvez utiliser l’en-tête `Surrogate-Control` dans l’exemple ci-dessus. De même, pour que le cache arrive à expiration à une date et une heure spécifiques, vous pouvez utiliser l’en-tête `Expires`. En outre, à l’aide des attributs `stale-while-revalidate` et `stale-if-error`, vous pouvez contrôler le traitement de l’état obsolète du contenu de la réponse. Le projet AEM WKND comporte une configuration du cache du réseau CDN de [traitement de l’état obsolète de référence](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155).

De même, vous pouvez mettre à jour les en-têtes de cache d’autres types de contenu (JSON, JS, CSS et Assets).

### Code Java™ personnalisé

Cette option est disponible pour les instances de publication et de création AEM. Toutefois, il n’est pas recommandé d’activer la mise en cache dans l’instance de création AEM et de conserver le comportement de mise en cache par défaut.

Pour mettre à jour les en-têtes de cache, utilisez l’objet `HttpServletResponse` dans le code Java™ personnalisé (servlet Sling, filtre de servlet Sling). La syntaxe générale se présente comme suit :

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
