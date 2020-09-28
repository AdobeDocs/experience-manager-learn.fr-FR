---
title: Comprendre le partage des ressources entre Origines (CORS) avec les AEM
description: Le Adobe Experience Manager Cross-Origine Resource Sharing (CORS) permet aux propriétés web non-AEM d’effectuer des appels côté client vers AEM, authentifiés et non authentifiés, pour récupérer du contenu ou interagir directement avec les AEM.
version: 6.3, 6,4, 6.5
sub-product: fondation, services de contenu, sites
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# Comprendre le partage des ressources entre Origines ([!DNL CORS])

L’activation du partage des ressources entre Origines ([!DNL CORS]) facilite les propriétés web non-AEM pour lancer des appels côté client à AEM, authentifiés ou non, pour récupérer le contenu ou interagir directement avec les AEM.

## Configuration OSGi de la stratégie de partage des ressources entre Origines de granit d&#39;Adobe

Les configurations CORS sont gérées comme des usines de configuration OSGi en AEM, chaque stratégie étant représentée comme une instance de l&#39;usine.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuration OSGi de la stratégie de partage des ressources entre Origines de granit d&#39;Adobe](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Sélection de stratégie

Une stratégie est sélectionnée en comparant la variable

* `Allowed Origin` avec l’en-tête `Origin` de requête
* et `Allowed Paths` avec le chemin d’accès à la requête.

La première stratégie correspondant à ces valeurs sera utilisée. Si aucune requête n&#39;est trouvée, toute [!DNL CORS] requête sera refusée.

Si aucune stratégie n’est configurée du tout, [!DNL CORS] les requêtes ne recevront pas de réponse car le gestionnaire sera désactivé et donc refusé de manière efficace - tant qu’aucun autre module du serveur ne répond [!DNL CORS].

### Propriétés Policy

#### [!UICONTROL Origines autorisées]

* `"alloworigin" <origin> | *`
* Liste de `origin` paramètres spécifiant les URI qui peuvent accéder à la ressource. Pour les requêtes sans informations d’identification, le serveur peut spécifier * comme caractère générique, ce qui permet à toute origine d’accéder à la ressource. *Il n&#39;est absolument pas recommandé d&#39;utiliser`Allow-Origin: *`en production car il permet à chaque site Web étranger (c.-à-d. attaquant) de faire des demandes que sans CORS sont strictement interdits par les navigateurs.*

#### [!UICONTROL Origines autorisées (Regexp)]

* `"alloworiginregexp" <regexp>`
* Liste d&#39;expressions `regexp` régulières spécifiant les URI qui peuvent accéder à la ressource. *Les expressions régulières peuvent conduire à des correspondances involontaires si elles ne sont pas créées avec soin, ce qui permet à un attaquant d’utiliser un nom de domaine personnalisé qui correspondrait également à la stratégie.* Il est généralement recommandé d’avoir des stratégies distinctes pour chaque nom d’hôte d’origine spécifique, en utilisant `alloworigin`, même si cela signifie une configuration répétée des autres propriétés de stratégie. Différentes origines ont tendance à avoir des cycles de vie et des exigences différents, ce qui profite d&#39;une séparation claire.

#### [!UICONTROL Chemins autorisés]

* `"allowedpaths" <regexp>`
* Liste d&#39;expressions `regexp` régulières spécifiant les chemins de ressources pour lesquels la stratégie s&#39;applique.

#### [!UICONTROL En-têtes exposés]

* `"exposedheaders" <header>`
* Liste des paramètres d’en-tête indiquant les en-têtes de demande auxquels les navigateurs sont autorisés à accéder.

#### [!UICONTROL Âge maximal]

* `"maxage" <seconds>`
* Un `seconds` paramètre indiquant la durée pendant laquelle les résultats d&#39;une demande de pré-vol peuvent être mis en cache.

#### [!UICONTROL En-têtes pris en charge]

* `"supportedheaders" <header>`
* Liste de `header` paramètres indiquant quels en-têtes HTTP peuvent être utilisés lors de l’exécution de la requête réelle.

#### [!UICONTROL Méthodes autorisées]

* `"supportedmethods"`
* Liste des paramètres de méthode indiquant les méthodes HTTP qui peuvent être utilisées lors de la demande réelle.

#### [!UICONTROL Prend en charge les informations d&#39;identification]

* `"supportscredentials" <boolean>`
* Indique `boolean` si la réponse à la demande peut être exposée au navigateur. Lorsqu&#39;elle est utilisée dans le cadre d&#39;une réponse à une demande de pré-vol, cela indique si la demande réelle peut être faite à l&#39;aide d&#39;informations d&#39;identification.

### Exemples de configuration

Le site 1 est un scénario de base, anonymement accessible, en lecture seule où le contenu est consommé via [!DNL GET] des requêtes :

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

Le site 2 est plus complexe et nécessite des demandes autorisées et non sécuritaires :

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

## Problèmes de mise en cache du répartiteur et configuration {#dispatcher-caching-concerns-and-configuration}

Les en-têtes de réponse Dispatcher 4.1.1+ peuvent être mis en cache. Cela permet de mettre en cache [!DNL CORS] les en-têtes avec les ressources [!DNL CORS]demandées, tant que la demande est anonyme.

En règle générale, les mêmes considérations relatives à la mise en cache du contenu au répartiteur peuvent être appliquées à la mise en cache des en-têtes de réponse CORS au répartiteur. Le tableau suivant définit quand [!DNL CORS] les en-têtes (et donc [!DNL CORS] les requêtes) peuvent être mis en cache.

| Catchable | Environnement | État d’authentification | Explication |
|-----------|-------------|-----------------------|-------------|
| Non | Publication AEM | Authentifié | La mise en cache du répartiteur sur AEM Author est limitée aux ressources statiques non créées. Cela rend difficile et difficile la mise en cache de la plupart des ressources sur AEM Author, y compris les en-têtes de réponse HTTP. |
| Non | Publication AEM | Authentifié | Evitez de mettre en cache les en-têtes CORS sur les requêtes authentifiées. Cela s’aligne sur les consignes courantes de non-mise en cache des requêtes authentifiées, car il est difficile de déterminer comment l’état d’authentification/d’autorisation de l’utilisateur demandeur aura un effet sur la ressource fournie. |
| Oui | Publication AEM | Anonyme | Les en-têtes de réponse des requêtes anonymes pouvant être mis en cache dans le répartiteur peuvent également être mis en cache, ce qui permet aux futures requêtes CORS d’accéder au contenu mis en cache. Toute modification de la configuration CORS sur AEM Publish **doit** être suivie d’une invalidation des ressources mises en cache affectées. Les bonnes pratiques dictent les déploiements de code ou de configuration du cache du répartiteur, car il est difficile de déterminer quel contenu mis en cache peut être affecté. |

Pour autoriser la mise en cache des en-têtes CORS, ajoutez la configuration suivante à tous les fichiers AEM Publish dispatcher.any pris en charge.

```
/cache { 
  ...
  /headers {
      "Access-Control-Allow-Origin",
      "Access-Control-Expose-Headers",
      "Access-Control-Max-Age",
      "Access-Control-Allow-Credentials",
      "Access-Control-Allow-Methods",
      "Access-Control-Allow-Headers"
  }
  ...
}
```

Pensez à **redémarrer l’application** de serveur Web après avoir apporté des modifications au `dispatcher.any` fichier.

Il est probable que l’effacement complet du cache sera nécessaire pour s’assurer que les en-têtes sont correctement mis en cache lors de la demande suivante après une mise à jour de la `/headers` configuration.

## Résolution des problèmes de CORS

La journalisation est disponible sous `com.adobe.granite.cors`:

* permettre `DEBUG` d’afficher les détails sur les raisons du refus d’une [!DNL CORS] demande
* permet `TRACE` d’afficher les détails sur toutes les requêtes qui passent par le gestionnaire CORS.

### Conseils :

* recréez manuellement les requêtes XHR à l’aide de l’instruction curl, mais veillez à copier tous les en-têtes et les détails, car chacun d’eux peut faire la différence ; certaines consoles de navigateur permettent de copier la commande curl
* Vérifiez si la demande a été refusée par le gestionnaire CORS et non par l’authentification, le filtre de jeton CSRF, les filtres du répartiteur ou d’autres couches de sécurité.
   * Si le gestionnaire CORS répond par 200, mais que `Access-Control-Allow-Origin` l’en-tête est absent sur la réponse, passez en revue les journaux pour les refus sous [!DNL DEBUG] la section `com.adobe.granite.cors`
* Si la mise en cache du répartiteur des [!DNL CORS] requêtes est activée
   * Assurez-vous que la `/headers` configuration est appliquée `dispatcher.any` et que le serveur Web a bien été redémarré.
   * Assurez-vous que le cache a été correctement effacé après toute modification de la configuration d&#39;OSGi ou de dispatcher.any.
* si nécessaire, vérifiez la présence des informations d’identification d’authentification sur la demande.

## Documents de support

* [aem usine de configuration OSGi pour les stratégies de partage de ressources entre Origines](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Partage des ressources entre Origines (W3C)](https://www.w3.org/TR/cors/)
* [CONTRÔLE D&#39;ACCÈS HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
