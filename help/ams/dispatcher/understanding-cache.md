---
title: Présentation de la mise en cache par Dispatcher
description: Découvrez comment le module de Dispatcher fonctionne son cache.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---


# Compréhension de la mise en cache

[Table des matières](./overview.md)

[&lt;- Précédent : Explication des fichiers de configuration](./explanation-config-files.md)

Ce document explique comment la mise en cache de Dispatcher se produit et comment elle peut être configurée.

## Mise en cache des répertoires

Nous utilisons les répertoires de cache par défaut suivants dans nos installations de base.

- Création
   - `/mnt/var/www/author`
- Éditeur
   - `/mnt/var/www/html`

Lorsque chaque requête traverse Dispatcher, les requêtes suivent les règles configurées pour conserver une version mise en cache localement en réponse aux éléments éligibles.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Nous gardons intentionnellement la charge de travail publiée distincte de celle de l’auteur, car lorsqu’Apache recherche un fichier dans DocumentRoot, il ne sait pas de quelle instance AEM il provient. Ainsi, même si le cache est désactivé dans la ferme de serveurs de création, si le DocumentRoot de l’auteur est identique à l’éditeur, il diffusera les fichiers du cache lorsqu’il est présent. En d’autres termes, vous diffuserez des fichiers d’auteur pour à partir du cache publié et vous fournirez une expérience de correspondance très difficile pour vos visiteurs.

Conserver des répertoires DocumentRoot distincts pour différents contenus publiés est également une très mauvaise idée. Vous devrez créer plusieurs éléments remis en cache qui ne diffèrent pas entre les sites comme les clientlibs et vous devrez configurer un agent de vidage de réplication pour chaque DocumentRoot que vous configurez. Augmentation de la quantité de vidage par-dessus la tête à chaque activation de page. Utilisez l’espace de noms des fichiers et leurs chemins d’accès entièrement mis en cache et évitez plusieurs DocumentRoot pour les sites publiés.
</div>

## Fichiers de configuration

Dispatcher contrôle les éléments qualifiés de pouvant être mis en cache dans la variable `/cache {` de tout fichier de ferme. 
Dans les fermes de configuration de ligne de base AMS, vous trouverez nos inclusions comme illustré ci-dessous :


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Lors de la création des règles pour ce qui doit être mis en cache ou non, reportez-vous à la documentation [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Création de mise en cache

Il y a beaucoup d’implémentations que nous avons vues où les gens ne mettent pas en cache le contenu de l’auteur. 
Ils passent à côté d’une énorme mise à niveau des performances et de la réactivité à leurs auteurs.

Parlons de la stratégie utilisée pour configurer notre ferme de serveurs de création pour qu’elle soit correctement mise en cache.

Voici un auteur de base `/cache {` de notre fichier de ferme d’auteur :


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

Ce qu&#39;il faut noter ici, c&#39;est que le `/docroot` est défini sur le répertoire du cache de l’auteur.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Assurez-vous que les `DocumentRoot` dans le rapport `.vhost` correspond aux fermes de serveurs `/docroot` parameter
</div>

L’instruction d’inclusion des règles de cache inclut le fichier . `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` qui contient les règles suivantes :

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

Dans un scénario de création, le contenu change tout le temps et volontairement. Vous souhaitez uniquement mettre en cache les éléments qui ne changeront pas fréquemment.
Nous avons des règles à mettre en cache. `/libs` car ils font partie de l’installation de base d’AEM et changent jusqu’à ce que vous ayez installé un Service Pack, un Cumulative Fix Pack, une mise à niveau ou un correctif. La mise en cache de ces éléments a donc un sens et bénéficie énormément de l’expérience de création des utilisateurs finaux qui utilisent le site.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Gardez à l’esprit que ces règles mettent également en cache <b>`/apps`</b> c’est là que réside le code d’application personnalisé. Si vous développez votre code sur cette instance, il s’avère très déroutant lorsque vous enregistrez votre fichier et que vous ne voyez pas s’il est reflété dans l’interface utilisateur en raison de la diffusion d’une copie mise en cache. L’intention ici est que si vous effectuez un déploiement de votre code dans AEM, il serait également rare et qu’une partie de vos étapes de déploiement devrait être d’effacer le cache de l’auteur. Encore une fois, l’avantage est énorme, ce qui accélère l’exécution de votre code pouvant être mis en cache pour les utilisateurs finaux.
</div>


## ServeOnStale (AKA Serve on Stale / SOS)

C’est l’un de ces atouts d’une fonctionnalité de Dispatcher. Si l’éditeur est en cours de chargement ou ne répond plus, il génère généralement un code de réponse http 502 ou 503. Si cela se produit et que cette fonction est activée, Dispatcher est invité à continuer à servir le contenu qui se trouve encore dans le cache comme un effort, même s’il ne s’agit pas d’une nouvelle copie. Il est préférable de diffuser quelque chose si vous l’avez plutôt que de simplement afficher un message d’erreur qui n’offre aucune fonctionnalité.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Gardez à l’esprit que si le moteur de rendu de l’éditeur a un délai d’expiration du socket ou un message d’erreur 500, cette fonctionnalité ne se déclenche pas. Si AEM est simplement inaccessible, cette fonctionnalité ne fait rien
</div>

Ce paramètre peut être défini dans n’importe quelle ferme, mais il est logique de l’appliquer uniquement aux fichiers de ferme de publication. Voici un exemple de syntaxe de la fonctionnalité activée dans un fichier de ferme :

```
/cache { 
    /serveStaleOnError "1"
```

## Mise en cache de pages avec des arguments/paramètres de requête

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

L’un des comportements normaux du module de Dispatcher est que si une requête comporte un paramètre de requête dans l’URI (généralement affiché comme `/content/page.html?myquery=value`) ignore la mise en cache du fichier et accède directement à l’instance AEM. Cette requête est considérée comme une page dynamique et ne doit pas être mise en cache. Cela peut avoir des effets néfastes sur l’efficacité du cache.
</div>
<br/>

Voir [article](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) montrant l’impact des paramètres de requête importants sur les performances de votre site.

Par défaut, vous souhaitez définir la variable `ignoreUrlParams` règles d’autorisation `*`.  Cela signifie que tous les paramètres de requête sont ignorés et permettent à toutes les pages d’être mises en cache, quels que soient les paramètres utilisés.

Voici un exemple où quelqu’un a créé un mécanisme de référence de lien profond sur les médias sociaux qui utilise la référence d’argument dans l’URI pour savoir d’où provient la personne.

<b>Exemple ignorable :</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

La page peut être mise en cache à 100 %, mais elle ne le fait pas car les arguments sont présents. 
Configuration de `ignoreUrlParams` en tant que liste autorisée, vous pouvez résoudre ce problème :

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Désormais, lorsque Dispatcher voit la demande, il ignore le fait que la demande a la valeur `query` du paramètre `?` référencer et mettre toujours la page en cache

<b>Exemple dynamique :</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Gardez à l’esprit que si vous disposez de paramètres de requête qui font une modification de page, la sortie est rendue. Vous devez alors les exclure de la liste ignorée et rendre la page inaccessible à nouveau.  Par exemple, une page de recherche qui utilise un paramètre de requête modifie le code HTML brut rendu.

Voici donc la source HTML de chaque recherche :

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

Si vous avez visité `/search.html?q=fruit` d’abord, il mettrait en cache le code html avec les résultats qui montrent fruit.

Ensuite, rendez-vous sur `/search.html?q=vegetables` deuxièmement, mais cela montrerait les résultats de fruit.
En effet, le paramètre de requête de `q` est ignorée en ce qui concerne la mise en cache.  Pour éviter ce problème, vous devez prendre note des pages qui effectuent le rendu de différents HTMLS en fonction des paramètres de requête et qui refusent la mise en cache de ces derniers.

Exemple :

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Les pages qui utilisent des paramètres de requête via JavaScript continueront à fonctionner entièrement en ignorant les paramètres de ce paramètre.  Parce qu&#39;ils ne modifient pas le fichier HTML au repos.  Ils utilisent JavaScript pour mettre à jour les navigateurs en temps réel sur le navigateur local.  En d’autres termes, si vous utilisez les paramètres de requête avec JavaScript, il est très probable que vous puissiez ignorer ce paramètre pour la mise en cache des pages.  Autorisez cette page à mettre en cache et à profiter de l’amélioration des performances !

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Le suivi de ces pages nécessite une certaine maintenance, mais cela vaut la peine d’être amélioré en termes de performances.  Vous pouvez demander à l’ingénieur du service client d’exécuter un rapport sur le trafic de vos sites web afin de vous donner une liste de toutes les pages utilisant des paramètres de requête au cours des 90 derniers jours pour que vous puissiez analyser et vous assurer que vous savez quelles pages examiner et quels paramètres de requête ne pas ignorer.
</div>
<br/>

## Mise en cache des en-têtes de réponse

Il est assez évident que Dispatcher met en cache `.html` pages et clientlibs (c.-à-d. `.js`, `.css`), mais saviez-vous qu’il peut également mettre en cache des en-têtes de réponse spécifiques le long du contenu dans un fichier portant le même nom, mais avec un `.h` extension de fichier. Cela permet à la prochaine réponse non seulement au contenu, mais aussi aux en-têtes de réponse qui doivent l’accompagner depuis le cache.

AEM peut gérer plus que le codage UTF-8.

Parfois, les éléments comportent des en-têtes spéciaux qui aident à contrôler les détails de codage du délai d’activation du cache et les horodatages de dernière modification.

Ces valeurs lorsqu’elles sont mises en cache sont supprimées par défaut et le serveur web Apache httpd s’acquitte de sa propre tâche de traitement de la ressource avec ses méthodes de gestion de fichiers normales, qui sont normalement limitées aux suppositions de type mime basées sur les extensions de fichier.

Si Dispatcher met en cache la ressource et les en-têtes souhaités, vous pouvez exposer l’expérience appropriée et vous assurer que tous les détails l’atteignent dans le navigateur des clients.

Voici un exemple de ferme de serveurs avec les en-têtes à mettre en cache spécifiés :

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


Dans l’exemple, ils ont configuré AEM pour diffuser des en-têtes que le réseau de diffusion de contenu recherche pour savoir quand invalider son cache. En d’autres termes, AEM peut désormais dicter correctement les fichiers qui sont invalidés en fonction des en-têtes.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Gardez à l’esprit que vous ne pouvez pas utiliser d’expressions régulières ou de correspondance glob. C&#39;est une liste littérale des en-têtes à mettre en cache. Placez uniquement dans une liste des en-têtes littéraux que vous souhaitez qu’il mette en cache.
</div>


## Invalidation automatique de la période de grâce

Sur les systèmes d’AEM qui ont une activité importante de la part des auteurs qui effectuent beaucoup d’activations de page, vous pouvez avoir une condition de concurrence où des invalidations répétées se produisent. Les requêtes de purge répétées ne sont pas nécessaires et vous pouvez créer une certaine tolérance pour ne pas répéter une purge tant que la période de grâce n’a pas été effacée.

### Exemple de fonctionnement :

Si vous avez 5 demandes à invalider `/content/exampleco/en/` tout se passe dans un délai de 3 secondes.

Avec cette fonction désactivée, vous invalidez le répertoire du cache. `/content/exampleco/en/` 5 fois

Avec cette fonction activée et définie sur 5 secondes, elle invaliderait le répertoire du cache. `/content/exampleco/en/` <b>once</b>

Voici un exemple de syntaxe de cette fonctionnalité configurée pour une période de grâce de 5 secondes :

```
/cache { 
    /gracePeriod "5"
```

## Invalidation basée sur TTL

Une nouvelle fonctionnalité du module de Dispatcher a été `Time To Live (TTL)` options d’invalidation basées sur les éléments mis en cache. Lorsqu’un élément est mis en cache, il recherche la présence d’en-têtes de contrôle du cache et génère un fichier dans le répertoire du cache portant le même nom et une `.ttl` extension .

Voici un exemple de la fonctionnalité configurée dans le fichier de configuration de la ferme :

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>
Gardez à l’esprit que AEM doit toujours être configuré pour envoyer des en-têtes TTL pour que Dispatcher les honore. Si vous activez cette fonction, seul le Dispatcher peut savoir quand supprimer les fichiers pour lesquels AEM a envoyé des en-têtes de contrôle du cache. Si AEM ne commence pas à envoyer des en-têtes TTL, Dispatcher ne fera rien de spécial ici.
</div>

## Règles de filtrage du cache

Voici un exemple de configuration de base pour laquelle les éléments à mettre en cache sur un éditeur :

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

Si des éléments rompent l’expérience lors de la mise en cache, vous pouvez ajouter des règles pour supprimer l’option de mise en cache de cet élément. Comme vous pouvez le voir dans l’exemple ci-dessus, les jetons csrf ne doivent jamais être mis en cache et ont été exclus. Vous trouverez plus de détails sur la rédaction de ces règles. [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Suivant -> Utilisation et compréhension des variables](./variables.md)