---
title: '"Chapitre 1 - Concepts, modèles et antimodèles du Dispatcher"'
description: Ce chapitre présente brièvement l’historique et les mécanismes de Dispatcher et explique en quoi cela influence la conception des composants par un développeur d’AEM.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
source-git-commit: 67e55e92cf95e03388ab3de49eff5a80786fb3a7
workflow-type: tm+mt
source-wordcount: '17487'
ht-degree: 0%

---


# Chapitre 1 - Concepts, modèles et antimodèles du Dispatcher

## Présentation

Ce chapitre présente brièvement l’historique et les mécanismes de Dispatcher et explique en quoi cela influence la conception des composants par un développeur d’AEM.

## Pourquoi les développeurs doivent-ils se préoccuper des infrastructures ?

Dispatcher est une partie essentielle de la plupart des installations, voire de toutes les installations AEM. Vous trouverez de nombreux articles en ligne qui expliquent comment configurer Dispatcher, ainsi que des conseils et astuces.

Ces éléments d’information démarrent toutefois toujours à un niveau très technique, en supposant que vous sachiez déjà ce que vous voulez faire et donc que vous fournissiez uniquement des détails sur la façon d’atteindre ce que vous voulez. Nous n’avons jamais trouvé d’articles conceptuels décrivant ce qu’est _et pourquoi_ quand il s’agit de ce que vous pouvez faire ou ne pouvez pas avec Dispatcher.

### Antipattern : Dispatcher en tant qu’après-pensée

Ce manque d&#39;informations de base mène à un certain nombre d&#39;anti-modèles que nous avons vus dans un certain nombre de projets AEM :

1. Comme Dispatcher est installé sur le serveur web Apache, c’est la tâche des &quot;dieux Unix&quot; dans le projet de le configurer. Un &quot;développeur java mortel&quot; n&#39;a pas besoin de s&#39;en préoccuper.

2. Le développeur Java doit s’assurer que son code fonctionne... Dispatcher le rendra plus rapide par magie. Dispatcher est toujours une pensée après coup. Cependant, cela ne fonctionne pas. Un développeur doit concevoir son code en tenant compte du Dispatcher. Et il a besoin de connaître ses concepts de base pour le faire.

### &quot;D&#39;abord, fais-le marcher, puis fais vite&quot; N&#39;a pas toujours raison

Vous avez peut-être entendu le conseil de programmation _&quot;Commencez par le faire marcher, puis le faire vite.&quot;_. Ce n&#39;est pas entièrement faux. Toutefois, sans le contexte approprié, il est généralement mal interprété et mal appliqué.

Ce conseil doit empêcher le développeur d’optimiser prématurément le code, qui ne peut jamais être exécuté - ou qui est exécuté si rarement, qu’une optimisation n’aurait pas un impact suffisant pour justifier l’effort d’optimisation. En outre, l’optimisation peut conduire à un code plus complexe et introduire ainsi des bogues. Ainsi, si vous êtes un développeur, ne passez pas trop de temps à la micro-optimisation de chaque ligne de code. Veillez simplement à choisir les structures de données, les algorithmes et les bibliothèques appropriés et attendez que l’analyse de la zone réactive d’un profileur indique où une optimisation plus approfondie pourrait augmenter les performances globales.

### Décisions architecturales et artefacts

Cependant, le conseil &quot;Commencez par le faire marcher, puis le faire vite&quot; est complètement faux quand il s&#39;agit de décisions &quot;architecturales&quot;. Quelles sont les décisions architecturales ? En termes simples, ce sont des décisions qui sont chères, difficiles et/ou impossibles à modifier par la suite. Gardez à l’esprit que &quot;cher&quot; est parfois identique à &quot;impossible&quot;.  Par exemple, lorsque votre projet manque de budget, des modifications coûteuses sont impossibles à mettre en oeuvre. Les changements d&#39;infrastructures sont le tout premier type de changements dans cette catégorie qui viennent à l&#39;esprit de la plupart des gens. Mais il y a aussi une autre sorte d&#39;artefacts &quot;architecturaux&quot; qui peuvent devenir très mauvais à changer :

1. Des éléments de code au &quot;centre&quot; d’une application, sur lesquels beaucoup d’autres éléments s’appuient. Pour les modifier, toutes les dépendances doivent être modifiées et testées à nouveau simultanément.

2. Les artefacts, qui sont impliqués dans certains scénarios asynchrones dépendant du timing où l’entrée - et donc le comportement du système peuvent varier de manière très aléatoire. Les changements peuvent avoir des effets imprévisibles et être difficiles à tester.

3. Des modèles de logiciels qui sont utilisés et réutilisés encore et encore, dans tous les éléments et parties du système. Si le modèle logiciel se révèle sous-optimal, tous les artefacts qui utilisent le modèle doivent être recodés.

Mémoriser? En haut de cette page, nous avons dit que Dispatcher est une partie essentielle d’une application AEM. L&#39;accès à une application web est très aléatoire : les utilisateurs vont et viennent à des moments imprévisibles. En fin de compte, tout le contenu sera (ou devrait) mis en cache dans Dispatcher. Donc, si vous avez fait très attention, vous vous êtes peut-être rendu compte que la mise en cache pouvait être considérée comme un artefact &quot;architectural&quot; et devrait donc être compris par tous les membres de l’équipe, les développeurs comme les administrateurs.

Nous ne disons pas qu’un développeur doit réellement configurer Dispatcher. Ils doivent connaître les concepts, en particulier les limites, pour s’assurer que leur code peut également être exploité par Dispatcher.

Dispatcher n’améliore pas la vitesse du code par magie. Un développeur doit créer ses composants en tenant compte de Dispatcher. Il a donc besoin de savoir comment ça marche.

## Mise en cache de Dispatcher - Principes de base

### Dispatcher en tant que mise en cache Http - équilibreur de charge

Qu’est-ce que Dispatcher et pourquoi est-il appelé &quot;Dispatcher&quot; en premier lieu ?

Dispatcher est

* Tout d’abord, un cache

* Un proxy inverse

* Module du serveur web Apache httpd, ajoutant AEM fonctions associées à la polyvalence d’Apache et fonctionnant sans problème avec tous les autres modules Apache (tels que SSL ou même SSI, comme nous le verrons plus loin).

Dans les premiers jours du web, vous vous attendriez à quelques centaines de visiteurs sur un site. Configuration d’un Dispatcher, &quot;distribué&quot; ou équilibré la charge des requêtes sur un certain nombre de serveurs de publication AEM et qui était généralement suffisant - par conséquent, le nom &quot;Dispatcher&quot;. Aujourd’hui, cependant, cette configuration n’est plus très souvent utilisée.

Nous verrons différentes manières de configurer les systèmes Dispatcher et Publish plus loin dans cet article. Commençons par quelques bases de mise en cache http.

![Fonctionnalités de base d’un cache de Dispatcher](assets/chapter-1/basic-functionality-dispatcher.png)

*Fonctionnalités de base d’un cache de Dispatcher*

<br> 

Les principes de base du dispatcher sont expliqués ici. Dispatcher est un proxy inverse simple de mise en cache avec la possibilité de recevoir et de créer des requêtes HTTP. Voici un cycle de requête/réponse normal :

1. Un utilisateur demande une page.
2. Dispatcher vérifie s’il dispose déjà d’une version rendue de cette page. Supposons qu’il s’agisse de la toute première requête pour cette page et que Dispatcher ne trouve pas de copie locale mise en cache.
3. Dispatcher demande la page au système de publication.
4. Sur le système de publication, la page est rendue par un JSP ou un modèle HTL.
5. La page est renvoyée au Dispatcher.
6. Dispatcher met la page en cache.
7. Dispatcher renvoie la page au navigateur.
8. Si la même page est demandée une seconde fois, elle peut être diffusée directement à partir du cache de Dispatcher sans avoir à la restituer à nouveau sur l’instance de publication. Cela permet d’économiser du temps d’attente pour les cycles de l’utilisateur et du processeur sur l’instance de publication.

Nous parlions de &quot;pages&quot; dans la dernière section. Mais le même schéma s’applique également à d’autres ressources telles que les images, les fichiers CSS, les téléchargements de PDF, etc.

#### Mise en cache des données

Le module de Dispatcher exploite les fonctionnalités fournies par le serveur Apache d’hébergement. Les ressources telles que les pages HTML, les téléchargements et les images sont stockées sous la forme de fichiers simples dans le système de fichiers Apache. C&#39;est aussi simple que ça.

Le nom de fichier est dérivé de l’URL de la ressource demandée. Si vous demandez un fichier `/foo/bar.html`, il est stocké par exemple sous /`var/cache/docroot/foo/bar.html`.

En principe, si tous les fichiers sont mis en cache et donc stockés de manière statique dans Dispatcher, vous pouvez extraire la module externe du système de publication et Dispatcher servira de serveur web simple. Mais c&#39;est juste pour illustrer ce principe. La vie réelle est plus compliquée. Vous ne pouvez pas tout mettre en cache, et le cache n’est jamais complètement &quot;plein&quot;, car le nombre de ressources peut être infini en raison de la nature dynamique du processus de rendu. Le modèle d’un système de fichiers statique permet de générer une image approximative des fonctionnalités du Dispatcher. Et cela aide à expliquer les limites du dispatcher.

#### Structure d’URL AEM et mappage du système de fichiers

Pour comprendre plus en détail Dispatcher, nous allons revoir la structure d’un simple exemple d’URL.  Examinons l’exemple ci-dessous :

`http://domain.com/path/to/resource/pagename.selectors.html/path/suffix.ext?parameter=value&amp;otherparameter=value#fragment`

* `http` indique le protocole

* `domain.com` est le nom de domaine ;

* `path/to/resource` est le chemin d’accès sous lequel la ressource est stockée dans CRX, puis dans le système de fichiers du serveur Apache.

A partir de là, les choses diffèrent légèrement entre le système de fichiers AEM et le système de fichiers Apache.

Dans AEM,

* `pagename` est le libellé des ressources

* `selectors` représente un certain nombre de sélecteurs utilisés dans Sling pour déterminer le mode de rendu de la ressource. Une URL peut avoir un nombre arbitraire de sélecteurs. Elles sont séparées par un point. Une section de sélecteurs peut, par exemple, être quelque chose comme &quot;French.mobile.fancy&quot;. Les sélecteurs ne doivent contenir que des lettres, des chiffres et des tirets.

* `html` comme étant le dernier des &quot;sélecteurs&quot; est appelé extension . Dans AEM/Sling, il détermine également en partie le script de rendu.

* `path/suffix.ext` est une expression de type chemin d’accès qui peut être un suffixe de l’URL.  Il peut être utilisé dans AEM scripts pour contrôler davantage le rendu d’une ressource. Nous aurons une section complète sur cette partie plus tard. Pour l’instant, il suffit de savoir que vous pouvez l’utiliser comme paramètre supplémentaire. Les suffixes doivent avoir une extension .

* `?parameter=value&otherparameter=value` est la section de requête de l’URL. Il est utilisé pour transmettre des paramètres arbitraires à AEM. Les URL avec des paramètres ne peuvent pas être mises en cache et les paramètres doivent donc être limités aux cas où ils sont absolument nécessaires.

* `#fragment`, la partie fragment d’une URL n’est pas transmise à AEM elle est utilisée uniquement dans le navigateur ; soit dans les frameworks JavaScript comme &quot;paramètres de routage&quot;, soit pour accéder à une certaine partie de la page.

Dans Apache (*référencez le diagramme ci-dessous*),

* `pagename.selectors.html` est utilisé comme nom de fichier dans le système de fichiers du cache.

Si l’URL comporte le suffixe `path/suffix.ext` , alors :

* `pagename.selectors.html` est créé en tant que dossier ;

* `path` un dossier dans le  `pagename.selectors.html` dossier

* `suffix.ext` est un fichier dans le  `path` dossier . Remarque : Si le suffixe ne comporte pas d’extension, le fichier n’est pas mis en cache.

![Disposition du système de fichiers après obtention des URL de Dispatcher](assets/chapter-1/filesystem-layout-urls-from-dispatcher.png)

*Disposition du système de fichiers après obtention des URL de Dispatcher*

<br> 

#### Limites de base

Le mappage entre une URL, la ressource et le nom de fichier est assez simple.

Vous avez peut-être remarqué quelques pièges, cependant :

1. Les URL peuvent devenir très longues. L’ajout de la partie &quot;path&quot; d’une `/docroot` sur le système de fichiers local peut facilement dépasser les limites de certains systèmes de fichiers. L’exécution de Dispatcher dans NTFS sous Windows peut s’avérer difficile. Vous êtes cependant en sécurité avec Linux.

2. Les URL peuvent contenir des caractères spéciaux et des points-virgules. Ce n’est généralement pas un problème pour le Dispatcher. Gardez toutefois à l’esprit que l’URL est interprétée à de nombreux endroits de votre application. Le plus souvent, nous avons vu des comportements étranges d&#39;une application, juste pour découvrir qu&#39;un morceau de code (personnalisé) rarement utilisé n&#39;a pas été testé minutieusement pour les caractères spéciaux. Tu devrais les éviter si tu peux. Et si vous ne le pouvez pas, planifiez des tests approfondis.

3. Dans CRX, les ressources ont des sous-ressources. Par exemple, une page comporte un certain nombre de sous-pages. Il ne peut pas y avoir de correspondance dans un système de fichiers, car les systèmes de fichiers comportent des fichiers ou des dossiers.

#### Les URL sans extension ne sont pas mises en cache

Les URL doivent toujours avoir une extension. Bien que vous puissiez fournir des URL sans extensions dans AEM. Ces URL ne seront pas mises en cache dans Dispatcher.

**Exemples**

`http://domain.com/home.html` est  **mise en cache**

`http://domain.com/home` ne peut  **pas être mis en cache**

La même règle s’applique lorsque l’URL contient un suffixe. Le suffixe doit disposer d’une extension pour pouvoir être mis en cache.

**Exemples**

`http://domain.com/home.html/path/suffix.html` est  **mise en cache**

`http://domain.com/home.html/path/suffix` ne peut  **pas être mis en cache**

Vous vous demandez peut-être, que se passe-t-il si la partie ressource n’a pas d’extension, mais que le suffixe en a une ? Eh bien, dans ce cas l&#39;URL n&#39;a aucun suffixe. Examinez l’exemple suivant :

**Exemple**

`http://domain.com/home/path/suffix.ext`

`/home/path/suffix` est le chemin d’accès à la ressource... Il n’y a donc aucun suffixe dans l’URL.

**Conclusion**

Ajoutez toujours des extensions au chemin et au suffixe. Les personnes qui ont conscience du SEO affirment parfois que cela vous place en bas dans les résultats de recherche. Mais une page non mise en cache serait très lente et serait classée encore plus bas.

#### Conflits d’URL de suffixes

Supposons que vous ayez deux URL valides

`http://domain.com/home.html`

et

`http://domain.com/home.html/suffix.html`

Ils sont absolument valides en AEM. Vous ne verriez aucun problème sur votre ordinateur de développement local (sans Dispatcher). Il est probable que vous ne rencontriez aucun problème dans les tests UAT ou de chargement. Le problème auquel nous sommes confrontés est si subtil qu&#39;il passe la plupart des tests.  Cela vous frappera durement lorsque vous êtes à l’heure de pointe et que vous serez limité à temps pour y faire face, que vous n’aurez probablement aucun accès au serveur et que vous ne disposerez pas des ressources pour le réparer. Nous y sommes allés...

Alors... quel est le problème ?

`home.html` dans un système de fichiers peut être un fichier ou un dossier. Pas les deux en même temps qu’en AEM.

Si vous demandez d’abord `home.html`, il sera créé sous la forme d’un fichier .

Les requêtes suivantes envoyées à `home.html/suffix.html` renvoient des résultats valides, mais comme le fichier `home.html` &quot;bloque&quot; la position dans le système de fichiers, `home.html` ne peut pas être créé une seconde fois en tant que dossier et donc `home.html/suffix.html` n’est pas mis en cache.

![Position de blocage des fichiers dans le système de fichiers empêchant la mise en cache des sous-ressources](assets/chapter-1/file-blocking-position-in-filesystem.png)

*Position de blocage des fichiers dans le système de fichiers empêchant la mise en cache des sous-ressources*

<br> 

Si vous le faites de l’autre manière, commencez par demander `home.html/suffix.html` , alors `suffix.html` est mis en cache dans un dossier `/home.html` au départ. Cependant, ce dossier est supprimé et remplacé par un fichier `home.html` lorsque vous demandez ensuite la ressource `home.html`.

![Suppression d’une structure de chemin d’accès lorsqu’un parent est récupéré en tant que ressource](assets/chapter-1/deleting-path-structure.png)

*Suppression d’une structure de chemin d’accès lorsqu’un parent est récupéré en tant que ressource*

<br> 

Ainsi, le résultat de ce qui est mis en cache est entièrement aléatoire et dépend de l’ordre des requêtes entrantes. Ce qui rend les choses encore plus compliquées, c&#39;est le fait que vous avez généralement plus d&#39;un répartiteur. Et les performances, le taux d’accès et le comportement du cache peuvent varier d’un Dispatcher à l’autre. Si vous souhaitez savoir pourquoi votre site web ne répond pas, vous devez vous assurer que vous consultez le Dispatcher correct avec l’ordre de mise en cache malheureux. Si vous regardez Dispatcher qui, par chance, a eu un modèle de demande plus favorable, vous serez perdu dans vos tentatives de trouver le problème.

#### Eviter les URL en conflit

Vous pouvez éviter les &quot;URL en conflit&quot;, lorsqu’un nom de dossier et un nom de fichier &quot;concourent&quot; pour le même chemin d’accès dans le système de fichiers, lorsque vous utilisez une extension différente pour la ressource lorsque vous disposez d’un suffixe.

**Exemple**

* `http://domain.com/home.html`

* `http://domain.com/home.dir/suffix.html`

Les deux sont parfaitement mises en cache,

![](assets/chapter-1/cacheable.png)

Choisissez une extension dédiée &quot;dir&quot; pour une ressource lorsque vous demandez un suffixe ou que vous évitez d’utiliser complètement le suffixe . Il existe de rares cas où elles sont utiles. Et il est facile de mettre en oeuvre ces cas correctement.  Comme nous le verrons dans le prochain chapitre, lorsque nous parlerons d’invalidation et de purge du cache.

#### Requêtes non mises en cache

Passons en revue un résumé rapide du dernier chapitre et quelques autres exceptions. Dispatcher peut mettre en cache une URL si elle est configurée comme pouvant être mise en cache et s’il s’agit d’une demande de GET. Il ne peut pas être mis en cache sous l’une des exceptions suivantes.

**Requêtes mises en cache**

* La requête est configurée pour pouvoir être mise en cache dans la configuration de Dispatcher.
* La requête est une requête de GET ordinaire

**Requêtes ou réponses non mises en cache**

* Requête à laquelle la mise en cache est refusée par configuration (chemin, modèle, type MIME)
* Réponses qui renvoie un &quot;Dispatcher&quot; : en-tête no-cache&quot;
* Réponse qui renvoie un &quot;Cache-Control: no-cache|private&quot; header
* Réponse qui renvoie un &quot;Pragma&quot; : en-tête no-cache&quot;
* Requête avec paramètres de requête
* URL sans extension
* URL avec un suffixe qui ne comporte pas d’extension
* Réponse qui renvoie un code d’état autre que 200
* Demande de POST

## Invalidation et purge du cache

### Présentation

Le dernier chapitre répertorie un grand nombre d’exceptions lorsque Dispatcher ne peut pas mettre en cache une requête. Mais il y a plus à prendre en compte : Ce n’est pas nécessairement parce que Dispatcher _peut_ mettre en cache une requête que cela signifie _devrait_.

L’idée est la suivante : La mise en cache est généralement facile. Dispatcher doit simplement stocker le résultat d’une réponse et le renvoyer la prochaine fois que la même requête est entrante. Droite? Faux !

La partie difficile est l’_invalidation_ ou la _purge_ du cache. Dispatcher doit le savoir lorsqu’une ressource a changé et doit être rendue à nouveau.

Cela semble être une tâche triviale à première vue... mais ce n&#39;est pas le cas. Lisez plus loin et vous découvrirez des différences délicates entre des ressources simples et simples et des pages qui reposent sur une structure hautement condensée de plusieurs ressources.

### Ressources simples et purge

Nous avons configuré notre système d’AEM pour créer dynamiquement un rendu de miniature pour chaque image lorsque cela est demandé avec un sélecteur de &quot;miniature&quot; spécial :

`/content/dam/path/to/image.thumb.png`

Et, bien sûr, nous fournissons une URL pour diffuser l’image d’origine avec une URL sans sélecteur :

`/content/dam/path/to/image.png`

Si nous téléchargeons à la fois la miniature et l&#39;image d&#39;origine, nous obtiendrons quelque chose comme :

```
/var/cache/dispatcher/docroot/content/dam/path/to/image.thumb.png

/var/cache/dispatcher/docroot/content/dam/path/to/image.png
```

dans le système de fichiers de Dispatcher.

Désormais, l’utilisateur charge et active une nouvelle version de ce fichier. En fin de compte, une demande d’invalidation est envoyée d’AEM à Dispatcher,

```
GET /invalidate
invalidate-path:  /content/dam/path/to/image

<no body>
```

L&#39;invalidation est si facile : Une requête de GET simple à une URL &quot;/invalidate&quot; spéciale sur Dispatcher. Un HTTP-body n’est pas obligatoire, la &quot;payload&quot; est simplement l’en-tête &quot;invalidate-path&quot;. Notez également que le chemin d’accès invalidate dans l’en-tête est la ressource que AEM connaît, et non le fichier ou les fichiers mis en cache par Dispatcher. AEM ne connaît que les ressources. Les extensions, sélecteurs et suffixes sont utilisés au moment de l’exécution lorsqu’une ressource est demandée. AEM n’effectue aucune conservation des signets sur les sélecteurs utilisés sur une ressource. Par conséquent, le chemin d’accès à la ressource est tout ce qu’il sait avec certitude lors de l’activation d’une ressource.

C&#39;est suffisant dans notre cas. Si une ressource a changé, nous pouvons supposer en toute sécurité que tous les rendus de cette ressource ont également changé. Dans notre exemple, si l’image a changé, une nouvelle miniature est également affichée.

Dispatcher peut supprimer la ressource en toute sécurité avec tous les rendus qu’il a mis en cache. Il fera quelque chose comme :

`$ rm /content/dam/path/to/image.*`

suppression de `image.png` et `image.thumb.png` et de tous les autres rendus correspondant à ce modèle.

Super simple en effet... tant que vous utilisez une seule ressource pour répondre à une requête.

### Références et contenu incorporé

#### Le problème du contenu bloqué

Contrairement aux images ou autres fichiers binaires téléchargés sur AEM, les pages HTML ne sont pas des animaux solitaires. Ils vivent dans des troupeaux et sont très interconnectés les uns aux autres par des liens hypertextes et des références. Le lien simple est inoffensif, mais il devient délicat lorsque nous parlons de références de contenu. La navigation supérieure ou les teasers omniprésents sur les pages sont des références de contenu.

#### Références de contenu et raisons de ce problème

Regardons un exemple simple. Une agence de voyages a une page Web qui fait la promotion d&#39;un voyage au Canada. Cette promotion est présentée dans la section du teaser sur deux autres pages, sur la page &quot;Accueil&quot; et sur une page &quot;Spéciaux d’hiver&quot;.

Puisque les deux pages affichent le même teaser, il serait inutile de demander à l’auteur de créer le teaser plusieurs fois pour chaque page sur laquelle il doit s’afficher. À la place, la page cible &quot;Canada&quot; réserve une section dans les propriétés de la page pour fournir les informations au teaser, ou mieux fournir une URL qui rend complètement ce teaser :

`<sling:include resource="/content/home/destinations/canada" addSelectors="teaser" />`

ou

`<sling:include resource="/content/home/destinations/canada/jcr:content/teaser" />`

![](assets/chapter-1/content-references.png)

Sur AEM uniquement qui fonctionne comme le charme, mais si vous utilisez un Dispatcher sur l’instance de publication, quelque chose d’étrange se produit.

Imaginez que vous ayez publié votre site web. Le titre de votre page Canada est &quot;Canada&quot;. Lorsqu’un visiteur demande votre page d’accueil (qui comporte une référence de teaser à cette page), le composant de la page &quot;Canada&quot; effectue le rendu de quelque chose comme

```
<div class="teaser">
  <h3>Canada</h3>
  <img …>
</div>
```

** dans la page d’accueil. La page d’accueil est stockée par Dispatcher sous la forme d’un fichier .html statique, incluant le teaser et son titre dans le fichier.

Le spécialiste du marketing a maintenant appris que les titres de teaser doivent être exploitables. Il décide donc de changer le titre de &quot;Canada&quot; en &quot;Visite du Canada&quot; et met à jour l&#39;image aussi.

Il publie la page &quot;Canada&quot; et consulte à nouveau la page d&#39;accueil précédemment publiée pour voir ses modifications. Mais rien n&#39;a changé là-bas. Il affiche toujours l’ancien teaser. Il double les chèques du &quot;spécial hiver&quot;. Cette page n’a jamais été demandée auparavant et n’est donc pas mise en cache de manière statique dans Dispatcher. Cette page est donc fraîchement rendue par l’élément Publier et elle contient maintenant le nouveau teaser &quot;Visite Canada&quot;.

![Dispatcher stockant le contenu obsolète inclus dans la page d’accueil](assets/chapter-1/dispatcher-storing-stale-content.png)

*Dispatcher stockant le contenu obsolète inclus dans la page d’accueil*

<br> 

Que s&#39;est-il passé ? Dispatcher stocke une version statique d’une page contenant tout le contenu et les balises qui ont été extraits d’autres ressources lors du rendu.

Dispatcher, qui est un simple serveur web basé sur un système de fichiers, est rapide mais aussi relativement simple. Si une ressource incluse change, elle ne s’en rend pas compte. Il reste collé au contenu qui se trouvait là lorsque la page d’inclusion a été rendue.

La page &quot;Hiver spécial&quot; n’a pas encore été rendue. Il n’existe donc pas de version statique sur Dispatcher et s’affichera donc avec le nouveau teaser, car il sera actualisé sur demande.

Vous pouvez penser que Dispatcher effectue le suivi de chaque ressource qu’il touche tout en effectuant le rendu et la purge de toutes les pages qui ont utilisé cette ressource, lorsque cette ressource change. Mais Dispatcher ne rend pas les pages. Le rendu est effectué par le système de publication. Dispatcher ne sait pas quelles ressources vont dans un fichier .html rendu.

Pas encore convaincu ? Vous pouvez penser que *&quot;il doit y avoir un moyen de mettre en oeuvre un type de suivi des dépendances&quot;*. Eh bien il y en a, ou plus précisément il y a *était*. Communiqué 3 : un outil de suivi des dépendances a été mis en oeuvre dans la _session_ utilisée pour le rendu d’une page pour l’arrière-arrière-grand-père de AEM.

Lors d’une requête, chaque ressource acquise via cette session était suivie en tant que dépendance de l’URL en cours de rendu.

Mais il s&#39;est avéré que garder une trace des dépendances était très cher. Les utilisateurs ont rapidement découvert que le site web était plus rapide s’ils désactivaient complètement la fonction de suivi des dépendances et s’ils comptaient sur le rendu de toutes les pages HTML après modification d’une page HTML. En outre, ce schéma n&#39;était pas parfait non plus - il y avait un certain nombre d&#39;écueils et d&#39;exceptions en chemin. Dans certains cas, vous n’utilisiez pas la session de requêtes par défaut pour obtenir une ressource, mais une session d’administration pour obtenir des ressources d’assistance pour effectuer le rendu d’une requête. Ces dépendances n’étaient généralement pas suivies et entraînaient des maux de tête et des appels téléphoniques à l’équipe op-Team pour demander de vider manuellement le cache. Vous avez eu de la chance s&#39;ils avaient une procédure standard pour faire ça. Il y avait d&#39;autres pièges en chemin mais... arrêtons de nous rappeler. Cela remonte à 2005. En fin de compte, cette fonctionnalité a été désactivée dans Communiqué 4 par défaut et elle n&#39;a pas été réactivée dans le CQ5 successeur qui est ensuite devenu AEM.

### Invalidation automatique

#### Lorsque La Purge Complète Est Moins chère Que Le Suivi De Dépendance

Puisque CQ5, nous dépendons entièrement de l&#39;invalidation, plus ou moins, de l&#39;ensemble du site si seulement une des pages change. Cette fonctionnalité est appelée &quot;Invalidation automatique&quot;.

Mais encore une fois - comment cela peut-il être, que jeter et re-rendre des centaines de pages est moins cher que d&#39;effectuer un suivi de dépendance approprié et un rendu partiel ?

Il existe deux raisons principales :

1. Sur un site web moyen, seul un petit sous-ensemble des pages est fréquemment demandé. Ainsi, même si vous supprimez tout le contenu rendu, seules quelques douzaines seront effectivement demandées immédiatement après. Le rendu de la longue traîne de pages peut être distribué au fil du temps, lorsqu’il est réellement demandé. En fait, la charge sur les pages de rendu n’est pas aussi élevée que prévu. Bien sûr, il y a toujours des exceptions... nous discuterons plus tard de quelques astuces pour gérer les chargements distribués de manière égale sur les sites web plus volumineux avec des caches de Dispatcher vides.

2. De toute façon, toutes les pages sont connectées par la navigation principale. Donc presque toutes les pages sont en fin de compte dépendantes les unes des autres. Cela signifie que même le dispositif de suivi de dépendance le plus intelligent trouvera ce que nous savons déjà : Si l’une des pages change, vous devez invalider toutes les autres.

Tu ne crois pas ? Illustrons le dernier point.

Nous utilisons le même argument que dans le dernier exemple avec des teasers faisant référence au contenu d’une page distante. Nous utilisons maintenant un exemple plus extrême : Navigation principale automatiquement rendue. Comme pour le teaser, le titre de navigation est tiré de la page liée ou &quot;distante&quot; comme référence de contenu. Les titres de navigation distante ne sont pas stockés dans la page actuellement rendue. N’oubliez pas que la navigation s’affiche sur chaque page de votre site web. Ainsi, le titre d’une page est utilisé plusieurs fois sur toutes les pages qui ont une navigation principale. Et si vous souhaitez modifier un titre de navigation, vous ne souhaitez le faire qu’une seule fois sur la page distante, et non sur chaque page référençant la page.

Ainsi, dans notre exemple, la navigation relie toutes les pages en utilisant le &quot;NavTitle&quot; de la page cible pour générer un nom dans la navigation. Le titre de navigation de &quot;Islande&quot; est tiré de la page &quot;Islande&quot; et rendu dans chaque page qui comporte une navigation principale.

![La navigation principale imbrique inévitablement le contenu de toutes les pages en extrayant leurs &quot;titres de navigation&quot;.](assets/chapter-1/nav-titles.png)

*La navigation principale imbrique inévitablement le contenu de toutes les pages en extrayant leurs &quot;titres de navigation&quot;.*

<br> 

Si vous changez le titre de navigation sur la page Islande de &quot;Islande&quot; à &quot;Belle Islande&quot;, ce titre change immédiatement dans le menu principal de toutes les autres pages. Ainsi, les pages rendues et mises en cache avant cette modification deviennent toutes obsolètes et doivent être invalidées.

#### Mise en oeuvre de l’invalidation automatique : Fichier .stat

Si vous avez un site volumineux de milliers de pages, il faudrait un certain temps pour parcourir toutes les pages et les supprimer physiquement. Pendant cette période, Dispatcher pouvait afficher involontairement du contenu obsolète. Pire encore, il se peut qu’il y ait des conflits lors de l’accès aux fichiers du cache, qu’une page soit demandée alors qu’elle est simplement en cours de suppression ou qu’une page soit à nouveau supprimée en raison d’une seconde invalidation survenue après une activation ultérieure immédiate. Voyons quel désordre cela pourrait bien être. Heureusement ce n&#39;est pas ce qui se passe. Dispatcher utilise une astuce intelligente pour éviter cela : Au lieu de supprimer des centaines et des milliers de fichiers, il place un fichier simple et vide à la racine de votre système de fichiers lorsqu’un fichier est publié et tous les fichiers dépendants sont donc considérés comme non valides. Ce fichier est appelé &quot;statfile&quot;. Le fichier stat est un fichier vide. Ce qui importe dans le fichier stat, c’est sa date de création uniquement.

Tous les fichiers du dispatcher, dont la date de création est antérieure au fichier stat, sont rendus avant la dernière activation (et invalidation) et sont donc considérés comme &quot;non valides&quot;. Ils sont toujours physiquement présents dans le système de fichiers, mais Dispatcher les ignore. Ils sont &quot;périmés&quot;. Chaque fois qu’une demande est envoyée à une ressource obsolète, Dispatcher demande au système AEM de rendre à nouveau la page. Cette page nouvellement rendue est alors stockée dans le système de fichiers, avec une nouvelle date de création et elle est à nouveau actualisée.

![La date de création du fichier .stat définit quel contenu est obsolète et quel contenu est actualisé.](assets/chapter-1/creation-date.png)

*La date de création du fichier .stat définit quel contenu est obsolète et quel contenu est actualisé.*

<br> 

Vous pouvez demander pourquoi il s’appelle &quot;.stat&quot; ? Et peut-être pas &quot;.invalidate&quot; ? Eh bien, vous pouvez imaginer que le fait d’avoir ce fichier dans votre système de fichiers aide Dispatcher à déterminer quelles ressources peuvent *statically* être diffusées, tout comme depuis un serveur web statique. Ces fichiers n’ont plus besoin d’être rendus dynamiquement.

La vraie nature du nom, cependant, est moins métaphorique. Il est dérivé de l’appel système Unix `stat()`, qui renvoie l’heure de modification d’un fichier (entre autres propriétés).

#### Mixage d’une validation simple et automatique

Mais attendez...plus tôt nous avons dit que les ressources uniques sont supprimées physiquement. Nous disons maintenant qu&#39;un fichier stat plus récent les rendrait virtuellement invalides aux yeux de Dispatcher. Pourquoi la suppression physique, d&#39;abord ?

La réponse est simple. Vous utilisez généralement les deux stratégies en parallèle, mais pour différents types de ressources. Les ressources binaires, telles que les images, sont autonomes. Ils ne sont pas connectés à d&#39;autres ressources dans la mesure où ils ont besoin que leurs informations soient rendues.

Les pages HTML, en revanche, sont très interdépendantes. Donc, vous appliquez l’invalidation automatique sur celles-ci. Il s’agit du paramètre par défaut dans Dispatcher. Tous les fichiers appartenant à une ressource invalidée sont supprimés physiquement. En outre, les fichiers qui se terminent par &quot;.html&quot; sont automatiquement invalidés.

Dispatcher décide de l’extension de fichier, d’appliquer ou non un schéma d’invalidation automatique.

Les fins de fichier pour l’invalidation automatique peuvent être configurées. En théorie, vous pouvez inclure toutes les extensions à l’invalidation automatique. Mais gardez à l&#39;esprit que cela a un prix très élevé. Vous ne verrez pas de ressources obsolètes diffusées involontairement, mais les performances de diffusion se dégradent considérablement en raison d’une sur-invalidation.

Imaginez, par exemple, que vous implémentiez un schéma dans lequel les fichiers PNG et JPG sont rendus dynamiquement et dépendent d’autres ressources pour le faire. Vous pouvez redimensionner les images haute résolution pour obtenir une résolution compatible avec le web plus petite. Lorsque vous y êtes, modifiez également le taux de compression. La résolution et le taux de compression dans cet exemple ne sont pas des constantes fixes, mais des paramètres configurables dans le composant qui utilise l’image. Maintenant, si ce paramètre est modifié, vous devez invalider les images.

Aucun problème : nous venons d’apprendre que nous pourrions ajouter des images à l’invalidation automatique et toujours avoir des images fraîchement rendues chaque fois que quelque chose change.

#### Jetez le bébé avec l&#39;eau de bain

C&#39;est vrai - et c&#39;est un énorme problème. Lis à nouveau le dernier paragraphe. &quot;...images fraîchement rendues chaque fois qu’un élément change.&quot;. Comme vous le savez, un bon site web est constamment modifié : ajout de nouveau contenu ici, correction d&#39;une faute de frappe là-bas, correction d&#39;un teaser ailleurs. Cela signifie que toutes vos images sont invalidées en permanence et doivent être restituées à nouveau. Ne sous-estimez pas cela. Le rendu et le transfert dynamiques des données d’image fonctionnent en millisecondes sur votre machine de développement locale. Votre environnement de production doit le faire cent fois plus souvent, par seconde.

Soyons clairs, vos jpgs doivent être rendus à nouveau, lorsqu’une page HTML change et vice versa. Il n’existe qu’un seul &quot;compartiment&quot; de fichiers à invalider automatiquement. Il est vidé dans son ensemble. Sans aucun moyen de se diviser en structures plus détaillées.

Il existe une bonne raison pour laquelle l’invalidation automatique est conservée dans &quot;.html&quot; par défaut. L&#39;objectif est de garder ce seau aussi petit que possible. Ne jetez pas le bébé avec l&#39;eau du bain en invalidant tout simplement - juste pour être du côté sûr.

Les ressources autonomes doivent être servies sur le chemin d’accès de cette ressource. Ça aide beaucoup l&#39;invalidation. Restez simple, ne créez pas de schémas de mappage comme &quot;resource /a/b/c&quot; est diffusé à partir de &quot;/x/y/z&quot;. Assurez-vous que vos composants fonctionnent avec les paramètres d’invalidation automatique de Dispatcher par défaut. N’essayez pas de réparer un composant mal conçu avec une sur-invalidation dans Dispatcher.

##### Exceptions à l’invalidation automatique : Invalidation ResourceOnly

La demande d’invalidation de Dispatcher est généralement déclenchée à partir du ou des systèmes de publication par un agent de réplication.

Si vous êtes très confiant quant à vos dépendances, vous pouvez essayer de créer votre propre agent de réplication invalidant.

Ce serait un peu plus que ce guide pour entrer dans les détails, mais nous voulons vous donner au moins quelques conseils.

1. Sais vraiment ce que tu fais. Obtenir l&#39;invalidation est vraiment difficile. C&#39;est une des raisons pour lesquelles l&#39;invalidation automatique est si rigoureuse ; pour éviter de diffuser du contenu obsolète.

2. Si votre agent envoie un en-tête HTTP `CQ-Action-Scope: ResourceOnly`, cela signifie que cette seule demande d’invalidation ne déclenche pas d’invalidation automatique. Cet élément de code ( [https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle](https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle)) peut être un bon point de départ pour votre propre agent de réplication.

3. `ResourceOnly`, empêche uniquement l’invalidation automatique. Pour résoudre et invalider les dépendances nécessaires, vous devez déclencher les demandes d’invalidation vous-même. Vous pouvez vérifier les règles de vidage du dispatcher de package ([https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html)) pour savoir comment cela pourrait se produire.

Nous vous déconseillons de créer un schéma de résolution de dépendance. Il y a juste trop d&#39;efforts et peu de gain - et comme nous l&#39;avons dit, il y a trop de choses que vous vous tromperez.

Au contraire, ce que vous devez faire est de déterminer quelles ressources n’ont aucune dépendance sur d’autres ressources et peuvent être invalidées sans invalidation automatique. Vous n’avez toutefois pas besoin d’utiliser un agent de réplication personnalisé. Il vous suffit de créer une règle personnalisée dans votre configuration Dispatcher qui exclut ces ressources de l’invalidation automatique.

Nous avons dit que la navigation principale ou les teasers sont une source de dépendances. Eh bien, si vous chargez la navigation et les teasers de manière asynchrone ou que vous les incluez avec un script SSI dans Apache, vous n’aurez pas cette dépendance à suivre. Nous expliquerons plus en détail le chargement asynchrone des composants plus loin dans ce document lorsque nous parlerons de &quot;Sling Dynamic Includes&quot;.

Il en va de même pour les fenêtres contextuelles ou le contenu chargé dans un cadre lumineux. Ces éléments comportent également rarement des navigations (c’est-à-dire des &quot;dépendances&quot;) et peuvent être invalidés en tant que ressource unique.

## Création de composants avec Dispatcher à l’esprit

### Application du mécanisme de Dispatcher dans un exemple réel

Dans le dernier chapitre, nous avons expliqué comment fonctionne la mécanique de base de Dispatcher, comment elle fonctionne en général et quelles sont les limites.

Nous voulons maintenant appliquer ces mécanismes à un type de composants que vous trouverez très probablement dans les exigences de votre projet. Nous choisissons délibérément le composant pour démontrer les problèmes auxquels vous devrez faire face tôt ou tard. N&#39;ayez pas peur - tous les composants n&#39;ont pas besoin de cette considération que nous présenterons. Mais si vous voyez la nécessité de construire un tel composant, vous êtes bien conscient des conséquences et savez comment les gérer.

### Modèle de composant de mise en réseau (anti)

#### Composant d’image réactive

Illustrons un modèle commun (ou anti-modèle) d’un composant avec des binaires interconnectés. Nous allons créer un composant &quot;respi&quot; pour &quot;responsive-image&quot;. Ce composant doit pouvoir adapter l’image affichée à l’appareil sur lequel elle est affichée. Sur les ordinateurs de bureau et les tablettes, il montre la résolution complète de l&#39;image, sur les téléphones une version plus petite avec un recadrage étroit - ou peut-être même un motif complètement différent (c&#39;est ce qu&#39;on appelle &quot;direction artistique&quot; dans le monde réactif).

Les ressources sont chargées dans la zone DAM de l’AEM et uniquement _référencées_ dans le composant Responsive-image.

Le composant de réponse s’occupe à la fois du rendu du balisage et de la diffusion des données d’image binaire.

La façon dont nous la mettons en oeuvre ici est un modèle commun que nous avons vu dans de nombreux projets et même l&#39;un des composants principaux AEM est basé sur ce modèle. Il est donc très probable que vous, en tant que développeur, adaptiez ce modèle. Il a ses points forts en termes d&#39;encapsulation, mais il nécessite beaucoup d&#39;efforts pour le préparer au Dispatcher. Nous discuterons plus tard de plusieurs options pour atténuer le problème.

Nous appelons le modèle utilisé ici le &quot;Spooler Pattern&quot;, parce que le problème remonte aux premiers jours de Communiqué 3 où il y avait une méthode &quot;spool&quot; qui pouvait être appelée sur une ressource pour diffuser ses données brutes binaires dans la réponse.

Le terme original &quot;mise en file d’attente&quot; fait en fait référence à des périphériques hors ligne lents partagés, tels que les imprimantes, de sorte qu’il n’est pas correctement appliqué ici. Mais nous aimons le terme de toute façon parce qu&#39;il est rarement dans le monde en ligne, donc reconnaissable. Et chaque modèle devrait avoir un nom distinctif de toute façon, n&#39;est-ce pas ? C&#39;est à vous de décider s&#39;il s&#39;agit d&#39;un motif ou d&#39;un anti-motif.

#### Mise en œuvre

Voici comment notre composant d’image réactive est mis en oeuvre :

Le composant comporte deux parties : la première partie effectue le rendu du balisage HTML de l’image, la seconde &quot;met en file d’attente&quot; les données binaires de l’image référencée. Comme il s’agit d’un site web moderne avec une conception réactive, nous ne rendons pas une balise `<img src"…">` simple, mais un ensemble d’images dans la balise `<picture/>`. Pour chaque appareil, nous chargeons deux images différentes dans la gestion des ressources numériques et nous les référençons à partir de notre composant d’image.

Le composant comporte trois scripts de rendu (implémentés en JSP, HTL ou en tant que servlet), chacun étant traité avec un sélecteur dédié :

1. `/respi.jsp` - sans sélecteur pour le rendu du balisage HTML
2. `/respi.img.java` pour effectuer le rendu de la version de bureau
3. `/respi.img.mobile.java` pour effectuer le rendu de la version mobile.


Le composant est placé dans le parsys de la page d’accueil. La structure résultante dans le CRX est illustrée ci-dessous.

![Structure de ressource de l’image réactive dans CRX](assets/chapter-1/responsive-image-crx.png)

*Structure de ressource de l’image réactive dans CRX*

<br> 

Le balisage des composants est rendu comme ceci,

```plain
  #GET /content/home.html

  <html>

  …

  <div class="responsive-image>

  <picture>
    <source src="/content/home/jcr:content/par/respi.img.mobile.jpg" …/>
    <source src="/content/home/jcr:content/par/respi.img.jpg …/>

    …

  </picture>
  </div>
  …
```

et... nous avons fini avec notre composant bien encapsulé.

#### Composant d’image réactive en action

Désormais, un utilisateur demande la page et les ressources via Dispatcher. Cela génère des fichiers dans le système de fichiers de Dispatcher comme illustré ci-dessous,

![Structure mise en cache du composant d’image réactive encapsulé](assets/chapter-1/cached-structure-encapsulated-image-comonent.png)

*Structure mise en cache du composant d’image réactive encapsulé*

<br> 

Imaginons qu’un utilisateur charge et active une nouvelle version des deux images de fleurs dans la gestion des ressources numériques. AEM enverra une demande d’invalidation pour

`/content/dam/flower.jpg`

et

`/content/dam/flower-mobile.jpg`

à Dispatcher. Ces demandes sont cependant vaines. Le contenu a été mis en cache sous la forme de fichiers sous la sous-structure du composant. Ces fichiers sont désormais obsolètes, mais toujours diffusés sur les demandes.

![Incohérence de la structure à l’origine de l’obsolescence du contenu](assets/chapter-1/structure-mismatch.png)

*Incohérence de la structure à l’origine de l’obsolescence du contenu*

<br> 

Il y a une autre mise en garde à cette approche. Pensez à utiliser le même fichier flower.jpg sur plusieurs pages. Vous disposez ensuite de la même ressource mise en cache sous plusieurs URL ou fichiers,

```
/content/home/products/jcr:content/par/respi.img.jpg

/content/home/offers/jcr:content/par/respi.img.jpg

/content/home/specials/jcr:content/par/respi.img.jpg

…
```

Chaque fois qu’une nouvelle page non mise en cache est demandée, les ressources sont récupérées à partir d’AEM à différentes URL. Aucune mise en cache de Dispatcher et aucune mise en cache du navigateur ne peut accélérer la diffusion.

#### L&#39;endroit où le motif du saboteur brille

Il existe une exception naturelle, où ce modèle, même sous sa forme simple, est utile : Si le fichier binaire est stocké dans le composant lui-même, et non dans la gestion des ressources numériques. Cela n’est toutefois utile que pour les images utilisées une seule fois sur le site web. Le fait de ne pas stocker de ressources dans la gestion des ressources numériques (DAM) vous rend difficile la gestion de vos ressources. Imaginez que votre licence d’utilisation pour une ressource particulière soit épuisée. Comment savoir quels composants vous avez utilisés la ressource ?

Vous voyez ? Le &quot;M&quot; dans la gestion des actifs numériques signifie &quot;Gestion&quot;, comme dans la gestion des actifs numériques. Vous ne voulez pas donner cette fonctionnalité.

#### Conclusion

Du point de vue d&#39;un AEM développeur, le motif semblait super élégant. Mais avec Dispatcher intégré dans l&#39;équation, vous pourriez être d&#39;accord, que l&#39;approche naïve pourrait ne pas être suffisante.

Nous vous laissons décider s&#39;il s&#39;agit d&#39;un motif ou d&#39;un anti-motif pour l&#39;instant. Et peut-être avez-vous déjà quelques bonnes idées en tête pour atténuer les problèmes expliqués ci-dessus ? Bien. Vous serez alors impatient de voir comment d&#39;autres projets ont résolu ces problèmes.

### Résolution des problèmes courants du Dispatcher

#### Présentation

Parlons de la façon dont cela aurait pu être mis en oeuvre un peu plus facile à mettre en cache. Il existe plusieurs options. Parfois, vous ne pouvez pas choisir la meilleure solution. Peut-être que vous venez dans un projet déjà en cours et que vous avez un budget limité pour résoudre le &quot;problème de cache&quot; à portée de main et pas assez pour effectuer une refactorisation complète. Ou vous rencontrez un problème, qui est plus complexe que l’exemple de composant d’image.

Nous présenterons les principes et les avertissements dans les sections suivantes.

Encore une fois, ceci est basé sur l&#39;expérience réelle. Nous avons déjà vu tous ces modèles dans la nature, ce n&#39;est donc pas un exercice universitaire. C&#39;est pourquoi nous vous montrons des anti-modèles, donc vous avez la chance d&#39;apprendre des erreurs que d&#39;autres ont déjà faites.

#### Tueur de cache

>[!WARNING]
>
>C&#39;est un anti-modèle. Ne l’utilisez pas. Jamais.

Avez-vous déjà vu des paramètres de requête tels que `?ck=398547283745` ? Ils sont appelés cache-kill (&quot;ck&quot;). L’idée est que si vous ajoutez un paramètre de requête, la ressource ne sera pas mise en cache. De plus, si vous ajoutez un nombre aléatoire comme valeur du paramètre (comme &quot;398547283745&quot;), l’URL devient unique et vous vous assurez qu’aucun autre cache entre le système AEM et votre écran ne peut mettre en cache non plus. En règle générale, les suspects intermédiaires seraient un cache &quot;vernis&quot; devant Dispatcher, un réseau de diffusion de contenu ou même le cache du navigateur. Encore une fois : Ne fais pas ça. Vous souhaitez que vos ressources soient mises en cache autant et aussi longtemps que possible. Le cache est votre ami. Ne tuez pas d&#39;amis.

#### Invalidation automatique

>[!WARNING]
>
>C&#39;est un anti-modèle. Évitez de l’utiliser pour les ressources numériques. Essayez de conserver la configuration par défaut de Dispatcher, qui > est l’invalidation automatique des fichiers &quot;.html&quot; uniquement.

À court terme, vous pouvez ajouter &quot;.jpg&quot; et &quot;.png&quot; à la configuration d’invalidation automatique dans Dispatcher. Cela signifie que, chaque fois qu’une invalidation se produit, tous les &quot;.jpg&quot;, &quot;.png&quot; et &quot;.html&quot; doivent être rendus à nouveau.

Ce modèle est extrêmement facile à mettre en oeuvre si les propriétaires d’entreprise se plaignent de ne pas voir leurs modifications se matérialiser suffisamment rapidement sur le site en ligne. Mais cela ne peut que vous faire gagner du temps pour trouver une solution plus sophistiquée.

Assurez-vous de bien comprendre les vastes répercussions sur les performances. Cela ralentira considérablement votre site web et pourrait même avoir un impact sur la stabilité, si votre site est un site web à forte charge avec des modifications fréquentes, comme un portail d’actualités.

#### empreinte URL

Une empreinte d’URL ressemble à un tueur de cache. Mais ce n&#39;est pas le cas. Il ne s’agit pas d’un nombre aléatoire, mais d’une valeur qui caractérise le contenu de la ressource. Il peut s’agir d’un hachage du contenu de la ressource ou, plus simple encore, d’un horodatage lorsque la ressource a été chargée, modifiée ou mise à jour.

Un horodatage Unix est suffisant pour une mise en oeuvre concrète. Pour une meilleure lisibilité, nous utilisons un format plus lisible dans ce tutoriel : `2018 31.12 23:59 or fp-2018-31-12-23-59`.

L’empreinte numérique ne doit pas être utilisée comme paramètre de requête, comme URL avec des paramètres de requête.   ne peuvent pas être mis en cache. Vous pouvez utiliser un sélecteur ou le suffixe pour l’empreinte.

Supposons que le fichier `/content/dam/flower.jpg` ait une date `jcr:lastModified` du 31 décembre 2018, 23:59. L’URL avec l’empreinte digitale est `/content/home/jcr:content/par/respi.fp-2018-31-12-23-59.jpg`.

Cette URL reste stable tant que le fichier de ressource référencée (`flower.jpg`) n’est pas modifié. Il peut donc être mis en cache pendant une durée indéfinie et ce n&#39;est pas un tueur de cache.

Notez que cette URL doit être créée et diffusée par le composant d’image réactive. Il ne s’agit pas d’une AEM d’usine.

C&#39;est le concept de base. Il y a cependant quelques détails qui peuvent facilement être négligés.

Dans notre exemple, le composant a été rendu et mis en cache à 23:59. Maintenant l&#39;image a été modifiée disons à 00h00.  Le composant _génère une nouvelle URL empreinte dans son balisage._

Vous pourriez penser que _devrait_... mais ce n&#39;est pas le cas. Comme seul le binaire de l’image a été modifié et que la page incluse n’a pas été touchée, le rendu du balisage HTML n’est pas nécessaire. Ainsi, Dispatcher diffuse la page avec l’ancienne empreinte numérique, et donc l’ancienne version de l’image.

![Composant d’image plus récent que l’image référencée, aucune nouvelle empreinte n’est rendue.](assets/chapter-1/recent-image-component.png)

*Composant d’image plus récent que l’image référencée, aucune nouvelle empreinte n’est rendue.*

<br> 

Désormais, si vous avez réactivé la page d’accueil (ou toute autre page de ce site), le fichier stat est mis à jour, Dispatcher considère l’obsolescence de home.html et effectue à nouveau le rendu avec une nouvelle empreinte dans le composant d’image.

Mais nous n&#39;avons pas activé la page d&#39;accueil, n&#39;est-ce pas ? Et pourquoi devrions-nous activer une page sur laquelle nous ne l&#39;avons pas touchée de toutes façons ? De plus, peut-être que nous n&#39;avons pas les droits suffisants pour activer les pages ou que le processus d&#39;approbation est si long et chronophage, que nous ne pouvons tout simplement pas le faire à court terme. Alors - que faire ?

#### Outil de l’administrateur différé - Diminution des niveaux de fichier stat

>[!WARNING]
>
>C&#39;est un anti-modèle. Utilisez-le seulement à court terme pour gagner du temps et trouver une solution plus sophistiquée.

L’administrateur différé &quot;_définit généralement l’invalidation automatique sur jpgs et le niveau du fichier stat sur zéro - ce qui aide toujours à mettre en cache des problèmes de tous types_&quot;. Vous trouverez ce conseil dans les forums technologiques, et cela vous aidera à résoudre votre problème d&#39;invalidation.

Jusqu’à présent, nous n’avons pas discuté du niveau du fichier stat. En fait, l’invalidation automatique ne fonctionne que pour les fichiers de la même sous-arborescence. Cependant, le problème est que les pages et les ressources ne vivent généralement pas dans la même sous-arborescence. Les pages se trouvent quelque part en dessous de `/content/mysite` tandis que les ressources se trouvent en dessous de `/content/dam`.

Le &quot;niveau du fichier stat&quot; définit où se trouvent les noeuds racine de profondeur des sous-arborescences. Dans l’exemple ci-dessus, le niveau serait &quot;2&quot; (1=/content, 2=/mysite,dam).

L’idée de &quot;réduire&quot; le niveau du fichier stat à 0 consiste à définir l’arborescence /content entière comme sous-arborescence unique et unique pour que les pages et les ressources vivent dans le même domaine d’invalidation automatique. Donc nous n&#39;aurions qu&#39;un gros arbre au niveau (au docroot &quot;/&quot;). Mais cela invalide automatiquement tous les sites sur le serveur chaque fois qu’un élément est publié - même sur des sites complètement indépendants. Faites-nous confiance : C&#39;est une mauvaise idée à long terme, car vous allez sérieusement dégrader le taux de accès global du cache. Tout ce que vous pouvez faire, c&#39;est espérer que vos serveurs AEM disposent d&#39;une puissance de feu suffisante pour fonctionner sans cache.

Vous comprendrez les avantages complets d’un niveau de fichier stat plus profond un peu plus tard.

#### Implémentation d’un agent d’invalidation personnalisé

Quoi qu’il en soit : nous devons indiquer au Dispatcher d’une manière ou d’une autre, d’invalider les pages HTML si un fichier &quot;.jpg&quot; ou &quot;.png&quot; a été modifié pour permettre le rendu avec une nouvelle URL.

Ce que nous avons vu dans les projets est, par exemple, des agents de réplication spéciaux sur le système de publication qui envoient des demandes d’invalidation pour un site chaque fois qu’une image de ce site est publiée.

Ici, cela peut être utile si vous pouvez dériver le chemin du site du chemin d’accès de la ressource en utilisant la convention d’affectation des noms.

En règle générale, il est préférable de faire correspondre les sites et les chemins d’accès aux ressources comme suit :

**Exemple**

```
/content/dam/site-a
/content/dam/site-b

/content/site-a
/content/site-b
```

Ainsi, votre agent de purge de Dispatcher personnalisé peut facilement envoyer une demande d’invalidation à /content/site-a lorsqu’il rencontre une modification sur `/content/dam/site-a`.

En fait, peu importe le chemin que vous demandez à Dispatcher d’invalider, tant qu’il se trouve sur le même site, dans la même &quot;sous-arborescence&quot;. Vous n’avez même pas besoin d’utiliser un véritable chemin d’accès aux ressources. Il peut également être &quot;virtuel&quot; :

```
GET /dispatcher-invalidate
Invalidate-path /content/mysite/dummy
```

![](assets/chapter-1/resource-path.png)

1. Un écouteur sur le système de publication est déclenché lorsqu’un fichier dans la gestion des actifs numériques change

2. L’écouteur envoie une demande d’invalidation à Dispatcher. En raison de l’invalidation automatique, le chemin que nous envoyons dans l’invalidation automatique importe peu, sauf s’il se trouve sous la page d’accueil du site - ou plus précisément au niveau du fichier stat du site.

3. Le fichier stat est mis à jour.

4. La prochaine fois que la page d’accueil sera demandée, elle sera de nouveau rendue. La nouvelle empreinte/date est extraite de la propriété lastModified de l’image en tant que sélecteur supplémentaire.

5. Cela crée implicitement une référence à une nouvelle image.

6. Si l’image est réellement demandée, un nouveau rendu est créé et stocké dans Dispatcher.


#### La nécessité du nettoyage

Ouf. Terminé. Hourra !

Eh bien.. pas encore tout à fait.

Le chemin,

`/content/mysite/home/jcr:content/par/respi.img.fp-2018-31-12-23-59.jpg`

ne correspond à aucune des ressources invalidées. Mémoriser? Nous avons uniquement invalidé une ressource &quot;factice&quot; et nous nous sommes appuyés sur l’invalidation automatique pour considérer que &quot;home&quot; est non valide. L’image elle-même peut ne jamais être _physiquement_ supprimée. Ainsi, le cache va grandir et grandir. Lorsque les images sont modifiées et activées, elles obtiennent de nouveaux noms de fichier dans le système de fichiers de Dispatcher.

Il existe trois problèmes liés à la non-suppression physique des fichiers mis en cache et à leur conservation indéfinie :

1. Vous gaspillez de la capacité de stockage - c&#39;est évident. Acceptée - le stockage est devenu moins cher et moins cher ces dernières années. Mais la résolution des images et la taille des fichiers ont également augmenté ces dernières années - avec l&#39;avènement d&#39;écrans de type rétine, avide d&#39;images pointues de cristal.

2. Même si les disques durs sont devenus moins chers, le &quot;stockage&quot; n&#39;est peut-être pas devenu moins cher. Nous avons constaté une tendance à ne pas avoir de stockage de disque dur métallique (bon marché), mais de stockage virtuel de location sur un NAS par votre fournisseur de centre de données. Ce type de stockage est un peu plus fiable et évolutif mais aussi un peu plus cher. Il se peut que vous ne vouliez pas le gaspiller en stockant des ordures obsolètes. Cela ne concerne pas seulement le stockage Principal, mais aussi les sauvegardes. Si vous disposez d’une solution de sauvegarde d’usine, vous ne pourrez peut-être pas exclure les répertoires de cache. En fin de compte, vous sauvegardez également les données de la mémoire.

3. Pire encore : Vous avez peut-être acheté des licences d’utilisation pour certaines images uniquement pendant une période limitée, à condition que vous en ayez besoin. Maintenant, si vous stockez toujours l&#39;image après l&#39;expiration d&#39;une licence, cela peut être considéré comme une violation de copyright. Vous n’utiliserez peut-être plus l’image dans vos pages web, mais Google les trouvera toujours.

Donc finalement, vous allez trouver un petit boulot de ménage pour nettoyer tous les dossiers plus anciens que... disons une semaine pour garder ce genre de détritus sous contrôle.

#### Abus des empreintes digitales d’URL pour les attaques par déni de service

Mais attendez, il y a un autre défaut dans cette solution :

Nous abusons en quelque sorte d’un sélecteur comme paramètre : fp-2018-31-12-23-59 est généré dynamiquement sous la forme d’une sorte de &quot;tueur de cache&quot;. Mais peut-être qu&#39;un enfant ennuyé (ou un robot de recherche qui s&#39;est déchaîné) commence à demander les pages :

```
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-00.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-01.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-02.jpg

…
```

Chaque requête contourne Dispatcher, ce qui entraîne un chargement sur une instance de publication. Et, pire encore, créez un fichier conforme sur Dispatcher.

Donc.. au lieu de simplement utiliser l&#39;empreinte comme simple tueur de cache, vous devez vérifier la date jcr:lastModified de l&#39;image et renvoyer une valeur 404 si ce n&#39;est pas la date attendue. Cela prend du temps et des cycles de processeur sur le système de publication... ce que vous souhaitez éviter en premier lieu.

#### Avertissements d’empreintes digitales d’URL dans les versions haute fréquence

Vous pouvez utiliser le schéma d’empreinte digitale non seulement pour les ressources provenant de la gestion des actifs numériques, mais également pour les fichiers JS et CSS et les ressources associées.

[Version ](https://adobe-consulting-services.github.io/acs-aem-commons/features/versioned-clientlibs/index.html) Clientlibs : module qui utilise cette approche.

Mais ici, vous pouvez faire face à une autre mise en garde avec les empreintes URL : Il lie l’URL au contenu. Vous ne pouvez pas modifier le contenu sans modifier également l’URL (c’est-à-dire mettre à jour la date de modification). C&#39;est pour cela que les empreintes digitales sont conçues en premier lieu. Mais pensez à déployer une nouvelle version, avec de nouveaux fichiers CSS et JS, et donc de nouvelles URL avec de nouvelles empreintes digitales. Toutes vos pages HTML contiennent toujours des références aux anciennes URL imprimées. Ainsi, pour que la nouvelle version fonctionne de manière cohérente, vous devez invalider toutes les pages HTML en même temps afin de forcer un nouveau rendu avec des références aux fichiers nouvellement imprimés. Si plusieurs sites reposent sur les mêmes bibliothèques, cela peut être un nombre considérable de rendus. Dans ce cas, vous ne pouvez pas exploiter le `statfiles`. Soyez donc prêt à afficher les pics de chargement sur vos systèmes de publication après un déploiement. Vous pouvez envisager un déploiement bleu/vert avec réchauffement du cache ou peut-être un cache basé sur TTL devant votre Dispatcher ... les possibilités sont infinies.

#### Saut court

Waouh - C&#39;est pas mal de détails, n&#39;est-ce pas ? Et il refuse d&#39;être compris, testé et débogué facilement. Et tout ça pour une solution apparemment élégante. Il est vrai qu&#39;elle est élégante - mais uniquement du point de vue de l&#39;AEM. Avec Dispatcher, cela devient méchant.

Et pourtant, cela ne résout pas un seul avertissement de base : si une image est utilisée plusieurs fois sur différentes pages, elles seront mises en cache sous ces pages. Il n&#39;y a pas beaucoup de synergie cachée là-bas.

En général, l’empreinte d’URL est un bon outil à utiliser dans votre boîte à outils, mais vous devez l’appliquer avec précaution, car cela peut entraîner de nouveaux problèmes tout en n’en résolvant que quelques existants.

C&#39;était un long chapitre. Mais nous avons vu ce schéma si souvent, que nous avons senti qu&#39;il était nécessaire de vous donner une image complète avec tous les pour et les contre. Les empreintes digitales de l’URL permettent de résoudre quelques-uns des problèmes inhérents au modèle de spouleur, mais l’effort de mise en oeuvre est plutôt élevé et vous devez également envisager d’autres solutions plus simples. Il est conseillé de toujours vérifier si vous pouvez baser vos URL sur les chemins de ressources diffusés et ne pas avoir de composant intermédiaire. Nous y reviendrons dans le prochain chapitre.

##### Résolution des dépendances d’exécution

La résolution des dépendances d’exécution est un concept que nous avons envisagé dans un projet. Mais le penser à travers est devenu assez complexe et nous avons décidé de ne pas le mettre en oeuvre.

Voici l&#39;idée de base :

Dispatcher ne connaît pas les dépendances des ressources. C&#39;est juste un tas de fichiers uniques avec peu de sémantique.

AEM ne sait pas grand chose des dépendances. Il manque une sémantique correcte ou un &quot;dispositif de suivi des dépendances&quot;.

AEM connaît certaines des références. Il utilise ces connaissances pour vous avertir lorsque vous essayez de supprimer ou de déplacer une page ou une ressource référencée. Pour ce faire, il interroge la recherche interne lors de la suppression d’une ressource. Les références au contenu ont une forme très particulière. Il s’agit d’expressions de chemin commençant par &quot;/content&quot;. Ils peuvent donc facilement être indexés en texte intégral - et interrogés lorsque cela est nécessaire.

Dans notre cas, nous aurions besoin d’un agent de réplication personnalisé sur le système de publication, qui déclenche une recherche pour un chemin spécifique lorsque ce chemin a changé.

Disons :

`/content/dam/flower.jpg`

A changé lors de la publication. L’agent déclenche une recherche pour &quot;/content/dam/flower.jpg&quot; et trouve toutes les pages référençant ces images.

Il peut ensuite envoyer un certain nombre de demandes d’invalidation au Dispatcher. Une pour chaque page contenant la ressource.

En théorie, cela devrait marcher. Mais uniquement pour les dépendances de premier niveau. Vous ne souhaitez pas appliquer ce modèle aux dépendances à plusieurs niveaux, par exemple lorsque vous utilisez l’image sur un fragment d’expérience utilisé sur une page. En fait, nous pensons que cette approche est trop complexe - et qu&#39;il pourrait y avoir des problèmes d&#39;exécution. Et en général le meilleur conseil est de ne pas faire de calcul coûteux dans les gestionnaires d&#39;événements. Et surtout les recherches peuvent devenir très coûteuses.

##### Conclusion

Nous espérons que nous avons suffisamment discuté du modèle de spouleur pour vous aider à décider quand l’utiliser et ne pas l’utiliser dans votre mise en oeuvre.

## Éviter les problèmes du Dispatcher

### URL basées sur des ressources

Une manière bien plus élégante de résoudre le problème de la dépendance est de ne pas avoir de dépendances du tout. Évitez les dépendances artificielles qui se produisent lors de l’utilisation d’une ressource pour en remplacer une autre, comme nous l’avons fait dans le dernier exemple. Essayez de voir les ressources comme des entités &quot;solitaires&quot; aussi souvent que possible.

Notre exemple est facile à résoudre :

![Mise en rotation de l’image avec une servlet liée à l’image, et non au composant.](assets/chapter-1/spooling-image.png)

*Mise en rotation de l’image avec une servlet liée à l’image, et non au composant.*

<br> 

Nous utilisons les chemins d’accès aux ressources d’origine pour effectuer le rendu des données. Si nous avons besoin de rendre l’image d’origine telle quelle, nous pouvons simplement utiliser AEM rendu par défaut pour les ressources.

Si nous devons effectuer un traitement spécial pour un composant spécifique, nous enregistrons une servlet dédiée sur ce chemin et un sélecteur pour effectuer la transformation pour le compte du composant. Nous l&#39;avons fait ici exemplaire avec le &quot;.respi&quot;. sélecteur. Il est conseillé de conserver une trace des noms de sélecteur utilisés dans l’espace URL global (tel que `/content/dam`) et de respecter une convention d’affectation des noms efficace pour éviter les conflits de noms.

Au fait, nous ne voyons aucun problème de cohérence du code. Le servlet peut être défini dans le même package Java que le modèle sling des composants.

Nous pouvons même utiliser des sélecteurs supplémentaires dans l’espace global, tels que :

`/content/dam/flower.respi.thumbnail.jpg`

Facile, n&#39;est-ce pas ? Alors pourquoi les gens trouvent-ils des motifs compliqués comme le Spooler ?

Eh bien, nous pourrions résoudre le problème en évitant la référence de contenu interne parce que le composant externe ajoutait peu de valeur ou d&#39;informations au rendu de la ressource interne, qu&#39;il pouvait facilement être encodé dans un ensemble de sélecteurs statiques qui contrôlent la représentation d&#39;une ressource solitaire.

Mais il existe une catégorie de cas que vous ne pouvez pas résoudre facilement avec une URL basée sur des ressources. Nous appelons ce type de cas &quot;Paramètre d’injection de composants&quot;, et nous en discutons dans le chapitre suivant.

### Paramètre d’ingestion des composants

#### Présentation

Le spouleur du dernier chapitre n&#39;était qu&#39;un mince wrapper autour d&#39;une ressource. Cela a causé plus de problèmes qu&#39;aider à résoudre le problème.

Nous pourrions facilement substituer cet encapsulage à l’aide d’un sélecteur simple et ajouter un servlet adéquat pour répondre à de telles requêtes.

Mais que se passe-t-il si le composant &quot;respi&quot; est plus qu&#39;un simple proxy. Que se passe-t-il si le composant contribue véritablement au rendu du composant ?

Introduisons une petite extension de notre composant &quot;respi&quot;, qui change un peu la donne. Encore une fois, nous allons d&#39;abord introduire des solutions naïves pour relever les nouveaux défis et montrer où ils ne sont pas à la hauteur.

#### Composant Respi2

Le composant respi2 est un composant qui affiche une image réactive, tout comme le composant de réponse. Mais il y a un petit supplément,

![Structure CRX : composant respi2 ajoutant une propriété de qualité à la diffusion](assets/chapter-1/respi2.png)

*Structure CRX : composant respi2 ajoutant une propriété de qualité à la diffusion*

<br> 

Les images sont des jpgs et les jpgs peuvent être compressés. Lorsque vous compressez une image jpeg, vous échangez la qualité sur la taille de fichier. La compression est définie comme un paramètre numérique de &quot;qualité&quot; compris entre &quot;1&quot; et &quot;100&quot;. &quot;1&quot; signifie &quot;petite mais mauvaise qualité&quot;, &quot;100&quot; signifie &quot;excellente qualité mais fichiers volumineux&quot;. Alors quelle est la valeur parfaite ?

Comme dans tous les services informatiques, la réponse est : &quot;Ça dépend.&quot;

Ici cela dépend du motif. Les motifs aux contours contrastés comme les motifs tels que le texte écrit, les photos de bâtiments, les illustrations, les croquis ou les photos de boîtes de produits (avec des contours nets et du texte écrit dessus) entrent généralement dans cette catégorie. Les motifs avec des transitions de couleur et de contraste plus douces, comme les paysages ou les portraits, peuvent être compressés un peu plus sans perte de qualité visible. Les photographies de la nature entrent généralement dans cette catégorie.

En outre, selon l’emplacement d’utilisation de l’image, vous pouvez utiliser un autre paramètre. Une petite miniature dans un teaser peut supporter une meilleure compression que celle utilisée dans une bannière principale à l’échelle de l’écran. Cela signifie que le paramètre de qualité n’est pas lié à l’image, mais à l’image et au contexte. Et au goût de l&#39;auteur.

En bref : Il n&#39;y a pas de cadre parfait pour toutes les photos. Il n&#39;y a pas de modèle unique. C&#39;est l&#39;auteur qui décide le mieux. Il adaptera le paramètre &quot;qualité&quot; en tant que propriété dans le composant jusqu’à ce qu’il soit satisfait de la qualité et qu’il n’aille pas plus loin pour ne pas sacrifier la bande passante.

Nous disposons désormais d’un fichier binaire dans la gestion des actifs numériques et d’un composant qui fournit une propriété de qualité. À quoi doit ressembler l’URL ? Quel composant est responsable de la mise en file d’attente ?

#### Approche naïve 1 : Transmission des propriétés en tant que paramètres de requête

>[!WARNING]
>
>C&#39;est un anti-modèle. Ne l’utilisez pas.

Dans le dernier chapitre, l’URL de l’image générée par le composant ressemblait à ceci :

`/content/dam/flower.respi.jpg`

Tout ce qui manque, c&#39;est la valeur de la qualité. Le composant sait quelle propriété est saisie par l’auteur. Il peut facilement être transmis au servlet de rendu d’image en tant que paramètre de requête lorsque le balisage est rendu, comme `flower.respi2.jpg?quality=60` :

```plain
  <div class="respi2">
  <picture>
    <source src="/content/dam/flower.respi2.jpg?quality=60" …/>
    …
  </picture>
  </div>
  …
```

C&#39;est une mauvaise idée. Mémoriser? Les requêtes avec des paramètres de requête ne peuvent pas être mises en cache.

#### Approche naïve 2 : Transmission d’informations supplémentaires en tant que sélecteur

>[!WARNING]
>
>Cela pourrait devenir un anti-modèle. Utilisez-le avec attention.

![Transmission des propriétés des composants en tant que sélecteurs](assets/chapter-1/passing-component-properties.png)

*Transmission des propriétés des composants en tant que sélecteurs*

<br> 

Il s’agit d’une légère variation de la dernière URL. Ce n’est que cette fois que nous utilisons un sélecteur pour transmettre la propriété à la servlet, de sorte que le résultat puisse être mis en cache :

`/content/dam/flower.respi.q-60.jpg`

C&#39;est bien mieux, mais souvenez-vous de ce méchant script-gamin du dernier chapitre qui cherche de tels modèles ? Il voyait jusqu&#39;où il pouvait aller en survolant les valeurs :

```plain
  /content/dam/flower.respi.q-60.jpg
  /content/dam/flower.respi.q-61.jpg
  /content/dam/flower.respi.q-62.jpg
  /content/dam/flower.respi.q-63.jpg
  …
```

Cette fois encore, le cache est contourné et la charge est créée sur le système de publication. Ça pourrait être une mauvaise idée. Vous pouvez atténuer ce problème en filtrant uniquement un petit sous-ensemble de paramètres. Vous souhaitez n’autoriser que `q-20, q-40, q-60, q-80, q-100`.

#### Filtrage des requêtes non valides lors de l’utilisation de sélecteurs

Réduire le nombre de sélecteurs était un bon début. En règle générale, vous devez toujours limiter le nombre de paramètres valides à un minimum absolu. Si vous le faites intelligemment, vous pouvez même utiliser un pare-feu d’application web en dehors de l’AEM en utilisant un ensemble statique de filtres sans connaissance approfondie du système AEM sous-jacent pour protéger vos systèmes :

```
Allow: /content/dam/(-\_/a-z0-9)+/(-\_a-z0-9)+
       \.respi\.q-(20|40|60|80|100)\.jpg
```

Si vous ne disposez pas d’un pare-feu d’application web, vous devez filtrer dans Dispatcher ou dans AEM lui-même. Si vous le faites en AEM, assurez-vous que

1. Le filtre est implémenté très efficacement, sans accéder trop à CRX et perdre de la mémoire et du temps.

2. Le filtre répond à un message d’erreur &quot;404 - Introuvable&quot;.

Faisons ressurgir le dernier point. La conversation HTTP ressemblerait à ceci :

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 404 – Not found
  << empty response body >>
```

Nous avons également vu des implémentations qui filtraient des paramètres non valides mais renvoyaient un rendu de secours valide lorsqu’un paramètre non valide est utilisé. Supposons que nous n’autorisions que les paramètres compris entre 20 et 100. Les valeurs intermédiaires sont mappées aux valeurs valides. Donc,

`q-41, q-42, q-43, …`

répond toujours à la même image que q-40 :

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 200 – OK
  << flower.jpg with quality = 40 >>
```

Cette approche n&#39;aide en rien. Ces requêtes sont en fait des requêtes valides.  Ils consomment de la puissance de traitement et occupent de l’espace dans le répertoire du cache de Dispatcher.

Il est préférable de renvoyer une valeur `301 – Moved permanently` :

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 301 – Moved permanently
  Location: /content/dam/flower.respi.q-40.jpg
```

AEM en parle au navigateur. &quot;Je n&#39;ai pas `q-41`. Mais hé - vous pouvez me poser des questions sur `q-40` &quot;.

Cela ajoute une boucle de demande-réponse supplémentaire à la conversation, qui est un peu de surcharge, mais c’est moins cher que d’effectuer le traitement complet sur `q-41`. Vous pouvez également exploiter le fichier déjà mis en cache sous `q-40`. Cependant, vous devez comprendre que les réponses 302 ne sont pas mises en cache dans Dispatcher. Il s’agit de la logique exécutée dans l’AEM. Encore et encore. Donc vous feriez mieux de le faire mince et rapide.

Nous apprécions personnellement le 404 qui répond le plus. Cela rend très évident ce qui se passe. et vous aide à détecter les erreurs sur votre site web lorsque vous analysez des fichiers journaux. Il est possible d&#39;envisager des 301, où l&#39;article 404 doit toujours être analysé et éliminé.

## Sécurité - Excursion

### Filtrage des requêtes

#### Où filtrer le mieux

À la fin du dernier chapitre, nous avons souligné la nécessité de filtrer le trafic entrant pour les sélecteurs connus. Reste la question : Où dois-je réellement filtrer les requêtes ?

Eh bien, ça dépend. Le plus tôt sera le mieux.

#### pare-feu d’applications web

Si vous disposez d’un outil de pare-feu d’applications web ou &quot;WAF&quot; conçu pour la sécurité web, vous devez absolument exploiter ces fonctionnalités. Mais vous pourriez découvrir que le WAF est géré par des personnes qui ne connaissent que peu votre application de contenu et qui filtrent les requêtes valides ou laissent passer trop de requêtes dangereuses. Vous découvrirez peut-être que les personnes qui gèrent le WAF sont assignées à un autre service avec des horaires et des horaires différents, la communication peut ne pas être aussi étroite qu&#39;avec vos coéquipiers directs et vous n&#39;obtenez pas toujours les changements dans le temps, ce qui signifie que finalement votre développement et votre vitesse de contenu souffrent.

Vous pourriez finir avec quelques règles générales ou même une liste bloquée, que votre instinct vous dit, pourrait être renforcée.

#### Filtrage de Dispatcher et de publication

L’étape suivante consiste à ajouter des règles de filtrage d’URL dans le noyau Apache et/ou dans Dispatcher.

Ici, vous avez accès aux URL uniquement. Vous êtes limité aux filtres basés sur des motifs. Si vous devez configurer un filtrage basé sur le contenu (comme autoriser les fichiers uniquement avec un horodatage correct) ou si vous souhaitez que le filtrage soit contrôlé sur votre auteur, vous finirez par écrire quelque chose comme un filtre de servlet personnalisé.

#### Surveillance et débogage

En pratique, vous disposez d’une certaine sécurité à chaque niveau. Veillez toutefois à disposer des moyens nécessaires pour savoir à quel niveau une requête est filtrée. Assurez-vous d’avoir un accès direct au système de publication, à Dispatcher et aux fichiers journaux sur le WAF pour savoir quel filtre de la chaîne bloque les demandes.

### Sélecteurs et prolifération de sélecteurs

L’approche utilisant &quot;selector-parameters&quot; dans le dernier chapitre est rapide et facile et peut accélérer le temps de développement des nouveaux composants, mais elle a des limites.

La définition d’une propriété &quot;qualité&quot; n’est qu’un exemple simple. Mais supposons que le servlet s’attend également à ce que les paramètres de &quot;largeur&quot; soient plus polyvalents.

Vous pouvez réduire le nombre d’URL valides en réduisant le nombre de valeurs de sélecteur possibles. Vous pouvez également faire de même avec la largeur :

qualité = q-20, q-40, q-60, q-80, q-100

largeur = w-100, w-200, w-400, w-800, w-1000, w-1200

Toutes les combinaisons sont désormais des URL valides :

```
/content/dam/flower.respi.q-40.w-200.jpg
/content/dam/flower.respi.q-60.w-400.jpg
…
```

Désormais, nous disposons déjà de 5x6=30 URL valides pour une ressource. Chaque propriété supplémentaire ajoute à la complexité. Et il peut y avoir des propriétés, qui ne peuvent pas être réduites à une quantité raisonnable de valeurs.

Donc, cette approche a aussi des limites.

#### Exposition accidentelle d’une API

Que se passe-t-il ici ? Si nous regardons attentivement, nous voyons que nous passons progressivement d’un rendu statique à un site web très dynamique. Et nous affichons par inadvertance une API de rendu d’image sur le navigateur du client qui était en fait destiné à être utilisé par les auteurs uniquement.

La définition de la qualité et de la taille d’une image doit être effectuée par l’auteur qui modifie la page. Le fait d’avoir les mêmes fonctionnalités exposées par un servlet peut être considéré comme une fonctionnalité ou comme un vecteur d’une attaque par déni de service. Ce que c&#39;est vraiment, dépend du contexte. Quel est le degré de criticité du site web ? Quelle charge y a-t-il sur les serveurs ? Combien reste-t-il de salle de bain ? Combien de budget avez-vous pour l&#39;implémentation ? Vous devez équilibrer ces facteurs. Vous devriez être conscient des avantages et des inconvénients.

## Le modèle du saboteur - revisité et réhabilité

### Comment le spouleur évite d’exposer l’API

Nous avons en quelque sorte discrédité le modèle du Spooler dans le dernier chapitre. Il est temps de le réhabiliter.

![](assets/chapter-1/spooler-pattern.png)

Le modèle de spouleur empêche le problème d’exposition d’une API dont nous avons parlé dans le dernier chapitre. Les propriétés sont stockées et encapsulées dans le composant. Tout ce dont nous avons besoin pour accéder à ces propriétés est le chemin d’accès au composant. Il n’est pas nécessaire d’utiliser l’URL comme véhicule pour transmettre les paramètres entre le balisage et le rendu binaire :

1. Le client effectue le rendu du balisage HTML lorsque le composant est demandé dans la boucle de demande Principale.

2. Le chemin d’accès au composant sert de référence de référence à partir du balisage vers le composant.

3. Le navigateur utilise cette référence pour demander le fichier binaire.

4. Lorsque la requête atteint le composant, nous avons toutes les propriétés dans la main pour redimensionner, compresser et spool les données binaires.

5. L’image est transmise par l’intermédiaire du composant au navigateur client.

Après tout, le modèle du Spooler n&#39;est pas si mauvais, c&#39;est pourquoi il est si populaire. Si c&#39;est seulement là où il n&#39;est pas si lourd quand il s&#39;agit d&#39;invalidation du cache...

### Le saboteur inversé - le meilleur des deux mondes ?

Cela nous amène à la question. Pourquoi ne pouvons-nous pas simplement avoir le meilleur des deux mondes ? La bonne encapsulation du modèle Spooler et les propriétés de mise en cache intéressantes d’une URL basée sur les ressources ?

Nous devons admettre que nous n&#39;avons pas vu ça dans un vrai projet en direct. Mais osons un peu de réflexion ici de toute façon - comme point de départ pour votre propre solution.

Nous appellerons ce modèle le _spouleur inversé_. Le spouleur inversé doit être basé sur la ressource images pour avoir toutes les propriétés d’invalidation du cache intéressantes.

Mais il ne doit exposer aucun paramètre. Toutes les propriétés doivent être encapsulées dans le composant . Mais nous pouvons exposer le chemin d’accès aux composants, en tant que référence opaque aux propriétés.

Cela conduit à une URL sous la forme :

`/content/dam/flower.respi3.content-mysite-home-jcrcontent-par-respi.jpg`

`/content/dam/flower` est le chemin d’accès à la ressource de l’image.

`.respi3` est un sélecteur permettant de sélectionner le servlet correct pour diffuser l’image.

`.content-mysite-home-jcrcontent-par-respi` est un sélecteur supplémentaire. Il code le chemin d’accès au composant qui stocke la propriété nécessaire à la transformation d’image. Les sélecteurs sont limités à une plus petite plage de caractères que les chemins d’accès. Le schéma de codage ici est exemplaire. Il remplace &quot;/&quot; par &quot;-&quot;. Il n’est pas pris en compte que le chemin d’accès lui-même peut également contenir &quot;-&quot;. Un système de codage plus sophistiqué serait conseillé dans un exemple concret. Base64 devrait être ok. Mais cela rend le débogage un peu plus difficile.

`.jpg` est le suffixe de fichiers

### Conclusion

Waouh.. la discussion du spouleur est devenue plus longue et plus compliquée que prévu. Nous vous devons une excuse. Mais nous avons estimé nécessaire de vous présenter une multitude d&#39;aspects - bons et mauvais - afin que vous puissiez développer une certaine intuition sur ce qui fonctionne bien dans Dispatcher-land et ce qui ne fonctionne pas.

## Statfile et Statfile-Level

### Principes élémentaires

#### Présentation

Nous avons déjà brièvement mentionné _statfile_ auparavant. Il est lié à l’invalidation automatique :

Tous les fichiers de cache du système de fichiers de Dispatcher configurés pour être invalidés automatiquement sont considérés comme non valides si leur date de dernière modification est antérieure à la date de dernière modification `statfile's`.

>[!NOTE]
>
>La date de dernière modification dont nous parlons est que le fichier mis en cache est la date à laquelle le fichier a été demandé par le navigateur du client et finalement créé dans le système de fichiers. Il ne s’agit pas de la date `jcr:lastModified` de la ressource.

La date de dernière modification du fichier stat (`.stat`) est la date de réception de la demande d’invalidation d’AEM sur Dispatcher.

Si vous disposez de plusieurs instances de Dispatcher, cela peut entraîner des effets étranges. Votre navigateur peut avoir une version plus récente d’un dispatcher (si vous en avez plusieurs). Ou un Dispatcher peut penser que la version du navigateur émise par l’autre Dispatcher est obsolète et envoie inutilement une nouvelle copie. Ces effets n’ont pas d’impact significatif sur les performances ou les exigences fonctionnelles. Et ils se stabiliseront au fil du temps, lorsque le navigateur aura la dernière version. Cependant, cela peut être un peu déroutant lorsque vous optimisez et déboguez le comportement de mise en cache du navigateur. Soyez donc averti.

#### Configuration de domaines d’invalidation avec /statfileslevel

Lorsque nous avons introduit l’invalidation automatique et le fichier stat que nous avons dit, que tous les *fichiers* sont considérés comme non valides en cas de modification et que tous les fichiers sont de toute façon interdépendants.

Ce n&#39;est pas tout à fait exact. En règle générale, tous les fichiers qui partagent une racine de navigation principale commune sont interdépendants. Mais une instance d’AEM peut héberger un certain nombre de sites web *indépendants*. Ne pas partager une navigation commune - en fait, ne rien partager.

Ne serait-ce pas un gaspillage d’invalider le site B parce qu’il y a un changement dans le site A ? Oui, c&#39;est vrai. Et ça n&#39;a pas à être comme ça.

Dispatcher fournit un moyen simple de séparer les sites les uns des autres : `statfiles-level`.

Il s’agit d’un nombre qui définit à partir de quel niveau dans le système de fichiers, deux sous-arborescences sont considérées comme &quot;indépendantes&quot;.

Examinons le cas par défaut où le niveau statfileslevel est 0.

![/statfileslevel &quot;0&quot; : Le_  _.stat_ _est créé dans le docroot. Le domaine d’invalidation couvre toute l’installation, y compris tous les sites](assets/chapter-1/statfile-level-0.png)

`/statfileslevel "0":` Le  `.stat` fichier est créé dans le docroot. Le domaine d’invalidation couvre toute l’installation, y compris tous les sites.

Quel que soit le fichier invalidé, le fichier `.stat` situé tout en haut du docroot des dispatchers est toujours mis à jour. Ainsi, lorsque vous invalidez `/content/site-b/home`, tous les fichiers de `/content/site-a` sont également invalidés, car ils sont désormais plus anciens que le fichier `.stat` dans le docroot. Clairement pas ce dont vous avez besoin, lorsque vous invalidez `site-b`.

Dans cet exemple, vous préférez définir `statfileslevel` sur `1`.

Maintenant, si vous publiez - et donc invalidez `/content/site-b/home` ou toute autre ressource sous `/content/site-b`, le fichier `.stat` est créé à l’adresse `/content/site-b/`.

Le contenu ci-dessous `/content/site-a/` n’est pas affecté. Ce contenu serait comparé à un fichier `.stat` à `/content/site-a/`. Nous avons créé deux domaines d’invalidation distincts.

![Un &quot;statfileslevel&quot; &quot;1&quot; crée différents domaines d’invalidation.](assets/chapter-1/statfiles-level-1.png)

*Un &quot;statfileslevel&quot; &quot;1&quot; crée différents domaines d’invalidation.*

<br> 

Les grandes installations sont généralement structurées un peu plus complexes et plus profondes. Un schéma commun consiste à structurer les sites par marque, pays et langue. Dans ce cas, vous pouvez définir le niveau de fichier statfileslevel encore plus haut. _1_ créerait des domaines d’invalidation par marque,  _2_  par pays et  _3_  par langue.

### Nécessité d’une structure de site homogène

Le niveau statfileslevel est appliqué de manière égale à tous les sites de votre configuration. Par conséquent, tous les sites doivent suivre la même structure et commencer au même niveau.

Supposons que votre portefeuille contienne des marques qui ne sont vendues que sur quelques petits marchés alors que d’autres sont vendues dans le monde entier. Il se trouve que les petits marchés n&#39;ont qu&#39;une seule langue locale alors qu&#39;il existe sur le marché mondial des pays où plusieurs langues sont parlées :

```plain
  /content/tiny-local-brand/finland/home
  /content/tiny-local-brand/finland/products
  /content/tiny-local-brand/finland/about
                              ^
                          /statfileslevel "2"
  …

  /content/tiny-local-brand/norway
  …

  /content/shiny-global-brand/canada/en
  /content/shiny-global-brand/canada/fr
  /content/shiny-global-brand/switzerland/fr
  /content/shiny-global-brand/switzerland/de
  /content/shiny-global-brand/switzerland/it
                                          ^
                                /statfileslevel "3"
  ..
```

La première nécessiterait un `statfileslevel` de _2_, la seconde un _3_.

Ce n&#39;est pas une situation idéale. Si vous la définissez sur _3_, l’invalidation automatique ne fonctionnerait pas dans les sites plus petits entre les sous-branches `/home`, `/products` et `/about`.

Si vous définissez ce paramètre sur _2_ , cela signifie que, dans les sites plus grands, vous déclarez `/canada/en` et `/canada/fr` dépendants, ce qui n’est peut-être pas le cas. Ainsi, chaque invalidation dans `/en` invaliderait également `/fr`. Le taux d’accès au cache sera ainsi légèrement réduit, mais il est toujours préférable de fournir du contenu mis en cache obsolète.

La meilleure solution, bien sûr, est de rendre les racines de tous les sites aussi profondes :

```
/content/tiny-local-brand/finland/fi/home
/content/tiny-local-brand/finland/fi/products
/content/tiny-local-brand/finland/fi/about
…
/content/tiny-local-brand/norway/no/home
                                 ^
                        /statfileslevel "3"
```

### Liens intersites

Quel est le bon niveau ? Cela dépend du nombre de dépendances que vous avez entre les sites. Les inclusions que vous résolvez pour le rendu d’une page sont considérées comme des &quot;dépendances hard&quot;. Nous avons démontré une telle _inclusion_ lorsque nous avons introduit le composant _Teaser_ au début de ce guide.

__ Les hyperliens sont une forme plus souple de dépendances. Il est très probable que vous créiez des liens hypertextes au sein d’un site web.. et il n’est pas improbable que vous ayez des liens entre vos sites web. Les hyperliens simples ne créent généralement pas de dépendances entre les sites web. Pensez simplement à un lien externe que vous définissez de votre site vers facebook... Vous n’aurez pas à effectuer le rendu de votre page si quelque chose change sur facebook et vice versa, n’est-ce pas ?

Une dépendance se produit lorsque vous lisez le contenu de la ressource liée (par exemple, le titre de navigation). Ces dépendances peuvent être évitées si vous vous fiez uniquement aux titres de navigation saisis localement et ne les tirez pas de la page cible (comme vous le feriez avec les liens externes).

#### Une dépendance inattendue

Cependant, il peut y avoir une partie de votre configuration, où - soi-disant indépendant - les sites se rassemblent. Regardons un scénario du monde réel que nous avons rencontré dans l&#39;un de nos projets.

Le client avait une structure de site comme celle esquissée dans le dernier chapitre :

```
/content/brand/country/language
```

Par exemple,

```
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de

/content/shiny-brand/france/fr

/content/shiny-brand/germany/de
```

Chaque pays avait son propre domaine,

```
www.shiny-brand.ch

www.shiny-brand.fr

www.shiny-brand.de
```

Il n’existait aucun lien navigable entre les sites de langue et aucune inclusion apparente. Nous avons donc défini le niveau statfileslevel sur 3.

Tous les sites diffusaient essentiellement le même contenu. La seule différence majeure a été la langue.

Les moteurs de recherche comme Google considèrent que le même contenu sur différentes URL est &quot;trompeur&quot;. Un utilisateur peut souhaiter obtenir un classement plus élevé ou être répertorié plus souvent en créant des fermes de serveurs proposant un contenu identique. Les moteurs de recherche reconnaissent ces tentatives et classent en fait les pages plus basses qui recyclent simplement le contenu.

Vous pouvez éviter d’être classé de manière moins importante en rendant transparent le fait que vous disposez en fait de plusieurs pages avec le même contenu et que vous n’essayez pas de &quot;lire&quot; le système (voir [&quot;Parlez à Google des versions localisées de votre page&quot;](https://support.google.com/webmasters/answer/189077?hl=en)) en définissant des balises `<link rel="alternate">` sur chaque page associée dans la section d’en-tête de chaque page :

```
# URL: www.shiny-brand.fr/fr/home/produits.html

<head>

  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch" 
        href="http://www.shiny-brand.ch/de/home/produkte.html">
  <link rel="alternate" 
        hreflang="de-de" 
        href="http://www.shiny-brand.de/de/home/produkte.html">

</head>

----

# URL www.shiny-brand.de/de/home/produkte.html

<head>

  <link rel="alternate" 
        hreflang="fr-fr" 
        href="http://www.shiny-brand.fr/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch"
         href="http://www.shiny-brand.ch/de/home/produits.html">

</head>
```

![Tous les interliens](assets/chapter-1/inter-linking-all.png)

*Tous les interliens*

<br> 

Certains experts de l&#39;optimisation pour les moteurs de recherche affirment même que cela pourrait transférer la réputation ou le &quot;link-jus&quot; d&#39;un site web de haut rang dans une langue vers le même site web dans une autre langue.

Ce schéma a créé non seulement un certain nombre de liens, mais aussi quelques problèmes. Le nombre de liens requis pour _p_ dans les langues _n_ est _p x (n<sup>2</sup>-n)_ : Chaque page est liée à une autre page (_n x n_) sauf à elle-même (_-n_). Ce modèle s’applique à chaque page. Si nous disposons d’un petit site en 4 langues comportant 20 pages, chacune correspond à des liens _240_.

Tout d’abord, vous ne souhaitez pas qu’un éditeur doive gérer ces liens manuellement : ils doivent être générés automatiquement par le système.

Deuxièmement, ils devraient être précis. Chaque fois que le système détecte un nouveau &quot;parent&quot;, vous souhaitez le lier à partir de toutes les autres pages avec le même contenu (mais dans une langue différente).

Dans notre projet, de nouvelles pages relatives s’affichaient fréquemment. Mais ils ne se sont pas matérialisés comme des liens &quot;alternatifs&quot;. Par exemple, lorsque la page `de-de/produkte` a été publiée sur le site allemand, elle n’était pas immédiatement visible sur les autres sites.

La raison était que dans notre configuration les sites étaient censés être indépendants. Un changement sur le site allemand n&#39;a donc pas déclenché d&#39;invalidation sur le site français.

Vous connaissez déjà une solution pour résoudre ce problème. Il vous suffit de réduire le niveau des états à 2 pour élargir le domaine d’invalidation. Bien sûr, cela réduit également le taux d’accès au cache - en particulier lorsque des publications - et donc des invalidations se produisent plus fréquemment.

Dans notre cas, c&#39;était encore plus compliqué :

Même si nous avions le même contenu, les noms de marque et non les noms de marque étaient différents dans chaque pays.

`shiny-brand` a été appelé  `marque-brillant` en France et  `blitzmarke` en Allemagne :

```
/content/marque-brillant/france/fr
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de
/content/blitzmarke/germany/de
…
```

Cela aurait signifié de définir le niveau `statfiles` sur 1, ce qui aurait entraîné un domaine d’invalidation trop important.

La restructuration du site aurait résolu ce problème. Fusion de toutes les marques sous une racine commune. Mais nous n&#39;avions pas la capacité à l&#39;époque, et - cela nous aurait donné seulement un niveau 2.

Nous avons décidé de rester au niveau 3 et de payer le prix de ne pas toujours avoir des liens &quot;alternatifs&quot; à jour. Pour atténuer ce problème, nous avions une tâche cron &quot;reaper&quot; qui s’exécutait sur Dispatcher et qui nettoyait de toute façon les fichiers de plus d’une semaine. Finalement, toutes les pages ont été rendues à un moment donné. Mais c&#39;est un compromis qui doit être décidé individuellement dans chaque projet.

## Conclusion

Nous avons traité quelques principes de base sur le fonctionnement général de Dispatcher et nous vous avons donné quelques exemples dans lesquels vous devrez peut-être consacrer un peu plus d’efforts à l’implémentation pour réussir et où vous souhaiterez peut-être faire des compromis.

Nous n’avons pas entré de détails sur la configuration dans Dispatcher. Nous voulions que vous compreniez d&#39;abord les concepts de base et les problèmes, sans vous perdre trop tôt dans la console. Et - le travail de configuration réel est bien documenté - si vous comprenez les concepts de base, vous devez savoir à quoi servent les différents commutateurs.

## Conseils et astuces pour Dispatcher

Nous conclurons la première partie de ce livre par une série d&#39;astuces et d&#39;astuces qui pourraient s&#39;avérer utiles dans une situation ou une autre. Comme nous l&#39;avons fait auparavant, nous ne présentons pas la solution, mais l&#39;idée, afin que vous ayez la possibilité de comprendre l&#39;idée, le concept et de créer un lien vers des articles décrivant la configuration réelle plus en détail.

### Minutage d’invalidation correct

Si vous installez un auteur AEM et une publication prêts à l’emploi, la topologie est un peu bizarre. L’auteur envoie simultanément le contenu aux systèmes de publication et la demande d’invalidation aux dispatchers. Comme les systèmes de publication et Dispatcher sont découplés de l’auteur par des files d’attente, le timing peut être un peu regrettable. Dispatcher peut recevoir la demande d’invalidation de l’auteur avant que le contenu ne soit mis à jour sur le système de publication.

Si un client demande ce contenu entre-temps, Dispatcher demande et stocke le contenu obsolète.

Une configuration plus fiable consiste à envoyer la demande d’invalidation des systèmes de publication _après avoir reçu le contenu._ L’article &quot;[Invalidation du cache de Dispatcher à partir d’une instance de publication](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)&quot; décrit les détails.

**Références**

[helpx.adobe.com - Invalidation du cache de Dispatcher à partir d’une instance de publication](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)

### Mise en cache des en-têtes et en-têtes HTTP

Autrefois, Dispatcher stockait uniquement des fichiers simples dans le système de fichiers. Si vous aviez besoin que des en-têtes HTTP soient diffusés au client, vous l’avez fait en configurant Apache en fonction des petites informations que vous disposiez du fichier ou de l’emplacement. C’était particulièrement embêtant lorsque vous avez mis en oeuvre une application web dans AEM qui reposait principalement sur des en-têtes HTTP. Tout fonctionnait correctement dans l’instance AEM uniquement, mais pas lorsque vous utilisiez un Dispatcher.

En règle générale, vous avez réappliqué les en-têtes manquants aux ressources du serveur Apache avec `mod_headers` en utilisant les informations que vous pouviez dériver par le chemin d’accès aux ressources et le suffixe. Mais cela n&#39;a pas toujours été suffisant.

Le fait que, même avec Dispatcher, la première réponse _non mise en cache_ au navigateur provient du système de publication avec une plage complète d’en-têtes, tandis que les réponses suivantes ont été générées par Dispatcher avec un ensemble limité d’en-têtes.

À partir de la version 4.1.11 de Dispatcher, celui-ci peut stocker les en-têtes générés par les systèmes de publication.

Cela vous évite de dupliquer la logique d’en-tête dans Dispatcher et libère la puissance expressive totale du HTTP et de l’AEM.

**Références**

* [helpx.adobe.com - Mise en cache des en-têtes de réponse](https://helpx.adobe.com/experience-manager/kb/dispatcher-cache-response-headers.html)

### Exceptions de mise en cache individuelles

Vous pouvez mettre en cache toutes les pages et images en général, mais vous pouvez faire une exception dans certains cas. Par exemple, vous souhaitez mettre en cache les images PNG, mais pas les images PNG affichant un captcha (qui doit changer à chaque demande). Le Dispatcher peut ne pas reconnaître un captcha comme un captcha... mais AEM le fait certainement. Il peut demander à Dispatcher de ne pas mettre en cache cette requête en envoyant un en-tête conforme avec la réponse :

```plain
  response.setHeader("Dispatcher", "no-cache");

  response.setHeader("Cache-Control: no-cache");

  response.setHeader("Cache-Control: private");

  response.setHeader("Pragma: no-cache");
```

Cache-Control et Pragma sont des en-têtes HTTP officiels, propagés et interprétés par les couches de mise en cache supérieures, telles qu’un réseau de diffusion de contenu. L’en-tête `Dispatcher` n’est qu’un indice que Dispatcher ne doit pas mettre en cache. Il peut être utilisé pour indiquer à Dispatcher de ne pas mettre en cache, tout en permettant aux calques de mise en cache supérieure de le faire. En fait, il est difficile de trouver un cas où cela pourrait être utile. Mais nous sommes sûrs qu&#39;il y en a, quelque part.

**Références**

* [Dispatcher - Pas de cache](https://helpx.adobe.com/experience-manager/kb/DispatcherNoCache.html)

### Mise en cache du navigateur

La réponse http la plus rapide est la réponse donnée par le navigateur lui-même. Lorsque la demande et la réponse n’ont pas à passer par un réseau vers un serveur web soumis à une charge élevée.

Vous pouvez aider le navigateur à décider quand demander au serveur une nouvelle version du fichier en définissant une date d’expiration sur une ressource.

En règle générale, vous procédez de manière statique en utilisant la balise `mod_expires` d’Apache ou en stockant les en-têtes Cache-Control et Expires qui proviennent d’AEM si vous avez besoin d’un contrôle plus individuel.

Un document mis en cache dans le navigateur peut comporter trois niveaux de mise à jour.

1. _Mise à jour garantie_  : le navigateur peut utiliser le document mis en cache.

2. _Potentiellement obsolète_  : le navigateur doit d’abord demander au serveur si le document mis en cache est toujours à jour,

3. _Stale_  : le navigateur doit demander une nouvelle version au serveur.

La première est garantie par la date d’expiration définie par le serveur. Si une ressource n’a pas expiré, il n’est pas nécessaire de redemander au serveur.

Si le document a atteint sa date d’expiration, il peut toujours être actualisé. La date d’expiration est définie lors de la diffusion du document. Mais souvent, vous ne savez pas à l&#39;avance quand de nouveaux contenus sont disponibles - c&#39;est donc juste une estimation prudente.

Pour déterminer si le document dans le cache du navigateur est toujours identique à celui qui sera diffusé lors d’une nouvelle requête, le navigateur peut utiliser la date `Last-Modified` du document. Le navigateur demande au serveur :

&quot;_J’ai une version à partir du 10 juin.. ai-je besoin d’une mise à jour ?_&quot; Et le serveur peut répondre par

&quot;_304 - Votre version est toujours à jour_&quot; sans retransmettre la ressource, ou le serveur peut répondre par

&quot;_200 - voici une version plus récente_&quot; dans l’en-tête HTTP et le contenu plus récent réel dans le corps HTTP.

Pour que cette deuxième partie fonctionne, veillez à transmettre la date `Last-Modified` au navigateur afin qu’il dispose d’un point de référence pour demander des mises à jour.

Nous avons expliqué précédemment que lorsque la date `Last-Modified` est générée par Dispatcher, elle peut varier en fonction des différentes requêtes, car le fichier mis en cache - et sa date - sont générés lorsque le fichier est demandé par le navigateur. Une alternative serait d’utiliser des &quot;e-tags&quot; : il s’agit de nombres qui identifient le contenu réel (par exemple en générant un code de hachage) au lieu d’une date.

&quot;[Prise en charge des balises ](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)&quot; du _package ACS Commons_ utilise cette approche. Mais cela a un prix : Comme la balise électronique doit être envoyée sous forme d’en-tête, mais que le calcul du code de hachage nécessite une lecture complète de la réponse, celle-ci doit être entièrement mise en mémoire tampon dans la mémoire principale avant de pouvoir être diffusée. Cela peut avoir un impact négatif sur la latence lorsque votre site web est plus susceptible d’avoir des ressources non mises en cache et, bien sûr, vous devez garder un oeil sur la mémoire consommée par votre système AEM.

Si vous utilisez des empreintes digitales d’URL, vous pouvez définir des dates d’expiration très longues. Vous pouvez mettre en cache les ressources empreinte à jamais dans le navigateur. Une nouvelle version est marquée par une nouvelle URL et les anciennes versions n’ont jamais besoin d’être mises à jour.

Nous avons utilisé les empreintes digitales des URL lorsque nous avons introduit le modèle de spouleur. Les fichiers statiques provenant de `/etc/design` (CSS, JS) sont rarement modifiés, ce qui les rend également bons candidats à l’utilisation comme empreintes digitales.

Pour les fichiers standard, nous configurons généralement un schéma fixe, comme vérifier à nouveau le code HTML toutes les 30 minutes, les images toutes les 4 heures, etc.

La mise en cache du navigateur est extrêmement utile sur le système de création. Vous souhaitez mettre en cache autant que possible dans le navigateur afin d’améliorer l’expérience de modification. Malheureusement, les ressources les plus coûteuses, les pages HTML ne peuvent pas être mises en cache... elles sont censées changer fréquemment sur l’auteur.

Les bibliothèques Granite, qui constituent AEM interface utilisateur, peuvent être mises en cache pendant un certain temps. Vous pouvez également mettre en cache les fichiers statiques de vos sites (polices, CSS et JavaScript) dans le navigateur. Même les images de `/content/dam` peuvent généralement être mises en cache pendant environ 15 minutes, car elles ne sont pas modifiées aussi souvent que du texte de copie sur les pages. Les images ne sont pas modifiées de manière interactive dans AEM. Elles sont d’abord modifiées et approuvées, avant d’être téléchargées vers AEM. Par conséquent, vous pouvez supposer qu’ils ne changent pas aussi souvent que du texte.

La mise en cache des fichiers d’interface utilisateur, des fichiers de bibliothèque de sites et des images peut accélérer considérablement le rechargement des pages lorsque vous êtes en mode d’édition.



**Références**

*[developer.mozilla.org - Mise en cache](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

* [apache.org - Mod Expires](https://httpd.apache.org/docs/current/mod/mod_expires.html)

* [ACS Commons - Prise en charge des balises Etag](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)

### Tronquer les URL

Vos ressources sont stockées sous

`/content/brand/country/language/…`

Mais bien sûr, il ne s’agit pas de l’URL que vous souhaitez transmettre au client. Pour des raisons esthétiques, de lisibilité et d’optimisation du référencement, vous pouvez tronquer la partie déjà représentée dans le nom de domaine.

Si vous disposez d’un domaine

`www.shiny-brand.fi`

il n’est généralement pas nécessaire de mettre la marque et le pays sur le chemin. Au lieu de :

`www.shiny-brand.fi/content/shiny-brand/finland/fi/home.html`

vous voudriez avoir,

`www.shiny-brand.fi/home.html`

Vous devez mettre en oeuvre ce mappage sur AEM, car AEM doit savoir comment effectuer le rendu des liens selon ce format tronqué.

Mais ne comptez pas uniquement sur l&#39;AEM. Si vous le faites, vous disposez de chemins d’accès tels que `/home.html` dans le répertoire racine de votre cache. Maintenant, est-ce là le &quot; foyer &quot; pour les Finlandais, les Allemands ou le site Web canadien? Et s’il existe un fichier `/home.html` dans Dispatcher, comment Dispatcher sait-il que celui-ci doit être invalidé lorsqu’une demande d’invalidation pour `/content/brand/fi/fi/home` entre en jeu.

Nous avons vu un projet qui avait des docroots séparés pour chaque domaine. C&#39;était un cauchemar à déboguer et à entretenir - et en fait, nous ne l&#39;avons jamais vu fonctionner sans erreur.

Nous pourrions résoudre les problèmes en réstructurant le cache. Nous avions un docroot unique pour tous les domaines, et les demandes d’invalidation pouvaient être traitées 1:1, car tous les fichiers du serveur commençaient par `/content`.

La partie tronquée était aussi très facile.  AEM ont généré des liens tronqués en raison d’une configuration conforme dans `/etc/map`.

Désormais, lorsqu’une requête `/home.html` atteint Dispatcher, la première chose qui se produit est d’appliquer une règle de réécriture qui développe le chemin en interne.

Cette règle a été configurée de manière statique dans chaque configuration vhost. En d&#39;autres termes, les règles ressemblaient à ça :

```plain
  # vhost www.shiny-brand.fi

  RewriteRule "^(.\*\.html)" "/content/shiny-brand/finland/fi/$1"
```

Dans le système de fichiers, nous disposons désormais de chemins simples basés sur `/content`, qui se trouvent également sur les instances de création et de publication - ce qui a beaucoup aidé au débogage. Sans parler de l&#39;invalidation correcte - ce n&#39;était plus un problème.

Notez que nous l’avons fait uniquement pour les URL &quot;visibles&quot;, qui s’affichent dans l’emplacement d’URL du navigateur. Les URL des images, par exemple, étaient toujours des URL &quot;/content&quot; pures. Nous pensons que l’embellissement de l’URL &quot;principale&quot; est suffisant en termes d’optimisation du moteur de recherche.

Avoir un docroot commun avait aussi une autre caractéristique. En cas de problème dans Dispatcher, nous pouvions nettoyer l’ensemble du cache en l’exécutant,

`rm -rf /cache/dispatcher/*`

(ce que vous pouvez ne pas faire à des pics de charge élevés).

**Références**

* [apache.org - Mod Rewrite](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)

* [helpx.adobe.com - Mappage des ressources](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/resource-mapping.html)

### Gestion des erreurs

Dans AEM classes, vous apprenez à programmer un gestionnaire d’erreurs dans Sling. Ce n&#39;est pas si différent de l&#39;écriture d&#39;un modèle habituel. Vous écrivez simplement un modèle dans JSP ou HTL, n&#39;est-ce pas ?

Oui, mais c&#39;est la partie AEM, seulement. Rappel : Dispatcher ne met pas en cache les réponses `404 – not found` ou `500 – internal server error`.

Si vous effectuez dynamiquement le rendu de ces pages pour chaque requête (en échec), la charge sur les systèmes de publication sera importante et inutile.

Ce que nous avons trouvé utile, c’est de ne pas rendre la page d’erreur complète lorsqu’une erreur se produit, mais seulement une version très simplifiée et petite - voire statique de cette page, sans aucun ornement ni logique.

Ce n&#39;est bien sûr pas ce que le client a vu. Dans Dispatcher, nous avons enregistré `ErrorDocuments` comme suit :

```
ErrorDocument 404 "/content/shiny-brand/fi/fi/edocs/error-404.html"
ErrorDocument 500 "/content/shiny-brand/fi/fi/edocs/error-500.html"
```

Le système d’AEM pouvait simplement avertir Dispatcher qu’un problème se produisait, et Dispatcher pouvait fournir une belle et brillante version du document d’erreur.

Il faut noter deux choses ici.

Tout d’abord, `error-404.html` est toujours la même page. Il n’existe donc aucun message individuel du type &quot;Votre recherche de &quot;_produckten_&quot; n’a pas généré de résultat&quot;. Nous pourrions facilement vivre avec ça.

Deuxièmement.. Eh bien, si nous voyons une erreur de serveur interne - ou pire, nous rencontrons une panne du système AEM, il n&#39;y a aucun moyen de demander à AEM de rendre une page d&#39;erreur, n&#39;est-ce pas ? La requête ultérieure nécessaire, telle que définie dans la directive `ErrorDocument`, échouerait également. Nous avons contourné ce problème en exécutant une tâche cron qui extrait régulièrement les pages d’erreur de leurs emplacements définis via `wget` et les stocke dans des emplacements de fichiers statiques définis dans la directive `ErrorDocuments`.

**Références**

* [apache.org - Documents d’erreur personnalisés](https://httpd.apache.org/docs/2.4/custom-error.html)

### Mise en cache de contenu protégé

Dispatcher ne vérifie pas les autorisations lorsqu’il diffuse une ressource par défaut. Il est mis en oeuvre de la sorte exprès - pour accélérer votre site web public. Si vous souhaitez protéger certaines ressources par une connexion, vous disposez essentiellement de trois options :

1. Protect de la ressource avant que la requête n’atteigne le cache, c’est-à-dire par une passerelle SSO (authentification unique) devant Dispatcher, ou en tant que module dans le serveur Apache.

2. Excluez les ressources sensibles de la mise en cache et, par conséquent, diffusez-les toujours en direct du système de publication.

3. Utilisation de la mise en cache sensible aux autorisations dans Dispatcher

Et bien sûr, vous pouvez appliquer votre propre combinaison des trois approches.

**Option 1**. Une passerelle &quot;SSO&quot; peut être appliquée par votre organisation de toute façon. Si votre schéma d’accès est très grossier, vous n’aurez peut-être pas besoin d’informations d’AEM pour décider d’accorder ou de refuser l’accès à une ressource.

>[!NOTE]
>
>Ce modèle nécessite une _passerelle_ que _intercepte_ chaque demande et exécute l’_autorisation_ réelle, qui permet d’accorder ou de refuser des demandes au Dispatcher. Si votre système d’authentification unique est un _authentificateur_, qui établit uniquement l’identité d’un utilisateur dont vous devez mettre en oeuvre l’option 3. Si vous lisez des termes tels que &quot;SAML&quot; ou &quot;OAauth&quot; dans le guide de votre système d’authentification unique, c’est un indicateur puissant que vous devez implémenter l’option 3.


**Option 2**. &quot;Ne pas mettre en cache&quot; est généralement une mauvaise idée. Si vous optez pour cette méthode, assurez-vous que le volume de trafic et le nombre de ressources sensibles qui sont exclues sont faibles. Vous pouvez également vous assurer que le système de publication dispose d’un cache en mémoire, afin que les systèmes de publication puissent gérer la charge qui en résulte - en savoir plus sur la partie III de cette série.

**Option 3**. La &quot;mise en cache sensible aux autorisations&quot; est une approche intéressante. Dispatcher met en cache une ressource, mais avant de la diffuser, il demande au système AEM s’il le fait. Cela crée une requête supplémentaire de Dispatcher vers l’instance de publication, mais évite généralement au système de publication de rendre à nouveau une page si elle est déjà mise en cache. Cependant, cette approche nécessite une implémentation personnalisée. Pour plus d’informations, reportez-vous à l’article [Mise en cache sensible aux autorisations](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html).

**Références**

* [helpx.adobe.com - Mise en cache sensible aux autorisations](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html)

### Définition de la période de grâce

Si vous effectuez fréquemment une invalidation en courte succession (par exemple, par une activation d’arborescence ou simplement par nécessité de garder votre contenu à jour, il se peut que vous videz constamment le cache et que vos visiteurs touchent presque toujours un cache vide.

Le diagramme ci-dessous illustre un timing possible lors de l’accès à une seule page.  Le problème s&#39;aggrave bien sûr quand le nombre de pages demandées augmente.

![Activations fréquentes générant un cache non valide la plupart du temps](assets/chapter-1/frequent-activations.png)

*Activations fréquentes générant un cache non valide la plupart du temps*

<br> 

Pour atténuer le problème de cette &quot;tempête d&#39;invalidation du cache&quot; comme on l&#39;appelle parfois, vous pouvez être moins rigoureux sur l&#39;interprétation `statfile`.

Vous pouvez définir Dispatcher de sorte qu’il utilise une balise `grace period` pour l’invalidation automatique. Cela ajouterait du temps supplémentaire en interne à la date de modification `statfiles`.

Supposons que votre `statfile` ait une heure de modification de 12h00 et que la valeur `gracePeriod` soit définie sur 2 minutes. Ensuite, tous les fichiers invalidés automatiquement seront considérés comme valides à 12h01 et à 12h02. Ils seront rendus à nouveau après 12h02.

La configuration de référence propose une valeur `gracePeriod` de deux minutes pour une bonne raison. Vous pourriez penser &quot;Deux minutes ? Ce n&#39;est presque rien. Je peux facilement attendre 10 minutes que le contenu s&#39;affiche...&quot;.  Vous pourriez donc être tenté de définir une période plus longue, disons 10 minutes, en supposant que votre contenu s’affiche au moins après ces 10 minutes.

>[!WARNING]
>
>Ce n’est pas ainsi que `gracePeriod` fonctionne. La période de grâce est _non_ le moment après lequel il est garanti qu’un document sera invalidé, mais une période sans invalidation se produit. Chaque invalidation ultérieure qui tombe dans ce cadre _prolonge_ la période, qui peut être indéfiniment longue.

Illustrons comment `gracePeriod` fonctionne réellement avec un exemple :

Imaginons que vous gérez un site multimédia et que votre personnel d’édition mette régulièrement à jour le contenu toutes les 5 minutes. Pensez à définir la période de grâce sur 5 minutes.

Commençons par un exemple rapide à 12h00.

12:00 - Statfile est défini sur 12:00. Tous les fichiers mis en cache sont considérés comme valides jusqu’à 12h05.

12:01 - Une invalidation se produit. Ceci prolonge le temps de grillage à 12h06.

12:05 - Un autre éditeur publie son article - prolongeant le temps de grâce d’une autre période de grâce à 12:10.

Et ainsi de suite.. le contenu n&#39;est jamais invalidé. Chaque invalidation *dans* la période de grâce prolonge efficacement le délai de grâce. Le `gracePeriod` est conçu pour résister à la tempête d&#39;invalidation.. mais vous devez sortir par la pluie... donc, garder le `gracePeriod` considérablement court pour empêcher à jamais de vous cacher dans l&#39;abri.

#### Période de grâce déterministe

Nous aimerions vous présenter une autre idée de la façon dont vous pourriez traverser une tempête d&#39;invalidation. Ce n&#39;est qu&#39;une idée. Nous ne l&#39;avons pas essayé en production, mais nous avons trouvé le concept assez intéressant pour partager l&#39;idée avec vous.

`gracePeriod` peut devenir incroyablement long si votre intervalle de réplication normal est plus court que votre `gracePeriod`.

L&#39;autre idée est la suivante : Invalider uniquement dans des intervalles de temps fixes. Le temps écoulé entre les deux signifie toujours diffuser du contenu obsolète. L’invalidation se produira ultérieurement, mais un certain nombre d’invalidations sont collectées pour une invalidation &quot;en bloc&quot;, de sorte que Dispatcher ait la possibilité de diffuser du contenu mis en cache entre-temps et de donner un peu d’air au système de publication.

L’implémentation se présenterait comme suit :

Vous utilisez un &quot;Script d’invalidation personnalisé&quot; (voir référence) qui s’exécute après l’invalidation. Ce script lirait la date de la dernière modification `statfile's` et l’arrondirait à l’intervalle d’arrêt suivant. La commande shell Unix `touch --time`, indiquez une heure.

Par exemple, si vous définissez la période de grâce sur 30 secondes, Dispatcher arrondit la date de dernière modification du fichier stat aux 30 secondes suivantes. Les demandes d’invalidation qui se produisent entre définissent simplement la même période complète de 30 secondes.

![Le fait de reporter l’invalidation à la suivante complète pendant 30 secondes augmente le taux d’accès.](assets/chapter-1/postponing-the-invalidation.png)

*Le fait de reporter l’invalidation à la suivante complète pendant 30 secondes augmente le taux d’accès.*

<br> 

Les accès au cache qui se produisent entre la demande d’invalidation et l’emplacement de 30 secondes suivant sont alors considérés comme obsolètes ; Une mise à jour a été apportée à la publication, mais Dispatcher diffuse toujours de l’ancien contenu.

Cette approche pourrait aider à définir des périodes de grâce plus longues sans avoir à craindre que les demandes suivantes prolongent la période de manière indéterministe. Bien que, comme nous l&#39;avons déjà dit, c&#39;est juste une idée et nous n&#39;avons pas eu la chance de la tester.

**Références**

[helpx.adobe.com - Configuration du Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

### Récupération automatique

Votre site a un modèle d’accès très particulier. Vous avez une charge importante de trafic entrant et la majeure partie du trafic est concentrée sur une petite fraction de vos pages. La page d’accueil, les landing pages de votre campagne et les pages de détails des produits les plus présentés reçoivent 90 % du trafic. Ou, si vous gérez un nouveau site, les articles les plus récents génèrent un trafic plus élevé que les plus anciens.

Désormais, ces pages sont très probablement mises en cache dans Dispatcher, car elles sont demandées si fréquemment.

Une demande d’invalidation arbitraire est envoyée à Dispatcher, ce qui entraîne l’invalidation de toutes les pages (y compris la plus populaire une fois).

Par la suite, comme ces pages sont si populaires, de nouvelles requêtes entrantes provenant de différents navigateurs sont disponibles. Prenons la page d’accueil comme exemple.

Comme le cache n’est plus valide, toutes les requêtes sur la page d’accueil qui arrivent en même temps sont transférées au système de publication qui génère une charge élevée.

![Requêtes parallèles vers la même ressource sur le cache vide : Les requêtes sont transférées à la publication.](assets/chapter-1/parallel-requests.png)

*Requêtes parallèles vers la même ressource sur le cache vide : Les requêtes sont transférées à la publication.*

Grâce à la récupération automatique, vous pouvez atténuer cela dans une certaine mesure. La plupart des pages invalidées sont toujours physiquement stockées sur Dispatcher après l’invalidation automatique. Ils ne sont que _considérés_ obsolètes. _La_ récupération automatique signifie que vous diffusez toujours ces pages obsolètes pendant quelques secondes tout en initiant  _une demande_ unique au système de publication pour récupérer à nouveau le contenu obsolète :

![Diffuser du contenu obsolète lors d’une récupération en arrière-plan](assets/chapter-1/fetching-background.png)

*Diffuser du contenu obsolète lors d’une récupération en arrière-plan*

<br> 

Pour activer la récupération, vous devez indiquer à Dispatcher les ressources à récupérer à la suite d’une invalidation automatique. N’oubliez pas que toute page que vous activez invalide également automatiquement toutes les autres pages, y compris les plus consultées.

La récupération signifie en fait que Dispatcher doit être informé dans chaque (!) demande d’invalidation pour récupérer à nouveau les plus populaires et les plus populaires.

Pour ce faire, placez une liste d’URL de ressources (URL réelles, et pas seulement les chemins) dans le corps des requêtes d’invalidation :

```
POST /dispatcher/invalidate.cache HTTP/1.1

CQ-Action: Activate
CQ-Handle: /content/my-brand/home/path/to/some/resource
Content-Type: Text/Plain
Content-Length: 207

/content/my-brand/home.html
/content/my-brand/campaigns/landing-page-1.html
/content/my-brand/campaigns/landing-page-2.html
/content/my-brand/products/product-1.html
/content/my-brand/products/product-2.html
```

Lorsque Dispatcher voit une telle requête, elle déclenche l’invalidation automatique comme d’habitude et met immédiatement les requêtes en file d’attente pour récupérer à nouveau du contenu du système de publication.

Comme nous utilisons maintenant un corps de requête, nous devons également définir le type de contenu et la longueur du contenu en fonction de la norme HTTP.

Dispatcher marque également les URL appropriées en interne afin qu’il sache qu’il peut envoyer ces ressources directement, même si elles sont considérées comme non valides par l’invalidation automatique.

Toutes les URL répertoriées sont demandées une par une. Ainsi, vous n’avez pas à vous soucier de créer une charge trop élevée sur les systèmes de publication. Mais vous ne voudriez pas mettre trop d&#39;URL dans cette liste non plus. En fin de compte, la file d’attente doit être traitée dans un délai limité afin de ne pas diffuser trop longtemps du contenu obsolète. Incluez simplement vos 10 pages les plus consultées.

Si vous examinez le répertoire du cache de Dispatcher, vous verrez des fichiers temporaires marqués d’horodatages. Il s’agit des fichiers actuellement chargés en arrière-plan.

**Références**

[helpx.adobe.com - Invalidation des pages mises en cache à partir d’AEM](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

### Protection du système de publication

Dispatcher offre un peu de sécurité supplémentaire en protégeant le système de publication des requêtes qui sont destinées uniquement à des fins de maintenance. Par exemple, vous ne souhaitez pas exposer vos URL `/crx/de` ou `/system/console` au public.

Le fait qu’un pare-feu d’application web (WAF) soit installé dans votre système n’est pas préjudiciable. Mais cela ajoute un nombre important à votre budget et tous les projets ne sont pas dans une situation où ils peuvent se permettre et - ne l&#39;oubliez pas - opérer et entretenir un WAF.

Nous voyons fréquemment un ensemble de règles de réécriture Apache dans la configuration de Dispatcher qui empêchent l’accès aux ressources les plus vulnérables.

Mais vous pouvez également envisager une approche différente :

Selon la configuration de Dispatcher, le module de Dispatcher est lié à un certain répertoire :

```
<Directory />
  SetHandler dispatcher-handler
  …
</Directory>
```

Mais pourquoi lier le gestionnaire à toute la docroot, quand vous avez besoin de filtrer par la suite ?

Vous pouvez d’abord réduire la liaison du gestionnaire. `SetHandler` il suffit de lier un gestionnaire à un répertoire pour le lier à une URL ou à un modèle d’URL :

```
<LocationMatch "^(/content|/etc/design|/dispatcher/invalidate.cache)/.\*">
  SetHandler dispatcher-handler
</LocationMatch>

<LocationMatch "^/dispatcher/invalidate.cache">
  SetHandler dispatcher-handler
</LocationMatch>

…
```

Si vous procédez de la sorte, n’oubliez pas de toujours lier le dispatcher-handler à l’URL d’invalidation de Dispatcher. Sinon, vous ne pourrez pas envoyer de requêtes d’invalidation d’AEM au Dispatcher.

Une autre alternative à l’utilisation de Dispatcher comme filtre consiste à configurer les directives de filtre dans la balise `dispatcher.any`

```
/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }
```

Nous n&#39;imposons pas l&#39;usage d&#39;une directive plutôt que d&#39;une autre, mais nous recommandons plutôt un mélange approprié de toutes les directives.

Mais nous proposons que vous envisagiez de réduire l’espace URL le plus tôt possible dans la chaîne, autant que vous le souhaitez, et ce de la manière la plus simple possible. Gardez tout de même à l’esprit que ces techniques ne remplacent pas une WAF sur des sites web hautement sensibles. Certains appellent ces techniques &quot;le pare-feu du pauvre&quot; - pour une raison.

**Références**

[apache.org- directive sethandler](https://httpd.apache.org/docs/2.4/mod/core.html#sethandler)

[helpx.adobe.com - Configuration de l’accès au filtre de contenu](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#ConfiguringAccesstoContentfilter)

### Filtrage à l’aide d’expressions régulières et de globes

Dans les premiers jours, vous ne pouviez utiliser que des &quot;globs&quot;, des espaces réservés simples pour définir des filtres dans la configuration de Dispatcher.

Heureusement, cela a changé dans les versions ultérieures de Dispatcher. Vous pouvez désormais également utiliser des expressions régulières POSIX et accéder à différentes parties d’une requête pour définir un filtre. Pour quelqu’un qui vient de commencer avec Dispatcher et qui pourrait être considéré comme acquis. Mais si vous avez l&#39;habitude d&#39;avoir des globes seulement, c&#39;est une sorte de surprise et on peut facilement l&#39;ignorer. En plus de la syntaxe des globs et des regex, est trop similaire. Comparons deux versions qui font la même chose :

```
# Version A

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }

# Version B

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url '/content.\*'  }
```

Vous voyez la différence ?

La version B utilise des guillemets simples `'` pour marquer un _modèle d’expression régulière_. &quot;N’importe quel caractère&quot; est exprimé à l’aide de `.*`.

_Les motifs_ de mondialisation, en revanche, utilisent des guillemets doubles  `"` et vous ne pouvez utiliser que des espaces réservés simples tels que  `*`.

Si vous connaissez cette différence, c&#39;est trivial - mais si ce n&#39;est pas le cas, vous pouvez facilement mélanger les guillemets et passer un après-midi ensoleillé à déboguer votre configuration. Maintenant on vous avertit.

&quot;Je reconnais `'/url'` dans la configuration ... Mais qu’est-ce que `'/glob'` dans le filtre que vous pouvez demander ?

Cette directive représente la chaîne de requête entière, y compris la méthode et le chemin d’accès. Cela pourrait être une bonne chose.

`"GET /content/foo/bar.html HTTP/1.1"`

il s’agit de la chaîne par rapport à laquelle votre modèle serait comparé. Les débutants ont tendance à oublier la première partie, la `method` (GET, POST, etc.). Donc, un modèle

`/0002  { /glob "/content/\*" /type "allow" }`

échoue toujours, car &quot;/content&quot; ne correspond pas à &quot;GET..&quot; de la requête.

Donc, lorsque vous souhaitez utiliser Globs,

`/0002  { /glob "GET /content/\*" /type "allow" }`

serait correct.

Pour une règle de refus initiale, comme

`/0001  { /glob "\*" /type "deny" }`

c&#39;est parfait. Toutefois, pour les autorisations suivantes, il est plus clair et plus expressif d’utiliser les parties d’une requête de manière plus sécurisée :

```
/method
/url
/path
/selector
/extension
/suffix
```

Comme :

```
/005  {

  /type "allow"
  /method "GET"
  /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

Notez que vous pouvez mélanger des expressions regex et glob dans une règle.

Un dernier mot sur les &quot;numéros de ligne&quot; comme `/005` devant chaque définition,

Ils n&#39;ont aucun sens ! Vous pouvez choisir des dénominateurs arbitraires pour les règles. L&#39;utilisation des nombres ne nécessite pas beaucoup d&#39;effort pour penser à un schéma, mais gardez à l&#39;esprit que l&#39;ordre est important.

Si vous avez des centaines de règles de ce type :

```
/001
/002
/003
…
/100
…
```

et vous souhaitez en insérer une entre /001 et /002 que se passe-t-il avec les numéros suivants ? Augmentez-vous leur nombre ? insère-t-on des nombres intermédiaires ?

```
/001
/001a
/002
/003
…
/100
…
```

Ou que se passe-t-il si vous changez pour réorganiser /003 et /001, allez-vous changer les noms et leur identité ou êtes-vous

```
/003
/002
/001
…
/100
…
```

La numérotation, bien qu&#39;un choix semble simple, atteint ses limites à long terme. Soyons honnêtes, choisir des nombres comme identifiants est de toute façon un mauvais style de programmation.

Nous aimerions proposer une approche différente : Vous ne trouverez probablement pas d’identifiants significatifs pour chaque règle de filtre individuelle. Mais elles ont probablement un but plus grand, et peuvent donc être regroupées d&#39;une manière ou d&#39;une autre selon ce but. Par exemple, &quot;configuration de base&quot;, &quot;exceptions spécifiques à l’application&quot;, &quot;exceptions globales&quot; et &quot;sécurité&quot;.

Vous pouvez ensuite nommer et regrouper les règles en conséquence et fournir au lecteur la configuration (votre cher collègue), une certaine orientation dans le fichier :

```plain
  # basic setup:

  /filter {

    # basic setup

    /basic_01  { /glob "\*"             /type "deny"  }
    /basic_02  { /glob "/content/\*"    /type "allow" }
    /basic_03  { /glob "/etc/design/\*" /type "allow" }

    /basic_04  { /extension '(json|xml)'  /type "deny"  }
    …


    # login

    /login_01 { /glob "/api/myapp/login/\*" /type "allow" }
    /login_02 { … }

    # global exceptions

    /global_01 { /method "POST" /url '.\*contact-form.html' }
```


Vous ajouterez probablement une nouvelle règle à l’un des groupes, voire vous créerez peut-être un nouveau groupe. Dans ce cas, le nombre d’éléments à renommer/renuméroter est limité à ce groupe.

>[!WARNING]
>
>Des configurations plus sophistiquées divisent les règles de filtrage en plusieurs fichiers, qui sont inclus par le fichier de configuration `dispatcher.any` principal. Un nouveau fichier n’introduit toutefois pas de nouvel espace de noms. Ainsi, si vous avez une règle &quot;001&quot; dans un fichier et &quot;001&quot; dans un autre, vous obtiendrez une erreur. Encore plus de raisons de trouver des noms sémantiquement forts.

**Références**

[helpx.adobe.com - Conception de modèles pour les propriétés glob](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#DesigningPatternsforglobProperties)

### Spécification du protocole

La dernière astuce n&#39;est pas une vraie astuce, mais nous avons eu l&#39;impression que ça valait la peine de la partager avec vous de toute façon.

AEM et Dispatcher fonctionnent dans la plupart des cas d’usine. Vous ne trouverez donc pas de spécification de protocole Dispatcher complète sur le protocole d’invalidation afin de créer votre propre application. L&#39;information est publique, mais un peu éparpillée sur un certain nombre de ressources.

Nous essayons de combler le vide dans une certaine mesure ici. Voici à quoi ressemble une requête d’invalidation :

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: <action>
CQ-Handle: <path-pattern>
[CQ-Action-Scope]
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]

<newline>

<refetch-url-1>
<refetch-url-2>

…

<refetch-url-n>
```

`POST /dispatcher/invalidate.cache HTTP/1.1` - La première ligne correspond à l’URL du point de terminaison de contrôle de Dispatcher et il est probable que vous ne la modifierez pas.

`CQ-Action: <action>` - Ce qui devrait se passer. `<action>` est :

* `Activate:` supprime  `/path-pattern.*`
* `Deactive:` delete  `/path-pattern.*`
AND delete  `/path-pattern/*`
* `Delete:`   delete  `/path-pattern.*`
AND delete 
`/path-pattern/*`
* `Test:`   Renvoie &quot;ok&quot; mais ne fait rien

`CQ-Handle: <path-pattern>` - Le chemin content-resource à invalider. Remarque : `<path-pattern>` est en fait un &quot;chemin&quot; et non un &quot;modèle&quot;.

`CQ-Action-Scope: ResourceOnly` - Facultatif : Si cet en-tête est défini, le  `.stat` fichier n’est pas modifié.

```
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]
```

Définissez ces en-têtes si vous définissez une liste d’URL de récupération automatique. `<bytes in request body>` est le nombre de caractères dans le corps HTTP

`<newline>` - Si vous disposez d’un corps de requête, il doit être séparé de l’en-tête par une ligne vide.

```
<refetch-url-1>
<refetch-url-2>
…
<refetch-url-n>
```

Répertorier les URL que vous souhaitez récupérer immédiatement après l’invalidation.

## Ressources supplémentaires

Présentation et présentation de la mise en cache de Dispatcher : [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html)

Plus Conseils et astuces sur l’optimisation : [https://helpx.adobe.com/experience-manager/kb/optimizing-the-dispatcher-cache.html#use-ttls](https://helpx.adobe.com/experience-manager/kb/optimizing-the-dispatcher-cache.html#use-ttls)

Documentation du Dispatcher avec toutes les directives expliquées : [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

Questions fréquentes : [https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html](https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html)

Enregistrement d’un webinaire sur l’optimisation de Dispatcher - vivement recommandé : [https://my.adobeconnect.com/p7th2gf8k43?proto=true](https://my.adobeconnect.com/p7th2gf8k43?proto=true)

Présentation &quot;La puissance sous-estimée de l&#39;invalidation du contenu&quot;, conférence &quot;adaptTo()&quot; à Potsdam 2018 [https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html](https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html)

Invalidation de pages mises en cache à partir d’AEM : [https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

## Étape suivante

* [2 - Modèle d’infrastructure](chapter-2.md)
