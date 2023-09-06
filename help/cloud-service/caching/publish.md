---
title: Mise en cache du service de publication AEM
description: Présentation générale de la mise en cache AEM service de publication as a Cloud Service.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
source-git-commit: 6cbd8f3c49d44e75337715c35c198008da8ae7b9
workflow-type: tm+mt
source-wordcount: '1320'
ht-degree: 4%

---


# Publication AEM

Le service de publication d’AEM dispose de deux couches de mise en cache principales, le réseau de diffusion de contenu as a Cloud Service AEM et le Dispatcher AEM. Un réseau de diffusion de contenu géré par le client peut éventuellement être placé devant le réseau de diffusion de contenu as a Cloud Service AEM. Le réseau de diffusion de contenu as a Cloud Service AEM fournit une diffusion de contenu à la pointe, ce qui garantit que les expériences sont diffusées avec une faible latence aux utilisateurs dans le monde entier. AEM Dispatcher fournit la mise en cache directement devant AEM Publish (Publication) et est utilisé pour atténuer la charge inutile sur l’instance de publication AEM.

![Diagramme de présentation de la mise en cache AEM](./assets/publish/publish-all.png){align="center"}

## Réseau de diffusion de contenu

La mise en cache du réseau de diffusion de contenu d’AEM as a Cloud Service est contrôlée par les en-têtes de cache de réponse HTTP et est conçue pour mettre en cache le contenu afin d’optimiser l’équilibre entre l’actualisation et les performances. Le réseau de diffusion de contenu se trouve entre l’utilisateur final et le Dispatcher AEM. Il est utilisé pour mettre en cache le contenu aussi près que possible de l’utilisateur final, assurant ainsi une expérience performante.

![AEM Publier le réseau de diffusion de contenu](./assets/publish/publish-cdn.png){align="center"}

La configuration du mode de mise en cache du contenu par le réseau CDN se limite à la définition des en-têtes de cache sur les réponses HTTP. Ces en-têtes de cache sont généralement définis dans AEM configurations de vhost Dispatcher à l’aide de `mod_headers`, mais peut également être défini dans du code Java™ personnalisé s’exécutant dans AEM Publier lui-même.

### Quand les requêtes/réponses HTTP sont-elles mises en cache ?

AEM CDN as a Cloud Service ne met en cache que les réponses HTTP, et tous les critères suivants doivent être satisfaits :

+ L’état de la requête HTTP est `2xx` ou `3xx`
+ La méthode de requête HTTP `GET` ou `HEAD`
+ Au moins un des en-têtes de réponse HTTP suivants est présent : `Cache-Control`, `Surrogate-Control`, ou  `Expires`
+ La réponse HTTP peut être n’importe quel type de contenu, y compris les fichiers HTML, JSON, CSS, JS et binaires.

Par défaut, les réponses HTTP ne sont pas mises en cache par [AEM Dispatcher](#aem-dispatcher) de supprimer automatiquement tous les en-têtes de cache de réponse HTTP pour éviter la mise en cache sur le réseau de diffusion de contenu. Ce comportement peut être soigneusement remplacé à l’aide de `mod_headers` avec la propriété `Header always set ...` le cas échéant.

### Qu’est-ce qui est mis en cache ?

AEM CDN as a Cloud Service met en cache les éléments suivants :

+ Corps de réponse HTTP
+ En-têtes de réponse HTTP

En règle générale, une requête/réponse HTTP pour une seule URL est mise en cache en tant qu’objet unique. Cependant, le réseau de diffusion de contenu peut gérer la mise en cache de plusieurs objets pour une seule URL, lorsque la variable `Vary` est défini sur la réponse HTTP. Évitez de spécifier `Vary` sur les en-têtes dont les valeurs n’ont pas de jeu de valeurs étroitement contrôlé, car cela peut entraîner de nombreuses pertes de cache, ce qui réduit le taux d’accès au cache. Pour prendre en charge la mise en cache de demandes variées dans AEM Dispatcher, [consulter la documentation sur la mise en cache des variantes](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html).

### Durée de mise en cache{#cdn-cache-life}

Le réseau de diffusion de contenu de publication AEM est basé sur la durée de vie (TTL), ce qui signifie que la durée de vie du cache est déterminée par la variable `Cache-Control`, `Surrogate-Control`, ou `Expires` En-têtes de réponse HTTP. Si les en-têtes de mise en cache de la réponse HTTP ne sont pas définis par le projet, et que la variable [critères d&#39;éligibilité](#when-are-http-requestsresponses-cached) sont remplies, Adobe définit une durée de vie par défaut du cache de 10 minutes (600 secondes).

Voici comment les en-têtes de cache influencent la vie du cache CDN :

+ [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) L’en-tête de réponse HTTP indique au navigateur web et au réseau de diffusion de contenu la durée de mise en cache de la réponse. La valeur est en secondes. Par exemple : `Cache-Control: max-age=3600` indique au navigateur web de mettre en cache la réponse pendant une heure. Cette valeur est ignorée par le réseau CDN si `Surrogate-Control` L’en-tête de réponse HTTP est également présent.
+ [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) L’en-tête de réponse HTTP indique au réseau de diffusion de contenu d’AEM combien de temps mettre la réponse en cache. La valeur est en secondes. Par exemple : `Surrogate-Control: max-age=3600` indique au réseau de diffusion de contenu de mettre en cache la réponse pendant une heure.
+ [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) L’en-tête de réponse HTTP indique au réseau de diffusion de contenu (et au navigateur web) d’AEM combien de temps la réponse mise en cache est valide. La valeur est une date. Par exemple : `Expires: Sat, 16 Sept 2023 09:00:00 EST` indique au navigateur web de mettre en cache la réponse jusqu’à la date et l’heure spécifiées.

Utilisation `Cache-Control` pour contrôler la durée de vie du cache lorsqu’elle est identique pour le navigateur et le réseau de diffusion de contenu. Utilisation `Surrogate-Control` lorsque le navigateur web doit mettre en cache la réponse pour une durée différente de celle du réseau de diffusion de contenu.

#### Durée de vie du cache par défaut

Si une réponse HTTP est admissible pour la mise en cache AEM Dispatcher [par qualificateurs ci-dessus](#when-are-http-requestsresponses-cached), les valeurs suivantes sont les valeurs par défaut, sauf si une configuration personnalisée est présente.

| Type de contenu | Durée de vie du cache CDN par défaut |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | 5 minutes |
| [Ressources (images, vidéos, documents, etc.)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | 10 minutes |
| [Requêtes persistantes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 2 heures |
| [Bibliothèques clientes (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 jours |
| [Autres](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Non mis en cache |

### Personnalisation des règles de cache

[Configuration du mode de mise en cache du contenu par le réseau CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#disp) se limite à la définition des en-têtes de cache sur les réponses HTTP. Ces en-têtes de cache sont généralement définis dans AEM Dispatcher. `vhost` configurations à l’aide `mod_headers`, mais peut également être défini dans du code Java™ personnalisé s’exécutant dans AEM Publier lui-même.

## AEM Dispatcher

![AEM Publication AEM Dispatcher](./assets/publish/publish-dispatcher.png){align="center"}

### Quand les requêtes/réponses HTTP sont-elles mises en cache ?

Les réponses HTTP pour les requêtes HTTP correspondantes sont mises en cache lorsque tous les critères suivants sont satisfaits :

+ La méthode de requête HTTP `GET` ou `HEAD`
   + `HEAD` Les requêtes HTTP mettent uniquement en cache les en-têtes de réponse HTTP. Ils n&#39;ont pas de corps de réaction.
+ L’état de réponse HTTP est `200`
+ La réponse HTTP n’est PAS pour un fichier binaire.
+ Le chemin d’accès URL de requête HTTP se termine par une extension, par exemple : `.html`, `.json`, `.css`, `.js`, etc.
+ La requête HTTP ne contient pas d’autorisation et n’est pas authentifiée par AEM.
   + Toutefois, la mise en cache des requêtes authentifiées [peut être activé globalement](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-when-authentication-is-used) ou sélectivement via [mise en cache sensible aux autorisations](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=fr).
+ La requête HTTP ne contient pas de paramètres de requête.
   + Toutefois, la configuration [Paramètres de requête ignorés](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#ignoring-url-parameters) permet aux requêtes HTTP avec les paramètres de requête ignorés d’être mises en cache/diffusées à partir du cache.
+ Chemin d’accès de la requête HTTP [correspond à une règle allow Dispatcher et ne correspond pas à une règle deny](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#specifying-the-documents-to-cache).
+ La réponse HTTP ne comporte aucun des en-têtes de réponse HTTP suivants définis par AEM Publish :

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### Qu’est-ce qui est mis en cache ?

AEM Dispatcher met en cache les éléments suivants :

+ Corps de réponse HTTP
+ En-têtes de réponse HTTP spécifiés dans le [configuration des en-têtes de cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-http-response-headers). Voir la configuration par défaut fournie avec la variable [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113).
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### Durée de mise en cache

AEM Dispatcher met en cache les réponses HTTP à l’aide des méthodes suivantes :

+ Tant que l’invalidation n’est pas déclenchée par des mécanismes tels que la publication ou l’annulation de la publication du contenu.
+ Durée de vie (TTL) lorsqu’elle est explicite [configuré dans la configuration de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#configuring-time-based-cache-invalidation-enablettl). Voir la configuration par défaut dans la section [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) en examinant la variable `enableTTL` configuration.

#### Durée de vie du cache par défaut

Si une réponse HTTP est admissible pour la mise en cache AEM Dispatcher [par qualificateurs ci-dessus](#when-are-http-requestsresponses-cached-1), les valeurs suivantes sont les valeurs par défaut, sauf si une configuration personnalisée est présente.

| Type de contenu | Durée de vie du cache CDN par défaut |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | Jusqu’à invalidation |
| [Ressources (images, vidéos, documents, etc.)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | Jamais |
| [Requêtes persistantes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 1 minute |
| [Bibliothèques clientes (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 jours |
| [Autres](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Jusqu’à invalidation |

### Personnalisation des règles de cache

Le cache de Dispatcher AEM peut être configuré via le [Configuration du Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#configuring-the-dispatcher-cache-cache) notamment :

+ Éléments mis en cache
+ Quelles parties du cache sont invalidées lors de la publication/annulation de la publication ?
+ Quels paramètres de requête de requête HTTP sont ignorés lors de l’évaluation du cache ?
+ En-têtes de réponse HTTP mis en cache
+ Activation ou désactivation de la mise en cache TTL
+ ... et bien plus encore

Utilisation `mod_headers` pour définir les en-têtes de cache `vhost` La configuration n’affecte pas la mise en cache de Dispatcher (basée sur TTL), car celles-ci sont ajoutées à la réponse HTTP une fois AEM Dispatcher a traité la réponse. Pour affecter la mise en cache de Dispatcher par le biais d’en-têtes de réponse HTTP, le code Java™ personnalisé qui s’exécute dans AEM Publier et qui définit les en-têtes de réponse HTTP appropriés est requis.
