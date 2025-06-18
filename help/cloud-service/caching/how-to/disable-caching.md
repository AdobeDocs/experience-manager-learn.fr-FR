---
title: Désactivation de la mise en cache du réseau CDN
description: Découvrez comment désactiver la mise en cache des réponses HTTP dans le réseau CDN d’AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: cf006f24abbc5aa4b91277b91d68538c41d33e15
workflow-type: ht
source-wordcount: '432'
ht-degree: 100%

---

# Désactivation de la mise en cache du réseau CDN

Découvrez comment désactiver la mise en cache des réponses HTTP dans le réseau CDN d’AEM as a Cloud Service. La mise en cache des réponses est contrôlée par par les en-têtes de cache de réponse HTTP `Cache-Control`, `Surrogate-Control` ou `Expires`.

Ces en-têtes de cache sont généralement définis dans les configurations vhost du Dispatcher d’AEM à l’aide de `mod_headers`, mais peuvent également être définis dans du code Java™ personnalisé s’exécutant dans l’instance de publication AEM directement.

## Comportement de mise en cache par défaut

La mise en cache des réponses HTTP dans le CDN d’AEM as a Cloud Service est contrôlée par les en-têtes de réponse HTTP suivants à partir des éléments `Cache-Control`, `Surrogate-Control` ou `Expires` d’origine.  Les réponses d’origine contenant des éléments `private`, `no-cache` ou `no-store` dans `Cache-Control` ne sont pas mises en cache.

Examinez le [comportement de mise en cache par défaut](./enable-caching.md#default-caching-behavior) pour l’instance de publication et de création AEM lors du déploiement d’un projet AEM basé sur l’archétype de projet AEM.


## Désactiver la mise en cache

La désactivation de la mise en cache peut avoir un impact négatif sur les performances de votre instance AEM as a Cloud Service. Faites donc preuve de prudence lorsque vous désactivez le comportement de mise en cache par défaut.

Cependant, il existe certains scénarios dans lesquels vous pouvez désactiver la mise en cache, par exemple :

- Vous développez une nouvelle fonctionnalité et vous voulez voir les changements immédiatement.
- Le contenu est sécurisé (destiné uniquement aux personnes authentifiées) ou dynamique (panier, informations sur la commande) et ne doit pas être mis en cache.

Pour désactiver la mise en cache, vous pouvez mettre à jour les en-têtes de cache de deux façons.

1. **Configuration vhost de Dispatcher :** disponible uniquement pour l’instance de publication AEM.
1. **Code Java™ personnalisé :** disponible pour les instances de publication et de création AEM.

Examinons chacune de ces options.

### Configuration vhost du Dispatcher

Cette option est l’approche recommandée pour désactiver la mise en cache, mais elle n’est disponible que pour l’instance de publication AEM. Pour mettre à jour les en-têtes de cache, utilisez le module `mod_headers` et la directive `<LocationMatch>` dans le fichier vhost du serveur Apache HTTP. La syntaxe générale se présente comme suit :

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the Browser and the CDN to not cache the response.
    Header always set Cache-Control "private"

    # Instructs only the CDN to not cache the response.
    Header always set Surrogate-Control "private"
</LocationMatch>
```

#### Exemple

Pour désactiver la mise en cache du réseau CDN des **types de contenu CSS** à des fins de dépannage, procédez comme suit.

Notez que, pour contourner le cache CSS existant, une modification du fichier CSS est nécessaire pour générer une nouvelle clé de cache pour le fichier CSS.

1. Dans votre projet AEM, recherchez le fichier vhost souhaité à partir du répertoire `dispatcher/src/conf.d/available_vhosts`.
1. Mettez à jour le fichier vhost (par exemple, `wknd.vhost`) comme suit :

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the Browser and the CDN to not cache the response.
       Header always set Cache-Control "private"
   </LocationMatch>
   ```

   Les fichiers vhost dans le répertoire `dispatcher/src/conf.d/enabled_vhosts` sont des **liens symboliques** vers des fichiers dans le répertoire `dispatcher/src/conf.d/available_vhosts`, veillez donc à créer des liens symboliques s’ils ne sont pas présents.
1. Déployez les modifications vhost dans l’environnement AEM as a Cloud Service souhaité à l’aide de [Cloud Manager – Pipeline de configuration de niveau web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=fr#web-tier-config-pipelines) ou des [Commandes du RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=fr#deploy-apache-or-dispatcher-configuration).

### Code Java™ personnalisé

Cette option est disponible pour les instances de publication et de création AEM. Pour mettre à jour les en-têtes de cache, utilisez l’objet `SlingHttpServletResponse` dans le code Java™ personnalisé (servlet Sling, filtre de servlet Sling). La syntaxe générale se présente comme suit :

```java
response.setHeader("Cache-Control", "private");
```
