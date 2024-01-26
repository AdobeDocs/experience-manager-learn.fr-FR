---
title: Comment désactiver la mise en cache du réseau CDN
description: Découvrez comment désactiver la mise en cache des réponses HTTP dans le réseau de diffusion de contenu d’AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 116
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 6%

---

# Comment désactiver la mise en cache du réseau CDN

Découvrez comment désactiver la mise en cache des réponses HTTP dans le réseau de diffusion de contenu d’AEM as a Cloud Service. La mise en cache des réponses est contrôlée par `Cache-Control`, `Surrogate-Control`, ou `Expires` En-têtes de cache de réponse HTTP.

Ces en-têtes de cache sont généralement définis dans les configurations vhost du Dispatcher d’AEM à l’aide de `mod_headers`, mais peut également être défini dans du code Java™ personnalisé s’exécutant dans l’instance de publication AEM directement.

## Comportement de mise en cache par défaut

Examinez le comportement de mise en cache par défaut pour AEM de publication et d’auteur lors d’une [AEM Archétype de projet](./enable-caching.md#default-caching-behavior) Le projet d’AEM basé est déployé.

## Désactiver la mise en cache

La désactivation de la mise en cache peut avoir un impact négatif sur les performances de votre instance as a Cloud Service AEM. Soyez donc prudent lorsque vous désactivez le comportement de mise en cache par défaut.

Cependant, il existe certains scénarios dans lesquels vous pouvez désactiver la mise en cache, par exemple :

- Le développement d’une nouvelle fonctionnalité et l’affichage immédiat des modifications.
- Le contenu est sécurisé (destiné uniquement aux utilisateurs authentifiés) ou dynamique (panier, détails de la commande) et ne doit pas être mis en cache.

Pour désactiver la mise en cache, vous pouvez mettre à jour les en-têtes de cache de deux façons.

1. **Configuration vhost de Dispatcher :** Disponible uniquement pour AEM publication.
1. **Code Java™ personnalisé :** Disponible pour AEM Publication et création.

Examinons chacune de ces options.

### Configuration du vhost de Dispatcher

Cette option est l’approche recommandée pour désactiver la mise en cache, mais elle n’est disponible que pour AEM Publier. Pour mettre à jour les en-têtes de cache, utilisez le `mod_headers` module et `<LocationMatch>` dans le fichier vhost du serveur HTTP Apache. La syntaxe générale est la suivante :

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### Exemple

Pour désactiver la mise en cache du réseau CDN de la fonction **Types de contenu CSS** à des fins de dépannage, procédez comme suit.

Notez que, pour contourner le cache CSS existant, une modification du fichier CSS est nécessaire pour générer une nouvelle clé de cache pour le fichier CSS.

1. Dans votre projet AEM, recherchez le fichier vhost souhaité à partir de `dispatcher/src/conf.d/available_vhosts` répertoire .
1. Mettez à jour le vhost (par ex. `wknd.vhost`) comme suit :

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   Les fichiers vhost dans `dispatcher/src/conf.d/enabled_vhosts` sont **symlinks** aux fichiers dans `dispatcher/src/conf.d/available_vhosts` , veillez donc à créer des liens symboliques s’ils ne sont pas présents.
1. Déployez les modifications vhost dans l’environnement as a Cloud Service AEM souhaité à l’aide de la fonction [Cloud Manager - Pipeline de configuration de niveau web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) ou [Commandes RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Code Java™ personnalisé

Cette option est disponible pour AEM Publication et Auteur. Pour mettre à jour les en-têtes de cache, utilisez le `SlingHttpServletResponse` dans le code Java™ personnalisé (servlet Sling, filtre de servlet Sling). La syntaxe générale est la suivante :

```java
response.setHeader("Cache-Control", "private");
```
