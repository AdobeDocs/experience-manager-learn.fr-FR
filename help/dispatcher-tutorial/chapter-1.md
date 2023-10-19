---
title: '« Chapitre 1 : concepts, modèles et antimodèles du Dispatcher »'
description: Destinée aux développeurs et développeuses AEM responsables de la conception des composants, cette section succincte présente l’historique et les mécanismes du Dispatcher.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
exl-id: 3bdb6e36-4174-44b5-ba05-efbc870c3520
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: ht
source-wordcount: '17460'
ht-degree: 100%

---

# Chapitre 1 : concepts, modèles et antimodèles du Dispatcher

## Vue d’ensemble

Destinée aux développeurs et développeuses AEM responsables de la conception des composants, cette section succincte présente l’historique et les mécanismes du Dispatcher.

## Pourquoi les développeurs et développeuses doivent-ils s’intéresser à l’infrastructure ?

Le Dispatcher est une partie essentielle de la plupart, voire de toutes les installations d’AEM. Vous trouverez de nombreux articles en ligne qui détaillent la configuration du Dispatcher, ainsi que des conseils et astuces.

Notez que ces articles sont ciblés et s’adressent à des utilisateurs et utilisatrices expérimentés. Vous devez donc avoir une idée précise de ce que vous souhaitez accomplir. Autant chercher un article dans une botte de foin lorsqu’il s’agit de se renseigner sur les _Fonctionnalités et utilités_ du Dispatcher.

### Antimodèle : le Dispatcher est relégué au second plan

Ce manque d’informations de base conduit à un certain nombre d’antimodèles dont plusieurs projets AEM sont coutumiers :

1. Comme le Dispatcher est installé sur le serveur web Apache, sa configuration incombe aux « dieux Unix » participant au projet. Les « développeurs et développeuses Java » ne doivent pas s’en préoccuper.

2. Les développeurs et développeuses Java doivent s’assurer que leur code fonctionne... Le Dispatcher le rendra plus rapide par magie. Le Dispatcher est toujours relégué au second plan. Cependant, ce paradigme est voué à l’échec. Les développeurs et développeuses doivent écrire leur code en tenant compte du Dispatcher. Ils et elles ont donc besoin de connaître les concepts de base du Dispatcher.

### Le dicton « Faites d’abord en sorte que ça marche, puis faites en sorte que ça aille vite » peut faire plus de mal que de bien.

On a vous a peut-être déjà seriné le conseil suivant en matière de programmation _« Faites d’abord en sorte que ça marche, puis faites en sorte que ça aille vite »._. Ce n’est pas entièrement faux. Toutefois, sans le contexte approprié, il est généralement mal interprété et mal appliqué.

Ce conseil vise à empêcher le développeur ou la développeuse d’optimiser prématurément du code qui serait rarement, voir jamais exécuté, à tel point que l’effort d’optimisation ne serait pas justifié. En outre, l’optimisation peut conduire à un code plus complexe et introduire ainsi des bugs. Ainsi, si vous effectuez du développement, ne passez pas trop de temps sur la micro-optimisation de chaque ligne de code. Veillez simplement à choisir les structures de données, les bibliothèques et les algorithmes appropriés. Attendez que l’analyse de la zone réactive d’un profileur indique où une optimisation plus approfondie pourrait augmenter les performances globales.

### Décisions architecturales et artefacts

Cependant, le conseil « Faites d’abord en sorte que ça marche, puis faites en sorte que ça aille vite » est complètement faux quand il s’agit de décisions « architecturales ». Que sont les décisions architecturales ? En termes simples, ce sont des décisions qui sont coûteuses, difficiles et/ou impossibles à modifier par la suite. Gardez à l’esprit que « coûteux » est parfois identique à « impossible ».  Par exemple, lorsque votre projet manque de budget, les modifications coûteuses sont impossibles à mettre en œuvre. Les changements d’infrastructures sont le tout premier type de changements de cette catégorie qui viennent à l’esprit de la plupart des gens. Mais il existe un autre type d’artefact « architectural » qui peut se révéler compliqués à modifier :

1. il s’agit des éléments de code « centraux » d’une application, sur lesquels beaucoup d’autres éléments s’appuient. Pour les modifier, toutes les dépendances doivent être également modifiées et testées à nouveau simultanément.

2. Les artefacts, qui sont impliqués dans certains scénarios asynchrones dépendant du timing où l’entrée – et donc le comportement du système – peut varier de manière très aléatoire. Les changements peuvent avoir des effets imprévisibles et être difficiles à tester.

3. Il s’agit des modèles de logiciels qui sont réutilisés à l’infini dans toutes les parties du système. Si le modèle logiciel se révèle sous-optimal, tous les artefacts qui l’utilisent doivent être recodés.

Mémoriser ? Nous avons expliqué plus haut que le Dispatcher est un élément essentiel d’une application AEM. L’accès à une application web est très aléatoire, car les utilisateurs et utilisatrices se connectent et se déconnectent à des moments imprévisibles. En fin de compte, tout le contenu est (ou doit être) mis en cache dans le Dispatcher. C’est la raison pour laquelle les personnes attentives auront peut-être remarqué que la mise en cache peut être considérée comme un artefact « architectural » et doit donc être maîtrisée par tous les membres de l’équipe, depuis le développement jusqu’à l’administration.

Même si les personnes affectées au développement n’ont pas réellement à configurer le Dispatcher, elles doivent en connaître les concepts, en particulier les limites, pour s’assurer que leur code peut également être exploité par le Dispatcher.

Le Dispatcher n’améliore pas la vitesse du code par magie. C’est pourquoi les développeurs et développeuses doivent créer leurs composants en gardant le Dispatcher à l’esprit. Il est donc nécessaire de savoir comment il fonctionne.

## Mise en cache du Dispatcher - Principes de base

### Le Dispatcher en tant que mise en cache Http - Répartition de charge

Qu’est-ce que le Dispatcher et pourquoi s’appelle-t-il « Dispatcher » ?

Le Dispatcher est

* Avant tout un cache

* Un proxy inverse

* Un module du serveur web Apache httpd, dotant la polyvalence d’Apache des fonctions associées à AEM et fonctionnant sans problème avec tous les autres modules Apache (tels que SSL ou même SSI, comme nous le verrons plus loin).

Aux prémices d’Internet, les sites pouvaient s’attendre à quelques centaines de visiteurs et visiteuses. La configuration d’un Dispatcher (ou répartiteur) « répartissait » ou équilibrait la charge des requêtes sur un certain nombre de serveurs de publication AEM, ce qui était généralement suffisant. Aujourd’hui, cependant, cette configuration n’est plus très souvent utilisée.

Nous verrons différentes manières de configurer les systèmes de Dispatcher et de publication plus loin dans cet article. Commençons par quelques bases de mise en cache http.

![Fonctionnalités de base d’un cache de Dispatcher](assets/chapter-1/basic-functionality-dispatcher.png)

*Fonctionnalités de base d’un cache de Dispatcher*

<br> 

Les principes de base du Dispatcher sont expliqués ici. Le Dispatcher est un proxy inverse simple de mise en cache permettant de recevoir et de créer des requêtes HTTP. Un cycle de requête/réponse normal se présente comme suit :

1. Une personne demande une page.
2. Le Dispatcher vérifie s’il dispose déjà d’une version rendue de cette page. Supposons qu’il s’agisse de la toute première requête pour cette page et que le Dispatcher ne trouve pas de copie locale mise en cache.
3. Le Dispatcher demande alors la page au système de publication.
4. Sur le système de publication, la page est rendue par un JSP ou un modèle HTL.
5. La page est renvoyée au Dispatcher.
6. Le Dispatcher met la page en cache.
7. Il renvoie la page au navigateur.
8. Si la même page est demandée une seconde fois, elle peut être diffusée directement à partir du cache du Dispatcher sans avoir à la restituer à nouveau sur l’instance de publication. Les temps d’attente sont ainsi réduits pour les cycles utilisateur et CPU sur l’instance de publication.

La section précédente couvrait les « pages ». Mais le même schéma s’applique également à d’autres ressources telles que des images, des fichiers CSS, des téléchargements de fichiers PDF, etc.

#### Mettre en cache les données

Le module Dispatcher tire parti des fonctionnalités fournies par le serveur Apache d’hébergement. Les ressources telles que les pages HTML, les téléchargements et les images sont stockées sous la forme de fichiers simples dans le système de fichiers Apache. C’est aussi simple que ça.

Le nom de fichier s’inspire de l’URL de la ressource demandée. Si vous demandez un fichier `/foo/bar.html`, il est stocké sous /`var/cache/docroot/foo/bar.html`, par exemple.

En principe, si tous les fichiers sont mis en cache et donc stockés de manière statique dans le Dispatcher, vous pouvez arrêter le système de publication et le Dispatcher agira comme un simple serveur web. Il ne s’agit que de théorie. La vie réelle est plus compliquée. Vous ne pouvez pas tout mettre en cache, et le cache n’est jamais complètement « plein », car le nombre de ressources peut être infini en raison de la nature dynamique du processus de rendu. Le modèle de système de fichiers statique donne une idée approximative des fonctionnalités du Dispatcher. Et renseigne sur ses limites.

#### Structure des URL AEM et mappage du système de fichiers

Pour comprendre plus en détail le Dispatcher, examinons la structure d’un simple exemple d’URL.  Prenons l’exemple ci-dessous :

`http://domain.com/path/to/resource/pagename.selectors.html/path/suffix.ext?parameter=value&amp;otherparameter=value#fragment`

* `http` indique le protocole.

* `domain.com` est le nom de domaine.

* `path/to/resource` est le chemin d’accès sous lequel la ressource est stockée dans CRX, puis dans le système de fichiers du serveur Apache.

À partir de là, les systèmes de fichiers AEM et Apache divergent.

Dans AEM,

* `pagename` est le libellé des ressources.

* `selectors` représente un certain nombre de sélecteurs utilisés dans Sling pour déterminer le mode de rendu de la ressource. Une URL peut avoir une infinité de sélecteurs. Ils sont séparées par un point. Une section de sélecteurs peut ressembler à « French.mobile.fancy », par exemple. Les sélecteurs ne doivent contenir que des lettres, des chiffres et des tirets.

* `html` est le dernier des « sélecteurs » et est donc appelé extension. Dans AEM/Sling, il détermine également en partie le script de rendu.

* `path/suffix.ext` est une expression de type chemin d’accès qui peut être un suffixe de l’URL.  Elle peut être utilisée dans les scripts AEM pour contrôler davantage le rendu d’une ressource. Nous aborderons ce sujet dans une section dédiée ci-dessous. Pour l’instant, retenez que vous pouvez l’utiliser comme paramètre supplémentaire. Les suffixes doivent avoir une extension.

* `?parameter=value&otherparameter=value` est la section de requête de l’URL. Elle est utilisée pour transmettre des paramètres arbitraires à AEM. Les URL avec des paramètres ne peuvent pas être mises en cache et les paramètres doivent donc être limités aux cas où ils sont absolument nécessaires.

* `#fragment`, la partie fragment d’une URL n’est pas transmise à AEM, elle est utilisée uniquement dans le navigateur : dans les frameworks JavaScript comme « paramètres de routage » ou pour accéder à une partie spécifique de la page.

Dans Apache (*consultez le diagramme ci-dessous*),

* `pagename.selectors.html` est utilisé comme nom de fichier dans le système de fichiers du cache.

Si l’URL comporte un suffixe `path/suffix.ext`, alors

* `pagename.selectors.html` est créé en tant que dossier.

* `path` est un dossier dans le dossier `pagename.selectors.html`.

* `suffix.ext` est un fichier dans le dossier `path`. Remarque : si le suffixe ne comporte pas d’extension, le fichier n’est pas mis en cache.

![Structure du système de fichiers après obtention des URL du Dispatcher](assets/chapter-1/filesystem-layout-urls-from-dispatcher.png).

*Structure du système de fichiers après obtention des URL du Dispatcher*.

<br> 

#### Limites de base

Le mappage entre une URL, la ressource et le nom de fichier est assez simple.

Vous avez peut-être remarqué quelques pièges, cependant :

1. Les URL peuvent devenir longues à n’en plus finir. L’ajout de la partie « chemin » d’un emplacement `/docroot` sur le système de fichiers local peut facilement dépasser les limites de certains systèmes de fichiers. L’exécution du Dispatcher dans les systèmes de fichiers NTFS sous Windows présente son lot de difficultés. Linux reste toutefois la panacée.

2. Les URL peuvent contenir des caractères spéciaux et des umlauts. En règle générale, le Dispatcher ne s’en soucie pas. Gardez toutefois à l’esprit que l’URL est interprétée à de nombreux endroits de votre application. Plutôt deux fois qu’une, nous avons constaté que le comportement erratique d’une application était dû à un morceau de code (personnalisé) rarement utilisé qui n’avait pas été testé suffisamment pour les caractères spéciaux. Évitez-les si vous le pouvez. S’ils sont indispensables, réalisez des tests minutieux.

3. Dans CRX, les ressources contiennent des sous-ressources. Par exemple, une page comporte un certain nombre de sous-pages. À l’inverse des systèmes de fichiers, qui peuvent contenir des fichiers ou des dossiers.

#### Les URL sans extension ne sont pas mises en cache.

Les URL doivent toujours avoir une extension. Toutefois, vous pouvez fournir des URL sans extensions dans AEM. Ces URL ne seront pas mises en cache dans le Dispatcher.

**Exemples**

`http://domain.com/home.html` peut être **mise en cache**.

`http://domain.com/home` **ne peut pas être mise en cache**.

La même règle s’applique lorsque l’URL contient un suffixe. Le suffixe doit disposer d’une extension pour pouvoir être mis en cache.

**Exemples**

`http://domain.com/home.html/path/suffix.html` peut être **mise en cache**.

`http://domain.com/home.html/path/suffix` **ne peut pas être mis en cache**.

Vous vous demandez peut-être, que se passe-t-il si la partie ressource n’a pas d’extension, mais que le suffixe en a une ? Dans ce cas, l’URL n’a aucun suffixe. Prenons l’exemple suivant :

**Exemple**

`http://domain.com/home/path/suffix.ext`

`/home/path/suffix` est le chemin d’accès à la ressource... il n’y a donc aucun suffixe dans l’URL.

**Conclusion**

Ajoutez toujours des extensions au chemin et au suffixe. Certains grands manitous du référencement voient cela d’un mauvais œil, car votre classement dans les résultats de recherche diminuerait. Mais une page non mise en cache est très lente et classée encore plus bas.

#### Conflits de suffixes d’URL

Supposons que vous ayez deux URL valides :

`http://domain.com/home.html`

et

`http://domain.com/home.html/suffix.html`

AEM les reconnaît sans problème. Vous ne rencontreriez aucun problème sur votre ordinateur de développement local (sans Dispatcher). Pas plus que lors des tests UAT ou de chargement. Le problème auquel nous sommes confrontés est si subtil qu’il passe la plupart des tests.  Cela pourrait devenir un souci important en cas d’activité élevée si vous êtes limité en temps sans accès au serveur ou sans ressource pour résoudre le problème. Nous y sommes allés...

Alors... quel est le problème ?

`home.html` dans un système de fichiers peut être un fichier ou un dossier. Pas les deux en même temps comme avec AEM.

Si vous demandez `home.html` en premier, il sera créé sous forme de fichier.

Les requêtes suivantes vers `home.html/suffix.html` renvoient des résultats valides, mais sous la forme de fichier ; `home.html` « bloque » la position dans le système de fichiers, `home.html` ne peut pas être créé une seconde fois en tant que dossier et, par conséquent, `home.html/suffix.html` n’est pas mis en cache.

![Position de blocage des fichiers dans le système de fichiers empêchant la mise en cache des sous-ressources](assets/chapter-1/file-blocking-position-in-filesystem.png)

*Position de blocage des fichiers dans le système de fichiers empêchant la mise en cache des sous-ressources*

<br> 

Si vous effectuez l’opération d’une autre façon, commencez par demander `home.html/suffix.html` puis `suffix.html` sera d’abord mis en cache dans un dossier `/home.html`. Cependant, ce dossier est supprimé et remplacé par un fichier `home.html` lorsque vous demandez ultérieurement `home.html` comme ressource.

![Supprimer une structure de chemin lorsqu’un parent est récupéré comme ressource](assets/chapter-1/deleting-path-structure.png)

*Supprimer une structure de chemin lorsqu’un parent est récupéré comme ressource*

<br> 

Ainsi, le résultat de ce qui est mis en cache est entièrement aléatoire et dépend de l’ordre des requêtes entrantes. Ce qui rend les choses encore plus compliquées, c’est qu’il y a habituellement plus d’un Dispatcher. Les performances, le taux d’accès et le comportement du cache peuvent varier d’un Dispatcher à l’autre. Si vous souhaitez savoir pourquoi votre site Web ne répond pas, vous devez vous assurer que vous consultez le bon Dispatcher avec l’ordre de mise en cache défectueux. Si vous vérifiez le Dispatcher qui, par chance, a un modèle de requête favorable, ce serait peine perdue de trouver le problème à résoudre.

#### Eviter les URL en conflit

Vous pouvez éviter les « URL en conflit », lorsqu’un nom de dossier et un nom de fichier « concourent » pour le même chemin d’accès dans le système de fichiers, lorsque vous utilisez une extension différente pour la ressource lorsque vous disposez d’un suffixe.

**Exemple**

* `http://domain.com/home.html`

* `http://domain.com/home.dir/suffix.html`

Les deux sont bien mises en cache,

![](assets/chapter-1/cacheable.png)

Choisissez une extension dédiée « dir » pour une ressource lorsque vous demandez un suffixe ou que vous évitez de l’utiliser avec le suffixe. Il existe de rares cas où ces éléments sont utiles. Ces cas sont relativement simples à mettre en œuvre.  Nous en reparlerons dans le prochain chapitre, lorsque nous aborderons l’invalidation et la purge du cache.

#### Requêtes impossibles à mettre en cache

Passons en revue rapidement le dernier chapitre et quelques autres exceptions. Le Dispatcher peut mettre en cache une URL si elle est configurée comme pouvant être mise en cache et s’il s’agit d’une requête GET. Elle ne peut pas être mise en cache sous l’une des conditions suivantes.

**Requêtes pouvant être mises en cache**

* La requête est configurée pour pouvoir être mise en cache dans la configuration du Dispatcher.
* La requête est une requête GET brute

**Requêtes ou réponses impossibles à mettre en cache**

* Requête refusée par le cache de par sa configuration (chemin, modèle, type MIME)
* Réponses qui renvoient un en-tête « Dispatcher: no-cache »
* Réponse qui renvoie un en-tête « Cache-Control: no-cache|private »
* Réponse qui renvoie un en-tête « Pragma: no-cache »
* Requête avec paramètres de requête
* URL sans extension
* URL avec un suffixe qui ne comporte pas d’extension
* Réponse qui renvoie un code d’état autre que 200
* Requête POST

## Invalider et purger le cache

### Vue d’ensemble

Le dernier chapitre répertorie un grand nombre d’exceptions de cas où le Dispatcher ne peut pas mettre en cache une requête. Il y a d’autres choses à prendre en compte : le fait qu’une requête _puisse_ être mise en cache par le Dispatcher ne signifie pas que ce soit une _obligation_.

La mise en cache est d’une facilité déconcertante. Le Dispatcher doit simplement stocker le résultat d’une réponse et le renvoyer la prochaine fois que la même requête est lancée. Droite ? Faux.

La partie difficile est l’_invalidation_ ou la _purge_ du cache. Le Dispatcher doit savoir quand une ressource a été modifiée et doit être rendue à nouveau.

Cela semble être du gâteau à première vue... mais ce n’est pas le cas. Poursuivez votre lecture pour découvrir les différences subtiles entre des ressources uniques et simples et des pages qui reposent sur une structure complexe de plusieurs ressources.

### Ressources simples et purge

Nous avons configuré le système AEM de façon à créer dynamiquement un rendu miniature pour chaque image lors d’une demande, au moyen d’un sélecteur de « miniature » spécial :

`/content/dam/path/to/image.thumb.png`

Et, bien sûr, nous fournissons une URL pour diffuser l’image d’origine avec une URL sans sélecteur :

`/content/dam/path/to/image.png`

Si nous téléchargeons à la fois la miniature et l’image d’origine, nous obtiendrons des chemins similaires à ceux-ci

```
/var/cache/dispatcher/docroot/content/dam/path/to/image.thumb.png

/var/cache/dispatcher/docroot/content/dam/path/to/image.png
```

dans notre système de fichiers du Dispatcher.

La personne charge et active ensuite une nouvelle version de ce fichier. Enfin, une requête d’invalidation est envoyée d’AEM au Dispatcher :

```
GET /invalidate
invalidate-path:  /content/dam/path/to/image

<no body>
```

L’action d’invalidation est sans fioritures : il s’agit d’une simple requête GET envoyée à une URL « /invalidate » spéciale sur le Dispatcher. Un corps HTTP n’est pas obligatoire, la « payload » est simplement l’en-tête « invalidate-path ». Notez également que « invalidate-path » dans l’en-tête est la ressource qu’AEM connaît, et non le fichier ou les fichiers mis en cache par le Dispatcher. AEM ne connaît que les ressources. Les extensions, sélecteurs et suffixes sont utilisés au moment de l’exécution lorsqu’une ressource est demandée. AEM ne conserve pas en mémoire les sélecteurs utilisés sur une ressource. Seul le chemin d’accès à la ressource est connu avec certitude lorsqu’une ressource est activée.

Cela suffit dans notre cas. Si une ressource a été modifiée, on peut en déduire que tous les rendus de cette ressource ont également été modifiés. Dans notre exemple, si l’image a été modifiée, une nouvelle miniature est également rendue.

Le Dispatcher peut supprimer la ressource en toute sécurité avec tous les rendus qu’il a mis en cache. Le résultat se présente comme suit :

`$ rm /content/dam/path/to/image.*`

`image.png`, `image.thumb.png` et tous les autres rendus correspondant à ce modèle sont supprimés.

Super simple, en effet... tant que vous n’utilisez qu’une seule ressource pour répondre à une requête.

### Références et contenu complexe

#### Le problème du contenu complexe

Contrairement aux images ou autres fichiers binaires chargés sur AEM, les pages HTML essaiment en groupe. Elles vivent en communauté et sont très interconnectées les unes aux autres au moyen d’hyperliens et de références. Le lien simple est inoffensif, mais la situation devient plus délicate lorsque des références de contenu sonnent la charge. La navigation supérieure ou les teasers omniprésents sur les pages sont des références de contenu.

#### Références de contenu et raisons du problème

Prenons un exemple simple. Une agence de voyage fait la promotion d’un voyage au Canada sur une page de son site web. Cette promotion s’affiche dans la section teaser de deux autres pages, la page d’accueil et la page des promotions d’hiver.

Comme les deux pages affichent le même teaser, il est inutile de demander à l’auteur ou à l’autrice de créer le teaser sur chaque page sur laquelle il doit s’afficher. À la place, la page cible « Canada » dispose d’une section dans les propriétés de la page permettant de renseigner les informations sur le teaser et de fournir une URL qui rend complètement ce teaser :

`<sling:include resource="/content/home/destinations/canada" addSelectors="teaser" />`

ou

`<sling:include resource="/content/home/destinations/canada/jcr:content/teaser" />`

![](assets/chapter-1/content-references.png)

Sur AEM, cela fonctionne à merveille, mais si vous utilisez un Dispatcher sur l’instance de publication, quelque chose d’étrange se produit.

Imaginez que vous ayez publié votre site web. Le titre de votre page Canada est « Canada ». Lorsqu’un visiteur ou une visiteuse consulte votre page d’accueil (qui comporte une référence de teaser à cette page), le composant de la page « Canada » affiche un code similaire à celui-ci :

```
<div class="teaser">
  <h3>Canada</h3>
  <img …>
</div>
```

*sur* la page d’accueil. La page d’accueil est stockée par le Dispatcher sous la forme d’un fichier .html statique, incluant le teaser et son titre dans le fichier.

La personne spécialisée dans le marketing sait désormais que les titres de teaser doivent être exploitables. Elle décide donc de remplacer le titre « Canada » par « Visitez le Canada » et de mettre à jour l’image.

Elle publie la page « Canada » modifiée et consulte à nouveau la page d’accueil précédemment publiée pour voir les modifications apportées. Mais rien n’a changé. L’ancien teaser apparaît toujours. Elle revérifie l’élément « Offre d’hiver ». Cette page n’a jamais été demandée auparavant et n’est donc pas mise en cache de manière statique dans le Dispatcher. Cette page est donc rendue par l’instance de publication et contient maintenant le nouveau teaser « Visitez le Canada ».

![Le Dispatcher stocke le contenu obsolète inclus dans la page d’accueil.](assets/chapter-1/dispatcher-storing-stale-content.png)

*Le Dispatcher stocke le contenu obsolète inclus dans la page d’accueil.*

<br> 

Que s’est-il passé ? Le Dispatcher stocke une version statique de la page avec tout le contenu et les balises qui ont été extraits d’autres ressources lors du rendu.

Le Dispatcher, qui est un simple serveur web basé sur un système de fichiers, est rapide mais aussi relativement simple. Si une ressource incluse est modifiée, cela n’entre pas en compte. Le contenu reste identique à celui de la page rendue.

La page « Offre d’hiver » n’a pas encore été rendue. Il n’existe donc pas de version statique sur le Dispatcher. Elle s’affiche donc avec le nouveau teaser, car il vient d’être rendu avec la requête.

Vous pourriez penser que le Dispatcher suit chaque modification de la ressource utilisée lors du rendu et de la purge des pages ayant utilisé ladite ressource. Cependant, le Dispatcher ne rend pas les pages. Le rendu est effectué par le système de l’instance de publication. Le Dispatcher ne sait pas quelles ressources seront contenues par le fichier .html rendu.

Besoin de plus d’explications ? Vous vous dites sûrement qu’*il doit y avoir un moyen de mettre en œuvre une sorte de suivi des dépendances*. Eh bien il y en a un, ou plus précisément, il y en *avait un*. Communiqué 3, l’ancêtre d’AEM, était équipé d’un dispositif de suivi des dépendances dans la _session_ utilisée pour effectuer le rendu d’une page.

Lors d’une requête, chaque ressource acquise via cette session était suivie en tant que dépendance de l’URL en cours de rendu.

Mais il s’est avéré que garder une trace des dépendances coûtait très cher. Les utilisateurs et les utilisatrices ont rapidement découvert que leur site web était plus rapide s’ils désactivaient complètement la fonctionnalité de suivi des dépendances et s’ils procédaient à un nouveau rendu de toutes les pages HTML après modification d’une page HTML. En outre, ce schéma n’était pas parfait non plus, il y avait un certain nombre de soucis et d’exceptions à gérer. Dans certains cas, vous n’utilisiez pas la session de requêtes par défaut pour obtenir une ressource, mais une session d’administration pour obtenir des ressources d’assistance pour effectuer le rendu d’une requête. Ces dépendances n’étaient généralement pas suivies et menaient à de nombreux problèmes qui pouvaient être résolus en contactant les personnes membres de l’équipe des opérations pour qu’elles vident le cache manuellement. Il fallait avoir de la chance pour avoir une procédure standardisée. Il y avaient plusieurs autres pièges, mais ne parlons pas trop du passé. Cela remonte à 2005. En fin de compte, cette fonctionnalité a été désactivée dans Communiqué 4 par défaut et elle n’a pas été réactivée dans CQ5, son successeur, qui est ensuite devenu AEM.

### Invalidation automatique

#### Quand la purge complète est moins chère que le suivi des dépendances

Depuis CQ5, la quasi totalité du site dépend de l’invalidation si une seule page est modifiée. Cette fonctionnalité s’appelle « Invalidation automatique ».

Cela dit, comment supprimer et rendre à nouveau des centaines de pages peut-il constituer un processus moins cher qu’effectuer un suivi de dépendance approprié et un rendu partiel ?

Il existe deux raisons principales :

1. Sur un site web moyen, seul un petit sous-ensemble des pages est fréquemment demandé. Ainsi, même si vous supprimez tout le contenu rendu, seules quelques douzaines seront effectivement demandées immédiatement après. Le rendu des mots-clés à faible demande des pages peut être distribué au fil du temps, lorsqu’il est demandé. En fait, la charge sur le rendu des pages rendu n’est pas aussi élevée qu’on pourrait le penser. Bien sûr, il y a toujours des exceptions... Nous discuterons plus tard de quelques astuces pour gérer les charges distribués de manière égale sur les sites web plus volumineux avec des caches de Dispatcher vides.

2. De toute façon, toutes les pages sont connectées par la navigation principale. Ainsi, presque toutes les pages sont dépendantes les unes des autres. Cela signifie que même le dispositif de suivi des dépendances le plus intelligent trouvera ce que nous savons déjà : si l’une des pages est modifiée, vous devez invalider toutes les autres.

Vous êtes d’accord ? Illustrons le dernier point.

Nous utilisons le même argument que dans le dernier exemple avec des teasers faisant référence au contenu d’une page distante. Ce n’est que maintenant que nous utilisons un exemple plus extrême : une navigation principale rendue automatiquement. Comme c’était le cas avec le teaser, le titre de navigation est tiré de la page liée ou « distante » comme référence de contenu. Les titres de navigation de la page distante ne sont pas stockés dans la page en cours de rendu. N’oubliez pas que la navigation s’affiche sur chaque page de votre site web. Ainsi, le titre d’une page est utilisé plusieurs fois sur toutes les pages qui ont une navigation principale. Si vous souhaitez modifier un titre de navigation, il est préférable de le faire une seule fois sur la page distante, et non sur chaque page référençant la page concernée.

Ainsi, dans notre exemple, la navigation relie toutes les pages en utilisant le « titre de navigation » de la page cible pour générer un nom dans la navigation. Le titre de navigation « Islande » est tiré de la page « Islande », puis rendu dans chaque page qui comporte une navigation principale.

![La navigation principale imbrique inévitablement le contenu de toutes les pages en extrayant leurs « titres de navigation ».](assets/chapter-1/nav-titles.png)

*La navigation principale imbrique inévitablement le contenu de toutes les pages en extrayant leurs « titres de navigation ».*

<br> 

Si vous changez le titre de navigation sur la page Islande de « Islande » à « Belle Islande », ce titre change immédiatement dans le menu principal de toutes les autres pages. Ainsi, les pages rendues et mises en cache avant cette modification deviennent toutes obsolètes et doivent être invalidées.

#### Mise en œuvre de l’invalidation automatique : fichier .stat

Si vous avez un site volumineux contenant des milliers de pages, il faudrait un certain temps pour parcourir toutes les pages et les supprimer physiquement. Pendant ce temps-là, le Dispatcher pourrait donc afficher involontairement du contenu obsolète. Pire encore, il pourrait y avoir des conflits d’accès aux fichiers du cache, si une page en cours de suppression était demandée ou si une page était à nouveau supprimée en raison d’une seconde invalidation survenue après une activation immédiate. Cela pourrait mettre la pagaille. Heureusement, ce n’est pas ainsi. Le Dispatcher utilise une astuce intelligente pour éviter cela : au lieu de supprimer des centaines et des milliers de fichiers, il place un simple fichier vide à la racine de votre système de fichiers lorsqu’un fichier est publié et tous les fichiers dépendants sont donc considérés comme non valides. Ce fichier est appelé « fichier stat ». Le fichier stat est un fichier vide. Ce qui importe dans le fichier stat, c’est sa date de création uniquement.

Tous les fichiers du Dispatcher, dont la date de création est antérieure au fichier stat, sont rendus avant la dernière activation (et invalidation) et sont donc considérés comme « non valides ». Ils sont toujours physiquement présents dans le système de fichiers, mais le Dispatcher les ignore. Ils sont « obsolètes ». Chaque fois qu’une requête est envoyée à une ressource obsolète, le Dispatcher demande au système AEM de rendre à nouveau la page. Cette page nouvellement rendue est alors stockée dans le système de fichiers, avec une nouvelle date de création et elle est à nouveau actualisée.

![La date de création du fichier .stat définit quel contenu est obsolète et quel contenu a été actualisé.](assets/chapter-1/creation-date.png)

*La date de création du fichier .stat définit quel contenu est obsolète et quel contenu a été actualisé.*

<br> 

Vous vous demandez certainement pourquoi il s’appelle « .stat » ? Et pourquoi pas « .invalide » ? Ce fichier permet à votre système de fichiers d’aider le Dispatcher à déterminer quelles ressources peuvent *statiquement* être diffusées, comme ce serait le cas avec un serveur web statique. Ces fichiers n’ont plus besoin d’être rendus de façon dynamique.

La vraie nature du nom, cependant, est moins métaphorique. Il est dérivé de l’appel système Unix `stat()`, qui renvoie l’heure de modification d’un fichier (entre autres propriétés).

#### Mêler validation simple et automatique

Mais attendez...Nous avons dit plus tôt que les ressources uniques sont supprimées physiquement. Nous disons maintenant qu’un fichier stat plus récent les rendrait virtuellement invalides pour le Dispatcher. Pourquoi parlons-nous de suppression physique d’abord ?

La réponse est simple. Vous utilisez généralement les deux stratégies en parallèle, mais pour différents types de ressources. Les fichiers binaires, telles que les images, sont autonomes. Ils ne sont pas connectés à d’autres ressources dans la mesure où ils ont besoin que leurs informations soient rendues.

Les pages HTML, en revanche, sont très interdépendantes. Vous devez donc appliquer l’invalidation automatique sur celles-ci. Il s’agit du paramètre par défaut dans le Dispatcher. Tous les fichiers appartenant à une ressource invalidée sont supprimés physiquement. En outre, les fichiers portant l’extension « .html » sont automatiquement invalidés.

Le Dispatcher décide d’appliquer ou non un schéma d’invalidation automatique selon l’extension du fichier.

La sélection des extenstions de fichier concernées par l’invalidation automatique peut être configurée. En théorie, vous pouvez inclure toutes les extensions dans l’invalidation automatique. Mais gardez à l’esprit que cela a un prix très élevé. Vous ne verrez pas de ressources obsolètes diffusées involontairement, mais les performances de diffusion se dégradent considérablement en raison d’une invalidation excessive.

Imaginez, par exemple, que vous implémentiez un schéma dans lequel les PNG et les JPG sont rendus dynamiquement et dépendent d’autres ressources pour l’être. Vous pouvez redimensionner les images haute résolution pour obtenir une résolution compatible avec le web plus petite. Vous pouvez également modifier le taux de compression. La résolution et le taux de compression dans cet exemple ne sont pas des constantes fixes, mais des paramètres configurables dans le composant qui utilise l’image. Si vous modifiez ce paramètre, vous devez invalider les images.

Aucun problème : nous venons d’apprendre que nous pouvons ajouter des images à l’invalidation automatique et toujours avoir des images nouvellement rendues chaque fois que quelque chose change.

#### Jeter le bébé avec l’eau du bain

C’est vrai, et cela suppose un grand problème. Lisez à nouveau le dernier paragraphe. « ...images nouvellement rendues chaque fois que quelque chose change ». Comme vous le savez, un bon site web fait constamment l’objet de modifications : ajout de nouveau contenu ici, correction d’une faute de frappe, retouche d’un teaser ailleurs. Cela signifie que toutes vos images sont invalidées en permanence et doivent faire l’objet d’un nouveau rendu. Ne sous-estimez pas ce point. Le rendu et le transfert dynamiques des données d’image fonctionnent en millisecondes sur votre ordinateur de développement local. Votre environnement de production doit le faire cent fois plus souvent, par seconde.

Soyons clairs, vos JPG doivent faire l’objet d’un nouveau rendu, lorsqu’une page HTML change et vice versa. Il n’y a qu’un seul « compartiment » de fichiers à invalider automatiquement. Il est vidé dans son ensemble. Sans aucun moyen de se diviser en structures plus détaillées.

Il existe une bonne raison pour laquelle l’invalidation automatique est conservée en « .html » par défaut. L’objectif est que ce compartiment reste aussi petit que possible. Ne jetez pas le bébé avec l’eau du bain en invalidant tout juste par sécurité.

Les ressources autonomes doivent être diffusées sur le chemin d’accès de cette ressource. Ça facilite beaucoup l’invalidation. Restez simple, ne créez pas de schémas de mappage tels que la « resource /a/b/c » est diffusée à partir de « /x/y/z ». Assurez-vous que vos composants fonctionnent avec les paramètres d’invalidation automatique de Dispatcher par défaut. N’essayez pas de réparer un composant mal conçu avec une invalidation excessive dans le Dispatcher.

##### Exceptions à l’invalidation automatique : invalidation ResourceOnly

La demande d’invalidation de Dispatcher est généralement déclenchée à partir du ou des systèmes de publication par un agent de réplication.

Si vous avez confiance en vos dépendances, vous pouvez essayer de créer votre propre agent de réplication invalidant.

Entrer dans les détails dépasse le cadre de ce guide, mais nous voulons vous donner au moins quelques conseils.

1. Vous devez savoir ce que vous faites. Obtenir l’invalidation de manière correcte est vraiment difficile. C’est l’une des raisons pour lesquelles l’invalidation automatique est si rigoureuse ; pour éviter de diffuser du contenu obsolète.

2. Si votre agent envoie un en-tête HTTP `CQ-Action-Scope: ResourceOnly`, cela signifie que cette seule demande d’invalidation ne déclenche pas d’invalidation automatique. Ce fragment de code ([https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle](https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle)) peut être un bon point de départ pour votre propre agent de réplication.

3. `ResourceOnly`, empêche uniquement l’invalidation automatique. Pour résoudre et invalider les dépendances nécessaires, vous devez déclencher les demandes d’invalidation vous-même. Vous pouvez consulter les règles de purge du Dispatcher de package ([https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html)) pour vous inspirer de la façon dont cela pourrait se produire.

Nous vous déconseillons de créer un schéma de résolution de dépendance. Cela représente trop d’efforts pour peu de bénéfices. Et comme nous l’avons dit, il y a tellement de choses que vous vous tromperez.

Ce que vous devez plutôt faire est de déterminer quelles ressources n’ont aucune dépendance sur d’autres ressources et peuvent être invalidées sans invalidation automatique. Vous n’avez pas besoin d’utiliser un agent de réplication personnalisé. Il vous suffit de créer une règle personnalisée dans votre configuration Dispatcher qui exclut ces ressources de l’invalidation automatique.

Nous avons dit que la navigation principale ou les teasers sont une source de dépendances. Si vous chargez la navigation et les teasers de manière asynchrone ou que vous les incluez avec un script SSI dans Apache, vous n’aurez pas à suivre cette dépendance. Nous expliquerons plus en détail le chargement asynchrone des composants plus loin dans ce document lorsque nous parlerons de « Sling Dynamic Include ».

Il en va de même pour les fenêtres pop-up ou le contenu chargé dans une lightbox. Ces éléments comportent rarement des navigations (c’est-à-dire des « dépendances ») et peuvent être invalidés en tant que ressources uniques.

## Créer des composants avec le Dispatcher à l’esprit

### Appliquer le mécanisme de Dispatcher dans un exemple réel

Dans le dernier chapitre, nous avons expliqué le mécanisme de base de Dispatcher, son fonctionnement général et ses limites.

Nous voulons maintenant appliquer ces mécanismes à un type de composants que vous trouverez très probablement dans les exigences de votre projet. Nous choisissons délibérément le composant pour démontrer les problèmes auxquels vous devrez faire face tôt ou tard. Ne vous inquiétez pas, tous les composants n’ont pas besoin de toute cette attention particulière. Mais si vous comprenez la nécessité de créer un tel composant, c’est que vous avez conscience des conséquences et que vous savez comment les gérer.

### (Anti-)modèle de composant de spooling

#### Composant d’image réactive

Illustrons un modèle commun (ou anti-modèle) d’un composant avec des fichiers binaires interconnectés. Nous allons créer un composant « respi » pour « responsive-image ». Ce composant doit pouvoir adapter l’image affichée à l’appareil sur lequel il est affiché. Sur les ordinateurs de bureau et les tablettes, il affiche la résolution complète de l’image ; sur les téléphones, une version plus petite avec un recadrage étroit, ou un motif complètement différent (c’est ce qu’on appelle « direction artistique » dans le monde réactif).

Les ressources sont chargées dans la zone de gestion des ressources numériques d’AEM et sont uniquement _référencées_ dans le composant responsive-image.

Le composant respi s’occupe à la fois du rendu du balisage et de la diffusion des données d’image binaire.

La façon dont nous l’implémentons ici est un modèle commun que nous avons vu dans de nombreux projets, et même l’un des composants principaux AEM est basé sur ce modèle. Il est donc très probable qu’en tant que développeur ou développeuse, vous puissiez adapter ce modèle. Il a ses points forts en termes d’encapsulation, mais sa préparation au Dispatcher requiert beaucoup de travail. Nous discuterons plus tard de plusieurs options pour atténuer le problème.

Nous appelons le modèle utilisé ici « Spooler Pattern », parce que le problème remonte aux premiers jours de Communiqué 3 où il y avait une méthode « spool » qui pouvait être appelée sur une ressource pour diffuser ses données brutes binaires dans la réponse.

Le terme original « spooling » (mise en file d’attente) fait référence à des périphériques hors ligne partagés lents, tels que les imprimantes. Il n’est donc pas correctement appliqué ici. Mais nous apprécions ce terme de toute façon parce qu’il est rarement utilisé dans le monde en ligne, et est donc facile à distinguer. Chaque modèle devrait avoir un nom distinctif, n’est-ce pas ? C’est à vous de décider s’il s’agit d’un modèle ou d’un anti-modèle.

#### Implémentation

Voici comment notre composant responsive-image est implémenté :

Le composant comporte deux parties. La première partie effectue le rendu du balisage HTML de l’image, la seconde « met en file d’attente » les données binaires de l’image référencée. Comme il s’agit d’un site web moderne avec une conception réactive, nous n’effectuons pas le rendu d’une balise `<img src"…">` simple, mais d’un ensemble d’images dans la balise `<picture/>`. Pour chaque appareil, nous chargeons deux images différentes dans la gestion des ressources numériques et nous les référençons à partir de notre composant d’image.

Le composant comporte trois scripts de rendu (implémentés en JSP, HTL ou en tant que servlet), chacun étant traité avec un sélecteur dédié :

1. `/respi.jsp`, sans sélecteur pour le rendu du balisage HTML.
2. `/respi.img.java`, pour effectuer le rendu de la version de bureau.
3. `/respi.img.mobile.java`, pour effectuer le rendu de la version mobile.


Le composant est placé dans le parsys de la page d’accueil. La structure résultante dans le CRX est illustrée ci-dessous.

![Structure de ressource de l’image réactive dans le CRX.](assets/chapter-1/responsive-image-crx.png)

*Structure de ressource de l’image réactive dans le CRX.*

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

et... nous avons terminé avec notre composant bien encapsulé.

#### Composant d’image réactive en action

Un utilisateur ou une utilisatrice demande la page et les ressources via Dispatcher. Cela génère des fichiers dans le système de fichiers de Dispatcher comme illustré ci-dessous.

![Structure mise en cache du composant d’image réactive encapsulé.](assets/chapter-1/cached-structure-encapsulated-image-comonent.png)

*Structure mise en cache du composant d’image réactive encapsulé.*

<br> 

Imaginons qu’un utilisateur ou une utilisatrice charge et active une nouvelle version des deux images de fleurs dans la gestion des ressources numériques. AEM enverra une demande d’invalidation pour

`/content/dam/flower.jpg`

et

`/content/dam/flower-mobile.jpg`

vers Dispatcher. Ces requêtes sont cependant effectuées en vain. Le contenu a été mis en cache sous forme de fichiers sous la sous-structure du composant. Ces fichiers sont désormais obsolètes, mais toujours diffusés en cas de requêtes.

![Incohérence de la structure à l’origine de l’obsolescence du contenu](assets/chapter-1/structure-mismatch.png).

*Incohérence de la structure à l’origine de l’obsolescence du contenu*.

<br> 

Il y a une autre mise en garde par rapport à cette approche. Imaginez que vous utilisez le même fichier flower.jpg sur plusieurs pages. Vous disposez ensuite de la même ressource mise en cache sous plusieurs URL ou fichiers.

```
/content/home/products/jcr:content/par/respi.img.jpg

/content/home/offers/jcr:content/par/respi.img.jpg

/content/home/specials/jcr:content/par/respi.img.jpg

…
```

Chaque fois qu’une nouvelle page non mise en cache est demandée, les ressources sont récupérées à partir d’AEM à différentes URL. Aucune mise en cache de Dispatcher et aucune mise en cache du navigateur ne peut accélérer la diffusion.

#### Où le modèle de spooling se démarque

Il existe une exception naturelle, où ce modèle même sous sa forme simple est utile : si le fichier binaire est stocké dans le composant lui-même, et non dans la gestion des ressources numériques (DAM). Cela n’est toutefois utile que pour les images utilisées une seule fois sur le site web. Le fait de ne pas stocker de ressources dans la gestion des ressources numériques (DAM) rend difficile la gestion de vos ressources. Imaginez maintenant que votre licence d’utilisation pour une ressource particulière est épuisée. Comment savoir sur quel.s composants vous avez utilisé la ressource ?

Vous comprenez ? Le « M » dans DAM veut dire « Management » (gestion), comme dans « Digital Asset Management » (gestion des ressources numériques). Vous ne voulez pas renoncer à cette fonctionnalité.

#### Conclusion

Du point de vue d’un développeur ou d’une développeuse AEM, le modèle semblait super élégant. Mais avec Dispatcher intégré dans l’équation, vous serez dans doute d’accord pour dire qu’une approche naïve pourrait ne pas être suffisante.

Nous vous laissons décider s’il s’agit d’un modèle ou d’un anti-modèle pour l’instant. Et peut-être avez-vous déjà quelques bonnes idées en tête pour atténuer les problèmes expliqués ci-dessus ? Bien. Vous attendez sans doute avec impatience de voir comment d’autres projets ont résolu ces problèmes.

### Résoudre des problèmes courants de Dispatcher

#### Vue d’ensemble

Parlons de la façon dont cela aurait pu être mis en œuvre de manière un peu plus conviviale pour le cache. Il existe plusieurs options. Parfois, vous ne pouvez pas choisir la meilleure solution. Peut-être que vous entrez dans un projet déjà en cours et que vous avez un budget limité pour résoudre uniquement le « problème de cache » à portée de main mais pas assez pour effectuer une refactorisation complète. Ou alors vous rencontrez un problème qui est plus complexe que l’exemple de composant d’image.

Nous présenterons les principes et les mises en garde dans les sections suivantes.

Encore une fois, ceci est basé sur une expérience réelle. Nous avons déjà vu tous ces modèles dans la nature ; il ne s’agit donc pas d’un exercice académique. C’est pourquoi nous vous montrons des anti-modèles ; vous aurez ainsi la chance d’apprendre des erreurs que d’autres personnes ont déjà commises.

#### Cache killer

>[!WARNING]
>
>Il s’agit d’un anti-modèle. Ne l’utilisez pas. Jamais.

Avez-vous déjà vu des paramètres de requête tels que `?ck=398547283745` ? Ils sont appelés cache-killer (« ck »). L’idée est que si vous ajoutez un paramètre de requête, la ressource ne sera pas mise en cache. De plus, si vous ajoutez un nombre aléatoire en tant que valeur du paramètre (comme « 398547283745 »), l’URL devient unique et vous vous assurez qu’aucun autre cache entre le système AEM et votre écran ne peut faire une mise en cache non plus. En règle générale, les suspects intermédiaires seraient un cache « Varnish » devant Dispatcher, un réseau CDN ou même la mémoire cache du navigateur. Encore une fois : ne faites jamais ça. Vous souhaitez que vos ressources soient mises en cache autant que possible. Le cache est votre ami. Ne l’effacez pas.

#### Invalidation automatique

>[!WARNING]
>
>Il s’agit d’un anti-modèle. Évitez de l’utiliser pour les ressources numériques. Essayez de conserver la configuration par défaut de Dispatcher, qui est l’invalidation automatique des fichiers « .html » uniquement.

À court terme, vous pouvez ajouter « .jpg » et « .png » à la configuration d’invalidation automatique dans Dispatcher. Cela signifie que chaque fois qu’une invalidation se produit, tous les « .jpg », « .png » et « .html » doivent faire l’objet d’un nouveau rendu.

Ce modèle est extrêmement facile à mettre en œuvre si les personnes propriétaires d’entreprise se plaignent de ne pas voir leurs modifications se matérialiser suffisamment rapidement sur le site en ligne. Mais cela peut uniquement vous faire gagner du temps pour trouver une solution plus évoluée.

Assurez-vous de bien comprendre les répercussions sur les performances. Cela ralentira considérablement votre site web et pourrait même avoir un impact sur la stabilité si votre site est un site web à forte charge avec des modifications fréquentes, comme un portail d’informations.

#### Empreinte URL

Une empreinte URL ressemble à un cache-killer. Mais ce n’est pas le cas. Ce n’est pas un nombre aléatoire, mais une valeur qui caractérise le contenu de la ressource. Il peut s’agir d’un hachage du contenu de la ressource ou, plus simple encore, d’un horodatage lorsque la ressource a été chargée, modifiée ou mise à jour.

Un horodatage Unix est suffisant pour une mise en œuvre concrète. Pour une meilleure lisibilité, nous utilisons un format plus lisible dans ce tutoriel : `2018 31.12 23:59 or fp-2018-31-12-23-59`.

L’empreinte ne doit pas être utilisée comme paramètre de requête, car les URL avec des paramètres de requête ne peuvent pas être mises en cache. Vous pouvez utiliser un sélecteur ou le suffixe pour l’empreinte.

Supposons que le fichier `/content/dam/flower.jpg` a une date `jcr:lastModified` du 31 décembre 2018, 23 h 59. L’URL avec l’empreinte est `/content/home/jcr:content/par/respi.fp-2018-31-12-23-59.jpg`.

Cette URL reste stable tant que le fichier (`flower.jpg`) de la ressource référencée n’est pas modifié. Elle peut donc être mise en cache pendant une durée indéfinie et ne constitue pas un cache-killer.

Notez que cette URL doit être créée et diffusée par le composant d’image réactive. Il ne s’agit pas d’une fonctionnalité AEM prête à l’emploi.

C’est le concept de base. Il y a cependant quelques détails qui peuvent facilement être négligés.

Dans notre exemple, le composant a été rendu et mis en cache à 23 h 59. Admettons maintenant que l’image ait été modifiée à 00 h 00.  Le composant _pourrait_ générer une nouvelle empreinte d’URL dans son balisage.

Vous pourriez penser qu’il _devrait_ le faire, mais ce n’est pas le cas. Comme seul le fichier binaire de l’image a été modifié et que la page incluse n’a pas été touchée, le rendu du balisage HTML n’est pas nécessaire. Ainsi, Dispatcher diffuse la page avec l’ancienne empreinte, et donc l’ancienne version de l’image.

![Composant d’image plus récent que l’image référencée, aucune nouvelle empreinte n’est rendue.](assets/chapter-1/recent-image-component.png)

*Composant d’image plus récent que l’image référencée, aucune nouvelle empreinte n’est rendue.*

<br> 

Désormais, si vous avez réactivé la page d’accueil (ou toute autre page de ce site), le fichier stat est mis à jour, Dispatcher considère l’obsolescence de home.html et effectue à nouveau le rendu avec une nouvelle empreinte dans le composant d’image.

Mais nous n’avons pas activé la page d’accueil, n’est-ce pas ? Et pourquoi devrions-nous activer une page que nous n’avons pas touchée ? De plus, nous n’avons peut-être pas les droits suffisants pour activer les pages, ou peut être que le workflow d’approbation est long et chronophage, ou que nous ne pouvons tout simplement pas le faire dans un court délai. Alors, que faire ?

#### Outil d’administration différée - Diminution des niveaux de fichier stat

>[!WARNING]
>
>Il s’agit d’un anti-modèle. Ne l’utilisez qu’à court terme pour gagner du temps et trouver une solution plus élaborée.

L’administration différée « _définit l’invalidation automatique des JPG et le niveau du fichier stat sur zéro, ce qui aide toujours avec les problèmes de cache de toutes sortes._ ». Vous trouverez ce conseil dans les forums technologiques, et cela vous aidera à résoudre votre problème d’invalidation.

Nous n’avons pas encore parlé du niveau du fichier stat. En fait, l’invalidation automatique ne fonctionne que pour les fichiers de la même sous-arborescence. Cependant, le problème est que les pages et les ressources ne se trouvent généralement pas dans la même sous-arborescence. Les pages se trouvent quelque part en dessous de `/content/mysite` tandis que les ressources se trouvent en dessous de `/content/dam`.

Le « niveau du fichier .stat » définit à quelle profondeur se trouvent les nœuds racine des sous-arborescences. Dans l’exemple ci-dessus, le niveau serait « 2 »(1=/contenu, 2=/monsite, gestion des ressources numériques).

L’idée de « réduire » le niveau du fichier .stat à 0 consiste à définir l’arborescence de contenu tout entier comme sous-arborescence seule et unique pour que les pages et les ressources vivent dans le même domaine d’invalidation automatique. Nous n’aurions donc qu’une grosse arborescence à ce niveau (au docroot « / »). Mais cela invalide automatiquement tous les sites sur le serveur chaque fois qu’un élément est publié - même sur des sites complètement indépendants. Faites-nous confiance : c’est une mauvaise idée à long terme, parce que vous allez sérieusement dégrader votre taux d’accès au cache global. Tout ce que vous pouvez faire, c’est espérer que vos serveurs AEM disposent d’une puissance suffisante pour fonctionner sans cache.

Vous comprendrez tous les avantages d’un niveau de fichier .stat plus profond un peu plus tard.

#### Implémenter un agent d’invalidation personnalisé

Quoi qu’il en soit, nous devons indiquer au Dispatcher d’une manière ou d’une autre, afin d’invalider les pages de HTML, si un fichier « .jpg » ou « .png » a été modifié pour permettre le rendu avec une nouvelle URL.

Ce que nous avons vu dans les projets est, par exemple, des agents de réplication spéciaux sur le système de publication qui envoient des demandes d’invalidation pour un site à chaque fois qu’une image de ce site est publiée.

Cela peut être utile ici si vous pouvez dériver le chemin d’accès du site du chemin d’accès de la ressource en utilisant la convention de nommage.

En règle générale, il est préférable de faire correspondre les sites et les chemins d’accès aux ressources comme suit :

**Exemple**

```
/content/dam/site-a
/content/dam/site-b

/content/site-a
/content/site-b
```

Ainsi, votre agent de purge du Dispatcher personnalisé peut facilement envoyer une demande d’invalidation à /content/site-a lorsqu’il rencontre une modification sur `/content/dam/site-a`.

En fait, peu importe le chemin que vous demandez à Dispatcher d’invalider, tant qu’il se trouve sur le même site, dans la même « sous-arborescence ». Vous n’avez même pas besoin d’utiliser un véritable chemin d’accès aux ressources. Il peut également être « virtuel » :

```
GET /dispatcher-invalidate
Invalidate-path /content/mysite/dummy
```

![](assets/chapter-1/resource-path.png)

1. Un listener sur le système de publication est déclenché lorsqu’un fichier dans la gestion des ressources numériques est modifié.

2. Le listener envoie une demande d’invalidation à Dispatcher. En raison de l’invalidation automatique, le chemin que nous envoyons dans l’invalidation automatique importe peu, sauf s’il se trouve sous la page d’accueil du site, ou plus précisément au niveau du fichier .stat du site.

3. Le fichier .stat est mis à jour.

4. La prochaine fois que la page d’accueil sera demandée, elle sera de nouveau rendue. La nouvelle empreinte/date est extraite de la dernière modification de la propriété de l’image en tant que sélecteur supplémentaire.

5. Cela crée implicitement une référence à une nouvelle image.

6. Si l’image est réellement demandée, un nouveau rendu est créé et stocké dans Dispatcher.


#### La nécessité du nettoyage

Ouf. Terminé. Hourra.

Bon... pas encore tout à fait.

Le chemin d’accès

`/content/mysite/home/jcr:content/par/respi.img.fp-2018-31-12-23-59.jpg`

ne correspond à aucune des ressources invalidées. Mémoriser ? Nous avons uniquement invalidé une ressource « factice » et nous avons compté sur l’invalidation automatique pour considérer que « Accueil » est non valide. L’image elle-même pourrait ne jamais être _physiquement_ supprimée. Ainsi, le cache va grandir de plus en plus. Lorsque les images sont modifiées et activées, elles obtiennent de nouveaux noms de fichier dans le système de fichiers de Dispatcher.

Il existe trois problèmes liés à la non-suppression physique des fichiers mis en cache et à leur conservation indéfinie :

1. Vous gaspillez de la capacité de stockage, c’est évident. Admettons, le stockage est devenu de moins en moins cher ces dernières années. Mais la résolution des images et la taille des fichiers ont également augmenté ces dernières années avec l’avènement des affichages Retina, avides d’images parfaitement nettes.

2. Même si les disques durs sont devenus moins chers, le « stockage », lui, n’est peut-être pas devenu moins cher. Nous avons constaté une tendance à rejeter le stockage sur disque dur métallique HDD (bon marché) et à privilégier le stockage virtuel en location sur un NAS fourni par votre fournisseur de centre de données. Ce type de stockage est un peu plus fiable et évolutif mais aussi un peu plus cher. Il se peut que vous ne vouliez pas le gaspiller en y enregistrant des données inutiles. Cela ne concerne pas seulement le stockage principal, mais aussi les sauvegardes. Si vous disposez d’une solution de sauvegarde prête à l’emploi, vous ne pourrez peut-être pas exclure les répertoires de cache. En fin de compte, vous sauvegardez également des données inutiles.

3. Pire encore, vous avez peut-être acheté des licences d’utilisation pour certaines images pour une durée limitée, aussi longtemps que vous en aviez besoin. Si vous stockez toujours l’image après l’expiration d’une licence, cela peut être considéré comme une violation de copyright. Vous pouvez ne plus utiliser l’image dans vos pages web, mais Google les trouvera toujours.

Vous allez donc lancer une tâche cron pour nettoyer tous les fichiers datant de plus d’une semaine, par exemple, pour garder ce genre de fichiers sous contrôle.

#### Abus des empreintes d’URL pour les attaques par déni de service

Mais attendez, il y a un autre défaut dans cette solution :

Nous abusons en quelque sorte d’un sélecteur comme paramètre : fp-2018-31-12-23-59 est généré dynamiquement comme une sorte de « cache-killer ». Mais peut-être qu’un jeune qui s’ennuie (ou un moteur de recherche qui s’est déchaîné) commence à demander les pages :

```
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-00.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-01.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-02.jpg

…
```

Chaque requête contourne le Dispatcher, ce qui entraîne une charge sur une instance de publication. Et, pire encore, la création d’un fichier conforme sur Dispatcher.

Donc, au lieu de d’utiliser l’empreinte comme simple cache-killer, vous devez vérifier la date jcr:lastModified de l’image et renvoyer une erreur 404 si ce n’est pas la date attendue. Cela prend du temps et des cycles de processeur sur le système de publication... ce que vous souhaitez éviter.

#### Avertissements d’empreintes d’URL dans les versions haute fréquence

Vous pouvez utiliser le schéma d’empreinte non seulement pour les ressources provenant de la gestion des ressources numériques, mais également pour les fichiers JS et CSS et les ressources associées.

[Versioned ClientLibs](https://adobe-consulting-services.github.io/acs-aem-commons/features/versioned-clientlibs/index.html) est un module qui utilise cette approche.

Toutefois, les empreintes d’URL présentent un autre inconvénient : elles lient les URL au contenu. Vous ne pouvez pas modifier le contenu sans modifier également l’URL (c’est-à-dire mettre à jour la date de modification). C’est pour cela que les empreintes ont été conçues. Mais prenez en compte que vous déployez une nouvelle version, avec de nouveaux fichiers CSS et JS, et donc de nouvelles URL avec de nouvelles empreintes. Toutes vos pages HTML contiennent toujours des références aux anciennes empreintes d’URL. Ainsi, pour que la nouvelle version fonctionne de manière cohérente, vous devez invalider toutes les pages HTML en même temps afin de forcer un nouveau rendu avec des références aux fichiers avec de nouvelles empreintes. Si plusieurs sites reposent sur les mêmes bibliothèques, cela peut représenter un nombre considérable de rendus. Dans ce cas, vous ne pouvez pas utiliser les `statfiles`. Attendez-vous à observer des pics de charge sur vos systèmes de publication après un déploiement. Vous pouvez envisager un déploiement bleu/vert avec un préchauffage du cache ou un cache basé sur TTL devant votre Dispatcher... les possibilités sont infinies.

#### Une courte pause

Waouh, il y a beaucoup de choses à prendre en compte, n’est-ce pas ? Et il refuse d’être compris, testé et débogué facilement. Et tout ça pour une solution apparemment élégante. C’est vrai qu’elle est élégante, mais uniquement du point de vue d’AEM. Avec le Dispatcher, elle est beaucoup moins élégante.

Et pourtant, cela ne résout pas le principal inconvénient : si une image est utilisée plusieurs fois sur différentes pages, elle est mise en cache sous ces pages. Il n’y a pas beaucoup de synergie au niveau du cache.

En général, l’empreinte d’URL est un bon outil à utiliser dans votre boîte à outils, mais vous devez l’appliquer avec précaution, car cela peut entraîner de nouveaux problèmes tout en n’en résolvant que quelques uns.

C’était un long chapitre. Mais nous avons vu ce modèle si souvent, que nous avons pensé qu’il était nécessaire de vous donner une vue d’ensemble avec tous les avantages et les inconvénients. Les empreintes d’URL permettent de résoudre certains problèmes inhérents au modèle de spooler, mais la mise en œuvre requiert beaucoup de travail et vous devez également envisager d’autres solutions plus simples. Il est conseillé de toujours vérifier si vous pouvez baser vos URL sur les chemins de ressources diffusés et ne pas avoir de composant intermédiaire. Nous y reviendrons dans le prochain chapitre.

##### Résolution des dépendances d’exécution

La résolution des dépendances d’exécution est un concept que nous avons envisagé pour un projet. Mais après réflexion, c’était assez complexe et nous avons décidé de ne pas la mettre en œuvre.

Voici l’idée de base :

Dispatcher ne connaît pas les dépendances des ressources. C’est juste un tas de fichiers uniques avec peu de sémantique.

AEM ne sait pas non plus grand chose des dépendances. Il manque une sémantique propre ou un « suivi de dépendance ».

AEM connaît certaines références. Il utilise ces connaissances pour vous avertir lorsque vous essayez de supprimer ou de déplacer une page ou une ressource référencée. Pour ce faire, il interroge la recherche interne lors de la suppression d’une ressource. Les références au contenu ont une forme très particulière. Il s’agit d’expressions de chemin commençant par « /content ». Elles peuvent donc facilement être indexées en texte intégral, et interrogées lorsque cela est nécessaire.

Dans notre cas, nous aurions besoin d’un agent de réplication personnalisé sur le système de publication, qui déclenche une recherche pour un chemin spécifique lorsque ce chemin a changé.

Disons

`/content/dam/flower.jpg`

A changé lors de la publication. L’agent déclenche une recherche pour « /content/dam/flower.jpg » et trouve toutes les pages référençant ces images.

Il peut ensuite envoyer un certain nombre de demandes d’invalidation au Dispatcher. Une pour chaque page contenant la ressource.

En théorie, cela devrait marcher. Mais uniquement pour les dépendances de premier niveau. Il ne faut pas appliquer cette méthode aux dépendances à plusieurs niveaux, par exemple lorsque vous utilisez l’image sur un fragment d’expérience utilisé sur une page. En fait, nous pensons que cette approche est trop complexe, et qu’il pourrait y avoir des problèmes d’exécution. Et en général, le meilleur conseil est de ne pas faire de calcul coûteux dans les gestionnaires d’événements. Et les recherches, surtout, peuvent devenir très coûteuses.

##### Conclusion

Nous espérons que nous avons suffisamment discuté du modèle de spooler pour vous aider à décider quand l’utiliser et ne pas l’utiliser dans votre mise en œuvre.

## Éviter les problèmes du Dispatcher

### URL basées sur des ressources

Une manière bien plus élégante de résoudre le problème de la dépendance est de ne pas avoir de dépendances du tout. Évitez les dépendances artificielles qui se produisent lors de l’utilisation d’une ressource pour en remplacer une autre, comme nous l’avons fait dans le dernier exemple. Essayez de voir les ressources comme des entités « solitaires » aussi souvent que possible.

Notre exemple est facile à résoudre :

![Spooling de l’image avec un servlet lié à l’image, et non au composant.](assets/chapter-1/spooling-image.png)

*Spooling de l’image avec un servlet lié à l’image, et non au composant.*

<br> 

Nous utilisons les chemins d’accès aux ressources d’origine pour effectuer le rendu des données. Si nous avons besoin de rendre l’image d’origine telle quelle, nous pouvons simplement utiliser le rendu par défaut AEM pour les ressources.

Si nous devons effectuer un traitement spécial pour un composant spécifique, nous enregistrons un servlet dédié sur ce chemin et un sélecteur pour réaliser la transformation pour le compte du composant. Nous l’avons fait ici à titre d’exemple avec le sélecteur « .respi. » . Il est conseillé de maintenir un suivi des noms des sélecteurs utilisés dans l’espace URL global (comme par exemple `/content/dam`) et d’avoir une bonne convention de nommage pour éviter les conflits d’affectation des noms.

Au fait, nous ne voyons aucun problème de cohérence du code. Le servlet peut être défini dans le même package Java que le modèle sling des composants.

Nous pouvons même utiliser des sélecteurs supplémentaires dans l’espace global, tels que :

`/content/dam/flower.respi.thumbnail.jpg`

Facile, n’est-ce pas ? Alors pourquoi des personnes trouvent-elles des motifs compliqués comme le spooler ?

Eh bien, nous pourrions résoudre le problème en évitant la référence de contenu interne parce que le composant externe ajoutait peu de valeur ou d’informations au rendu de la ressource interne, qu’il pouvait facilement être encodé dans un ensemble de sélecteurs statiques qui contrôlent la représentation d’une ressource solitaire.

Mais il existe une catégorie de cas que vous ne pouvez pas résoudre facilement avec une URL basée sur des ressources. Nous appelons ce type de cas « Composants d’injection de paramètre », et nous en discutons dans le chapitre suivant.

### Composants d’injection de paramètre

#### Vue d’ensemble

Le spooler du dernier chapitre n’était qu’un mince wrapper autour d’une ressource. Plûtot que de résoudre le problème, il en a causé davantage.

Nous pourrions facilement remplacer ce wrapper en utilisant un sélecteur simple et en ajoutant un servlet adéquat pour répondre à de telles requêtes.

Mais que se passe-t-il si le composant « respi » est plus qu’un simple proxy ? Que se passe-t-il si le composant contribue véritablement au rendu du composant ?

Introduisons une petite extension de notre composant « respi » qui change un peu la donne. Encore une fois, nous allons d’abord introduire des solutions naïves pour relever les nouveaux défis et montrer où elles ne sont pas à la hauteur.

#### Composant respi2

Le composant respi2 est un composant qui affiche une image réactive, tout comme le composant respi. Mais il ajoute une valeur supplémentaire,

![Structure CRX : le composant respi2 ajoute une propriété de qualité à la diffusion](assets/chapter-1/respi2.png).

*Structure CRX : le composant respi2 ajoute une propriété de qualité à la diffusion*.

<br> 

Les images sont au format JPEG et peuvent être compressées. Lorsque vous compressez une image JPEG, vous diminuez la qualité au profit d’une taille de fichier moindre. La compression est définie comme un paramètre numérique de « qualité » compris entre 1 et 100. 1 signifie « petite taille mais de piètre qualité » tandis que 100 indique une « qualité optimale mais un fichier volumineux ». Où se situe le juste milieu ?

Comme dans toutes les domaines informatique, la réponse est : ça dépend.

Dans le cas présent, cela dépend du motif de l’image. Les motifs aux contours contrastés comme le texte écrit, les photos de bâtiments, les illustrations, les croquis ou les photos de boîtes de produits (avec des contours nets et du texte écrit dessus) entrent généralement dans cette catégorie. Les motifs présentant des transitions de couleur et de contraste plus douces, comme les paysages ou les portraits, peuvent être compressés un peu plus sans perte de qualité visible. Les photos de nature entrent généralement dans cette catégorie.

En outre, selon l’endroit où l’image sera affichée, vous pouvez utiliser un autre paramètre. Une petite miniature dans un teaser peut supporter une meilleure compression que celle affichée dans la bannière principale, qui occupe toute la largeur de l’écran. Ainsi, le paramètre de qualité ne dépend pas que de l’image, mais de l’image et du contexte. Et aux goûts et couleurs de l’auteur ou de l’autrice.

En bref : le paramètre magique et unique pour toutes les images n’existe pas. Chaque image aura une valeur différente. La personne en charge de la création a le dernier mot. Elle doit ajuster le paramètre de qualité en tant que propriété dans le composant jusqu’à ce que le compromis entre la qualité et la bande passante lui donne entière satisfaction.

Nous disposons désormais d’un fichier binaire dans la gestion des ressources numériques et d’un composant qui fournit une propriété de qualité. À quoi ressemble l’URL ? Quel composant est responsable de la mise en file d’attente ?

#### Approche naïve 1 : transmettre des propriétés en tant que paramètres de requête

>[!WARNING]
>
>Il s’agit d’un anti-modèle. Évitez-la.

Dans le chapitre précédent, l’URL de l’image générée par le composant ressemblait à ceci :

`/content/dam/flower.respi.jpg`

Seule la valeur de qualité est manquante. Le composant sait quelle propriété est saisie par l’auteur ou par l’autrice... Elle peut facilement être transmise au servlet de rendu d’image en tant que paramètre de requête lorsque le balisage est rendu, comme `flower.respi2.jpg?quality=60` :

```plain
  <div class="respi2">
  <picture>
    <source src="/content/dam/flower.respi2.jpg?quality=60" …/>
    …
  </picture>
  </div>
  …
```

C’est une mauvaise idée. Mémoriser ? Les requêtes avec des paramètres de requête ne peuvent pas être mises en cache.

#### Approche naïve 2 : transmettre des informations supplémentaires à l’aide du sélecteur

>[!WARNING]
>
>Cela peut devenir un anti-modèle. Redoublez de prudence si vous l’utilisez.

![Transmission des propriétés de composants en tant que sélecteurs](assets/chapter-1/passing-component-properties.png).

*Transmission des propriétés de composants en tant que sélecteurs*.

<br> 

Il s’agit d’une légère variation de la dernière URL. Nous utilisons seulement cette fois un sélecteur pour transmettre la propriété au servlet, de sorte que le résultat puisse être mis en cache :

`/content/dam/flower.respi.q-60.jpg`

C’est beaucoup mieux, mais vous vous souvenez de ce jeune aux mauvaises intentions du dernier chapitre qui détecte de tels schémas ? Il verrait jusqu’où il peut aller en parcourant des valeurs en boucle :

```plain
  /content/dam/flower.respi.q-60.jpg
  /content/dam/flower.respi.q-61.jpg
  /content/dam/flower.respi.q-62.jpg
  /content/dam/flower.respi.q-63.jpg
  …
```

Cette fois encore, le cache est contourné et la charge est créée sur le système de publication. C’est sans doute une mauvaise idée. Vous pouvez atténuer ce problème en filtrant uniquement un petit sous-ensemble de paramètres. Vous souhaitez autoriser uniquement `q-20, q-40, q-60, q-80, q-100`.

#### Filtrer les requêtes non valides lors de l’utilisation de sélecteurs

La réduction du nombre de sélecteurs était une bonne initiative. En règle générale, vous devez toujours limiter le nombre de paramètres valides à un minimum absolu. Si vous le faites intelligemment, vous pouvez même utiliser un pare-feu d’application web en dehors d’AEM en utilisant un ensemble statique de filtres sans connaissance approfondie du système AEM sous-jacent pour protéger vos systèmes :

```
Allow: /content/dam/(-\_/a-z0-9)+/(-\_a-z0-9)+
       \.respi\.q-(20|40|60|80|100)\.jpg
```

Si vous ne disposez pas d’un pare-feu d’application web, vous devez filtrer dans le Dispatcher ou dans AEM. Si vous le faites dans AEM, assurez-vous que :

1. le filtre est implémenté de manière très efficace, sans accéder trop souvent à CRX et perdre de la mémoire et du temps ;

2. le filtre répond à un message d’erreur « 404 - Introuvable ».

Revenons sur le dernier point. La conversation HTTP ressemblerait à ceci :

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 404 – Not found
  << empty response body >>
```

Nous avons également vu des implémentations qui filtraient des paramètres non valides mais renvoyaient un rendu de secours valide lorsqu’un paramètre non valide est utilisé. Supposons que nous n’autorisions que les paramètres compris entre 20 et 100. Les valeurs intermédiaires sont mappées aux valeurs valides. De ce fait,

`q-41, q-42, q-43, …`

répondrait toujours la même image, car q-40 aurait :

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 200 – OK
  << flower.jpg with quality = 40 >>
```

Cette approche n’est d’aucune utilité. Ces requêtes sont en réalité des requêtes valides.  Elles consomment de la puissance de traitement et occupent de l’espace dans le répertoire du cache du Dispatcher.

Mieux vaut retourner à `301 – Moved permanently` :

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 301 – Moved permanently
  Location: /content/dam/flower.respi.q-40.jpg
```

Voici ce qu’AEM indique au navigateur. « Je n’ai pas `q-41`. Mais, vous pouvez me poser des questions sur `q-40`. »

Cela ajoute une boucle de requête-réponse supplémentaire à la conversation, ce qui représente une certaine surcharge, mais c’est moins coûteux que d’effectuer le traitement complet sur `q-41`. Vous pouvez également exploiter le fichier déjà mis en cache sous `q-40`. Cependant, vous devez comprendre que les réponses 302 ne sont pas mises en cache dans le Dispatcher. Il s’agit de la logique exécutée dans AEM. Encore et encore. Vous avez donc intérêt à ce que cela soit léger et rapide.

C’est la réponse 404 qui nous plaît le plus. Elle permet de savoir ce qui se passe. Et vous aide à détecter les erreurs sur votre site web lorsque vous analysez des fichiers journaux. Les réponses 301 peuvent être intentionnelles, alors que les réponses 404 doivent toujours être analysées et éliminées.

## Sécurité - Excursion

### Filtrer des requêtes

#### Où filtrer le mieux

À la fin du dernier chapitre, nous avons souligné la nécessité de filtrer le trafic entrant pour les sélecteurs connus. Cela nous amène à la question : où dois-je réellement filtrer les requêtes ?

Eh bien, cela dépend. Le plus tôt sera le mieux.

#### Pare-feu d’application web

Si vous disposez d’un outil de pare-feu d’application web conçu pour la sécurité web, vous devez absolument exploiter ces fonctionnalités. Mais vous pourriez découvrir que le pare-feu d’application web est géré par des personnes qui ne connaissent que peu votre application de contenu et qui filtrent des requêtes valides ou laissent passer trop de requêtes dangereuses. Vous découvrirez peut-être que les personnes qui gèrent le pare-feu d’application web sont affectées à un autre service avec des horaires et des plannings de publication différents, que la communication n’est peut être pas aussi étroite qu’avec les personnes membres directes de votre équipe et que vous ne recevez pas toujours les modifications à temps, ce qui signifie finalement que votre développement et la vitesse de contenu en pâtissent.

Vous pourriez vous retrouver avec quelques règles générales ou même une liste bloquée qui, selon votre intuition, pourraient être renforcées.

#### Filtrer le Dispatcher et l’instance de publication

L’étape suivante consiste à ajouter des règles de filtrage d’URL dans le noyau Apache et/ou dans le Dispatcher.

Ici, vous avez accès aux URL uniquement. Vous subissez la limite des filtres basés sur des modèles. Si vous devez configurer un filtrage basé sur le contenu (par exemple, autoriser les fichiers uniquement avec une heure et une date correctes) ou si vous souhaitez que le filtrage soit contrôlé sur votre instance de création, vous finirez par écrire quelque chose comme un filtre de servlet personnalisé.

#### Surveillance et débogage

En pratique, vous disposez d’une certaine sécurité à chaque niveau. Veillez toutefois à disposer des moyens nécessaires pour savoir à quel niveau une requête est filtrée. Assurez-vous d’avoir un accès direct au système de publication, au Dispatcher et aux fichiers journaux sur le pare-feu d’application web pour savoir quel filtre de la chaîne bloque les requêtes.

### Sélecteurs et prolifération de sélecteurs

L’approche utilisant « selector-parameters » dans le dernier chapitre est rapide et facile et peut réduire le temps de développement des nouveaux composants, mais elle a des limites.

La définition d’une propriété « qualité » n’est qu’un exemple simple. Mais supposons que le servlet attende également des paramètres de « largeur » plus polyvalents.

Vous pouvez réduire le nombre d’URL valides en réduisant le nombre de valeurs de sélecteur possibles. Vous pouvez faire de même avec la largeur :

qualité = q-20, q-40, q-60, q-80, q-100

largeur = w-100, w-200, w-400, w-800, w-1000, w-1200

Toutes les combinaisons sont désormais des URL valides :

```
/content/dam/flower.respi.q-40.w-200.jpg
/content/dam/flower.respi.q-60.w-400.jpg
…
```

Désormais, nous disposons déjà de 5x6 = 30 URL valides pour une ressource. Chaque propriété supplémentaire rajoute de la complexité. Et il peut y avoir des propriétés qui ne peuvent pas être ramenées à une quantité raisonnable de valeurs.

Cette approche a donc aussi des limites.

#### Exposition accidentelle d’une API

Que se passe-t-il ici ? Si nous regardons attentivement, nous voyons que nous passons progressivement d’un rendu statique à un site web très dynamique. Et nous affichons par inadvertance une API de rendu d’image sur le navigateur du client ou de la cliente qui était en fait destinée à être utilisée uniquement par les créateurs et créatrices.

La définition de la qualité et de la taille d’une image doit être effectuée par le créateur ou la créatrice qui modifie la page. Le fait d’avoir les mêmes fonctionnalités exposées par un servlet peut être considéré comme une fonctionnalité ou comme un vecteur d’attaque par déni de service. Mais cela dépend du contexte. Quelle est l’importance du site web pour l’entreprise ? Quelle charge y a-t-il sur les serveurs ? Quelle est la marge de manœuvre ? Combien de budget avez-vous pour l’implémentation ? Vous devez trouver un équilibre entre ces facteurs. Vous devez connaître les avantages et les inconvénients.

## Modèle du spooler - revisité et réhabilité

### Comment le spooler évite d’exposer l’API

Nous avons en quelque sorte discrédité le modèle du spooler dans le dernier chapitre. Il est temps de le réhabiliter.

![](assets/chapter-1/spooler-pattern.png)

Le modèle du spooler permet d’éviter le problème de l’exposition d’une API dont nous avons parlé dans le dernier chapitre. Les propriétés sont stockées et encapsulées dans le composant. Tout ce dont nous avons besoin pour accéder à ces propriétés est le chemin d’accès au composant. Il n’est pas nécessaire d’utiliser l’URL comme véhicule pour transmettre les paramètres entre le balisage et le rendu binaire :

1. Le client effectue le rendu du balisage HTML lorsque le composant est demandé dans la boucle de demande principale.

2. Le chemin d’accès au composant sert de référence au composant à partir du balisage.

3. Le navigateur utilise cette référence pour demander le fichier binaire.

4. Lorsque la demande atteint le composant, nous disposons de toutes les propriétés pour redimensionner, compresser et spooler les données binaires.

5. L’image est transmise par l’intermédiaire du composant au navigateur client.

Après tout, le modèle du spooler n’est pas si mal, c’est pour ça qu’il est si populaire. Si seulement il n’était pas aussi fastidieux au niveau de l’invalidation du cache...

### Spooler inversé : le meilleur des deux mondes ?

Cela nous amène à la question suivante. Pourquoi ne pouvons-nous pas simplement avoir le meilleur des deux mondes ? La bonne encapsulation du modèle du spooler et les propriétés de mise en cache intéressantes d’une URL basée sur les ressources ?

Nous devons admettre que nous n’avons jamais vu ça dans un vrai projet. Mais tentons un petit exercice, qui sera le point de départ pour votre solution.

Nous appellerons ce modèle _Spooler inversé_. Le spooler inversé doit être basé sur la ressource images pour disposer de toutes les propriétés d’invalidation du cache intéressantes.

Mais il ne doit exposer aucun paramètre. Toutes les propriétés doivent être encapsulées dans le composant. Mais nous pouvons exposer le chemin d’accès aux composants, en tant que référence opaque aux propriétés.

Cela conduit à une URL sous la forme :

`/content/dam/flower.respi3.content-mysite-home-jcrcontent-par-respi.jpg`

`/content/dam/flower` est le chemin d’accès à la ressource de l’image.

`.respi3` est un sélecteur permettant de choisir le bon servlet pour diffuser l’image.

`.content-mysite-home-jcrcontent-par-respi` est un sélecteur supplémentaire. Il code le chemin d’accès au composant qui stocke la propriété nécessaire à la transformation d’image. Les sélecteurs sont limités à une plus petite plage de caractères que les chemins d’accès. Le schéma de codage est ici donné uniquement à titre d’exemple. Il remplace « / » par « - ». Cela ne prend pas en compte le fait que le chemin d’accès lui-même peut également contenir « - ». Un système de codage plus sophistiqué serait conseillé dans un exemple concret. Base64 devrait convenir. Mais cela rend le débogage un peu plus difficile.

`.jpg` est le suffixe de fichier.

### Conclusion

Waouh... la discussion sur le spooler est devenue plus longue et plus compliquée que prévu. Nous vous devons des excuses. Mais nous avons estimé nécessaire de vous présenter une multitude d’aspects, bons et mauvais, afin que vous puissiez développer un certain mode de réflexion sur ce qui fonctionne bien et pas du tout dans l’univers du Dispatcher.

## Fichier stat et niveau du fichier stat

### Principes élémentaires

#### Présentation

Nous avons déjà brièvement mentionné le _fichier stat_ avant. Il est lié à l’invalidation automatique :

Tous les fichiers de cache du système de fichiers de Dispatcher configurés pour être invalidés automatiquement sont considérés comme non valides si leur date de dernière modification est antérieure à celle du `statfile's`.

>[!NOTE]
>
>La date de dernière modification dont nous parlons est celle du fichier mis en cache, c’est la date à laquelle le fichier a été demandé par le navigateur du client ou de la cliente et finalement créé dans le système de fichiers. Il ne s’agit pas de la date de `jcr:lastModified` de la ressource.

La date de la dernière modification du fichier stat (`.stat`) est la date à laquelle la demande d’invalidation d’AEM a été reçue sur Dispatcher.

Si vous disposez de plusieurs instances de Dispatcher, cela peut entraîner des effets étranges. Votre navigateur peut avoir une version plus récente d’un Dispatcher (si vous en avez plusieurs). Ou un Dispatcher peut penser que la version du navigateur émise par l’autre Dispatcher est obsolète et envoie inutilement une nouvelle copie. Ces effets n’ont pas d’impact significatif sur les performances ou les exigences fonctionnelles. Et ils se stabiliseront au fil du temps, lorsque le navigateur aura la dernière version. Cependant, cela peut être un peu déroutant lorsque vous optimisez et déboguez le comportement de mise en cache du navigateur. C’est donc quelque chose à garder à l’esprit.

#### Configurer des domaines d’invalidation avec /statfileslevel

Quand nous avons introduit l’invalidation automatique et le fichier stat, nous avons dit que *tous* les fichiers sont considérés comme non valides en cas de modification et que tous les fichiers sont interdépendants de toute façon.

Ce n’est pas tout à fait exact. En règle générale, tous les fichiers qui partagent une racine de navigation principale commune sont interdépendants. Mais une instance d’AEM peut héberger un certain nombre de sites web, et ces sites web sont *indépendants*. Ils ne partagent pas une navigation commune - en fait, ils ne partagent rien.

Ne serait-ce pas inutile d’invalider le site B parce qu’il y a un changement dans le site A ? Si. Et il n’en a pas à être ainsi.

Le Dispatcher offre un moyen simple de séparer les sites les uns des autres : le `statfiles-level`.

Il s’agit d’un nombre qui définit à partir de quel niveau dans le système de fichiers deux sous-arborescences sont considérées comme « indépendantes ».

Examinons le cas par défaut où le niveau statfileslevel est 0.

![/statfileslevel 0 : le fichier _.stat_ est créé dans le docroot. Le domaine d’invalidation couvre toute l’installation, y compris tous les sites.](assets/chapter-1/statfile-level-0.png)

`/statfileslevel "0":` Le fichier `.stat` est créé dans le docroot. Le domaine d’invalidation couvre toute l’installation, y compris tous les sites.

Quel que soit le fichier invalidé, le fichier `.stat` situé tout en haut du docroot de Dispatcher est toujours mis à jour. Ainsi, lorsque vous invalidez `/content/site-b/home`, tous les fichiers dans `/content/site-a` sont également invalidés, car ils sont désormais plus anciens que le fichier `.stat` dans le docroot. Ce n’est pas ce dont vous avez besoin lorsque vous invalidez `site-b`.

Dans cet exemple, il vaut mieux définir `statfileslevel` sur `1`.

Si vous publiez, et donc invalidez `/content/site-b/home` ou toute autre ressource sous `/content/site-b`, le fichier `.stat` est créé à l’emplacement `/content/site-b/`.

Le contenu sous `/content/site-a/` n’est pas affecté. Ce contenu serait comparé au fichier `.stat` à l’emplacement `/content/site-a/`. Nous avons créé deux domaines d’invalidation distincts.

![Un statfileslevel 1 crée différents domaines d’invalidation.](assets/chapter-1/statfiles-level-1.png)

*Un statfileslevel 1 crée différents domaines d’invalidation.*

<br> 

Les grandes installations sont généralement structurées de manière un peu plus complexe. Les sites sont généralement structurés par marque, pays et langue. Dans ce cas, vous pouvez définir le statfileslevel encore plus haut. _1_ créerait des domaines d’invalidation par marque, _2_ par pays et _3_ par langue.

### Nécessité d’une structure de site homogène

Le niveau statfileslevel est appliqué de la même manière à tous les sites de votre configuration. Par conséquent, tous les sites doivent suivre la même structure et commencer au même niveau.

Supposons que votre portfolio contienne des marques qui ne sont vendues que sur quelques petits marchés alors que d’autres sont vendues dans le monde entier. Les petits marchés ne sont concernés que par une seule langue tandis que sur le marché mondial, il y a des pays où l’on parle plusieurs langues :

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

Les petits marchés auraient besoin d’un `statfileslevel` _2_, tandis que le marché mondial nécessite un niveau _3_.

Ce n’est pas une situation idéale. Si vous le définissez sur _3_, l’invalidation automatique ne fonctionnera pas dans les sites plus petits entre les sous-branches `/home`, `/products` et `/about`.

Définir sur le niveau _2_ signifie que dans les plus grands sites, vous déclarez `/canada/en` et `/canada/fr` comme dépendantes, ce qu’elles ne sont peut-être pas. Ainsi, chaque invalidation dans `/en` invaliderait également `/fr`. Le taux d’accès au cache sera ainsi légèrement réduit, mais c’est toujours mieux que de fournir du contenu mis en cache obsolète.

La meilleure solution, bien sûr, est que les racines de tous les sites soient au même niveau :

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

Quel est le bon niveau ? Cela dépend du nombre de dépendances que vous avez entre les sites. Les inclusions que vous résolvez pour le rendu d’une page sont considérées comme des « dépendances dures ». Nous avons démontré ce type d’_inclusion_ lorsque nous avons introduit le composant _Teaser_ au début de ce guide.

Les _hyperliens_ sont une forme plus douce de dépendances. Il est très probable que vous créiez des hyperliens dans un site web, et il n’est pas improbable que vous ayez des liens entre vos sites web. Les hyperliens simples ne créent généralement pas de dépendances entre les sites web. Pensez simplement à un lien externe que vous définissez de votre site vers Facebook... Vous n’avez pas à effectuer le rendu de votre page si quelque chose change sur Facebook et vice versa, n’est-ce pas ?

Une dépendance se produit lorsque vous lisez le contenu de la ressource en lien (par exemple, le titre de navigation). Ces dépendances peuvent être évitées si vous utilisez uniquement les titres de navigation saisis localement et ne les tirez pas de la page cible (comme vous le feriez avec les liens externes).

#### Dépendance inattendue

Cependant, il peut y avoir une partie de votre configuration dans laquelle les sites, censés être indépendants, se rassemblent. Examinons un scénario réel que nous avons rencontré dans l’un de nos projets.

La personne cliente avait une structure de site comme celle présentée dans le dernier chapitre :

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

Chaque pays avait son propre domaine.

```
www.shiny-brand.ch

www.shiny-brand.fr

www.shiny-brand.de
```

Il n’existait aucun lien navigable entre les sites de langue et aucune inclusion apparente. Nous avons donc défini le niveau statfileslevel sur 3.

Tous les sites diffusaient essentiellement le même contenu. La seule grande différence était la langue.

Les moteurs de recherche tels que Google considèrent comme « trompeur » le même contenu sur différentes URL. Une personne peut essayer d’être mieux classée ou d’être répertoriée plus souvent en créant des batteries de serveurs proposant un contenu identique. Les moteurs de recherche reconnaissent ces tentatives et classent en fait plus bas les pages qui recyclent simplement le contenu.

Vous pouvez éviter un classement plus bas en indiquant de manière transparente que vous avez effectivement plusieurs pages avec le même contenu et que vous n’essayez pas de « jouer » avec le système (voir [« Informer Google des versions localisées de votre page »](https://support.google.com/webmasters/answer/189077?hl=fr)) en définissant des balises `<link rel="alternate">` vers chaque page associée dans la section d’en-tête de chaque page :

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

![Tout relier.](assets/chapter-1/inter-linking-all.png)

*Tout relier.*

<br> 

Certaines personnes expertes en SEO affirment même que cela pourrait transférer la réputation ou le « jus de lien » (link juice) d’un site web avec un bon classement dans une langue vers le même site web dans une autre langue.

Ce schéma a créé non seulement un certain nombre de liens, mais aussi quelques problèmes. Le nombre de liens requis pour _p_ dans _n_ langues est _p x (n<sup>2</sup>-n)_ : chaque page est associée à une autre page (_n x n_) sauf à elle-même (_-n_). Ce schéma s’applique à chaque page. Si nous avons un petit site de 20 pages en 4 langues, chacune équivaut à _240_ liens.

Tout d’abord, vous ne souhaitez pas qu’une personne chargée de l’édition ait à gérer ces liens manuellement. Ils doivent être générés automatiquement par le système.

Ils doivent également être précis. Chaque fois que le système détecte un nouvel « élément relatif », vous souhaitez le lier depuis toutes les autres pages qui ont le même contenu (mais dans une langue différente).

Dans notre projet, de nouvelles pages relatives s’affichaient fréquemment. Mais elles ne se matérialisaient pas sous forme de liens « alternatifs ». Par exemple, lorsque la page `de-de/produkte` a été publiée sur le site web allemand, elle n’était pas visible immédiatement sur les autres sites.

Cela venait du fait que, dans notre configuration, les sites étaient censés être indépendants. Une modification sur le site web allemand n’a donc pas déclenché d’invalidation sur le site web français.

Vous connaissez déjà une solution pour résoudre ce problème. Il vous suffit de réduire le statfileslevel au niveau 2 pour élargir le domaine d’invalidation. Bien sûr, cela réduit également le taux d’accès au cache, en particulier lors de publications. Des invalidations se produisent donc plus fréquemment.

Dans notre cas, c’était encore plus compliqué :

Même si nous avions le même contenu, les noms de marque et autres étaient différents dans chaque pays.

`shiny-brand` s’appelait `marque-brillant` en France et `blitzmarke` en Allemagne :

```
/content/marque-brillant/france/fr
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de
/content/blitzmarke/germany/de
…
```

Cela aurait impliqué de définir le niveau `statfiles` sur 1, ce qui aurait abouti à un domaine d’invalidation trop vaste.

La restructuration du site aurait résolu ce problème. La fusion de toutes les marques sous une racine commune. Mais nous n’avions pas la capacité à l’époque et cela nous aurait donné seulement un niveau 2.

Nous avons décidé de rester au niveau 3. En contrepartie, nous ne disposons pas toujours des derniers liens « alternatifs ». Pour atténuer ce problème, nous avions une tâche cron « reaper » qui s’exécutait sur Dispatcher et qui nettoyait de toute façon les fichiers de plus d’une semaine. Finalement, toutes les pages ont été rendues à nouveau à un moment donné. Mais il s’agit d’un compromis qui doit être décidé individuellement pour chaque projet.

## Conclusion

Nous avons abordé quelques principes de base sur le fonctionnement général de Dispatcher et nous vous avons donné quelques exemples pour lesquels vous devrez peut-être déployer quelques efforts de mise en œuvre supplémentaires pour bien faire les choses et pour lesquels vous souhaiterez peut-être faire des compromis.

Nous n’avons pas été trop loin dans les détails concernant la façon dont cela est configuré dans le Dispatcher. Nous voulions que vous compreniez d’abord les concepts et les problèmes de base, sans vous perdre trop tôt dans la console. Le travail de configuration réel est bien documenté. Si vous comprenez les concepts de base, vous devez savoir à quoi servent les différents commutateurs.

## Conseils et astuces pour Dispatcher

Nous conclurons la première partie de ce livre par une collection aléatoire de conseils et astuces qui pourraient être utiles dans une situation ou une autre. Comme nous l’avons fait auparavant, nous ne présentons pas la solution, mais plutôt l’idée afin que vous ayez l’occasion de comprendre l’idée et le concept, ainsi que des liens vers des articles décrivant la configuration réelle plus en détail.

### Durée d’invalidation correcte

Si vous installez une instance de création et de publication AEM prête à l’emploi, la topologie est un peu étrange. L’instance de création envoie simultanément le contenu aux systèmes de publication et la demande d’invalidation aux Dispatchers. Comme les systèmes de publication et le Dispatcher sont découplés de l’instance de création par des files d’attente, le timing peut être un peu malheureux. Le Dispatcher peut recevoir la demande d’invalidation de l’instance de création avant que le contenu ne soit mis à jour sur le système de publication.

Si un client ou une cliente demande ce contenu entre-temps, le Dispatcher demandera et stockera du contenu obsolète.

Une configuration plus fiable consiste à envoyer la demande d’invalidation depuis les systèmes de publication _après_ avoir reçu le contenu. L’article « [Invalidation du cache de Dispatcher depuis une instance de publication](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html?lang=fr#InvalidatingDispatcherCachefromaPublishingInstance) » décrit les détails.

**Références**

[helpx.adobe.com. Invalidation du cache de Dispatcher depuis une instance de publication](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html?lang=fr#InvalidatingDispatcherCachefromaPublishingInstance)

### Mettre en cache des en-têtes et en-têtes HTTP

Autrefois, le Dispatcher ne faisait que stocker des fichiers simples dans le système de fichiers. Si vous aviez besoin que les en-têtes HTTP soient transmis à la clientèle, vous procédiez en configurant Apache en fonction des informations limitées du fichier ou de l’emplacement. C’était particulièrement embêtant lorsque vous avez implémenté une application web dans AEM qui reposait principalement sur des en-têtes HTTP. Tout fonctionnait correctement dans l’instance AEM seule, mais pas lorsque vous utilisiez un Dispatcher.

En règle générale, vous commenciez à réappliquer les en-têtes manquants aux ressources du serveur Apache avec `mod_headers` en utilisant des informations que vous pouviez dériver du chemin et du suffixe des ressources. Mais cela n’était pas toujours suffisant.

Ce qui était particulièrement embêtant, c’est que même avec le Dispatcher, la première réponse _non mise en cache_ au navigateur provenait du système de publication avec un ensemble complet d’en-têtes, tandis que les réponses suivantes étaient générées par le Dispatcher avec un ensemble limité d’en-têtes.

À partir de la version 4.1.11 du Dispatcher, celui-ci peut stocker les en-têtes générés par les systèmes de publication.

Cela vous évite de dupliquer la logique d’en-tête dans le Dispatcher et libère toute la puissance d’expression de HTTP et d’AEM.

**Références**

* [helpx.adobe.com - Mise en cache des en-têtes de réponse](https://helpx.adobe.com/experience-manager/kb/dispatcher-cache-response-headers.html?lang=fr)

### Exceptions de mise en cache individuelle

Vous voulez peut-être mettre en cache toutes les pages et toutes les images en général, mais vous pouvez faire une exception dans certains cas. Par exemple, vous souhaitez mettre en cache les images PNG, mais pas les images PNG affichant un Captcha (qui doit changer à chaque demande). Le Dispatcher peut ne pas reconnaître un Captcha comme un Captcha… contrairement à AEM. Il peut demander au Dispatcher de ne pas mettre en cache cette requête en envoyant un en-tête conforme avec la réponse :

```plain
  response.setHeader("Dispatcher", "no-cache");

  response.setHeader("Cache-Control: no-cache");

  response.setHeader("Cache-Control: private");

  response.setHeader("Pragma: no-cache");
```

Cache-Control et Pragma sont des en-têtes HTTP officiels, propagés et interprétés par les couches de mise en cache supérieures, telles qu’un réseau CND. L’en-tête `Dispatcher` n’est qu’un indice pour que le Dispatcher ne mette pas en cache. Il peut être utilisé pour indiquer au Dispatcher de ne pas mettre en cache, tout en permettant aux couches de mise en cache supérieures de le faire. En réalite, il est difficile de trouver un cas où cela pourrait être utile. Mais nous savons que certains existent, quelque part.

**Références**

* [Dispatcher - Pas de cache](https://helpx.adobe.com/experience-manager/kb/DispatcherNoCache.html?lang=fr)

### Mise en cache du navigateur

La réponse HTTP la plus rapide est la réponse donnée par le navigateur lui-même. Lorsque la requête et la réponse n’ont pas à passer par un réseau vers un serveur web soumis à une charge élevée.

Vous pouvez aider le navigateur à décider quand demander au serveur une nouvelle version du fichier en définissant une date d’expiration pour une ressource.

En règle générale, vous effectuez cette opération de manière statique à l’aide de `mod_expires` d’Apache ou en stockant les en-têtes Cache-Control et Expires qui proviennent d’AEM si vous avez besoin d’un contrôle plus individuel.

Un document mis en cache dans le navigateur peut comporter trois niveaux de mise à jour.

1. _Garanti à jour_ : le navigateur peut utiliser le document mis en cache.

2. _Potentiellement obsolète_ : le navigateur doit d’abord demander au serveur si le document mis en cache est toujours à jour.

3. _Obsolète_ : le navigateur doit demander une nouvelle version au serveur.

La première est garantie par la date d’expiration définie par le serveur. Si une ressource n’a pas expiré, il n’est pas nécessaire de redemander au serveur.

Si le document a atteint sa date d’expiration, il peut toujours être à jour. La date d’expiration est définie lors de la diffusion du document. Mais souvent, vous ne savez pas à l’avance lorsqu’un nouveau contenu est disponible. Il ne s’agit donc que d’une estimation prudente.

Pour déterminer si le document dans la mémoire cache du navigateur est toujours le même que celui qui sera diffusé lors d’une nouvelle requête, le navigateur peut utiliser la date `Last-Modified` du document. Le navigateur demande au serveur :

« _J’ai une version du 10 juin…Ai-je besoin d’une mise à jour ?_ ». Et le serveur peut répondre par :

« _304 - Votre version est toujours à jour_ » sans retransmettre la ressource, ou le serveur peut répondre par :

« _200 - Voici une version plus récente_ » dans l’en-tête HTTP et le contenu plus récent réel dans le corps HTTP.

Pour que cette deuxième partie fonctionne, veillez à transmettre la date `Last-Modified` au navigateur afin qu’il dispose d’un point de référence pour demander des mises à jour.

Nous avons expliqué plus tôt que lorsque la date `Last-Modified` est générée par le Dispatcher, elle peut varier en fonction des différentes requêtes, car le fichier mis en cache (et sa date) sont générés lorsque le fichier est demandé par le navigateur. Une alternative serait d’utiliser des « ETags ». Il s’agit de nombres qui identifient le contenu réel (par exemple en générant un code de hachage) au lieu d’une date.

La « [prise en charge des ETags](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html) » du _package ACS Commons_ utilise cette approche. Cela s’accompagne toutefois d’un prix : comme l’ETag doit être envoyée sous forme d’en-tête, mais que le calcul du code de hachage nécessite une lecture complète de la réponse, celle-ci doit être entièrement mise en mémoire tampon dans la mémoire principale avant de pouvoir être diffusée. Cela peut avoir un impact négatif sur la latence lorsque votre site web est plus susceptible d’avoir des ressources non mises en cache et vous devez évidemment surveiller la mémoire consommée par votre système AEM.

Si vous utilisez des empreintes d’URL, vous pouvez définir des dates d’expiration très longues. Vous pouvez mettre en cache les ressources marquées pour toujours dans le navigateur. Une nouvelle version est marquée par une nouvelle URL et les anciennes versions n’ont jamais besoin d’être mises à jour.

Nous avons utilisé les empreintes d’URL lorsque nous avons introduit le modèle de spooler. Les fichiers statiques provenant de `/etc/design` (CSS, JS) sont rarement modifiés, ce qui en fait de bonnes options à utiliser comme empreintes.

Pour les fichiers standards, nous configurons généralement un schéma fixe, comme vérifier de nouveau le HTML toutes les 30 minutes, les images toutes les 4 heures, etc.

La mise en cache du navigateur est extrêmement utile sur le système de création. Il vous faut mettre en cache autant que possible dans le navigateur afin d’améliorer l’expérience de modification. Malheureusement, les ressources les plus coûteuses, les pages HTML, ne peuvent pas être mises en cache...elles sont censées changer fréquemment sur l’instance de création.

Les bibliothèques Granite, qui constituent interface utilisateur d’AEM, peuvent être mises en cache pendant un certain temps. Vous pouvez également mettre en cache les fichiers statiques de vos sites (polices, CSS et JavaScript) dans le navigateur. Même les images dans `/content/dam` peuvent généralement être mises en cache pendant environ 15 minutes, car elles ne sont pas modifiées aussi souvent que du texte de copie sur les pages. Les images ne sont pas modifiées de manière interactive dans AEM. Avant d’être chargées sur AEM, elles sont d’abord modifiées et approuvées. Par conséquent, vous pouvez supposer qu’elles ne changent pas aussi souvent que du texte.

La mise en cache des fichiers d’interface utilisateur, des fichiers de bibliothèque de sites et des images peut accélérer considérablement le rechargement des pages lorsque vous êtes en mode d’édition.



**Références**

*[developer.mozilla.org - Mise en cache](https://developer.mozilla.org/fr-FR/docs/Web/HTTP/Caching)

* [apache.org - Mod Expires](https://httpd.apache.org/docs/current/mod/mod_expires.html)

* [ACS Commons - Prise en charge des ETags](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)

### Tronquer des URL

Vos ressources sont stockées sous

`/content/brand/country/language/…`

Mais bien sûr, il ne s’agit pas de l’URL que vous souhaitez transmettre au client ou à la cliente. Pour des raisons esthétiques, de lisibilité et d’optimisation du moteur de recherche, vous pouvez tronquer la partie déjà représentée dans le nom de domaine.

Si vous avez un domaine,

`www.shiny-brand.fi`

il n’est généralement pas nécessaire de mettre la marque et le pays dans le chemin. À la place,

`www.shiny-brand.fi/content/shiny-brand/finland/fi/home.html`

vous voudriez avoir,

`www.shiny-brand.fi/home.html`

Vous devez implémenter ce mappage sur AEM, car AEM doit savoir comment effectuer le rendu des liens selon ce format tronqué.

Mais ne comptez pas uniquement sur AEM. Si vous le faites, vous aurez des chemins tels que `/home.html` dans le répertoire racine de votre cache. Maintenant, est-ce là l’« accueil » pour le site web finlandais, allemand ou canadien ? Et s’il existe un fichier `/home.html` dans Dispatcher, comment Dispatcher sait-il qu’il doit être invalidé lorsqu’une demande d’invalidation pour `/content/brand/fi/fi/home` entre ?

Nous avons vu un projet qui avait des docroots séparées pour chaque domaine. C’était un cauchemar à déboguer et à entretenir - et en fait, nous ne l’avons jamais vu fonctionner sans erreur.

Nous pourrions résoudre les problèmes en restructurant le cache. Nous avions une docroot unique pour tous les domaines, et les demandes d’invalidation pouvaient être traitées 1:1, car tous les fichiers sur le serveur commençaient par `/content`.

La partie tronquée était aussi très facile.  AEM a généré des liens tronqués en raison d’une configuration appropriée dans `/etc/map`.

Maintenant, lorsqu’une requête `/home.html` arrive au Dispatcher, la première chose qui se produit est l’application d’une règle de réécriture qui développe le chemin en interne.

Cette règle a été configurée de manière statique dans chaque configuration vhost. En d’autres termes, les règles ressemblaient à ça :

```plain
  # vhost www.shiny-brand.fi

  RewriteRule "^(.\*\.html)" "/content/shiny-brand/finland/fi/$1"
```

Dans le système de fichiers, nous avons maintenant des chemins basés sur `/content` simples, qui se trouvent également sur l’instance de création et de publication, ce qui a beaucoup facilité le débogage. Sans parler de l’invalidation correcte, qui n’était plus un problème.

Notez que nous l’avons fait uniquement pour les URL « visibles », qui s’affichent dans l’emplacement d’URL du navigateur. Les URL des images, par exemple, étaient toujours des URL « /content » pures. Nous pensons que l’embellissement de l’URL « principale » est suffisant en matière d’optimisation du moteur de recherche.

Avoir une docroot commune offrait également une autre fonctionnalité appréciable. En cas de problème dans Dispatcher, nous pouvions nettoyer l’ensemble du cache en l’exécutant,

`rm -rf /cache/dispatcher/*`

chose qu’il ne faut pas faire lorsque la charge est élevée.

**Références**

* [apache.org - Mod Rewrite](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)

* [helpx.adobe.com - Mappage des ressources](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/resource-mapping.html?lang=fr)

### Gestion des erreurs

Dans les cours AEM, vous apprenez à programmer un gestionnaire d’erreurs dans Sling. Ce n’est pas si différent de l’écriture d’un modèle habituel. Vous écrivez simplement un modèle en JSP ou HTL, n’est-ce pas ?

Oui, mais c’est la partie AEM uniquement. Rappel : Dispatcher ne met pas en cache les réponses `404 – not found` ou `500 – internal server error`.

Si vous effectuez dynamiquement le rendu de ces pages pour chaque requête (en échec), la charge sur les systèmes de publication sera importante et inutile.

Nous avons trouvé utile de ne pas afficher le rendu complet de la page d’erreur lorsqu’une erreur se produit, mais seulement une version très simplifiée et réduite, voire une version statique de cette page, sans aucun effet graphique ni logique.

Ce n’est bien sûr pas ce que le client ou la cliente a vu. Dans Dispatcher, nous avons enregistré `ErrorDocuments` comme ceci :

```
ErrorDocument 404 "/content/shiny-brand/fi/fi/edocs/error-404.html"
ErrorDocument 500 "/content/shiny-brand/fi/fi/edocs/error-500.html"
```

Désormais, le système AEM peut simplement notifier à Dispatcher que quelque chose ne va pas, et Dispatcher peut fournir une version attrayante et claire du document d’erreur.

Deux points sont à noter.

Tout d’abord, l’`error-404.html` est toujours la même page. Il n’existe donc pas de message individuel indiquant « Votre recherche pour « _produckten_ » n’a pas donné de résultat ». Nous pouvons facilement nous en accommoder.

Deuxièmement, si nous voyons une erreur de serveur interne, ou pire, si nous rencontrons une panne du système AEM, il n’y a aucun moyen de demander à AEM d’afficher le rendu d’une page d’erreur. La requête suivante nécessaire, telle que définie dans la directive `ErrorDocument`, échouerait également. Nous avons contourné ce problème en exécutant une tâche cron qui extrait régulièrement les pages d’erreur de leurs emplacements définis via `wget` et en les stockant dans des emplacements de fichiers statiques définis dans la directive `ErrorDocuments`.

**Références**

* [apache.org - Documents d’erreur personnalisés](https://httpd.apache.org/docs/2.4/custom-error.html)

### Mettre en cache du contenu protégé

Le Dispatcher ne vérifie pas les autorisations lorsqu’il diffuse une ressource par défaut. Il est mis en œuvre de cette manière à dessein, afin d’accélérer votre site web public. Si vous souhaitez protéger certaines ressources par une identification, vous disposez principalement de trois solutions :

1. Protégez la ressource avant que la requête n’atteigne le cache, par exemple, par une passerelle SSO (Single Sign On) devant le Dispatcher, ou en tant que module dans le serveur Apache.

2. Excluez les ressources sensibles de la mise en cache et, par conséquent, diffusez-les toujours en direct à partir du système de publication.

3. Utilisez la mise en cache sensible aux autorisations dans Dispatcher.

Et bien sûr, vous pouvez appliquer votre propre combinaison des trois approches.

**Option 1**. Une passerelle SSO peut de toute façon être appliquée par votre organisation. Si votre schéma d’accès est très général, vous n’aurez peut-être pas besoin d’informations d’AEM pour décider d’accorder ou de refuser l’accès à une ressource.

>[!NOTE]
>
>Ce modèle nécessite une _passerelle_ qui _intercepte_ chaque requête et effectue _l’autorisation_ (octroi ou refus de requêtes à Dispatcher). Si votre système d’authentification unique est un _authentificateur_ qui établit uniquement l’identité d’un utilisateur ou d’une utilisatrice, vous devez implémenter l’option 3. Des termes tels que SAML ou OAauth mentionnés dans le guide de votre système d’authentification unique indiquent que vous devez très probablement implémenter l’option 3.


**Option 2**. L’absence de mise en cache n’est généralement pas souhaitable. Si vous optez pour cette méthode, assurez-vous que le volume de trafic et le nombre de ressources sensibles qui sont exclues sont faibles. Vous pouvez également vous assurer que le système de publication dispose d’un cache en mémoire installé, afin que les systèmes de publication puissent gérer la charge qui en résulte. Plus de détails sont disponibles dans la partie III de cette série.

**Option 3**. La mise en cache sensible aux autorisations est une approche intéressante. Le Dispatcher met en cache une ressource, mais avant de la diffuser, il demande au système AEM l’autorisation de le faire. Cela crée une requête supplémentaire du Dispatcher vers l’instance de publication, mais évite généralement au système de publication d’afficher à nouveau le rendu d’une page si elle est déjà mise en cache. Cependant, cette approche nécessite une implémentation personnalisée. Pour plus d’informations, reportez-vous à l’article [Mise en cache sensible aux autorisations](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html?lang=fr).

**Références**

* [helpx.adobe.com - Mise en cache sensible aux autorisations](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html?lang=fr)

### Définir le délai de grâce

Si vous effectuez fréquemment une invalidation en courte succession, par exemple par une activation d’arborescence ou simplement par nécessité de garder votre contenu à jour, il se peut que vous vidiez constamment le cache et que vos visiteurs et visiteuses arrivent presque toujours sur un cache vide.

Le diagramme ci-dessous illustre un minutage possible lors de l’accès à une seule page.  Le problème s’aggrave bien sûr quand le nombre de pages différentes demandées augmente.

![Activations fréquentes générant un cache non valide la plupart du temps.](assets/chapter-1/frequent-activations.png)

*Activations fréquentes générant un cache non valide la plupart du temps.*

<br> 

Pour atténuer le problème de cette « tempête d’invalidation du cache », comme on l’appelle parfois, vous pouvez interpréter le `statfile` de manière moins rigoureuse.

Vous pouvez définir le Dispatcher pour utiliser `grace period` pour l’invalidation automatique. Cela ajouterait en interne du temps à la date de modification de `statfiles`.

Supposons que votre `statfile` ait une heure de modification fixée à aujourd’hui 12 h 00 et que votre `gracePeriod` soit défini à 2 minutes. Alors, tous les fichiers invalidés automatiquement sont considérés comme valides à 12 h 01 et à 12 h 02. Leur nouveau rendu est affiché après 12 h 02.

La configuration de référence propose un `gracePeriod` de deux minutes pour une bonne raison. Vous pourriez penser « Deux minutes ? C’est presque rien. Je peux facilement attendre 10 minutes que le contenu s’affiche... ».  Vous pouvez donc penser que définir une période plus longue, disons 10 minutes, est préférable, en supposant que le contenu s’affiche au moins après ces 10 minutes.

>[!WARNING]
>
>Ce n’est pas ainsi que le `gracePeriod` fonctionne. Le délai de grâce n’est _pas_ la durée au bout de laquelle un document est garanti invalidé, mais pendant laquelle aucune invalidation ne se produit. Chaque invalidation ultérieure qui tombe dans ce cadre _prolonge_ la période, qui peut être d’une durée indéfinie.

Illustrons le fonctionnement du `gracePeriod` par un exemple :

Imaginons que vous gérez un site multimédia et que votre personnel d’édition mette à jour le contenu toutes les 5 minutes. Pensez à définir le délai de grâce sur 5 minutes.

Nous commencerons par un court exemple à 12 h 00.

12 : 00 - Le fichier stat est défini sur 12 : 00. Tous les fichiers mis en cache sont considérés comme valides jusqu’à 12 h 05.

12 : 01 - Une invalidation se produit. Ceci prolonge le délai de grâce à 12 h 06.

12 : 05 - Une autre personne publie son article, prolongeant le délai de grâce avec un nouveau délai de grâce à 12 : 10.

Et ainsi de suite. Le contenu n’est alors jamais invalidé. Chaque invalidation *pendant* le délai de grâce prolonge effectivement le délai de grâce. Le `gracePeriod` est conçu pour résister à la tempête d’invalidation, mais vous devrez malgré tout sortir sous la pluie... alors gardez le `gracePeriod` beaucoup plus court pour éviter de devoir vous abriter pour le restant de votre vie.

#### Délai de grâce déterministe

Nous aimerions vous présenter une autre idée concernant la façon dont vous pouvez traverser une tempête d’invalidation. Ce n’est qu’une suggestion. Nous ne l’avons pas essayée en production, mais nous avons trouvé le concept assez intéressant pour avoir envie de vous faire part de l’idée.

Le `gracePeriod` peut devenir trop long si votre intervalle de réplication régulier est plus court que votre `gracePeriod`.

La solution alternative est la suivante : ne procédez à l’invalidation qu’à intervalles de temps fixes. Le temps écoulé entre les deux implique toujours de diffuser du contenu obsolète. L’invalidation se produira ultérieurement, mais un certain nombre d’invalidations sont collectées pour une invalidation « en bloc », de sorte que Dispatcher ait la possibilité de diffuser du contenu mis en cache entre-temps et de soulager le système de publication.

L’implémentation peut se présenter comme suit :

Vous utilisez un « Script d’invalidation personnalisé » (voir référence) qui s’exécute après l’invalidation. Ce script lit la date de la dernière modification du `statfile's` et l’arrondit à l’intervalle d’arrêt suivant. La commande Shell Unix `touch --time` vous permet de spécifier une heure.

Par exemple, si vous définissez le délai de grâce sur 30 secondes, Dispatcher arrondit la date de dernière modification du fichier stat aux 30 secondes suivantes. Les demandes d’invalidation qui se produisent dans l’intervalle définissent simplement la même période complète de 30 secondes.

![Le report de l’invalidation à la prochaine période complète de 30 secondes augmente le taux de réussite.](assets/chapter-1/postponing-the-invalidation.png)

*Le report de l’invalidation à la prochaine période complète de 30 secondes augmente le taux de réussite.*

<br> 

Les occurrences dans le cache qui se produisent entre la demande d’invalidation et le prochain cycle de 30 secondes sont alors considérées comme périmées ; il y a eu une mise à jour sur l’instance de publication, mais le Dispatcher continue de diffuser d’anciens contenus.

Cette approche peut aider à définir des délais de grâce plus longs sans avoir à craindre que les demandes suivantes prolongent le délai de manière indéterminée. Bien que, comme nous l’avons déjà dit, ceci ne soit qu’une suggestion et que nous n’ayons pas eu la chance de la tester.

**Références**

[helpx.adobe.com - Configuration de Dispatcher](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/dispatcher-configuration.html)

### Récupération automatique

Votre site a un modèle d’accès très particulier. Vous avez une charge importante de trafic entrant et la plupart du trafic est concentrée sur une petite partie de vos pages. La page d’accueil, les pages de destination de votre campagne et les pages détaillées de produits les plus présentés reçoivent 90 % du trafic. Ou, si vous gérez un nouveau site, les produits les plus récents génèrent un trafic plus élevé que les plus anciens.

Désormais, ces pages sont très probablement mises en cache dans le Dispatcher, car elles sont demandées fréquemment.

Une requête d’invalidation arbitraire est envoyée au Dispatcher, ce qui entraîne l’invalidation de toutes les pages (y compris les plus consultées).

Par la suite, comme ces pages sont si consultées, de nouvelles requêtes entrantes sont faites en provenance de différents navigateurs. Prenons la page d’accueil comme exemple.

Comme le cache n’est plus valide, toutes les requêtes sur la page d’accueil qui arrivent en même temps sont transférées au système de publication, ce qui génère une charge élevée.

![Requêtes parallèles vers la même ressource sur le cache vide : les requêtes sont transférées vers le système de publication.](assets/chapter-1/parallel-requests.png)

*Requêtes parallèles vers la même ressource sur le cache vide : les requêtes sont transférées vers le système de publication.*

Grâce à la récupération automatique, vous pouvez atténuer cela dans une certaine mesure. La plupart des pages invalidées sont toujours physiquement stockées sur le Dispatcher après l’invalidation automatique. Elles sont seulement _considérées_ comme obsolètes. La _récupération automatique_ signifie que vous diffusez toujours ces pages obsolètes pendant quelques secondes lors de l’initialisation d’_une seule_ requête au système de publication afin de récupérer à nouveau le contenu obsolète :

![Diffusion de contenu obsolète lors d’une récupération en arrière-plan.](assets/chapter-1/fetching-background.png)

*Diffusion de contenu obsolète lors d’une récupération en arrière-plan.*

<br> 

Pour activer la récupération, vous devez indiquer au Dispatcher les ressources à récupérer à la suite d’une invalidation automatique. N’oubliez pas que toute page que vous activez invalide également automatiquement toutes les autres pages, y compris les plus consultées.

La récupération signifie en réalité que le Dispatcher doit être informé, dans chaque (!) requête d’invalidation, que vous souhaitez récupérer les pages les plus consultées, et quelles sont les pages les plus consultées.

Pour ce faire, placez une liste d’URL de ressources (URL réelles, et pas seulement les chemins) dans le corps des requêtes d’invalidation :

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

Lorsque le Dispatcher voit une telle requête, il déclenche l’invalidation automatique comme d’habitude et met immédiatement les requêtes en file d’attente pour récupérer à nouveau du contenu du système de publication.

Comme nous utilisons maintenant un corps de requête, nous devons également définir le type de contenu et la longueur du contenu en fonction de la norme HTTP.

Le Dispatcher marque également les URL appropriées en interne afin de savoir qu’il peut diffuser ces ressources directement, même si elles sont considérées comme non valides par l’invalidation automatique.

Toutes les URL répertoriées sont demandées une par une. Ainsi, vous n’avez pas à vous soucier de créer une charge trop élevée sur les systèmes de publication. Mais il ne faut pas non plus inclure trop d’URL dans cette liste. En fin de compte, la file d’attente doit être traitée dans un délai limité afin de ne pas diffuser trop longtemps du contenu obsolète. Incluez simplement vos 10 pages les plus consultées.

Si vous examinez le répertoire du cache du Dispatcher, vous verrez des fichiers temporaires horodatés. Il s’agit des fichiers actuellement chargés en arrière-plan.

**Références**

[helpx.adobe.com - Invalidation de pages mises en cache depuis AEM](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/page-invalidate.html)

### Protéger le système de publication

Le Dispatcher offre un peu de sécurité supplémentaire en protégeant le système de publication des requêtes qui sont destinées uniquement à des fins de maintenance. Par exemple, vous ne souhaitez pas vous exposer vos URL `/crx/de` ou `/system/console` au public.

Il n’est pas inutile d’installer un pare-feu d’application web (WAF) dans votre système. Mais cela augmentera votre budget et tous les projets ne sont pas dans une situation où ils peuvent se permettre et surtout gérer et maintenir un pare-feu d’application web.

Nous voyons fréquemment un ensemble de règles de réécriture Apache dans la configuration du Dispatcher qui empêchent l’accès aux ressources les plus vulnérables.

Mais vous pouvez également envisager une approche différente :

Selon la configuration du Dispatcher, le module du Dispatcher est lié à un certain répertoire :

```
<Directory />
  SetHandler dispatcher-handler
  …
</Directory>
```

Mais pourquoi lier le gestionnaire à toute la docroot, alors que vous avez besoin de filtrer par la suite ?

Vous pouvez d’abord réduire la liaison du gestionnaire. `SetHandler` lie un gestionnaire à un répertoire, vous pouvez lier le gestionnaire à une URL ou à un modèle d’URL :

```
<LocationMatch "^(/content|/etc/design|/dispatcher/invalidate.cache)/.\*">
  SetHandler dispatcher-handler
</LocationMatch>

<LocationMatch "^/dispatcher/invalidate.cache">
  SetHandler dispatcher-handler
</LocationMatch>

…
```

Si vous procédez de cette manière, n’oubliez pas de toujours lier le Dispatcher/gestionnaire à l’URL d’invalidation de Dispatcher. Sinon, vous ne pourrez pas envoyer de requêtes d’invalidation d’AEM au Dispatcher.

Une alternative à l’utilisation du Dispatcher comme filtre consiste à configurer des directives de filtre dans le `dispatcher.any`.

```
/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }
```

Nous n’imposons pas l’usage d’une directive plutôt qu’une autre. Nous recommandons plutôt une combinaison appropriée de toutes les directives.

Mais nous proposons que vous envisagiez de réduire l’espace URL le plus tôt possible dans la chaîne, autant que vous le souhaitez, et de la manière la plus simple possible. Gardez tout de même à l’esprit que ces techniques ne remplacent pas un WAF sur des sites web hautement sensibles. Certaines personnes appellent ces techniques le « pare-feu du pauvre » pour une raison.

**Références**

[apache.org - Directive sethandler](https://httpd.apache.org/docs/2.4/mod/core.html#sethandler)

[helpx.adobe.com - Configuration de l’accès au filtre de contenu](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html?lang=fr#ConfiguringAccesstoContentfilter)

### Filtrage à l’aide d’expressions régulières et de globs

Jadis, vous ne pouviez utiliser que des « globs », des espaces réservés simples pour définir des filtres dans la configuration de Dispatcher.

Heureusement, cela a changé dans les versions ultérieures du Dispatcher. Vous pouvez désormais aussi utiliser des expressions régulières POSIX et accéder à différentes parties d’une demande pour définir un filtre. Pour une personne qui vient de commencer avec le Dispatcher, cela peut être considéré comme allant de soi. Mais si vous avez l’habitude d’utiliser uniquement des globs, c’est une sorte de surprise que l’on peut facilement négliger. De plus, la syntaxe des globs et des regex est trop similaire. Comparons deux versions qui font la même chose :

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

Vous voyez la différence ?

La version B utilise des guillemets simples `'` pour marquer un _modèle d’expression régulière_. « N’importe quel caractère » est exprimé à l’aide de `.*`.

Les _modèles d’extension métacaractère_, en revanche, utilisent des guillemets doubles `"` et vous pouvez uniquement utiliser des espaces réservés simples tels que `*`.

Si vous connaissez cette différence, aucun problème. Mais si ce n’est pas le cas, vous pouvez facilement mélanger les guillemets et passer un après-midi ensoleillé à déboguer votre configuration. Maintenant, vous savez.

« Je reconnais `'/url'` dans la configuration... Mais qu’est-ce que ce `'/glob'` dans le filtre ? ». Vous pourriez vous poser cette question.

Cette directive représente l’ensemble de la chaîne de requête, y compris la méthode et le chemin d’accès. Cela pourrait signifier :

`"GET /content/foo/bar.html HTTP/1.1"`

il s’agit de la chaîne à laquelle votre modèle serait comparé. Les personnes débutantes ont tendance à oublier la première partie, la `method` (GET, POST...). Donc, un modèle

`/0002  { /glob "/content/\*" /type "allow" }`

échoue toujours, car « /content » ne correspond pas au « GET » de la requête.

Donc, lorsque vous souhaitez utiliser des globs,

`/0002  { /glob "GET /content/\*" /type "allow" }`

cela serait correct.

Pour une règle de refus initiale,

`/0001  { /glob "\*" /type "deny" }`

c’est parfait. Toutefois, pour les autorisations suivantes, il est préférable, plus clair, plus expressif et beaucoup plus sûr d’utiliser les différentes parties d’une requête :

```
/method
/url
/path
/selector
/extension
/suffix
```

Comme ceci :

```
/005  {

  /type "allow"
  /method "GET"
  /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

Notez que vous pouvez mélanger des expressions regex et glob dans une règle.

Un dernier mot sur les « numéros de ligne », tels que `/005` devant chaque définition,

Ils n’ont aucun sens. Vous pouvez choisir des dénominateurs arbitraires pour les règles. L’utilisation des nombres ne nécessite pas beaucoup d’effort pour penser à un schéma, mais gardez à l’esprit que l’ordre est important.

Si vous avez des centaines de règles de ce type :

```
/001
/002
/003
…
/100
…
```

et vous souhaitez en insérer une entre /001 et /002, que se passe-t-il avec les numéros suivants ? Augmentez-vous leur nombre ? Insérez-vous des nombres intermédiaires ?

```
/001
/001a
/002
/003
…
/100
…
```

Et si vous changez pour réorganiser /003 et /001, allez-vous changer les noms et leur identité ou

```
/003
/002
/001
…
/100
…
```

La numérotation, qui semble être un choix simple au départ, atteint ses limites à long terme. Soyons honnêtes, choisir des nombres comme identifiants est de toute façon un mauvais style de programmation.

Nous aimerions proposer une approche différente. Il est probable que vous ne trouverez pas d’identifiants significatifs pour chaque règle de filtre individuelle. Mais ils ont probablement un but plus important, et peuvent donc être regroupés d’une manière ou d’une autre selon ce but. Par exemple, « configuration de base », « exceptions spécifiques à l’application », « exceptions globales » et « sécurité ».

Vous pouvez ensuite nommer et associer les règles en conséquence et fournir au lecteur ou à la lectrice de la configuration (votre collègue), une certaine orientation dans le fichier :

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


Vous ajouterez probablement une nouvelle règle à l’un des groupes, voire vous créerez un nouveau groupe. Dans ce cas, le nombre d’éléments à renommer/renuméroter est limité à ce groupe.

>[!WARNING]
>
>Des configurations plus sophistiquées répartissent les règles de filtrage dans plusieurs fichiers, qui sont inclus par le fichier de configuration principal `dispatcher.any`. Un nouveau fichier n’introduit toutefois pas de nouvel espace de noms. Ainsi, si vous avez une règle « 001 » dans un fichier et « 001 » dans un autre, vous obtiendrez une erreur. Raison de plus pour trouver des noms sémantiquement forts.

**Références**

[helpx.adobe.com - Création de modèles pour les propriétés glob](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html?lang=fr#DesigningPatternsforglobProperties)

### Spécification du protocole

Notre dernier conseil n’en est pas vraiment un, mais nous avons pensé que nous devions vous en faire part.

Dans la plupart des cas, AEM et Dispatcher sont prêts à l’emploi. Vous ne trouverez donc pas de spécification de protocole Dispatcher complète sur le protocole d’invalidation pour créer votre propre application. L’information est publique, mais un peu éparpillée sur un certain nombre de ressources.

Nous essayons ici de combler cette lacune dans une certaine mesure. Voici à quoi ressemble une requête d’invalidation :

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

`POST /dispatcher/invalidate.cache HTTP/1.1` : la première ligne correspond à l’URL du point d’entrée de contrôle du Dispatcher et il est probable que vous ne la modifierez pas.

`CQ-Action: <action>` : ce qui devrait se passer. `<action>` est :

* `Activate:` supprime `/path-pattern.*`.
* `Deactive:` supprime `/path-pattern.*` ET supprime `/path-pattern/*`.
* `Delete:` supprime `/path-pattern.*` ET supprime `/path-pattern/*`.
* `Test:` renvoie « OK » mais ne fait rien.

`CQ-Handle: <path-pattern>` : chemin de la ressource de contenu à invalider. Remarque : `<path-pattern>` est en réalité un « chemin » et non un « motif ».

`CQ-Action-Scope: ResourceOnly` : facultatif, si cet en-tête est défini, le fichier `.stat` n’est pas modifié.

```
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]
```

Définissez ces en-têtes si vous définissez une liste d’URL de récupération automatique. `<bytes in request body>` est le nombre de caractères dans le corps HTTP.

`<newline>` : si vous disposez d’un corps de requête, il doit être séparé de l’en-tête par une ligne vide.

```
<refetch-url-1>
<refetch-url-2>
…
<refetch-url-n>
```

Liste des URL que vous souhaitez récupérer immédiatement après l’invalidation.

## Ressources supplémentaires

Vue d’ensemble et présentation de la mise en cache de Dispatcher : [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/dispatcher.html).

Documentation du Dispatcher avec toutes les directives expliquées : [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/dispatcher-configuration.html).

Questions fréquentes : [https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html](https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html?lang=fr).

Enregistrement d’un webinaire sur l’optimisation de Dispatcher (vivement recommandé) : [https://my.adobeconnect.com/p7th2gf8k43?proto=true](https://my.adobeconnect.com/p7th2gf8k43?proto=true).

Présentation de la conférence « adaptTo() », « Le pouvoir sous-estimé de l’invalidation du contenu » de 2018 à Potsdam [https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html](https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html).

Invalidation de pages mises en cache à partir d’AEM : [https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html](https://helpx.adobe.com/fr/experience-manager/dispatcher/using/page-invalidate.html).

## Étape suivante

* [2 - Modèle d’infrastructure](chapter-2.md)
