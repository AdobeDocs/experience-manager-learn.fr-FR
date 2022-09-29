---
title: Comprendre le partage des ressources cross-origin (CORS) avec AEM
description: Le partage des ressources cross-origin (CORS) de Adobe Experience Manager facilite les propriétés web non-AEM pour effectuer des appels côté client vers AEM, authentifiés et non authentifiés, afin de récupérer du contenu ou d’interagir directement avec l’.
version: 6.4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 1%

---

# Comprendre le partage des ressources cross-origin ([!DNL CORS])

Partage des ressources cross-origin Adobe Experience Manager ([!DNL CORS]) facilite les propriétés web non AEM pour effectuer des appels côté client vers AEM, authentifiés et non authentifiés, pour récupérer du contenu ou interagir directement avec l’.

## Configuration OSGi de la stratégie de partage des ressources cross-origin Adobe

Les configurations CORS sont gérées comme des usines de configuration OSGi dans AEM, chaque stratégie étant représentée comme une instance de la fabrique.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuration OSGi de la stratégie de partage des ressources cross-origin Adobe](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Sélection de stratégie

Une stratégie est sélectionnée en comparant la variable

* `Allowed Origin` avec le `Origin` en-tête de requête
* et `Allowed Paths` avec le chemin d’accès de la requête.

La première stratégie correspondant à ces valeurs est utilisée. Si aucun n’est trouvé, n’importe lequel [!DNL CORS] la demande est refusée.

Si aucune stratégie n’est configurée, [!DNL CORS] les demandes ne seront pas non plus traitées puisque le gestionnaire est désactivé et donc refusé de manière efficace, à condition qu’aucun autre module du serveur ne réponde à [!DNL CORS].

### Propriétés Policy

#### [!UICONTROL Origines autorisées]

* `"alloworigin" <origin> | *`
* Liste de `origin` paramètres spécifiant les URI pouvant accéder à la ressource. Pour les demandes sans informations d’identification, le serveur peut spécifier &#42; comme caractère générique, permettant ainsi à toute origine d’accéder à la ressource. *Il n’est absolument pas recommandé d’utiliser `Allow-Origin: *` en production puisqu’il permet à chaque site web étranger (c’est-à-dire à un attaquant) de faire des demandes qui, sans CORS, sont strictement interdites par les navigateurs.*

#### [!UICONTROL Origines autorisées (Regexp)]

* `"alloworiginregexp" <regexp>`
* Liste de `regexp` expressions régulières spécifiant des URI pouvant accéder à la ressource. *Les expressions régulières peuvent générer des correspondances inattendues si elles ne sont pas soigneusement créées, ce qui permet à un attaquant d’utiliser un nom de domaine personnalisé qui correspondrait également à la stratégie.* Il est généralement recommandé de disposer de stratégies distinctes pour chaque nom d’hôte d’origine spécifique, en utilisant `alloworigin`, même si cela signifie une configuration répétée des autres propriétés de stratégie. Les origines différentes ont tendance à avoir des cycles de vie et des exigences différents, ce qui profite d&#39;une séparation claire.

#### [!UICONTROL Chemins autorisés]

* `"allowedpaths" <regexp>`
* Liste de `regexp` expressions régulières spécifiant les chemins d’accès aux ressources pour lesquels la stratégie s’applique.

#### [!UICONTROL En-têtes exposés]

* `"exposedheaders" <header>`
* Liste des paramètres d’en-tête indiquant les en-têtes de requête auxquels les navigateurs sont autorisés à accéder.

#### [!UICONTROL Âge maximal]

* `"maxage" <seconds>`
* A `seconds` indiquant la durée pendant laquelle les résultats d’une demande de pré-vol peuvent être mis en cache.

#### [!UICONTROL En-têtes pris en charge]

* `"supportedheaders" <header>`
* Liste de `header` paramètres indiquant quels en-têtes HTTP peuvent être utilisés lors de la requête réelle.

#### [!UICONTROL Méthodes autorisées]

* `"supportedmethods"`
* Liste des paramètres de méthode indiquant les méthodes HTTP pouvant être utilisées lors de l’exécution de la requête réelle.

#### [!UICONTROL Prend en charge les informations d’identification]

* `"supportscredentials" <boolean>`
* A `boolean` indiquant si la réponse à la requête peut être exposée au navigateur. Utilisé dans le cadre d’une réponse à une demande de pré-vol, cela indique si la demande réelle peut être effectuée à l’aide des informations d’identification.

### Exemples de configurations

Le site 1 est un scénario de base, accessible anonymement et en lecture seule où le contenu est utilisé par [!DNL GET] requests :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

Le site 2 est plus complexe et nécessite des requêtes autorisées et risquées :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Problèmes de mise en cache et configuration du Dispatcher {#dispatcher-caching-concerns-and-configuration}

À partir de la version 4.1.1+ de Dispatcher, les en-têtes de réponse peuvent être mis en cache. Cela permet de mettre en cache [!DNL CORS] en-têtes le long de la propriété [!DNL CORS]- ressources demandées, tant que la demande est anonyme.

En règle générale, les mêmes considérations relatives à la mise en cache de contenu dans Dispatcher peuvent être appliquées à la mise en cache des en-têtes de réponse CORS au Dispatcher. Le tableau suivant définit quand [!DNL CORS] en-têtes (et donc [!DNL CORS] requêtes) peuvent être mises en cache.

| Mise en cache | Environnement | État d’authentification | Explication |
|-----------|-------------|-----------------------|-------------|
| Non | Publication AEM | Authentifié | La mise en cache de Dispatcher sur l’auteur AEM est limitée aux ressources statiques non créées. Il est ainsi difficile et impossible de mettre en cache la plupart des ressources sur AEM Author, y compris les en-têtes de réponse HTTP. |
| Non | Publication AEM | Authentifié | Évitez de mettre en cache les en-têtes CORS sur les requêtes authentifiées. Cela s’aligne sur les conseils courants de ne pas mettre en cache les requêtes authentifiées, car il est difficile de déterminer comment l’état d’authentification/d’autorisation de l’utilisateur demandeur affectera la ressource diffusée. |
| Oui | Publication AEM | Anonyme | Les en-têtes de réponse des requêtes anonymes pouvant être mises en cache dans Dispatcher peuvent également être mis en cache, ce qui garantit que les futures requêtes CORS pourront accéder au contenu mis en cache. Toute modification de configuration CORS sur AEM Publish **must** être suivie d’une invalidation des ressources mises en cache affectées. Les bonnes pratiques dictent les déploiements de code ou de configuration que le cache du Dispatcher est purgé, car il est difficile de déterminer quel contenu mis en cache peut être affecté. |

Pour permettre la mise en cache des en-têtes CORS, ajoutez la configuration suivante à tous les fichiers dispatcher.any de publication AEM pris en charge.

```
/cache { 
  ...
  /headers {
      "Origin"
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

N’oubliez pas de **redémarrez l’application du serveur web.** après avoir apporté des modifications à la variable `dispatcher.any` fichier .

Il est probable que l’effacement complet du cache soit nécessaire pour s’assurer que les en-têtes sont correctement mis en cache dans la requête suivante après une `/cache/headers` mise à jour de la configuration.

## Dépannage de CORS

La journalisation est disponible sous `com.adobe.granite.cors`:

* enable `DEBUG` pour afficher des détails sur les raisons pour lesquelles une [!DNL CORS] demande refusée
* enable `TRACE` pour afficher les détails sur toutes les requêtes qui passent par le gestionnaire CORS

### Conseils :

* Recréez manuellement les requêtes XHR à l’aide de curl, mais veillez à copier tous les en-têtes et détails, car chacun d’eux peut faire une différence. certaines consoles de navigateur permettent de copier la commande curl
* Vérifiez si la demande a été refusée par le gestionnaire CORS et non par l’authentification, le filtre de jeton CSRF, les filtres Dispatcher ou d’autres couches de sécurité.
   * Si le gestionnaire CORS répond par 200, mais `Access-Control-Allow-Origin` L’en-tête est absent de la réponse ; passez en revue les journaux pour les refus sous [!DNL DEBUG] in `com.adobe.granite.cors`
* Si la mise en cache du dispatcher de [!DNL CORS] les requêtes sont activées.
   * Assurez-vous que la variable `/cache/headers` La configuration s’applique à `dispatcher.any` et le redémarrage du serveur web
   * Assurez-vous que le cache a été correctement effacé après toute modification de la configuration OSGi ou dispatcher.any.
* si nécessaire, vérifiez la présence des informations d’identification d’authentification sur la requête.

## Documents annexes

* [AEM fabrique de configuration OSGi pour les stratégies de partage des ressources cross-origin](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Partage des ressources cross-origin (W3C)](https://www.w3.org/TR/cors/)
* [Contrôle d’accès HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
