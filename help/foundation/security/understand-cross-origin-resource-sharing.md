---
title: Comprendre le partage des ressources cross-origin (CORS) avec AEM
description: Le partage des ressources cross-origin (CORS) de Adobe Experience Manager facilite les propriétés web non-AEM pour effectuer des appels côté client vers AEM, authentifiés et non authentifiés, afin de récupérer du contenu ou d’interagir directement avec l’.
version: 6.3, 6,4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Sécurité
role: Developer
level: Intermediate
source-git-commit: 1c99c319fba5048904177fc82c43554b0cf0fc15
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 1%

---


# Comprendre le partage des ressources cross-origin ([!DNL CORS])

Le partage des ressources cross-origin Adobe Experience Manager ([!DNL CORS]) facilite les propriétés web non-AEM pour effectuer des appels côté client à AEM, authentifiés et non authentifiés, pour récupérer du contenu ou interagir directement avec l’.

## Configuration OSGi de la stratégie de partage des ressources cross-origin Adobe

Les configurations CORS sont gérées comme des usines de configuration OSGi dans AEM, chaque stratégie étant représentée comme une instance de la fabrique.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuration OSGi de la stratégie de partage des ressources cross-origin Adobe](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Sélection de stratégie

Une stratégie est sélectionnée en comparant la variable

* `Allowed Origin` avec l’en-tête  `Origin` de requête
* et `Allowed Paths` avec le chemin de requête.

La première stratégie correspondant à ces valeurs sera utilisée. Si aucune requête n’est trouvée, toute requête [!DNL CORS] sera refusée.

Si aucune stratégie n’est configurée du tout, les demandes [!DNL CORS] ne recevront pas de réponse, car le gestionnaire sera désactivé et donc refusé de manière efficace, à condition qu’aucun autre module du serveur ne réponde à [!DNL CORS].

### Propriétés Policy

#### [!UICONTROL Origines autorisées]

* `"alloworigin" <origin> | *`
* Liste des paramètres `origin` spécifiant les URI pouvant accéder à la ressource. Pour les demandes sans informations d’identification, le serveur peut spécifier * comme caractère générique, ce qui permet à toute origine d’accéder à la ressource. *Il n’est absolument pas recommandé d’utiliser  `Allow-Origin: *` en production, car cela permet à chaque site web étranger (c’est-à-dire à chaque attaquant) de faire des demandes qui, sans CORS, sont strictement interdites par les navigateurs.*

#### [!UICONTROL Origines autorisées (Regexp)]

* `"alloworiginregexp" <regexp>`
* Liste des expressions régulières `regexp` spécifiant les URI pouvant accéder à la ressource. *Les expressions régulières peuvent générer des correspondances inattendues si elles ne sont pas soigneusement créées, ce qui permet à un attaquant d’utiliser un nom de domaine personnalisé qui correspondrait également à la stratégie.* Il est généralement recommandé de disposer de stratégies distinctes pour chaque nom d’hôte d’origine spécifique, à l’aide de  `alloworigin`, même si cela signifie une configuration répétée des autres propriétés de stratégie. Les origines différentes ont tendance à avoir des cycles de vie et des exigences différents, ce qui profite d&#39;une séparation claire.

#### [!UICONTROL Chemins autorisés]

* `"allowedpaths" <regexp>`
* Liste des expressions régulières `regexp` spécifiant les chemins d’accès aux ressources pour lesquels la stratégie s’applique.

#### [!UICONTROL En-têtes exposés]

* `"exposedheaders" <header>`
* Liste des paramètres d’en-tête indiquant les en-têtes de requête auxquels les navigateurs sont autorisés à accéder.

#### [!UICONTROL Âge maximal]

* `"maxage" <seconds>`
* Un paramètre `seconds` indiquant la durée pendant laquelle les résultats d’une demande de pré-vol peuvent être mis en cache.

#### [!UICONTROL En-têtes pris en charge]

* `"supportedheaders" <header>`
* Liste des paramètres `header` indiquant quels en-têtes HTTP peuvent être utilisés lors de l’exécution de la requête réelle.

#### [!UICONTROL Méthodes autorisées]

* `"supportedmethods"`
* Liste des paramètres de méthode indiquant les méthodes HTTP pouvant être utilisées lors de l’exécution de la requête réelle.

#### [!UICONTROL Prend en charge les informations d’identification]

* `"supportscredentials" <boolean>`
* `boolean` indiquant si la réponse à la requête peut être exposée ou non au navigateur. Utilisé dans le cadre d’une réponse à une demande de pré-vol, cela indique si la demande réelle peut être effectuée à l’aide des informations d’identification.

### Exemples de configurations

Le site 1 est un scénario de base, anonymement accessible, en lecture seule où le contenu est consommé via des demandes [!DNL GET] :

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

À partir de la version 4.1.1+ de Dispatcher, les en-têtes de réponse peuvent être mis en cache. Cela permet de mettre en cache les en-têtes [!DNL CORS] avec les ressources demandées [!DNL CORS] tant que la demande est anonyme.

En règle générale, les mêmes considérations relatives à la mise en cache de contenu dans Dispatcher peuvent être appliquées à la mise en cache des en-têtes de réponse CORS au Dispatcher. Le tableau suivant définit le moment où les en-têtes [!DNL CORS] (et donc les demandes [!DNL CORS]) peuvent être mis en cache.

| Mise en cache | Environnement | État d’authentification | Explication |
|-----------|-------------|-----------------------|-------------|
| Non | Publication AEM | Authentifié | La mise en cache de Dispatcher sur l’auteur AEM est limitée aux ressources statiques non créées. Il est ainsi difficile et impossible de mettre en cache la plupart des ressources sur AEM Author, y compris les en-têtes de réponse HTTP. |
| Non | Publication AEM | Authentifié | Évitez de mettre en cache les en-têtes CORS sur les requêtes authentifiées. Cela s’aligne sur les conseils courants de ne pas mettre en cache les requêtes authentifiées, car il est difficile de déterminer comment l’état d’authentification/d’autorisation de l’utilisateur demandeur affectera la ressource diffusée. |
| Oui | Publication AEM | Anonyme | Les en-têtes de réponse des requêtes anonymes pouvant être mises en cache dans Dispatcher peuvent également être mis en cache, ce qui garantit que les futures requêtes CORS pourront accéder au contenu mis en cache. Toute modification de configuration CORS sur AEM Publish **doit** être suivie d’une invalidation des ressources mises en cache affectées. Les bonnes pratiques dictent les déploiements de code ou de configuration que le cache du Dispatcher est purgé, car il est difficile de déterminer quel contenu mis en cache peut être affecté. |

Pour permettre la mise en cache des en-têtes CORS, ajoutez la configuration suivante à tous les fichiers dispatcher.any de publication AEM pris en charge.

```
/cache { 
  ...
  /headers {
      "Origin",
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

N’oubliez pas de **redémarrer l’application du serveur web** après avoir apporté des modifications au fichier `dispatcher.any`.

Il est probable que l’effacement complet du cache sera nécessaire pour s’assurer que les en-têtes sont correctement mis en cache dans la requête suivante après une mise à jour de la configuration `/cache/headers`.

## Dépannage de CORS

La journalisation est disponible sous `com.adobe.granite.cors` :

* activer `DEBUG` pour afficher les détails sur les raisons du refus d’une requête [!DNL CORS] ;
* Activez `TRACE` pour afficher les détails sur toutes les requêtes transitant par le gestionnaire CORS.

### Conseils :

* Recréez manuellement les requêtes XHR à l’aide de curl, mais veillez à copier tous les en-têtes et détails, car chacun d’eux peut faire une différence. certaines consoles de navigateur permettent de copier la commande curl
* Vérifiez si la demande a été refusée par le gestionnaire CORS et non par l’authentification, le filtre de jeton CSRF, les filtres Dispatcher ou d’autres couches de sécurité.
   * Si le gestionnaire CORS répond avec 200, mais que l’en-tête `Access-Control-Allow-Origin` est absent de la réponse, passez en revue les journaux pour les refus sous [!DNL DEBUG] dans `com.adobe.granite.cors`
* Si la mise en cache du Dispatcher des requêtes [!DNL CORS] est activée
   * Assurez-vous que la configuration `/cache/headers` est appliquée à `dispatcher.any` et que le serveur web a bien été redémarré.
   * Assurez-vous que le cache a été correctement effacé après toute modification de la configuration OSGi ou dispatcher.any.
* si nécessaire, vérifiez la présence des informations d’identification d’authentification sur la requête.

## Documents annexes

* [AEM fabrique de configuration OSGi pour les stratégies de partage des ressources cross-origin](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Partage des ressources cross-origin (W3C)](https://www.w3.org/TR/cors/)
* [Contrôle d’accès HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
