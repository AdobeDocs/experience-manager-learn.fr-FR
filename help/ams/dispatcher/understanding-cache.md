---
title: Présentation de la mise en cache par Dispatcher
description: Découvrez comment le module de Dispatcher gère son cache.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '1747'
ht-degree: 100%

---

# Présentation de la mise en cache

[Table des matières](./overview.md)

[&lt;- Précédent : Explication des fichiers de configuration](./explanation-config-files.md)

Ce document explique comment se déroule la mise en cache de Dispatcher et comment elle peut être configurée.

## Mise en cache des répertoires

Nous utilisons les répertoires de cache par défaut suivants dans nos installations de référence.

- Création
   - `/mnt/var/www/author`
- Publication
   - `/mnt/var/www/html`

Lorsque chaque requête traverse Dispatcher, les requêtes suivent les règles configurées pour conserver une version locale mise en cache de la réponse des éléments éligibles.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Nous gardons intentionnellement la charge de travail publiée distincte de celle de l’instance de création, car lorsqu’Apache recherche un fichier dans DocumentRoot, il ne sait pas de quelle instance AEM il provient. Ainsi, même si le cache est désactivé dans la batterie de serveurs de création, si la DocumentRoot de l’instance de création est identique à l’instance de publication, il diffusera les fichiers du cache s’ils sont disponibles. En d’autres termes, vous diffuserez des fichiers d’instance de création à partir du cache publié et vous fournirez à vos visiteurs et visiteuses une expérience combinée très difficile qui ne leur correspondra pas.

Conserver des répertoires DocumentRoot distincts pour différents contenus publiés est également fortement déconseillé. Vous devrez créer plusieurs éléments remis en cache qui ne diffèrent pas entre les sites comme les clientlibs et vous devrez configurer un agent de vidage de réplication pour chaque DocumentRoot que vous configurez. Augmentation de la quantité de vidage supplémentaire à chaque activation de la page. Utilisez l’espace de noms des fichiers et leurs chemins d’accès entièrement mis en cache et évitez plusieurs DocumentRoot pour les sites publiés.
</div>

## Fichiers de configuration

Dispatcher contrôle les éléments qualifiés pour la mise en cache dans la section `/cache {` de tout fichier de batterie.
Dans les batteries de configuration de référence AMS, vous trouverez nos inclusions comme illustré ci-dessous :


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Lors de la création des règles pour ce qui doit être mis en cache ou non, reportez-vous à la documentation [ici](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#configuring-the-dispatcher-cache-cache).


## Mise en cache de l’instance de création

Nous avons vu beaucoup d’applications dans lesquelles le contenu de l’instance de création n’est pas mis en cache.
Les performances s’en trouvent largement dégradées, tout comme la réactivité des auteurs et des autrices.

Parlons de la stratégie utilisée pour configurer notre batterie de serveurs de création pour qu’elle soit correctement mise en cache.

Voici une section d’instance de création de base `/cache {` de notre fichier de batterie d’instance de création :


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    } 
    /invalidate { 
        /0000 { 
            /glob "*" 
            /type "allow" 
        } 
    } 
    /allowedClients { 
        /0000 { 
            /glob "*.*.*.*" 
            /type "deny" 
        } 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

Notons ici que la `/docroot` est définie sur le répertoire du cache de l’instance de création.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Assurez-vous que la `DocumentRoot` dans le fichier `.vhost` de l’instance de création correspond au paramètre de la batterie de serveurs `/docroot`.
</div>

L’instruction d’inclusion des règles de cache inclut le fichier `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` qui contient les règles suivantes :

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

Dans un scénario de création, le contenu change en permanence et volontairement. Mieux vaut mettre en cache les éléments qui ne changeront pas fréquemment.
Nous avons des règles pour mettre les `/libs` en cache car elles font partie de l’installation de base d’AEM et changent jusqu’à ce que vous ayez installé un pack de services, un pack de correctifs cumulatifs, une mise à niveau ou un correctif. La mise en cache de ces éléments a donc un sens et bénéficie énormément de l’expérience de création des personnes qui utilisent le site.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Gardez à l’esprit que ces règles mettent également en cache les <b>`/apps`</b>, c’est là que réside le code d’application personnalisé. Si vous développez votre code sur cette instance, il sera très perturbant d’enregistrer votre fichier et de ne pas voir s’il est visible dans l’interface utilisateur parce qu’il s’agit d’une copie mise en cache. Si vous effectuez un déploiement de votre code dans AEM, cette opération ne sera pas fréquente non plus et une partie des étapes du déploiement consistera à vider le cache de l’instance de création. Encore une fois, l’avantage est important, et l’exécution de votre code pouvant être mis en cache pour les personnes finales s’en verra accélérée.
</div>


## ServeOnStale (ou Serve on Stale/SOS)

C’est l’une des merveilleuses fonctionnalités du Dispatcher. Si l’éditeur est en cours de chargement ou ne répond plus, il génère généralement un code de réponse http 502 ou 503. Si cela se produit et que cette fonction est activée, le Dispatcher est invité à continuer à servir le contenu qui se trouve encore dans le cache, même s’il ne s’agit pas d’une nouvelle copie. Il est préférable de diffuser quelque chose si vous l’avez plutôt que de simplement afficher un message d’erreur qui n’offre aucune fonctionnalité.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Souvenez-vous que si le moteur de rendu de l’éditeur a un délai d’expiration du socket ou un message d’erreur 500, cette fonctionnalité ne se déclenche pas. Si AEM est simplement inaccessible, cette fonctionnalité ne fait rien.
</div>

Ce paramètre peut être défini dans n’importe quelle batterie, mais il est logique de l’appliquer uniquement aux fichiers de batterie de publication. Voici un exemple de syntaxe de la fonctionnalité activée dans un fichier de batterie :

```
/cache { 
    /serveStaleOnError "1"
```

## Mise en cache de pages avec des arguments/paramètres de requête

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Parmi les comportements normaux du module Dispatcher, si une requête comporte un paramètre de requête dans l’URI (généralement affiché comme `/content/page.html?myquery=value`), il ne met pas le fichier en cache et accède directement à l’instance AEM. Cette requête est considérée comme une page dynamique et ne doit pas être mise en cache. Cela peut avoir des effets néfastes sur l’efficacité du cache.
</div>
<br/>

Consultez cet [article](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) pour connaître l’impact des paramètres de requête importants sur les performances de votre site.

Par défaut, il faut définir les règles `ignoreUrlParams` pour autoriser `*`.  Cela signifie que tous les paramètres de requête sont ignorés et permettent à toutes les pages d’être mises en cache, quels que soient les paramètres utilisés.

Voici un exemple où une personne a créé un mécanisme de référence de lien profond sur les médias sociaux qui utilise la référence d’argument dans l’URI pour savoir d’où provient la personne.

<b>Exemple ignorable :</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

La page peut être mise en cache à 100 %, mais elle ne le fait pas car les arguments sont présents.
Configurer votre `ignoreUrlParams` en tant que liste autorisée vous aidera à résoudre ce problème :

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Désormais, lorsque Dispatcher voit la demande, il ignore le fait que la demande a le paramètre `query` de la référence `?` et met toujours la page en cache.

<b>Exemple dynamique :</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Gardez à l’esprit que si vous disposez de paramètres de requête qui modifient le rendu de sortie d’une page, vous devez alors les exclure de la liste ignorée et empêcher la page d’être mise en cache à nouveau.  Par exemple, une page de recherche qui utilise un paramètre de requête modifie le code HTML brut rendu.

Voici donc la source HTML de chaque recherche :

`/search.html?q=fruit` :

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

`/search.html?q=vegetables` :

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

Si vous avez d’abord consulté `/search.html?q=fruit`, il mettrait en cache le code html avec les résultats qui montrent des fruits.

Ensuite, vous consultez `/search.html?q=vegetables` en deuxième, mais vous obtenez les résultats de fruits.
En effet, le paramètre de requête de `q` est ignoré en ce qui concerne la mise en cache.  Pour éviter ce problème, vous devez prendre note des pages qui effectuent le rendu de codes HTML différents en fonction des paramètres de requête et qui refusent la mise en cache de ces derniers.

Exemple :

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Les pages qui utilisent des paramètres de requête via JavaScript continueront à fonctionner entièrement en ignorant les paramètres de cette configuration.  Parce qu’ils ne modifient pas le fichier HTML au repos.  Ils utilisent JavaScript pour mettre à jour les navigateurs en temps réel sur le navigateur local.  En d’autres termes, si vous utilisez les paramètres de requête avec JavaScript, il est très probable que vous puissiez ignorer ce paramètre pour la mise en cache des pages.  Autorisez cette page à être mise en cache et profitez à nouveau des performances.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Le suivi de ces pages nécessite une certaine maintenance, mais cela en vaut bien la peine pour retrouver les performances.  Vous pouvez demander à votre CSE de réaliser un rapport sur le trafic de vos sites web, afin de vous donner une liste de toutes les pages utilisant des paramètres de requête au cours des 90 derniers jours, pour que vous puissiez analyser et vous assurer que vous savez quelles pages examiner et quels paramètres de requête ne pas ignorer.
</div>
<br/>

## Mise en cache des en-têtes de réponse

Il est assez évident que Dispatcher met en cache les pages `.html` et les clientlibs (c’est-à-dire `.js` et `.css`), mais saviez-vous qu’il peut également mettre en cache des en-têtes de réponse spécifiques le long du contenu dans un fichier portant le même nom, mais avec une extension de fichier `.h`. Cela autorise l’accès de la prochaine réponse non seulement au contenu, mais aussi aux en-têtes de réponse qui doivent l’accompagner depuis le cache.

AEM peut gérer bien plus que le codage UTF-8.

Parfois, les éléments comportent des en-têtes spéciaux qui aident à contrôler les détails de codage du TTL du cache et les horodatages de dernière modification.

Lorsqu’elles sont mises en cache, ces valeurs sont supprimées par défaut et le serveur web Apache httpd s’acquitte de sa propre tâche de traitement de la ressource avec ses méthodes de gestion de fichiers normales, qui sont normalement limitées aux suppositions de type MIME basées sur les extensions de fichier.

Si le Dispatcher met en cache la ressource et les en-têtes souhaités, vous pouvez exposer l’expérience appropriée et vous assurer que tous les détails atteignent le navigateur des clientes et clients.

Voici un exemple de batterie de serveurs avec les en-têtes à mettre en cache spécifiés :

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


Dans l’exemple, ils ont configuré AEM pour diffuser des en-têtes que le réseau CDN recherche pour savoir quand invalider son cache. En d’autres termes, AEM peut désormais indiquer correctement les fichiers qui sont invalidés en fonction des en-têtes.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Gardez à l’esprit que vous ne pouvez pas utiliser d’expressions régulières ou de correspondance glob. Il s’agit d’une liste littérale des en-têtes à mettre en cache. Placez uniquement dans une liste les en-têtes littéraux que vous souhaitez mettre en cache.
</div>


## Invalider automatiquement le délai de grâce

Sur les systèmes AEM qui ont une activité importante de la part des créateurs et créatices qui effectuent de nombreuses activations de page, vous pouvez avoir une condition de concurrence où des invalidations répétées se produisent. Les requêtes de purge répétées ne sont pas nécessaires et vous pouvez créer une certaine tolérance pour ne pas répéter une purge tant que le délai de grâce n’a pas été effacé.

### Exemple de fonctionnement :

Si vous avez 5 demandes d’invalidation de `/content/exampleco/en/`, elles se produisent toutes dans un délai de 3 secondes.

Si cette fonction est désactivée, vous invalidez le répertoire du cache `/content/exampleco/en/` 5 fois.

Si cette fonction est activée et définie sur 5 secondes, elle invalide le répertoire du cache `/content/exampleco/en/` <b>une fois</b>.

Voici un exemple de syntaxe de cette fonctionnalité configurée pour un délai de grâce de 5 secondes :

```
/cache { 
    /gracePeriod "5"
```

## Invalidation basée sur TTL

Le module Dispatcher possède de nouvelles fonctionnalités, les options d’invalidation basée sur `Time To Live (TTL)` sur les éléments mis en cache. Lorsqu’un élément est mis en cache, il recherche la présence d’en-têtes de contrôle du cache et génère un fichier dans le répertoire du cache portant le même nom et une extension `.ttl`.

Voici un exemple de la fonctionnalité configurée dans le fichier de configuration de la batterie :

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>
Gardez à l’esprit qu’AEM doit quand même être configuré pour envoyer des en-têtes TTL pour que Dispatcher les respecte. Si vous activez cette fonction, seul le Dispatcher peut savoir quand supprimer les fichiers pour lesquels AEM a envoyé des en-têtes de contrôle du cache. Si AEM ne commence pas à envoyer des en-têtes TTL, Dispatcher ne fera rien de spécial.
</div>

## Règles de filtrage du cache

Voici un exemple de configuration de base pour les éléments à mettre en cache sur une instance de publication :

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

Nous voulons rendre notre site publié aussi gourmand que possible et tout mettre en cache.

Si des éléments rompent l’expérience lors de la mise en cache, vous pouvez ajouter des règles pour supprimer l’option de mise en cache de cet élément. Comme vous pouvez le voir dans l’exemple ci-dessus, les jetons csrf ne doivent jamais être mis en cache et ont été exclus. Vous trouverez plus de détails sur la rédaction de ces règles [ici](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#configuring-the-dispatcher-cache-cache).

[Suivant -> Utiliser et comprendre les variables](./variables.md)
